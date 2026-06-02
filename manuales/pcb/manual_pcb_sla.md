---
title: Manual PCB SLA
parent: PCB
nav_order: 1
---

# Manual para prototipado de PCB con impresora de resina, plasificadora y mesa UV

**Enfoque:** Validación de circuitería (componentes DIP) previa a rediseño SMD y fabricación profesional en JLCPCB
**Tiempo por prototipo (sin máscara):** 1 – 1.5 horas
**Tiempo por prototipo (con máscara permanente):** 2 – 2.5 horas

## 1. Propósito de este manual

| **SÍ se valida** | **NO se valida (para eso está JLCPCB después)** |
| --- | --- |
| Topología del circuito | Optimización EMC |
| Niveles de tensión y corriente | Ruteo de alta frecuencia |
| Lógica de compuertas, microcontroladores, sensores | Acoples y filtros definitivos |
| Footprints funcionales (versión DIP) | Footprints SMD finales |
| Interconexión entre bloques | Integridad de señal en alta velocidad |

El prototipo resultante es **funcionalmente equivalente** al diseño final, pero **físicamente diferente**.

## 2. Materiales y equipos necesarios

### 2.1 Equipos principales

| **Equipo** | **Especificación** |
| --- | --- |
| Impresora de resina 3D | Photon o similar (405 nm) |
| Plastificadora | Sin calor o a baja temperatura (<40°C) |
| Mesa de curado UV | Para post-exposición |
| Horno (opcional, para máscara permanente) | Capaz de mantener 150°C |
| CNC | Perforado y corte |

### 2.2 Consumibles base (siempre necesarios)

| **Material** | **Especificación** |
| --- | --- |
| Placa FR4 cobreada | Doble cara |
| Resina UV estándar (para grabado) | Transparente o verde, 405 nm |
| Lámina superior | PET (reutilizable) o acetato (desechable) |
| Alcohol isopropílico | 99% |
| Cloruro férrico o persulfato | Grabado químico |
| Acetona | Remoción de resina (uso puntual) |
| Lija fina | #1000 – #1500 |
| Paños sin pelusa | Limpieza |
| Pasadores (clavos sin cabeza o alambre rígido) | Registro mecánico |
| Guantes de nitrilo, gafas, ventilación | Seguridad |

### 2.3 Consumible opcional (para máscara antisoldante permanente)

| **Material** | **Especificación** |
| --- | --- |
| Resina antisoldante UV permanente | Líquida fotoreactiva (LPI). Marcas: Mechanic UV, Taiyo Ink, Hotop. Color: verde, azul, rojo, negro. |
| Carbonato de sodio | (Sosa Solvay) para revelado – **NO usar alcohol** |
| Horno | Capaz de mantener 150°C durante 30 minutos |

## 3. Reglas de diseño para este prototipo

| **Parámetro** | **Valor mínimo recomendado** |
| --- | --- |
| Ancho de pista | 0.4 mm |
| Espacio entre pistas | 0.4 mm |
| Diámetro de pad (DIP) | 1.5 mm (agujero 0.8 mm) |
| Diámetro de vía manual | Pad 1.2 mm / agujero 0.8 mm |
| Agujeros de registro | 3 en esquinas, diámetro 2.5 mm |

**No usar este proceso para:** pistas <0.3 mm, BGA, QFN de paso fino, alta frecuencia no validada.

## 4. Lámina superior: PET o acetato

| **Tipo** | **Ventaja** | **Precaución** |
| --- | --- | --- |
| PET (poliéster) | Reutilizable, resistente a alcohol/acetona | Más caro |
| Acetato (papelería o dibujo técnico) | Bajo costo, fácil de conseguir | **Retirar antes del revelado** (se disuelve con alcohol) |

**Recomendación:** usa PET si tienes. Si usas acetato, retíralo inmediatamente después de la exposición y antes del baño de alcohol.

## 5. Flujo base (sin máscara antisoldante) – Para validación rápida

