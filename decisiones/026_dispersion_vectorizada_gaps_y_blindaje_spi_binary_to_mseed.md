---
id: ADR-026
titulo: Dispersión Vectorizada O(N) para Gaps Temporales, Preservación de Fase y Blindaje de Ruido SPI en binary_to_mseed
estado: Aceptado
fecha: 2026-10-26
temas: [binary_to_mseed, livelock, gaps, fase_sismica, numpy, steim1, obspy, spi_glitch, deduplicacion, resiliencia]
entorno: acelerografo
---

# ADR-026: Dispersión Vectorizada O(N) para Gaps Temporales, Preservación de Fase y Blindaje de Ruido SPI en binary_to_mseed

## Estado

**Aceptado** | Fecha: 2026-10-26

## Contexto

Durante las pruebas de despliegue y contingencia de empaquetado diario SFTP en la estación acelerográfica `DEV00` ([ADR-025](025_robustecimiento_cerrojos_y_control_espacio_disco.md)), se detectó un bloqueo severo de CPU en el proceso `binary_to_mseed.py` al intentar convertir binarios `.dat` procedentes de la estación remota `CHA02` que contenían pérdidas temporales masivas de paquetes (más de $3500$ segundos faltantes). En una Raspberry Pi 3B+, la conversión de un único archivo horario demoraba más de 20 minutos con la CPU al 100%, congelando el pipeline de adquisición e impidiendo la subida de datos a Google Drive.

La investigación y diagnóstico técnico forense detallado en `RSA-Acelerografo/docs/analysis/2026-10-22_diagnostico_bloqueo_cpu_relleno_gaps_binary_to_mseed.md` identificó siete defectos interrelacionados en el motor de conversión:

1. **Livelock de CPU $O(M \cdot N)$ por `np.insert` recursivo (H1)**: Al rellenar los segundos faltantes con ceros, `obtenerTraza` ejecutaba `np.insert` dentro de un bucle Python para cada segundo de gap. Al insertar $M$ gaps en un arreglo de $N$ muestras, NumPy reasignaba y copiaba la memoria repetidamente, alcanzando complejidades de $O(M \cdot N)$ ($> 9 \times 10^8$ operaciones para 3600 gaps).
2. **Corrupción de Fase Sismológica (H2)**: La inserción recursiva modificaba las posiciones absolutas de los bloques ya copiados, desfasando en el tiempo las ondas sísmicas posteriores y truncando el final de la traza para forzar la dimensión horaria.
3. **Inflación Dimensional Doble en `npts` (H3)**: El cálculo original `npts = len(data) + len(segundos_faltantes) * fsample` sumaba los ceros a un arreglo que ya los contenía, duplicando artificialmente las dimensiones de la traza MiniSEED.
4. **Falsos Cruces de Medianoche y Desfases Diurnos (H4a)**: La detección de saltos temporales asumía ciegamente que cualquier salto negativo ($t_{\text{sig}} < t_{\text{ant}}$) correspondía a un cruce de medianoche, sumando $+86400$ s a eventos ocurridos en pleno día.
5. **Colapso por Timestamps Duplicados (H4c)**: Tramas consecutivas con el mismo timestamp generaban saltos de 0 segundos que no eran filtrados, inflando la señal.
6. **Desacople del Timestamp de Referencia (H6)**: `extraer_tiempo_binario()` leía únicamente la trama física 0; si dicha trama contenía ruido, el MiniSEED se nombraba y fechaba erróneamente, ignorando que `leer_archivo_binario()` sí descartaba esa trama y empezaba en la trama 1.
7. **Inflación Masiva a 25 horas por Glitches de Bus SPI (H7)**: En el archivo patológico `CHA2_260928-170000.dat`, una trama corrupta con bytes repetidos `0x04 0x04 0x04` (`04:04:04`) superó la máscara de rango numérico diurno; el conversor interpretó el salto $17:47 \to 04:04$ como medianoche, inyectando $86{,}402$ segundos de ceros e inflando el MiniSEED a $22{,}500{,}000$ muestras (81 MB).

---

## Opciones Evaluadas

