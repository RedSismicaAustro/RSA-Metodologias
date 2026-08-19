---
id: ADR-015
titulo: Índice Centralizado de Eventos Sísmicos en InfluxDB y Ciclo Cerrado de Clasificación MQTT
estado: Aceptado
fecha: 2026-08-19
temas: [influxdb, mqtt, telegraf, event-analyzer, nodered, correlador, streaming, sismologia, performance]
entorno: RSA-Intern-TIG-MQTT
---

# ADR-015: Índice Centralizado de Eventos Sísmicos en InfluxDB y Ciclo Cerrado de Clasificación MQTT

## Estado

**Aceptado** | Fecha: 2026-08-19

## Contexto

El sistema de monitorización y análisis de la Red Sísmica del Austro (RSA) procesa cuatro tipos de eventos sísmicos:
1. **Tipo 1 (Automático)**: Detectado en tiempo real por $\ge 2$ acelerógrafos y confirmado por el Correlador Regional.
2. **Tipo 2 (Manual)**: Extracción solicitada manualmente por un operador desde el panel Node-RED.
3. **Tipo 3 (Confirmado)**: Evento clasificado como sismo real por el operador en el Event Analyzer.
4. **Tipo 4 (Descartado)**: Evento clasificado como falsa alarma / ruido antrópico en el Event Analyzer.

En el esquema original, la aplicación web Event Analyzer (Streamlit) indexaba el catálogo escaneando recursivamente el sistema de archivos de Google Drive (`/data/events` montado vía `rclone`/FUSE). Conforme crecía el histórico (>2,300 eventos y decenas de miles de archivos MiniSEED), este escaneo introducía demoras de decenas de segundos, alto consumo de I/O de red y degradación severa del navegador. Además, las clasificaciones manuales no se persistían en una base de datos central ni se sincronizaban entre componentes.

## Opciones Evaluadas

### Opción A: Escaneo Híbrido Continuo en Disco (Drive) y Base de Datos
- **Ventajas:** Descubrimiento automático de archivos MiniSEED copiados directamente en disco sin pasar por la red MQTT.
- **Desventajas:** Mantiene el cuello de botella de I/O y latencia de FUSE/Google Drive cada vez que el usuario presiona "Recargar Catálogo" o navega por fechas.

### Opción B: InfluxDB como Fuente Exclusiva para el Catálogo con Tópico Central MQTT y Lazy Loading (Seleccionada)
- **Ventajas:** 
  - Consultas de catálogo y calendario en milisegundos (<50 ms) mediante consultas Flux (`rsa_events`).
  - Google Drive se consulta exclusivamente bajo demanda (*Lazy Loading*) al seleccionar un evento puntual.
  - Publicación y persistencia desacoplada de los 4 tipos de eventos mediante un único tópico MQTT (`rsa/seismic/smart/events/metadata`) con QoS 1 y consumidor Telegraf `json_v2`.
  - Jerarquía clara de estados (`confirmed`/`discarded` > `manual` > `auto`) y preservación temporal estricta de `timestamp_utc`.
- **Desventajas:** Los eventos históricos previos en Google Drive requieren un script de backfill/migración (Fase 5) para quedar indexados en InfluxDB.

## Decisión

Se eligió la **Opción B**:
1. **Esquema de Bucket InfluxDB (`rsa_events`)**:
   - Medición: `seismic_event`.
   - Tags: `event_id`, `event_type` (`auto` | `manual` | `confirmed` | `discarded`), `source` (`correlator` | `nodered` | `event_analyzer`).
   - Fields: `stations` (CSV), `n_stations` (int), `duration_s` (float), `request_id` (string), `details` (JSON string).
   - Retención: Infinita (`-r 0`).
2. **Ingesta en Telegraf (`telegraf.conf`)**:
   - Consumidor MQTT `json_v2` con sesión persistente y QoS 1.
   - Configuración de `timestamp_path = "timestamp_utc"` y `timestamp_format = "2006-01-02T15:04:05.000Z07:00"` para preservar la hora original del sismo en todas las actualizaciones de estado.
   - Segmentación de outputs: `namedrop = ["seismic_event"]` para el bucket `telemetry` y `namepass = ["seismic_event"]` para `rsa_events`.
3. **Emisión de Metadatos en Componentes**:
   - **Correlador Regional**: Publica metadatos con `event_type: "auto"` al confirmar $\ge 2$ estaciones.
   - **Node-RED**: Emite en paralelo la orden `cmd/extract_event` y los metadatos con `event_type: "manual"` al tópico central.
   - **Event Analyzer**: Botones interactivos que emiten `event_type: "confirmed"` o `"discarded"` con actualización optimista inmediata en UI.
4. **Lazy Loading de Trazas MiniSEED (`reader.py`)**:
   - `scan_event()` resuelve archivos puntuales por fecha `YYYYMMDD` en ventana de 120s y normaliza códigos de estación (`CHA2` $\leftrightarrow$ `CHA02`).

## Consecuencias

- **Positivas:**
  - Carga inicial y navegación del catálogo instantáneas sin consumo apreciable de CPU ni I/O.
  - Trazabilidad y ciclo cerrado de auditoría sismológica (confirmación y descarte en tiempo real).
  - Tolerancia a desconexiones gracias a la persistencia QoS 1 en broker Mosquitto y Telegraf.
- **Negativas / Trabajo Futuro:**
  - Los eventos pasados existentes en Google Drive antes de esta implementación no aparecen hasta ejecutar el script de backfill/migración (programado para la Fase 5).

## Referencias

- Contexto técnico Event Analyzer: [RSA-Intern-TIG-MQTT/docs/context/event_analyzer_context.md](https://github.com/RedSismicaAustro/RSA-Intern-TIG-MQTT/blob/main/docs/context/event_analyzer_context.md)
- Contexto técnico Correlador: [RSA-Intern-TIG-MQTT/docs/context/regional_event_correlator_context.md](https://github.com/RedSismicaAustro/RSA-Intern-TIG-MQTT/blob/main/docs/context/regional_event_correlator_context.md)
- Contexto técnico Telegraf / Stack TIG: [RSA-Intern-TIG-MQTT/docs/context/docker-compose-tig-mqtt_context.md](https://github.com/RedSismicaAustro/RSA-Intern-TIG-MQTT/blob/main/docs/context/docker-compose-tig-mqtt_context.md)
- Contexto técnico Node-RED: [RSA-Intern-TIG-MQTT/docs/context/docker-compose-nodered_context.md](https://github.com/RedSismicaAustro/RSA-Intern-TIG-MQTT/blob/main/docs/context/docker-compose-nodered_context.md)