Este es el flujo mínimo para obtener un prototipo funcional en **1-1.5 horas**.

### 5.1 Preparación del diseño en CAD

* Dibujar el circuito con componentes **DIP**.
* Ruteo funcional (sin optimización EMC).
* Añadir **tres agujeros de registro** de 2.5 mm en esquinas.
* Exportar imágenes BMP/PNG monocromas escala 1:1:
  + cara\_superior.bmp (negro = resina curada)
  + cara\_inferior.bmp (espejada horizontalmente)

### 5.2 Preparación mecánica de la placa

1. Cortar la placa FR4 con sobrante de 5-10 mm por lado.
2. Lijar **ambas caras** con lija #1000 hasta superficie mate.
3. Limpiar con alcohol isopropílico y paño sin pelusa.
4. **Perforar los agujeros de registro** con broca de 2.5 mm (CNC o taladro de columna).
5. Introducir los pasadores en los agujeros sobre una base plana.

### 5.3 Aplicación de resina estándar con plastificadora (cara superior)

1. Depositar **4-5 gotas de resina estándar** en el centro de la placa.
2. Cubrir con lámina superior (PET o acetato).
3. Pasar por la plastificadora:
   * **Temperatura: ambiente o <40°C**
   * Presión máxima
   * Velocidad lenta
4. Retirar exceso de resina de los bordes.

### 5.4 Exposición en impresora (cara superior)

* Tiempo: **2.0 – 2.2 segundos** (resina estándar)
* Cargar imagen superior

### 5.5 Revelado (cara superior)

* Retirar lámina superior (con acetato: antes del alcohol; con PET: puede ser después)
* Sumergir en alcohol isopropílico 30-60 segundos
* Cepillo suave
* Enjuagar

### 5.6 Curado con mesa UV (cara superior)

* 2-3 minutos en mesa UV

### 5.7 Repetir pasos 5.3 a 5.6 para la cara inferior

* Imagen espejada
* Mismo tiempo de exposición

### 5.8 Grabado químico

* Cloruro férrico o persulfato a 40-50°C
* 10-15 minutos, agitación cada 2-3 minutos

### 5.9 Remoción de la máscara de resina

* Alcohol + cepillo suave
* Acetona solo si es necesario

### 5.10 Perforado funcional (CNC)

* Usando agujeros de registro como referencia

### 5.11 Vías manuales

* Alambre estañado 0.6-0.7 mm
* Soldar ambas caras

### 5.12 Corte final

* Fresa de 1.5-2 mm

### 5.13 Prueba eléctrica rápida

* Continuidad y ausencia de cortos

## 6. Módulo opcional: Máscara antisoldante permanente

**¿Cuándo usarlo?**

* El prototipo funcionó y quieres usarlo durante semanas o meses.
* Vas a realizar múltiples sesiones de soldadura/desoldadura.
* Necesitas proteger las pistas de oxidación o humedad.

**?? No usar resina estándar para esto.** La resina común de impresora no resiste la temperatura de soldadura. Se necesita **resina antisoldante UV permanente** (LPI).

### 6.1 Materiales adicionales necesarios

* Resina antisoldante UV permanente (Mechanic UV, Taiyo Ink, o similar)
* Carbonato de sodio (para revelado)
* Horno (capaz de 150°C)

### 6.2 Momento de aplicación

**Después del paso 5.11 (vías manuales) y antes de la prueba eléctrica.**

La placa ya tiene:

* Pistas grabadas
* Agujeros perforados
* Vías manuales soldadas

### 6.3 Flujo de aplicación de máscara permanente

#### Paso A – Limpieza profunda

* Limpiar la placa con alcohol isopropílico
* Dejar secar completamente
* **No debe haber grasa, huellas dactilares ni residuos**

#### Paso B – Preparación del diseño de máscara