### Decisión 1: Estrategia de Relleno de Gaps en `obtenerTraza`
- **Opción A (Seleccionada)**: *Preallocado Directo y Dispersión Vectorial $O(N)$ en NumPy*.
  Se preasigna un arreglo de ceros de dimensión total exacta `npts_completo = duracion_total_seg * fsample`. Para cada segundo presente en el binario, se calcula su posición relativa `idx_destino = seg_relativo * fsample` y se copia en un solo paso el bloque de 250 muestras.
  - *Ventajas*: Complejidad estrictamente lineal $O(N)$, cero reasignaciones de memoria, ejecución en $< 0.1$ s en Raspberry Pi, preservación matemática exacta de la fase de las ondas sísmicas.
  - *Desventajas*: Asume que los datos están desglosados en bloques enteros de muestreo por segundo (lo cual es la especificación del firmware del acelerógrafo a 250 Hz).
- **Opción B**: Crear trazas discontinuas independientes y ensamblarlas con `obspy.Stream.merge(method=0, fill_value=0)`.
  - *Ventajas*: Reutiliza funcionalidad nativa de ObsPy.
  - *Desventajas*: Alto consumo de memoria intermedia en RPi 3B+, overhead de creación de cientos de objetos `Trace`, y lentitud en archivos con micro-gaps frecuentes.
- **Opción C**: Mantener `np.insert` pero acumulando los índices para una única llamada masiva.
  - *Ventajas*: Reduce reasignaciones.
  - *Desventajas*: `np.insert` masivo en NumPy sigue duplicando el arreglo completo y no resuelve la corrupción de fase relativa de bloques posteriores.

### Decisión 2: Acotamiento Contextual de Cruce de Medianoche
- **Opción A (Seleccionada)**: *Ventana Limítrofe Condicional de Medianoche*.
  El ajuste modular $+86400$ s solo se aplica si la trama anterior pertenece a la hora 23 ($t_{\text{ant}} \ge 86000$ s) y la trama posterior a la hora 00 ($t_{\text{sig}} \le 3600$ s). Cualquier salto negativo fuera de esta ventana se clasifica como anomalía o trama corrupta y se ignora para el cálculo de gaps.
  - *Ventajas*: Elimina falsos cruces de medianoche diurnos y neutraliza saltos de 24 horas ante glitches de hardware.
  - *Desventajas*: Ninguna.
- **Opción B**: Ajuste modular incondicional en `np.diff(tiempos) % 86400`.
  - *Ventajas*: Matemáticamente simple.
  - *Desventajas*: Cualquier salto anómalo diurno hacia atrás (ej. $17:47 \to 04:04$) se convierte automáticamente en $+72777$ s de ceros falsos.

### Decisión 3: Triple Barrera contra Ruido de Bus SPI y Tramas Corruptas
- **Opción A (Seleccionada)**: *Triple Barrera de Filtrado e Integridad*.
  1. *Filtro de bytes idénticos en cabecera cruda*: Purga inmediata en desempaquetado si `año == mes == día == hora == minuto == segundo` (`glitch_bytes_identicos`).
  2. *Filtro de coherencia cronológica*: Purga de tramas intermedias outlier ($|t_k - t_{k-1}| > 60$ s) cuando la trama siguiente $k+1$ mantiene continuidad con $k-1$ ($0 \le t_{k+1} - t_{k-1} \le 60$ s).
  3. *Principio de descarte sin aproximación*: Si una trama tiene cabecera inválida, se purga; nunca se interpola ni se inventa el tiempo de muestras no certificadas.
  - *Ventajas*: Erradica el $100\%$ de los casos de inflación dimensional en archivos reales de CHA02 sin afectar la continuidad sísmica legítima.
  - *Desventajas*: Descarta el segundo correspondiente a la trama corrupta (rellenándolo con ceros sismológicos).
- **Opción B**: Intentar reparar el timestamp de la trama corrupta interpolando $t_k = t_{k-1} + 1$.
  - *Ventajas*: Conserva las muestras de datos de la trama.
  - *Desventajas*: Violación del principio sismológico de trazabilidad de tiempo; si el ruido SPI afectó la cabecera, los datos analógicos de aceleración de esa trama están comprometidos.

### Decisión 4: Deduplicación Vectorial de Timestamps
- **Opción A (Seleccionada)**: `np.unique` sobre `tiempos_np` preservando primera ocurrencia cronológica y rebanado vectorial tridimensional `(3, N_tramas, fsample)`.
  - *Ventajas*: Overhead de cálculo $O(1)$, deduplica simultáneamente los 3 canales triaxiales sin bucles Python.
  - *Desventajas*: Ninguna.
- **Opción B**: Bucle manual `if t not in vistos:`.
  - *Ventajas*: Intuitivo.
  - *Desventajas*: Lento en Python para arreglos de 3600 elementos por canal.

