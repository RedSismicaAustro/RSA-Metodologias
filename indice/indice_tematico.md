---
tipo: indice_maestro
actualizado: 2026-07-07
version: 2.0
published: false
---

# Índice de Conocimiento RSA

Índice federado del exocortex de la Red Sísmica del Austro.
Las sesiones se referencian por usuario usando el formato `@Usuario: ruta/relativa/archivo.md`.
Para resolver rutas, consulta `catalogo_contribuidores.md`.

---

## Contextos Técnicos (Código y Arquitectura)

- **acelerografo**:
  - `binary_to_mseed.py`: Convierte archivos binarios del acelerógrafo a formato miniSEED → [RSA-Acelerografo/docs/context/binary_to_mseed_context.md](https://github.com/RedSismicaAustro/RSA-Acelerografo/blob/main/docs/context/binary_to_mseed_context.md)
  - `extract_segment.py`: Extrae segmentos de tiempo de archivos miniSEED → [RSA-Acelerografo/docs/context/extract_segment_context.md](https://github.com/RedSismicaAustro/RSA-Acelerografo/blob/main/docs/context/extract_segment_context.md)
  - `event_extractor.py`: Orquestador de extracción de eventos desde el ring buffer y miniSEED → [RSA-Acelerografo/docs/context/event_extractor_context.md](https://github.com/RedSismicaAustro/RSA-Acelerografo/blob/main/docs/context/event_extractor_context.md)
  - `firmware`: Firmware del acelerógrafo (C) → [RSA-Acelerografo/docs/context/firmware_context.md](https://github.com/RedSismicaAustro/RSA-Acelerografo/blob/main/docs/context/firmware_context.md)
  - `frame_decoder.py`: Decodificador y validador de tramas binarias de 2506 bytes del acelerógrafo → [RSA-Acelerografo/docs/context/frame_decoder_context.md](https://github.com/RedSismicaAustro/RSA-Acelerografo/blob/main/docs/context/frame_decoder_context.md)
  - `gestor_archivos_acq.py`: Gestor de archivos de adquisición → [RSA-Acelerografo/docs/context/gestor_archivos_acq_context.md](https://github.com/RedSismicaAustro/RSA-Acelerografo/blob/main/docs/context/gestor_archivos_acq_context.md)
  - `mqtt_coordinator.py`: Agente MQTT daemon en Raspberry Pi; desde Fase 4 maneja detecciones GPD locales con extracción automática y registro CSV → [RSA-Acelerografo/docs/context/mqtt_coordinator_context.md](https://github.com/RedSismicaAustro/RSA-Acelerografo/blob/main/docs/context/mqtt_coordinator_context.md)
  - `event_logger.py`: Registro CSV mensual thread-safe de detecciones sísmicas GPD, compartido entre worker y coordinador MQTT → [RSA-Acelerografo/docs/context/event_logger_context.md](https://github.com/RedSismicaAustro/RSA-Acelerografo/blob/main/docs/context/event_logger_context.md)
  - `structured_logger.py`: Logger estructurado con niveles personalizados y métodos semánticos por dominio (MQTT, GPD, telemetría, ring buffer) → [RSA-Acelerografo/docs/context/structured_logger_context.md](https://github.com/RedSismicaAustro/RSA-Acelerografo/blob/main/docs/context/structured_logger_context.md)
  - `mseed_event_extractor.py`: Extractor de eventos miniSEED → [RSA-Acelerografo/docs/context/mseed_event_extractor_context.md](https://github.com/RedSismicaAustro/RSA-Acelerografo/blob/main/docs/context/mseed_event_extractor_context.md)
  - `mseed_event_inspector.py`: Inspector de eventos miniSEED → [RSA-Acelerografo/docs/context/mseed_event_inspector_context.md](https://github.com/RedSismicaAustro/RSA-Acelerografo/blob/main/docs/context/mseed_event_inspector_context.md)
  - `registro_continuo.py`: Registro continuo de datos sísmicos → [RSA-Acelerografo/docs/context/registro_continuo_context.md](https://github.com/RedSismicaAustro/RSA-Acelerografo/blob/main/docs/context/registro_continuo_context.md)
  - `gpd_stream_worker.py`: Daemon de inferencia GPD en tiempo real — buffer 8 s, TFLite, bifurcación online/offline, registro CSV mensual y extracción autónoma (Fase 4) → [RSA-Acelerografo/docs/context/gpd_stream_worker_context.md](https://github.com/RedSismicaAustro/RSA-Acelerografo/blob/main/docs/context/gpd_stream_worker_context.md)
  - `ring_buffer_store.py`: Almacén rotativo FIFO de tramas en disco con consultas temporales → [RSA-Acelerografo/docs/context/ring_buffer_store_context.md](https://github.com/RedSismicaAustro/RSA-Acelerografo/blob/main/docs/context/ring_buffer_store_context.md)
  - `shared_memory_publisher.py`: Publica y lee tramas decodificadas del acelerógrafo a memoria compartida (/dev/shm) usando Seqlock → [RSA-Acelerografo/docs/context/shared_memory_publisher_context.md](https://github.com/RedSismicaAustro/RSA-Acelerografo/blob/main/docs/context/shared_memory_publisher_context.md)
  - `signal_preprocessor.py`: Módulo de downsampling (250 a 100 Hz), filtrado Butterworth pasabanda y normalización para GPD → [RSA-Acelerografo/docs/context/signal_preprocessor_context.md](https://github.com/RedSismicaAustro/RSA-Acelerografo/blob/main/docs/context/signal_preprocessor_context.md)
  - `test_ring_buffer_store.py`: Pruebas unitarias de almacenamiento rotativo, retención FIFO y simulación de regresión temporal (cambio de día) → [RSA-Acelerografo/docs/context/test_ring_buffer_store_context.md](https://github.com/RedSismicaAustro/RSA-Acelerografo/blob/main/docs/context/test_ring_buffer_store_context.md)
  - `web_context.md`: Entorno web de configuración (Flask/Vanilla JS) → [RSA-Acelerografo/docs/context/web_context.md](https://github.com/RedSismicaAustro/RSA-Acelerografo/blob/main/docs/context/web_context.md)
- **analisis-datos**:
  - `main.py`: Lanzador interactivo basado en menús que valida el entorno y ejecuta scripts de análisis de forma aislada. → [RSA-Analisis-Datos/docs/context/main_context.md](https://github.com/RedSismicaAustro/RSA-Analisis-Datos/blob/main/docs/context/main_context.md)
  - `mseed_event_extractor.py`: Interfaz gráfica interactiva para seleccionar, previsualizar y exportar eventos sísmicos en formato miniSEED. → [RSA-Analisis-Datos/docs/context/mseed_event_extractor_context.md](https://github.com/RedSismicaAustro/RSA-Analisis-Datos/blob/main/docs/context/mseed_event_extractor_context.md)
  - `mseed_event_inspector.py`: Interfaz gráfica para la visualización de formas de onda e inspección detallada de metadatos de archivos miniSEED. → [RSA-Analisis-Datos/docs/context/mseed_event_inspector_context.md](https://github.com/RedSismicaAustro/RSA-Analisis-Datos/blob/main/docs/context/mseed_event_inspector_context.md)
- **tig**:
  - `docker-compose-tig-mqtt`: Stack TIG con MQTT (Telegraf/InfluxDB/Grafana) → [RSA-Intern-TIG-MQTT/docs/context/docker-compose-tig-mqtt_context.md](https://github.com/RedSismicaAustro/RSA-Intern-TIG-MQTT/blob/main/docs/context/docker-compose-tig-mqtt_context.md)
  - `docker-compose-nodered`: Stack Node-RED → [RSA-Intern-TIG-MQTT/docs/context/docker-compose-nodered_context.md](https://github.com/RedSismicaAustro/RSA-Intern-TIG-MQTT/blob/main/docs/context/docker-compose-nodered_context.md)
- **edge-device**: (pendiente)

---

## Sesiones por Entorno

- **acelerografo**:
  - @Milton: 2026/01/2026-01-15_optimizacion_logging.md, 2026/01/2026-01-28_correcion_bug_subida_drive.md, 2026/02/2026-02-03_refactorizacion_mqtt_coordinator.md, 2026/02/2026-02-11_migracion_trixie_bullseye.md, 2026/02/2026-02-23_conectividad_remota_estaciones.md, 2026/02/2026-02-24_planeacion_migracion_entornos_virtuales.md, 2026/04/2026-04-21_resolucion_bug_mqtt_coordinator.md, 2026/04/2026-04-23_configuracion_tailscale_estaciones.md, 2026/04/2026-04-24_reversion_debouncing_mqtt_coordinator.md, 2026/04/2026-04-27_correccion_deploy_acelerografo.md, 2026/05/2026-05-11_extraccion_remota_mqtt.md, 2026/05/2026-05-15_comandos_broadcast_mqtt.md, 2026/06/2026-06-03_unificacion_configuracion.md, 2026/06/2026-06-04_panel_web_config_fase4.md, 2026/06/2026-06-04_wifi_ap_seguro.md, 2026/06/2026-06-11_diagnostico_registro_continuo.md, 2026/06/2026-06-16_ring_buffer_acelerografo.md, 2026/06/2026-06-17_ring_buffer_fase4_event_extractor.md, 2026/06/2026-06-22_migracion_gui_analisis.md, 2026/06/2026-06-23_correccion_rotacion_ring_buffer.md, 2026/06/2026-06-30_shm_preprocesador_gpd.md, 2026/07/2026-07-02_worker_inferencia_gpd_fase3.md, 2026/07/2026-07-07_pipeline_extraccion_automatica_fase4.md
- **edge-device**:
  - @Milton: 2026/04/2026-04-23_migracion_sensor_ultrasonico.md
- **entornos-virtuales**:
  - @Milton: 2026/05/2026-05-26_micromamba_w11.md, 2026/06/2026-06-22_migracion_gui_analisis.md
- **tig**:
  - @Milton: 2026/02/2026-02-13_grafana_dashboard_persistence.md, 2026/02/2026-02-20_consolidacion_tig_mqtt.md, 2026/05/2026-05-12_nodered_dashboard_layout.md, 2026/05/2026-05-14_correccion_datos_duplicados_grafana.md, 2026/05/2026-05-15_estabilizacion_nodered_dashboard.md, 2026/05/2026-05-22_quiosco_grafana_seguro.md

---

## Sesiones por Tema

- **arquitectura**:
  - @Milton: 2026/05/2026-05-15_comandos_broadcast_mqtt.md
- **automatizacion**:
  - @Milton: 2026/06/2026-06-03_unificacion_configuracion.md
- **bash**:
  - @Milton: 2026/04/2026-04-27_correccion_deploy_acelerografo.md
- **broadcast**:
  - @Milton: 2026/05/2026-05-15_comandos_broadcast_mqtt.md
- **bugfix**:
  - @Milton: 2026/01/2026-01-28_correcion_bug_subida_drive.md, 2026/04/2026-04-21_resolucion_bug_mqtt_coordinator.md, 2026/04/2026-04-24_reversion_debouncing_mqtt_coordinator.md, 2026/05/2026-05-14_correccion_datos_duplicados_grafana.md, 2026/06/2026-06-23_correccion_rotacion_ring_buffer.md
- **cloudflare**:
  - @Milton: 2026/02/2026-02-23_conectividad_remota_estaciones.md
- **configuracion**:
  - @Milton: 2026/06/2026-06-03_unificacion_configuracion.md, 2026/06/2026-06-04_panel_web_config_fase4.md, 2026/06/2026-06-23_correccion_rotacion_ring_buffer.md
- **conectividad**:
  - @Milton: 2026/04/2026-04-23_configuracion_tailscale_estaciones.md
- **dashboards**:
  - @Milton: 2026/02/2026-02-13_grafana_dashboard_persistence.md, 2026/05/2026-05-15_estabilizacion_nodered_dashboard.md
- **dependencias**:
  - @Milton: 2026/02/2026-02-11_migracion_trixie_bullseye.md, 2026/02/2026-02-24_planeacion_migracion_entornos_virtuales.md
- **deploy**:
  - @Milton: 2026/04/2026-04-27_correccion_deploy_acelerografo.md
- **diagnostico**:
  - @Milton: 2026/06/2026-06-11_diagnostico_registro_continuo.md
- **csv**:
  - @Milton: 2026/07/2026-07-07_pipeline_extraccion_automatica_fase4.md
- **deteccion-sismica**:
  - @Milton: 2026/07/2026-07-02_worker_inferencia_gpd_fase3.md, 2026/07/2026-07-07_pipeline_extraccion_automatica_fase4.md
- **dnsmasq**:
  - @Milton: 2026/06/2026-06-04_wifi_ap_seguro.md
- **docker**:
  - @Milton: 2026/02/2026-02-20_consolidacion_tig_mqtt.md, 2026/05/2026-05-12_nodered_dashboard_layout.md, 2026/05/2026-05-15_estabilizacion_nodered_dashboard.md
- **documentacion**:
  - @Milton: 2026/02/2026-02-20_consolidacion_tig_mqtt.md, 2026/02/2026-02-24_planeacion_migracion_entornos_virtuales.md
- **drive**:
  - @Milton: 2026/01/2026-01-28_correcion_bug_subida_drive.md
- **dsp**:
  - @Milton: 2026/04/2026-04-23_migracion_sensor_ultrasonico.md, 2026/06/2026-06-30_shm_preprocesador_gpd.md
- **esp32**:
  - @Milton: 2026/04/2026-04-23_migracion_sensor_ultrasonico.md
- **fase4**:
  - @Milton: 2026/07/2026-07-07_pipeline_extraccion_automatica_fase4.md
- **flask**:
  - @Milton: 2026/06/2026-06-04_panel_web_config_fase4.md
- **flux**:
  - @Milton: 2026/02/2026-02-13_grafana_dashboard_persistence.md
- **frontend**:
  - @Milton: 2026/06/2026-06-11_diagnostico_registro_continuo.md
- **git**:
  - @Milton: 2026/02/2026-02-23_conectividad_remota_estaciones.md
- **gpd**:
  - @Milton: 2026/06/2026-06-30_shm_preprocesador_gpd.md, 2026/07/2026-07-02_worker_inferencia_gpd_fase3.md, 2026/07/2026-07-07_pipeline_extraccion_automatica_fase4.md
- **gpo**:
  - @Milton: 2026/05/2026-05-26_micromamba_w11.md
- **grafana**:
  - @Milton: 2026/02/2026-02-13_grafana_dashboard_persistence.md, 2026/05/2026-05-14_correccion_datos_duplicados_grafana.md, 2026/05/2026-05-22_quiosco_grafana_seguro.md
- **gui**:
  - @Milton: 2026/06/2026-06-22_migracion_gui_analisis.md
- **hostapd**:
  - @Milton: 2026/06/2026-06-04_wifi_ap_seguro.md
- **influxdb**:
  - @Milton: 2026/02/2026-02-13_grafana_dashboard_persistence.md, 2026/05/2026-05-14_correccion_datos_duplicados_grafana.md
- **json**:
  - @Milton: 2026/06/2026-06-11_diagnostico_registro_continuo.md
- **kiosk**:
  - @Milton: 2026/05/2026-05-22_quiosco_grafana_seguro.md
- **logging**:
  - @Milton: 2026/01/2026-01-15_optimizacion_logging.md, 2026/01/2026-01-28_correcion_bug_subida_drive.md, 2026/06/2026-06-17_ring_buffer_fase4_event_extractor.md
- **memoria-compartida**:
  - @Milton: 2026/06/2026-06-30_shm_preprocesador_gpd.md
- **micromamba**:
  - @Milton: 2026/05/2026-05-26_micromamba_w11.md
- **migracion**:
  - @Milton: 2026/02/2026-02-11_migracion_trixie_bullseye.md
- **mqtt**:
  - @Milton: 2026/02/2026-02-03_refactorizacion_mqtt_coordinator.md, 2026/02/2026-02-20_consolidacion_tig_mqtt.md, 2026/04/2026-04-21_resolucion_bug_mqtt_coordinator.md, 2026/04/2026-04-24_reversion_debouncing_mqtt_coordinator.md, 2026/05/2026-05-11_extraccion_remota_mqtt.md, 2026/05/2026-05-12_nodered_dashboard_layout.md, 2026/05/2026-05-15_estabilizacion_nodered_dashboard.md, 2026/05/2026-05-15_comandos_broadcast_mqtt.md, 2026/06/2026-06-17_ring_buffer_fase4_event_extractor.md
- **mseed**:
  - @Milton: 2026/01/2026-01-15_optimizacion_logging.md, 2026/05/2026-05-11_extraccion_remota_mqtt.md, 2026/06/2026-06-16_ring_buffer_acelerografo.md, 2026/06/2026-06-17_ring_buffer_fase4_event_extractor.md
- **node-red**:
  - @Milton: 2026/05/2026-05-12_nodered_dashboard_layout.md, 2026/05/2026-05-15_estabilizacion_nodered_dashboard.md
- **os**:
  - @Milton: 2026/02/2026-02-11_migracion_trixie_bullseye.md
- **plantillas**:
  - @Milton: 2026/06/2026-06-03_unificacion_configuracion.md
- **redes**:
  - @Milton: 2026/04/2026-04-23_configuracion_tailscale_estaciones.md, 2026/04/2026-04-24_reversion_debouncing_mqtt_coordinator.md
- **refactorizacion**:
  - @Milton: 2026/02/2026-02-03_refactorizacion_mqtt_coordinator.md, 2026/06/2026-06-22_migracion_gui_analisis.md
- **scripts**:
  - @Milton: 2026/04/2026-04-27_correccion_deploy_acelerografo.md, 2026/05/2026-05-11_extraccion_remota_mqtt.md
- **seguridad**:
  - @Milton: 2026/06/2026-06-04_wifi_ap_seguro.md
- **sensor**:
  - @Milton: 2026/04/2026-04-23_migracion_sensor_ultrasonico.md
- **shell**:
  - @Milton: 2026/05/2026-05-26_micromamba_w11.md
- **streaming**:
  - @Milton: 2026/06/2026-06-16_ring_buffer_acelerografo.md, 2026/06/2026-06-17_ring_buffer_fase4_event_extractor.md, 2026/06/2026-06-23_correccion_rotacion_ring_buffer.md, 2026/06/2026-06-30_shm_preprocesador_gpd.md, 2026/07/2026-07-02_worker_inferencia_gpd_fase3.md
- **tailscale**:
  - @Milton: 2026/04/2026-04-23_configuracion_tailscale_estaciones.md
- **telegraf**:
  - @Milton: 2026/05/2026-05-14_correccion_datos_duplicados_grafana.md
- **telemetria**:
  - @Milton: 2026/02/2026-02-03_refactorizacion_mqtt_coordinator.md, 2026/04/2026-04-21_resolucion_bug_mqtt_coordinator.md, 2026/05/2026-05-11_extraccion_remota_mqtt.md, 2026/05/2026-05-15_comandos_broadcast_mqtt.md, 2026/06/2026-06-16_ring_buffer_acelerografo.md
- **ubuntu**:
  - @Milton: 2026/05/2026-05-22_quiosco_grafana_seguro.md
- **ui**:
  - @Milton: 2026/05/2026-05-12_nodered_dashboard_layout.md, 2026/05/2026-05-15_estabilizacion_nodered_dashboard.md
- **unificacion**:
  - @Milton: 2026/06/2026-06-03_unificacion_configuracion.md
- **ultrasonico**:
  - @Milton: 2026/04/2026-04-23_migracion_sensor_ultrasonico.md
- **venv**:
  - @Milton: 2026/02/2026-02-11_migracion_trixie_bullseye.md, 2026/02/2026-02-23_conectividad_remota_estaciones.md, 2026/02/2026-02-24_planeacion_migracion_entornos_virtuales.md
- **vpn**:
  - @Milton: 2026/02/2026-02-23_conectividad_remota_estaciones.md, 2026/04/2026-04-23_configuracion_tailscale_estaciones.md
- **vscode**:
  - @Milton: 2026/05/2026-05-26_micromamba_w11.md
- **web**:
  - @Milton: 2026/06/2026-06-04_panel_web_config_fase4.md, 2026/06/2026-06-11_diagnostico_registro_continuo.md
- **wayland**:
  - @Milton: 2026/05/2026-05-22_quiosco_grafana_seguro.md
- **wifi**:
  - @Milton: 2026/06/2026-06-04_wifi_ap_seguro.md
- **windows**:
  - @Milton: 2026/05/2026-05-26_micromamba_w11.md, 2026/06/2026-06-22_migracion_gui_analisis.md

---

## Decisiones de Arquitectura

- ADR-001: decisiones/001_unificacion_configuracion_acelerografo.md (configuracion, unificacion, plantillas, automatizacion) — Unificación de la configuración del acelerógrafo mediante plantillas y script de hidratación.
- ADR-002: decisiones/002_panel_web_configuracion_flask.md (web, configuracion, seguridad) — Selección de Flask sobre FastAPI para el servidor web de configuración del acelerógrafo por consumo de memoria.
- ADR-003: decisiones/003_wifi_ap_aislamiento_firewall.md (wifi, seguridad, redes) — Aislamiento del puerto 5000 en la interfaz eth0 mediante iptables durante el funcionamiento del WiFi AP.
- ADR-004: decisiones/004_implementacion_ring_buffer_acelerografo.md (acelerografo, streaming, ring-buffer, telemetria) — Implementación del Ring Buffer en archivos planos para adquisición y recuperación rápida e inmune a fallos de reloj.
- ADR-009: decisiones/009_memoria_compartida_seqlock_ipc_streaming.md (streaming, memoria-compartida, ipc, telemetria) — Implementación de IPC en memoria compartida con protocolo Seqlock para transmisión de tramas a baja latencia.
- ADR-010: decisiones/010_pipeline_inferencia_gpd_streaming_stride_buffer_cooldown.md (gpd, inferencia, streaming, tflite, deteccion_sismica) — Stride de 1 s, buffer de padding de 8 s, umbrales configurables (0.95) y cooldown anti-spam (30 s) para el worker de inferencia GPD en tiempo real.
- ADR-011: decisiones/011_bifurcacion_online_offline_pipeline_gpd_postdeteccion.md (gpd, deteccion_sismica, mqtt, streaming, fase4) — Bifurcación del pipeline GPD post-detección según `modo_adquisicion`: en online el coordinador MQTT extrae; en offline el worker extrae directamente. CSV mensual como estado compartido thread-safe.
- ADR-012: decisiones/012_formato_timestamp_extraccion_mseed.md (gpd, mseed, extraccion, formato, time) — Adaptación de formato de timestamp start_str en coordinador y worker para usar el formato heredado de la RSA.