* En tu CAD, exportar la **capa de máscara antisoldante** (solder mask)
* Polaridad: **negro = zonas SIN máscara (pads y agujeros deben quedar libres)**
* Blanco = zonas CON máscara (pistas protegidas)

#### Paso C – Aplicación de la resina antisoldante

**Diferencia clave:** la resina antisoldante es más viscosa que la resina estándar.

* Depositar una línea de resina antisoldante en un borde de la placa.
* Extender con **tarjeta de plástico rígida** (no plastificadora, es demasiado agresiva).
* Capa **muy fina** (debe verse el cobre al trasluz).
* Si usas plastificadora: presión mínima, pasar solo una vez.

#### Paso D – Pre-secado al horno (¡obligatorio!)

* **80°C durante 5-7 minutos**
* Esto elimina solventes volátiles. Sin este paso, la máscara quedará pegajosa.

#### Paso E – Exposición en impresora

* Colocar la placa sobre la pantalla (resina hacia abajo, o hacia arriba según tu montaje).
* **Tiempo de exposición: 15 – 30 segundos** (mucho más que con resina estándar)
* Usar la imagen de la máscara (blanco = zonas a curar)

| **Color de máscara** | **Tiempo estimado (405 nm)** |
| --- | --- |
| Verde claro | 15-18 s |
| Verde oscuro | 20-25 s |
| Azul / Rojo | 20-30 s |
| Negro | 30-40 s |

#### Paso F – Revelado (¡NO USAR ALCOHOL!)

**El alcohol destruye la máscara no expuesta.**

Revelador correcto: **Carbonato de sodio al 1%**

* Disolver **10 g de carbonato de sodio** en **1 litro de agua caliente** (40-50°C).
* Sumergir la placa.
* Frotar suavemente con una brocha o cepillo suave.
* La máscara no expuesta se disolverá como una "lechada".
* Tiempo: 1-2 minutos.

**Resultado esperado:** los pads y agujeros quedan **limpios (cobre visible)** y el resto de la placa está cubierta por máscara.

#### Paso G – Curado final al horno (resistencia térmica)

* **150°C durante 30 minutos**
* Esto polimeriza completamente la máscara y le da resistencia a la soldadura.

#### Paso H – Enfriamiento

* Dejar enfriar la placa a temperatura ambiente antes de manipular.

### 6.4 Verificación de la máscara

* La máscara debe estar dura, lisa y bien adherida.
* Los pads deben estar completamente libres de residuos.
* Si hay máscara sobre un pad: repetir revelado local con un pincel y carbonato.

## 7. Calibración (resina estándar para grabado)

| Tiempo en impresora (s) | Con mesa UV post-revelado | Pista 0.4 mm | Espacio 0.4 mm | Estado |
| --- | --- | --- | --- | --- |
| 1.8 | Sí | Rota | Bien | ? |
| **2.0** | Sí | Bien | Bien | ? Mínimo óptimo |
| **2.2** | Sí | Bien | Bien | ? Recomendado |
| 2.5 | Sí | Ligeramente engrosada | Bien | ?? Aceptable |
| 3.0 | Sí | Engrosada | Cerrado | ? |

**Tu tiempo objetivo: 2.0 – 2.2 segundos**

## 8. Calibración de máscara antisoldante permanente

| **Tiempo en impresora (s)** | **Pre-secado 80°C** | **Revelado con carbonato** | **Curado 150°C** | **Resultado** |
| --- | --- | --- | --- | --- |
| 10 | Sí | Se despega toda | Sí | ? Subexpuesto |
| **18** | Sí | Pads limpios, máscara firme | Sí | ? Óptimo (verde claro) |
| **25** | Sí | Pads limpios, máscara firme | Sí | ? Óptimo (verde oscuro) |
| 35 | Sí | Difícil de revelar | Sí | ?? Sobreexpuesto |

**Ajusta según tu resina y color.**

## 9. Resumen de tiempos por etapa