### Decisión 5: Desacople y Retorno de `tiempo_binario_validado`
- **Opción A (Seleccionada)**: Extender el retorno de `leer_archivo_binario` de 5 a 6 valores, entregando `tiempo_binario_validado` basado en la primera trama que efectivamente superó todos los filtros de integridad, y sincronizarlo en `procesar_archivo_individual`.
  - *Ventajas*: Erradica el desacople donde el archivo se fechaba con una trama corrupta descartada.
  - *Desventajas*: Requiere actualizar los desempaquetados internos (manteniendo la interfaz externa `extraer_tiempo_binario` para compatibilidad de subida).

---

## Decisión

Se aprueba e implementa la combinación de:
1. **Dispersión Vectorial $O(N)$** con preallocado directo en `obtenerTraza`.
2. **Preservación estricta de fase** mediante mapeo indexado de bloques de 250 muestras.
3. **Deduplicación vectorial tridimensional** en `leer_archivo_binario`.
4. **Acotamiento contextual de medianoche** restringido a la ventana $23 \to 00$.
5. **Triple barrera contra glitches SPI** (descarte por bytes repetidos, filtro de coherencia cronológica y política de no aproximación).
6. **Sincronización de timestamp validado** en la firma de `leer_archivo_binario`.
7. **Suite de pruebas unitarias sintéticas (T1 a T11)** en `test_binary_to_mseed.py`.

---

## Consecuencias

### Consecuencias Positivas
- **Erradicación del livelock**: El tiempo de conversión en Raspberry Pi 3B+ bajó de **> 20 minutos a 0.08 segundos** en archivos con gaps masivos, y a **0.95 segundos** en archivos nominales de 1 hora (aceleración superior a $1000\times$).
- **Integridad de datos sísmicos**: Se garantiza que la llegada de fases $P$ y $S$ tras un gap conserve su estampa de tiempo absoluto real.
- **Normalización de almacenamiento**: El archivo patológico `CHA2_260928-170000.dat` se redujo de **81 MB (25 horas)** a su tamaño nominal exacto de **8.6 MB (1.00 hora)**.
- **Validación masiva exitosa**: 24 de 25 archivos reales de la jornada de prueba de CHA02 convirtieron exitosamente con 900,000 muestras exactas.
- **Cobertura de tests**: La suite `test_binary_to_mseed.py` ejecuta 12 pruebas sintéticas en $< 0.1$ s, protegiendo al repositorio contra futuras regresiones.

### Consecuencias Negativas / Deuda Técnica Identificada
- En archivos interrumpidos por corte de energía donde la última trama física en disco queda truncada (menos de 2506 bytes), la función secundaria `extraer_timestamp_completo_binario` produce `None`, provocando un fallo en cascada por falta de claves `'anio'` en `tiempo_binario_validado` (aislado y documentado en el diagnóstico `2026-10-26_diagnostico_desacople_timestamp_archivos_incompletos_binary_to_mseed.md`).

---

## Referencias

- Diagnóstico técnico de origen: [`montajes/acelerografo-DEV00/docs/analysis/2026-10-22_diagnostico_bloqueo_cpu_relleno_gaps_binary_to_mseed.md`](file:///home/rsa/git/montajes/acelerografo-DEV00/docs/analysis/2026-10-22_diagnostico_bloqueo_cpu_relleno_gaps_binary_to_mseed.md)
- Blueprint de implementación: [`montajes/acelerografo-DEV00/docs/blueprints/2026-10-23_plan_refactorizacion_gaps_binary_to_mseed.md`](file:///home/rsa/git/montajes/acelerografo-DEV00/docs/blueprints/2026-10-23_plan_refactorizacion_gaps_binary_to_mseed.md)
- Contexto técnico del conversor: [`montajes/acelerografo-DEV00/docs/context/binary_to_mseed_context.md`](file:///home/rsa/git/montajes/acelerografo-DEV00/docs/context/binary_to_mseed_context.md)
- Contexto técnico de tests: [`montajes/acelerografo-DEV00/docs/context/test_binary_to_mseed_context.md`](file:///home/rsa/git/montajes/acelerografo-DEV00/docs/context/test_binary_to_mseed_context.md)
- Diagnóstico técnico derivado (trabajo futuro): [`montajes/acelerografo-DEV00/docs/analysis/2026-10-26_diagnostico_desacople_timestamp_archivos_incompletos_binary_to_mseed.md`](file:///home/rsa/git/montajes/acelerografo-DEV00/docs/analysis/2026-10-26_diagnostico_desacople_timestamp_archivos_incompletos_binary_to_mseed.md)
