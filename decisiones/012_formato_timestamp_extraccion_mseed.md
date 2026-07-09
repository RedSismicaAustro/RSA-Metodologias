---
id: ADR-012
titulo: Adaptación de Formato de Timestamp en Pipeline de Extracción GPD
estado: Aceptado
fecha: 2026-07-09
temas: [gpd, mseed, extraccion, formato, time]
entorno: [RSA-Acelerografo / Pipeline GPD]
---

# ADR-012: Adaptación de Formato de Timestamp en Pipeline de Extracción GPD

## Estado

**Aceptado** | Fecha: 2026-07-09

## Contexto

Durante la integración de la Fase 4 y Fase 5 del pipeline de inferencia GPD en tiempo real, se detectó una incompatibilidad en el formato de fecha/hora de inicio (`start_str`) al invocar a los componentes de extracción (`event_extractor.py` y `extract_segment.py`).

* El coordinador MQTT (`mqtt_coordinator.py`) y el worker de inferencia (`gpd_stream_worker.py`) calculaban y publicaban la fecha de inicio del evento utilizando el estándar **ISO8601 clásico**:
  * Ejemplo: `YYYY-MM-DDTHH:MM:SS.mmmZ` (ej. `2026-07-09T15:29:00.000Z`)
* Sin embargo, el núcleo de extracción heredado del acelerógrafo RSA espera un formato de timestamp no estándar con una **`'Z'` intermedia separando el día de la hora, y sin `'Z'` al final**:
  * Ejemplo: `YYYY-MM-DDZHH:MM:SS.mmm` (ej. `2026-07-09Z15:29:00.000`)
* Al recibir el formato clásico con `'T'` intermedia y `'Z'` al final, la lógica interna de los extractores hacía un reemplazo de `'Z'` por un espacio, lo que resultaba en `YYYY-MM-DDTHH:MM:SS.mmm `, fallando en el parsing con `datetime.strptime()` y provocando el colapso del proceso de extracción.

## Opciones Evaluadas

### Opción A: Refactorizar y Estandarizar el Núcleo de Extracción a ISO8601 Clásico
Modificar `extract_segment.py` y `event_extractor.py` para que acepten nativamente formatos ISO8601 universales, y migrar todas las dependencias del sistema y comandos de red correlacionados a este nuevo estándar.
* **Ventajas:** Limpieza de deuda técnica, alineación con estándares internacionales modernos de intercambio de datos (ISO8601).
* **Desventajas:** Alto riesgo de regresión en código crítico de producción que lleva meses estable en las estaciones y en servidores centrales de red que ya disparan comandos de extracción utilizando el formato de `'Z'` intermedia.

### Opción B: Adaptación en la Frontera del Pipeline GPD (Modificar Coordinador y Worker)
Modificar únicamente los componentes nuevos desarrollados en la Fase 4 y 5 (`mqtt_coordinator.py` y `gpd_stream_worker.py`) para que, al calcular el timestamp de la ventana de extracción, lo formateen al estándar no estándar heredado (`YYYY-MM-DDZHH:MM:SS.mmm`).
* **Ventajas:** Cero riesgo de alterar el código del extractor en producción, rapidez de corrección e integración y compatibilidad inmediata con la infraestructura de red actual de la RSA.
* **Desventajas:** Mantiene la deuda técnica de un formato propietario dentro del core del acelerógrafo.

## Decisión

Se eligió la **Opción B** en esta fase del proyecto. Se priorizó la estabilidad operativa de las estaciones y servidores centrales, mitigando el riesgo de introducir regresiones o incompatibilidades en el protocolo de red actual de la RSA. La corrección se realiza adaptando la frontera de integración de los nuevos procesos GPD.

Adicionalmente, se modificó la función auxiliar de logging `_log` en `event_extractor.py` para usar `hasattr()` y evitar fallos ante logs de tipo `debug` si se usa un logger estructurado personalizado.

## Consecuencias

* **Logro:** Se resolvió el fallo de formato en la simulación de detecciones GPD del coordinador MQTT y del flujo offline autónomo del worker.
* **Deuda Técnica:** Permanece el formato propietario heredado en el núcleo de extracción.
* **Trabajo Futuro:** Se documenta la necesidad de realizar una transición técnica unificada a mediano plazo para migrar todas las herramientas de la RSA al formato estándar ISO8601 clásico, tanto en los comandos de red como en el parser de los extractores.

## Referencias

* Sesión de origen: [2026-07-09_pipeline_extraccion_automatica_fase4.md](file:///home/rsa/git/personal/RSA-Bitacora-LLM-Milton/sesiones/2026/07/2026-07-07_pipeline_extraccion_automatica_fase4.md)
* Contexto relacionado: [extract_segment.py](file:///home/rsa/git/montajes/acelerografo-DEV00/scripts/operation/mseed/extract_segment.py#L33-L67)
* Contexto relacionado: [event_extractor.py](file:///home/rsa/git/montajes/acelerografo-DEV00/scripts/operation/mqtt/event_extractor.py#L144-L154)