| **Etapa** | **Sin máscara** | **Con máscara permanente** |
| --- | --- | --- |
| Lijado + limpieza | 10 min | 10 min |
| Agujeros de registro | 5 min | 5 min |
| Aplicación resina grabado (2 caras) | 5 min | 5 min |
| Exposición grabado (2 caras) | 5 min | 5 min |
| Revelado grabado | 10 min | 10 min |
| Curado UV grabado | 5 min | 5 min |
| Grabado químico | 10-15 min | 10-15 min |
| Remoción resina | 5 min | 5 min |
| Perforado funcional | 15 min | 15 min |
| Vías manuales | 10-20 min | 10-20 min |
| **Subtotal** | **1.5 – 2 h** | **1.5 – 2 h** |
| Aplicación máscara permanente | – | 5 min |
| Pre-secado (horno 80°C) | – | 7 min |
| Exposición máscara | – | 30 s |
| Revelado (carbonato) | – | 2 min |
| Curado final (horno 150°C) | – | 30 min |
| Corte final | 5 min | 5 min |
| **Total** | **1.5 – 2 h** | **2 – 2.5 h** |

## 10. Tabla de decisiones rápida

| Situación | ¿Usar máscara permanente? |
| --- | --- |
| Validación rápida (unas horas, un día) | ? No |
| Pruebas durante una semana | ?? Opcional |
| Prototipo que se usará durante meses | ? Sí |
| Múltiples sesiones de soldadura/desoldadura | ? Sí |
| Ambiente húmedo o sucio | ? Sí |
| El prototipo se guardará como referencia | ? Sí |

## 11. Problemas típicos y soluciones

| **Problema** | **Causa probable** | **Solución** |
| --- | --- | --- |
| La resina de grabado se despega | Mala limpieza, cobre liso | Lijar más, limpiar con alcohol |
| Pistas rotas o muy finas | Subexposición | Aumentar tiempo 0.2-0.3 s |
| Pistas engrosadas o puentes | Sobreexposición o capa gruesa | Reducir tiempo o usar plastificadora |
| Máscara permanente pegajosa post-revelado | Falta de pre-secado | Horno 80°C 5-7 min antes de exponer |
| La máscara permanente no revela (todo cubierto) | Sobreexposición | Reducir tiempo de exposición |
| La máscara permanente se despega toda | Subexposición o carbonato muy concentrado | Aumentar tiempo, diluir carbonato al 1% |
| La máscara se levanta al soldar | Curado final insuficiente | Horno 150°C 30 minutos mínimo |

## 12. Notas finales importantes

### Sobre la plastificadora

* **No usar calor** para resina de grabado.
* Para máscara permanente: mejor usar tarjeta manual. La plastificadora puede ser demasiado agresiva.

### Sobre la mesa de curado UV

* Para resina de grabado: 2-3 minutos por cara.
* Para máscara permanente: el curado final se hace en el horno, no en la mesa UV (aunque una pasada rápida por mesa UV antes del horno ayuda).

### Sobre el horno

* Necesitas un horno que mantenga **150°C estables**.
* Un horno de cocina pequeño sirve, pero usa un termómetro interno para verificar.
* **No uses el mismo horno para comida después de curar resinas.**

### Sobre el destino del prototipo

* Este prototipo **no se convierte en el producto final**.
* La máscara permanente ayuda a que dure durante las pruebas, pero el diseño final se rediseña en SMD para JLCPCB.

## 13. Cierre

| Flujo | Tiempo | Cuándo usarlo |
| --- | --- | --- |
| **Base (sin máscara)** | 1-1.5 h | Validación rápida, "¿funciona mi circuito?" |
| **Con máscara permanente** | 2-2.5 h | Prototipo para pruebas prolongadas |

Tu equipamiento (Photon + plastificadora + mesa UV + horno) cubre **ambos flujos por completo**.

* Fallar cuesta **horas**.
* Corregir cuesta **minutos**.
* Llegar a JLCPCB con un diseño validado ahorra **semanas**.
