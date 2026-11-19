---
id: ADR-029
titulo: Desacoplamiento de Runtime y Pipeline de Empaquetado ZIP Mensual para Auscultación Óptica de Presas
estado: Aceptado
fecha: 2026-11-19
temas: [concentrador, coordinometro, desacoplamiento-runtime, zip, rsync, adhoc, windows11, almacenamiento]
entorno: concentrador, coordinometro
---

# ADR-029: Desacoplamiento de Runtime y Pipeline de Empaquetado ZIP Mensual para Auscultación Óptica de Presas

## Estado

**Aceptado** | Fecha: 2026-11-19

## Contexto

El sistema de auscultación de péndulos en galerías y pozos verticales de presas opera mediante coordinómetros ópticos (Raspberry Pi con Raspbian 9 Stretch) y concentradores pasarela (Raspberry Pi con Raspbian 11 Bullseye) comunicados a través de una red inalámbrica Ad-Hoc 802.11 (IBSS Canal 5 / 2432 MHz).

Durante la fase de laboratorio y preparación para campo se identificaron cuatro limitaciones arquitectónicas críticas:
1. **Acoplamiento de Runtime con Git:** Los repositorios se clonaban en las Raspberry Pi mezclando código fuente con configuraciones locales por estación (`concentrador.json`, `coordinometro.json`), logs rotativos y almacenamiento masivo de imágenes. Esto ocasionaba conflictos de fusión al ejecutar `git pull` y riesgo de sobreescritura de parámetros de campo y credenciales.
2. **Incompatibilidad de Nombres de Fotos con Windows 11:** El software de captura legacy nombraba las fotos con fecha y hora completa incluyendo espacios y dos puntos (`2023-02-24 14:00:10.710880.jpg`). Los caracteres `:` son ilegales en sistemas de archivos NTFS y FAT de Windows, impidiendo la extracción de archivos, navegación en red o sincronización con PCs de auscultación en oficina técnica.
3. **Limitación Severa de MicroSD en Coordinómetros:** Cada coordinómetro almacena miles de fotografías históricas (~5.805 fotos por nodo, ~520 MB). La tarjeta MicroSD no cuenta con espacio libre suficiente para soportar procesos de migración que dupliquen archivos temporalmente en disco.
4. **Saturación de Inodos y Rendimiento de Exploración:** Mantener miles de archivos JPEG individuales sueltos en disco degrada el rendimiento de los sistemas de archivos y colapsa el Explorador de Windows al acceder vía SFTP/SMB.

## Opciones Evaluadas

### Opción A: Mantener Descarga Monolítica Suelta con Nombres Legacy
- **Ventajas:** No requería modificaciones al código existente de captura.
- **Desventajas:** Incompatibilidad insalvable con Windows 11; colapso de Windows Explorer al listar miles de imágenes; persistencia del acoplamiento entre Git y las configuraciones de campo.

### Opción B: Empaquetado en Tarballs Comprimidos (`tar.gz` o `7z`)
- **Ventajas:** Buena compresión global en un único archivo por estación.
- **Desventajas:** Windows 11 no abre nativamente archivos `.tar.gz` sin instalar software de terceros (7-Zip / WinRAR). Además, las imágenes JPEG ya poseen compresión con pérdidas por transformada DCT; re-comprimirlas con gzip o lzma no reduce tamaño apreciable y satura la CPU de la Raspberry Pi innecesariamente.

### Opción C: Desacoplamiento Tripartito + Pipeline en 2 Etapas + Contenedor ZIP Estándar Mensual (ELEGIDA)
- **1. Desacoplamiento de Runtime (Jerarquía Tripartita):**
  - `*_GIT_ROOT`: Repositorio inmutable gobernado exclusivamente por Git.
  - `*_LOCAL_ROOT`: Configuraciones (`configuration/`), logs rotativos (`logs-files/`) y entornos virtuales (`venv/`). Inicializado de forma idempotente con `update.sh` que copia plantillas `*.example.json` únicamente si no existen previamente.
  - `*_DATA_DIR`: Directorio dedicado para almacenamiento masivo de mediciones e imágenes, aislable en particiones o discos USB/SSD.
  - Carga automática en Python mediante `cargar_archivo_env()` y `expandir_variables_entorno()` para resolver dinámicamente `${RSA_*}` sin requerir `source .env`.
- **2. Estandarización de Nomenclatura de Fotos:**
  - Formato canónico: `<ESTACION>_YYYYMMDD_HHMMSS.jpg` (ej: `DER-N3-X_20230224_140010.jpg`). Cero espacios y cero dos puntos.
  - Migración en coordinómetro mediante `migrar_nombres_fotos.py`: utiliza `os.rename()` atómico en ext4 (modificación de inodo con **cero consumo de espacio adicional** en MicroSD).
- **3. Pipeline de Extracción y Empaquetado Desacoplado en 2 Etapas:**
  - **Etapa 1 (`sincronizar_fotos_coordinometros.py`):** Transporte Ad-Hoc 802.11, pre-check SSH (`BatchMode=yes`), descarga incremental `rsync -avz --partial` hacia el buffer local `inbox/` con barra de progreso en vivo (`--info=progress2`), y **purga remota atómica condicionada estrictamente a código de salida 0**.
  - **Etapa 2 (`empaquetador_fotos.py`):** Clasificación temporal por año y mes en `<AÑO>/<AÑO-MES>.zip` usando compresión estándar `zipfile.ZIP_DEFLATED`. Validación de integridad CRC-32 con `zf.testzip()` antes de eliminar los archivos individuales del `inbox/`.
- **4. Modo Unificado Histórico Masivo y Recolección Diaria:**
  - Utiliza el modo append (`'a'`) de `zipfile`. Permite tanto clasificar lotes retenidos de años anteriores en sus respectivos meses como anexar las fotos del día a día (cada 24h) al ZIP del mes en curso sin sobreescritura.
- **5. Compatibilidad Nativa con Windows 11:**
  - Los archivos `.zip` se abren con doble clic como carpetas virtuales nativas en Windows 11 sin advertencias ni herramientas externas.

## Decisión

Se eligió la **Opción C** porque resuelve integralmente la gobernanza del ciclo de vida de los datos, garantiza cero pérdida de mediciones durante la recolección, elimina los conflictos de Git en campo y ofrece una experiencia de usuario perfecta para los ingenieros de auscultación en Windows 11.

## Consecuencias

- **Positivas:**
  - **Zero Data Loss:** La purga remota en el coordinómetro se ejecuta **únicamente** si `rsync` finalizó con éxito absoluto (código 0).
  - **Garantía de Integridad CRC:** Ninguna foto se borra del buffer `inbox/` del concentrador sin que el archivo `.zip` resultante supere la verificación interna de sumas de comprobación (`zf.testzip()`).
  - **MicroSD Protegida:** La migración y la recolección no generan sobreconsumo de almacenamiento en el coordinómetro.
  - **Repositorios Limpios:** Los despliegues vía `git pull` son 100% seguros y predecibles; no tocan configuraciones ni datos.
  - **Usabilidad en Oficina:** Visualización y extracción inmediata de fotos mensuales en Windows 11 con doble clic.
- **Negativas / Consideraciones:**
  - La transferencia inalámbrica Ad-Hoc de lotes históricos masivos (~5.805 fotos / 520 MB) requiere entre 7 y 10 minutos debido a las características del enlace en pozo vertical.
- **Trabajo Futuro:**
  - Actualizar el software de captura autónoma de fotos en `RSA-Coordinometro-Optico` para que genere de origen los archivos en el formato estándar `<ESTACION>_YYYYMMDD_HHMMSS.jpg`, eliminando la necesidad de renombrado posterior.

## Referencias

- Blueprint de Implementación: [`2026-11-18_entorno_laboratorio_desacoplamiento_runtime_adhoc.md`](../../montajes/server-pi/RSA-Concentrador-Presas/docs/blueprints/2026-11-18_entorno_laboratorio_desacoplamiento_runtime_adhoc.md)
- Contexto Técnico de Sincronización: [`sincronizar_fotos_coordinometros_context.md`](../../montajes/server-pi/RSA-Concentrador-Presas/docs/context/sincronizar_fotos_coordinometros_context.md)
- Contexto Técnico de Empaquetado: [`empaquetador_fotos_context.md`](../../montajes/server-pi/RSA-Concentrador-Presas/docs/context/empaquetador_fotos_context.md)
- Contexto Técnico de Migración: [`migrar_nombres_fotos_context.md`](../../montajes/server-pi/RSA-Coordinometro-Optico/docs/context/migrar_nombres_fotos_context.md)
- ADR Institucional Previo: `ADR-002: Estandarización Ad-Hoc nl80211 Multirol`
