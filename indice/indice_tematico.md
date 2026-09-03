---
tipo: indice_maestro
actualizado: 2026-09-02
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
  - `acquisition_watchdog.py`: Monitor de latencia y salud de adquisición del Ring Buffer con emisión periódica de estado en MQTT → [RSA-Acelerografo/docs/context/acquisition_watchdog_context.md](https://github.com/RedSismicaAustro/RSA-Acelerografo/blob/main/docs/context/acquisition_watchdog_context.md)
  - `stream_processor.py`: Daemon de lectura del named pipe (/tmp/my_pipe) al Ring Buffer con reintentos resilientes y backoff exponencial → [RSA-Acelerografo/docs/context/stream_processor_context.md](https://github.com/RedSismicaAustro/RSA-Acelerografo/blob/main/docs/context/stream_processor_context.md)
  - `mqtt_coordinator.py`: Agente MQTT daemon en Raspberry Pi; maneja telemetría de salud, watchdog de latencia del Ring Buffer (status/acquisition) y detecciones GPD locales → [RSA-Acelerografo/docs/context/mqtt_coordinator_context.md](https://github.com/RedSismicaAustro/RSA-Acelerografo/blob/main/docs/context/mqtt_coordinator_context.md)
  - `event_logger.py`: Registro CSV mensual thread-safe de detecciones sísmicas GPD, compartido entre worker y coordinador MQTT → [RSA-Acelerografo/docs/context/event_logger_context.md](https://github.com/RedSismicaAustro/RSA-Acelerografo/blob/main/docs/context/event_logger_context.md)
  - `structured_logger.py`: Logger estructurado con niveles personalizados y métodos semánticos por dominio (MQTT, GPD, telemetría, ring buffer) → [RSA-Acelerografo/docs/context/structured_logger_context.md](https://github.com/RedSismicaAustro/RSA-Acelerografo/blob/main/docs/context/structured_logger_context.md)
  - `mseed_event_extractor.py`: Extractor de eventos miniSEED → [RSA-Acelerografo/docs/context/mseed_event_extractor_context.md](https://github.com/RedSismicaAustro/RSA-Acelerografo/blob/main/docs/context/mseed_event_extractor_context.md)
  - `mseed_event_inspector.py`: Inspector de eventos miniSEED → [RSA-Acelerografo/docs/context/mseed_event_inspector_context.md](https://github.com/RedSismicaAustro/RSA-Acelerografo/blob/main/docs/context/mseed_event_inspector_context.md)
  - `registro_continuo.py`: Registro continuo de datos sísmicos (v4.5.0 en C) gobernado por systemd con auto-reinicio y reseteo por hardware dsPIC → [RSA-Acelerografo/docs/context/registro_continuo_context.md](https://github.com/RedSismicaAustro/RSA-Acelerografo/blob/main/docs/context/registro_continuo_context.md)
  - `wait_for_ntp.sh`: Helper de espera activa con timeout (120 s) para sincronización NTP en ExecStartPre de systemd → [RSA-Acelerografo/docs/context/wait_for_ntp_context.md](https://github.com/RedSismicaAustro/RSA-Acelerografo/blob/main/docs/context/wait_for_ntp_context.md)
  - `gpd_stream_worker.py`: Daemon de inferencia GPD en tiempo real — buffer 8 s, TFLite, bifurcación online/offline, registro CSV mensual y extracción autónoma (Fase 4) → [RSA-Acelerografo/docs/context/gpd_stream_worker_context.md](https://github.com/RedSismicaAustro/RSA-Acelerografo/blob/main/docs/context/gpd_stream_worker_context.md)
  - `ring_buffer_store.py`: Almacén rotativo FIFO de tramas en disco con consultas temporales → [RSA-Acelerografo/docs/context/ring_buffer_store_context.md](https://github.com/RedSismicaAustro/RSA-Acelerografo/blob/main/docs/context/ring_buffer_store_context.md)
  - `shared_memory_publisher.py`: Publica y lee tramas decodificadas del acelerógrafo a memoria compartida (/dev/shm) usando Seqlock → [RSA-Acelerografo/docs/context/shared_memory_publisher_context.md](https://github.com/RedSismicaAustro/RSA-Acelerografo/blob/main/docs/context/shared_memory_publisher_context.md)
  - `signal_preprocessor.py`: Módulo de downsampling (250 a 100 Hz), filtrado Butterworth pasabanda y normalización para GPD → [RSA-Acelerografo/docs/context/signal_preprocessor_context.md](https://github.com/RedSismicaAustro/RSA-Acelerografo/blob/main/docs/context/signal_preprocessor_context.md)
  - `normalizacion_canal_gpd.md`: Análisis técnico y contraste de la normalización por canal con el paper de Ross et al. (2018) → [RSA-Acelerografo/docs/analysis/normalizacion_canal_gpd.md](https://github.com/RedSismicaAustro/RSA-Acelerografo/blob/main/docs/analysis/normalizacion_canal_gpd.md)
  - `test_ring_buffer_store.py`: Pruebas unitarias de almacenamiento rotativo, retención FIFO y simulación de regresión temporal (cambio de día) → [RSA-Acelerografo/docs/context/test_ring_buffer_store_context.md](https://github.com/RedSismicaAustro/RSA-Acelerografo/blob/main/docs/context/test_ring_buffer_store_context.md)
  - `web_context.md`: Entorno web de configuración (Flask/Vanilla JS) → [RSA-Acelerografo/docs/context/web_context.md](https://github.com/RedSismicaAustro/RSA-Acelerografo/blob/main/docs/context/web_context.md)
- **analisis-datos**:
  - `main.py`: Lanzador interactivo basado en menús que valida el entorno y ejecuta scripts de análisis de forma aislada. → [RSA-Analisis-Datos/docs/context/main_context.md](https://github.com/RedSismicaAustro/RSA-Analisis-Datos/blob/main/docs/context/main_context.md)
  - `mseed_event_extractor.py`: Interfaz gráfica interactiva para seleccionar, previsualizar y exportar eventos sísmicos en formato miniSEED. → [RSA-Analisis-Datos/docs/context/mseed_event_extractor_context.md](https://github.com/RedSismicaAustro/RSA-Analisis-Datos/blob/main/docs/context/mseed_event_extractor_context.md)
  - `mseed_event_inspector.py`: Interfaz gráfica para la visualización de formas de onda e inspección detallada de metadatos de archivos miniSEED. → [RSA-Analisis-Datos/docs/context/mseed_event_inspector_context.md](https://github.com/RedSismicaAustro/RSA-Analisis-Datos/blob/main/docs/context/mseed_event_inspector_context.md)
- **tig**:
  - `docker-compose-tig-mqtt`: Stack TIG con MQTT (Telegraf/InfluxDB/Grafana/Correlator/Event-Analyzer/DB-Sync) con perfiles multi-servidor → [RSA-Intern-TIG-MQTT/docs/context/docker-compose-tig-mqtt_context.md](https://github.com/RedSismicaAustro/RSA-Intern-TIG-MQTT/blob/main/docs/context/docker-compose-tig-mqtt_context.md)
  - `docker-compose-nodered`: Stack Node-RED con extracción manual y registro automático de metadatos en InfluxDB → [RSA-Intern-TIG-MQTT/docs/context/docker-compose-nodered_context.md](https://github.com/RedSismicaAustro/RSA-Intern-TIG-MQTT/blob/main/docs/context/docker-compose-nodered_context.md)
  - `regional_event_correlator`: Servicio daemon de correlación regional MQTT con sesión persistente (clean_session=False), emisión broadcast y metadatos a InfluxDB → [RSA-Intern-TIG-MQTT/docs/context/regional_event_correlator_context.md](https://github.com/RedSismicaAustro/RSA-Intern-TIG-MQTT/blob/main/docs/context/regional_event_correlator_context.md)
  - `event_analyzer`: Visualizador web interactivo (Streamlit + ObsPy + Plotly) con índice rápido en InfluxDB y clasificación cerrada MQTT → [RSA-Intern-TIG-MQTT/docs/context/event_analyzer_context.md](https://github.com/RedSismicaAustro/RSA-Intern-TIG-MQTT/blob/main/docs/context/event_analyzer_context.md)
  - `influx_client.py`: Módulo de consultas analíticas Flux y publicación de clasificaciones de eventos sísmicos con QoS 1 → [RSA-Intern-TIG-MQTT/docs/context/influx_client_context.md](https://github.com/RedSismicaAustro/RSA-Intern-TIG-MQTT/blob/main/docs/context/influx_client_context.md)
  - `fix_event_ids.py`: Script de migración retroactiva y sincronización temporal de event_id en InfluxDB v2 → [RSA-Intern-TIG-MQTT/docs/context/fix_event_ids_context.md](https://github.com/RedSismicaAustro/RSA-Intern-TIG-MQTT/blob/main/docs/context/fix_event_ids_context.md)
  - `backup_events.sh`: Protocolo de respaldo diario de InfluxDB (snapshot binario + CSV) con subida a Drive, rotación de 7 días y telemetría MQTT → [RSA-Intern-TIG-MQTT/docs/context/backup_events_context.md](https://github.com/RedSismicaAustro/RSA-Intern-TIG-MQTT/blob/main/docs/context/backup_events_context.md)
  - `restore_events.sh`: Protocolo de restauración destructiva de InfluxDB desde Google Drive con confirmación interactiva → [RSA-Intern-TIG-MQTT/docs/context/restore_events_context.md](https://github.com/RedSismicaAustro/RSA-Intern-TIG-MQTT/blob/main/docs/context/restore_events_context.md)
  - `mqtt_notify.py`: Microservicio en contenedor rsa-db-sync para publicación de telemetría de respaldos a MQTT con QoS 1 → [RSA-Intern-TIG-MQTT/docs/context/mqtt_notify_context.md](https://github.com/RedSismicaAustro/RSA-Intern-TIG-MQTT/blob/main/docs/context/mqtt_notify_context.md)
  - `manage_backup_timer.sh`: Gestor de instalación y control de automatización systemd agnóstico y portable con plantillas → [RSA-Intern-TIG-MQTT/docs/context/manage_backup_timer_context.md](https://github.com/RedSismicaAustro/RSA-Intern-TIG-MQTT/blob/main/docs/context/manage_backup_timer_context.md)
- **edge-device**: (pendiente)

---

## Sesiones por Entorno

- **acelerografo**:
  - @Milton: 2026/01/2026-01-15_optimizacion_logging.md, 2026/01/2026-01-28_correcion_bug_subida_drive.md, 2026/02/2026-02-03_refactorizacion_mqtt_coordinator.md, 2026/02/2026-02-11_migracion_trixie_bullseye.md, 2026/02/2026-02-23_conectividad_remota_estaciones.md, 2026/02/2026-02-24_planeacion_migracion_entornos_virtuales.md, 2026/04/2026-04-21_resolucion_bug_mqtt_coordinator.md, 2026/04/2026-04-23_configuracion_tailscale_estaciones.md, 2026/04/2026-04-24_reversion_debouncing_mqtt_coordinator.md, 2026/04/2026-04-27_correccion_deploy_acelerografo.md, 2026/05/2026-05-11_extraccion_remota_mqtt.md, 2026/05/2026-05-15_comandos_broadcast_mqtt.md, 2026/06/2026-06-03_unificacion_configuracion.md, 2026/06/2026-06-04_panel_web_config_fase4.md, 2026/06/2026-06-04_wifi_ap_seguro.md, 2026/06/2026-06-11_diagnostico_registro_continuo.md, 2026/06/2026-06-16_ring_buffer_acelerografo.md, 2026/06/2026-06-17_ring_buffer_fase4_event_extractor.md, 2026/06/2026-06-22_migracion_gui_analisis.md, 2026/06/2026-06-23_correccion_rotacion_ring_buffer.md, 2026/06/2026-06-30_shm_preprocesador_gpd.md, 2026/07/2026-07-02_worker_inferencia_gpd_fase3.md, 2026/07/2026-07-07_pipeline_extraccion_automatica_fase4.md, 2026/07/2026-07-07_configuracion_pruebas_fase5_gpd.md, 2026/07/2026-07-10_depuracion_gpd_logs.md, 2026/07/2026-07-15_depuracion_mqtt_gpd.md, 2026/09/2026-09-01_automatizacion_despliegue_acelerografo.md, 2026/09/2026-09-01_diagnostico_adquisicion_cha01.md, 2026/09/2026-09-02_resiliencia_pipeline_adquisicion.md, 2026/09/2026-09-03_sincronizacion_ntp_systemd.md
- **edge-device**:
  - @Milton: 2026/04/2026-04-23_migracion_sensor_ultrasonico.md, 2026/07/2026-07-28_planificacion_pasantia_sensor_ultrasonico.md
- **entornos-virtuales**:
  - @Milton: 2026/05/2026-05-26_micromamba_w11.md, 2026/06/2026-06-22_migracion_gui_analisis.md
- **pasantias**:
  - @Milton: 2026/07/2026-07-15_planificacion_pasantias.md, 2026/07/2026-07-29_ensamblaje_red_monitorizacion_shm.md, 2026/07/2026-07-28_planificacion_pasantia_sensor_ultrasonico.md
- **tig**:
  - @Milton: 2026/02/2026-02-13_grafana_dashboard_persistence.md, 2026/02/2026-02-20_consolidacion_tig_mqtt.md, 2026/05/2026-05-12_nodered_dashboard_layout.md, 2026/05/2026-05-14_correccion_datos_duplicados_grafana.md, 2026/05/2026-05-15_estabilizacion_nodered_dashboard.md, 2026/05/2026-05-22_quiosco_grafana_seguro.md, 2026/07/2026-07-22_zona_horaria_nodered.md, 2026/07/2026-07-23_correlador_eventos_regionales_mqtt.md, 2026/07/2026-07-24_event_analyzer_fase1_streamlit.md, 2026/07/2026-07-31_despliegue_nuevo_servidor_ubuntu.md, 2026/08/2026-08-06_seleccion_calendario_downsampling_event_analyzer.md, 2026/08/2026-08-19_integracion_influxdb_event_analyzer_nodered.md, 2026/08/2026-08-21_diagnostico_resolucion_trazas_correlador_event_analyzer.md, 2026/08/2026-08-25_correccion_resolucion_trazas_migracion_influxdb.md, 2026/08/2026-08-26_plan_implementacion_fase5_respaldo_recuperacion.md, 2026/08/2026-08-27_respaldo_influxdb_multiservidor_fase5.md, 2026/08/2026-08-31_reubicacion_respaldos_influxdb_gdrive.md

---

## Sesiones por Tema

- **adquisicion**:
  - @Milton: 2026/09/2026-09-01_diagnostico_adquisicion_cha01.md, 2026/09/2026-09-02_resiliencia_pipeline_adquisicion.md, 2026/09/2026-09-03_sincronizacion_ntp_systemd.md
- **arquitectura**:
  - @Milton: 2026/05/2026-05-15_comandos_broadcast_mqtt.md
- **automatizacion**:
  - @Milton: 2026/06/2026-06-03_unificacion_configuracion.md, 2026/09/2026-09-01_automatizacion_despliegue_acelerografo.md
- **backups**:
  - @Milton: 2026/08/2026-08-26_plan_implementacion_fase5_respaldo_recuperacion.md, 2026/08/2026-08-27_respaldo_influxdb_multiservidor_fase5.md, 2026/08/2026-08-31_reubicacion_respaldos_influxdb_gdrive.md
- **bash**:
  - @Milton: 2026/04/2026-04-27_correccion_deploy_acelerografo.md
- **bom**:
  - @Milton: 2026/07/2026-07-15_planificacion_pasantias.md
- **broadcast**:
  - @Milton: 2026/05/2026-05-15_comandos_broadcast_mqtt.md, 2026/07/2026-07-23_correlador_eventos_regionales_mqtt.md
- **bugfix**:
  - @Milton: 2026/01/2026-01-28_correcion_bug_subida_drive.md, 2026/04/2026-04-21_resolucion_bug_mqtt_coordinator.md, 2026/04/2026-04-24_reversion_debouncing_mqtt_coordinator.md, 2026/05/2026-05-14_correccion_datos_duplicados_grafana.md, 2026/06/2026-06-23_correccion_rotacion_ring_buffer.md, 2026/07/2026-07-10_depuracion_gpd_logs.md, 2026/07/2026-07-15_depuracion_mqtt_gpd.md
- **cloudflare**:
  - @Milton: 2026/02/2026-02-23_conectividad_remota_estaciones.md
- **configuracion**:
  - @Milton: 2026/06/2026-06-03_unificacion_configuracion.md, 2026/06/2026-06-04_panel_web_config_fase4.md, 2026/06/2026-06-23_correccion_rotacion_ring_buffer.md, 2026/07/2026-07-07_configuracion_pruebas_fase5_gpd.md
- **conectividad**:
  - @Milton: 2026/04/2026-04-23_configuracion_tailscale_estaciones.md
- **correlador**:
  - @Milton: 2026/07/2026-07-23_correlador_eventos_regionales_mqtt.md, 2026/08/2026-08-21_diagnostico_resolucion_trazas_correlador_event_analyzer.md, 2026/08/2026-08-25_correccion_resolucion_trazas_migracion_influxdb.md, 2026/08/2026-08-27_respaldo_influxdb_multiservidor_fase5.md
- **dashboards**:
  - @Milton: 2026/02/2026-02-13_grafana_dashboard_persistence.md, 2026/05/2026-05-15_estabilizacion_nodered_dashboard.md
- **dependencias**:
  - @Milton: 2026/02/2026-02-11_migracion_trixie_bullseye.md, 2026/02/2026-02-24_planeacion_migracion_entornos_virtuales.md, 2026/07/2026-07-10_depuracion_gpd_logs.md
- **deploy**:
  - @Milton: 2026/04/2026-04-27_correccion_deploy_acelerografo.md, 2026/09/2026-09-01_automatizacion_despliegue_acelerografo.md
- **diagnostico**:
  - @Milton: 2026/06/2026-06-11_diagnostico_registro_continuo.md, 2026/09/2026-09-01_diagnostico_adquisicion_cha01.md
- **csv**:
  - @Milton: 2026/07/2026-07-07_pipeline_extraccion_automatica_fase4.md
- **deteccion-sismica**:
  - @Milton: 2026/07/2026-07-02_worker_inferencia_gpd_fase3.md, 2026/07/2026-07-07_pipeline_extraccion_automatica_fase4.md, 2026/07/2026-07-15_depuracion_mqtt_gpd.md, 2026/07/2026-07-23_correlador_eventos_regionales_mqtt.md
- **dnsmasq**:
  - @Milton: 2026/06/2026-06-04_wifi_ap_seguro.md
- **docker**:
  - @Milton: 2026/02/2026-02-20_consolidacion_tig_mqtt.md, 2026/05/2026-05-12_nodered_dashboard_layout.md, 2026/05/2026-05-15_estabilizacion_nodered_dashboard.md, 2026/07/2026-07-23_correlador_eventos_regionales_mqtt.md, 2026/07/2026-07-24_event_analyzer_fase1_streamlit.md, 2026/07/2026-07-31_despliegue_nuevo_servidor_ubuntu.md, 2026/08/2026-08-27_respaldo_influxdb_multiservidor_fase5.md, 2026/08/2026-08-31_reubicacion_respaldos_influxdb_gdrive.md
- **documentacion**:
  - @Milton: 2026/02/2026-02-20_consolidacion_tig_mqtt.md, 2026/02/2026-02-24_planeacion_migracion_entornos_virtuales.md
- **drive**:
  - @Milton: 2026/01/2026-01-28_correcion_bug_subida_drive.md, 2026/07/2026-07-23_correlador_eventos_regionales_mqtt.md, 2026/08/2026-08-31_reubicacion_respaldos_influxdb_gdrive.md, 2026/09/2026-09-01_diagnostico_adquisicion_cha01.md
- **dsp**:
  - @Milton: 2026/04/2026-04-23_migracion_sensor_ultrasonico.md, 2026/06/2026-06-30_shm_preprocesador_gpd.md, 2026/07/2026-07-24_event_analyzer_fase1_streamlit.md, 2026/08/2026-08-06_seleccion_calendario_downsampling_event_analyzer.md
- **dspic**:
  - @Milton: 2026/07/2026-07-29_ensamblaje_red_monitorizacion_shm.md
- **eagle**:
  - @Milton: 2026/07/2026-07-15_planificacion_pasantias.md
- **esp32**:
  - @Milton: 2026/04/2026-04-23_migracion_sensor_ultrasonico.md
- **event-analyzer**:
  - @Milton: 2026/07/2026-07-24_event_analyzer_fase1_streamlit.md, 2026/08/2026-08-06_seleccion_calendario_downsampling_event_analyzer.md, 2026/08/2026-08-19_integracion_influxdb_event_analyzer_nodered.md, 2026/08/2026-08-21_diagnostico_resolucion_trazas_correlador_event_analyzer.md, 2026/08/2026-08-25_correccion_resolucion_trazas_migracion_influxdb.md
- **fase4**:
  - @Milton: 2026/07/2026-07-07_pipeline_extraccion_automatica_fase4.md
- **flask**:
  - @Milton: 2026/06/2026-06-04_panel_web_config_fase4.md
- **flux**:
  - @Milton: 2026/02/2026-02-13_grafana_dashboard_persistence.md
- **frontend**:
  - @Milton: 2026/06/2026-06-11_diagnostico_registro_continuo.md
- **fuse**:
  - @Milton: 2026/07/2026-07-23_correlador_eventos_regionales_mqtt.md
- **git**:
  - @Milton: 2026/02/2026-02-23_conectividad_remota_estaciones.md, 2026/09/2026-09-01_automatizacion_despliegue_acelerografo.md
- **gpd**:
  - @Milton: 2026/06/2026-06-30_shm_preprocesador_gpd.md, 2026/07/2026-07-02_worker_inferencia_gpd_fase3.md, 2026/07/2026-07-07_pipeline_extraccion_automatica_fase4.md, 2026/07/2026-07-07_configuracion_pruebas_fase5_gpd.md, 2026/07/2026-07-10_depuracion_gpd_logs.md, 2026/07/2026-07-15_depuracion_mqtt_gpd.md
- **gpo**:
  - @Milton: 2026/05/2026-05-26_micromamba_w11.md
- **grafana**:
  - @Milton: 2026/02/2026-02-13_grafana_dashboard_persistence.md, 2026/05/2026-05-14_correccion_datos_duplicados_grafana.md, 2026/05/2026-05-22_quiosco_grafana_seguro.md, 2026/07/2026-07-31_despliegue_nuevo_servidor_ubuntu.md
- **gui**:
  - @Milton: 2026/06/2026-06-22_migracion_gui_analisis.md
- **hostapd**:
  - @Milton: 2026/06/2026-06-04_wifi_ap_seguro.md
- **influxdb**:
  - @Milton: 2026/02/2026-02-13_grafana_dashboard_persistence.md, 2026/05/2026-05-14_correccion_datos_duplicados_grafana.md, 2026/07/2026-07-31_despliegue_nuevo_servidor_ubuntu.md, 2026/08/2026-08-19_integracion_influxdb_event_analyzer_nodered.md, 2026/08/2026-08-21_diagnostico_resolucion_trazas_correlador_event_analyzer.md, 2026/08/2026-08-25_correccion_resolucion_trazas_migracion_influxdb.md, 2026/08/2026-08-26_plan_implementacion_fase5_respaldo_recuperacion.md, 2026/08/2026-08-27_respaldo_influxdb_multiservidor_fase5.md, 2026/08/2026-08-31_reubicacion_respaldos_influxdb_gdrive.md
- **json**:
  - @Milton: 2026/06/2026-06-11_diagnostico_registro_continuo.md
- **kicad**:
  - @Milton: 2026/07/2026-07-15_planificacion_pasantias.md
- **kiosk**:
  - @Milton: 2026/05/2026-05-22_quiosco_grafana_seguro.md
- **logging**:
  - @Milton: 2026/01/2026-01-15_optimizacion_logging.md, 2026/01/2026-01-28_correcion_bug_subida_drive.md, 2026/06/2026-06-17_ring_buffer_fase4_event_extractor.md, 2026/07/2026-07-10_depuracion_gpd_logs.md
- **memoria-compartida**:
  - @Milton: 2026/06/2026-06-30_shm_preprocesador_gpd.md
- **microc**:
  - @Milton: 2026/07/2026-07-29_ensamblaje_red_monitorizacion_shm.md
- **micropython**:
  - @Milton: 2026/07/2026-07-28_planificacion_pasantia_sensor_ultrasonico.md
- **micromamba**:
  - @Milton: 2026/05/2026-05-26_micromamba_w11.md
- **migracion**:
  - @Milton: 2026/02/2026-02-11_migracion_trixie_bullseye.md
- **mqtt**:
  - @Milton: 2026/02/2026-02-03_refactorizacion_mqtt_coordinator.md, 2026/02/2026-02-20_consolidacion_tig_mqtt.md, 2026/04/2026-04-21_resolucion_bug_mqtt_coordinator.md, 2026/04/2026-04-24_reversion_debouncing_mqtt_coordinator.md, 2026/05/2026-05-11_extraccion_remota_mqtt.md, 2026/05/2026-05-12_nodered_dashboard_layout.md, 2026/05/2026-05-15_estabilizacion_nodered_dashboard.md, 2026/05/2026-05-15_comandos_broadcast_mqtt.md, 2026/06/2026-06-17_ring_buffer_fase4_event_extractor.md, 2026/07/2026-07-07_configuracion_pruebas_fase5_gpd.md, 2026/07/2026-07-15_depuracion_mqtt_gpd.md, 2026/07/2026-07-22_zona_horaria_nodered.md, 2026/07/2026-07-23_correlador_eventos_regionales_mqtt.md, 2026/07/2026-07-31_despliegue_nuevo_servidor_ubuntu.md, 2026/08/2026-08-19_integracion_influxdb_event_analyzer_nodered.md, 2026/08/2026-08-26_plan_implementacion_fase5_respaldo_recuperacion.md, 2026/08/2026-08-27_respaldo_influxdb_multiservidor_fase5.md, 2026/09/2026-09-01_automatizacion_despliegue_acelerografo.md, 2026/09/2026-09-02_resiliencia_pipeline_adquisicion.md
- **multi-servidor**:
  - @Milton: 2026/08/2026-08-27_respaldo_influxdb_multiservidor_fase5.md
- **mseed**:
  - @Milton: 2026/01/2026-01-15_optimizacion_logging.md, 2026/05/2026-05-11_extraccion_remota_mqtt.md, 2026/06/2026-06-16_ring_buffer_acelerografo.md, 2026/06/2026-06-17_ring_buffer_fase4_event_extractor.md, 2026/07/2026-07-24_event_analyzer_fase1_streamlit.md, 2026/08/2026-08-21_diagnostico_resolucion_trazas_correlador_event_analyzer.md, 2026/08/2026-08-25_correccion_resolucion_trazas_migracion_influxdb.md
- **named-pipe**:
  - @Milton: 2026/09/2026-09-01_diagnostico_adquisicion_cha01.md, 2026/09/2026-09-02_resiliencia_pipeline_adquisicion.md
- **node-red**:
  - @Milton: 2026/05/2026-05-12_nodered_dashboard_layout.md, 2026/05/2026-05-15_estabilizacion_nodered_dashboard.md, 2026/07/2026-07-22_zona_horaria_nodered.md, 2026/07/2026-07-31_despliegue_nuevo_servidor_ubuntu.md, 2026/08/2026-08-19_integracion_influxdb_event_analyzer_nodered.md
- **ntp**:
  - @Milton: 2026/09/2026-09-03_sincronizacion_ntp_systemd.md
- **obspy**:
  - @Milton: 2026/07/2026-07-24_event_analyzer_fase1_streamlit.md
- **os**:
  - @Milton: 2026/02/2026-02-11_migracion_trixie_bullseye.md
- **pasantias**:
  - @Milton: 2026/07/2026-07-15_planificacion_pasantias.md, 2026/07/2026-07-29_ensamblaje_red_monitorizacion_shm.md, 2026/07/2026-07-28_planificacion_pasantia_sensor_ultrasonico.md
- **plantillas**:
  - @Milton: 2026/06/2026-06-03_unificacion_configuracion.md
- **plotly**:
  - @Milton: 2026/07/2026-07-24_event_analyzer_fase1_streamlit.md, 2026/08/2026-08-06_seleccion_calendario_downsampling_event_analyzer.md
- **redes**:
  - @Milton: 2026/04/2026-04-23_configuracion_tailscale_estaciones.md, 2026/04/2026-04-24_reversion_debouncing_mqtt_coordinator.md
- **rclone**:
  - @Milton: 2026/07/2026-07-23_correlador_eventos_regionales_mqtt.md, 2026/08/2026-08-26_plan_implementacion_fase5_respaldo_recuperacion.md, 2026/08/2026-08-27_respaldo_influxdb_multiservidor_fase5.md, 2026/08/2026-08-31_reubicacion_respaldos_influxdb_gdrive.md
- **refactorizacion**:
  - @Milton: 2026/02/2026-02-03_refactorizacion_mqtt_coordinator.md, 2026/06/2026-06-22_migracion_gui_analisis.md
- **resiliencia**:
  - @Milton: 2026/09/2026-09-02_resiliencia_pipeline_adquisicion.md, 2026/09/2026-09-03_sincronizacion_ntp_systemd.md
- **scripts**:
  - @Milton: 2026/04/2026-04-27_correccion_deploy_acelerografo.md, 2026/05/2026-05-11_extraccion_remota_mqtt.md
- **seguridad**:
  - @Milton: 2026/06/2026-06-04_wifi_ap_seguro.md
- **sensor**:
  - @Milton: 2026/04/2026-04-23_migracion_sensor_ultrasonico.md, 2026/07/2026-07-28_planificacion_pasantia_sensor_ultrasonico.md
- **shell**:
  - @Milton: 2026/05/2026-05-26_micromamba_w11.md
- **shm**:
  - @Milton: 2026/07/2026-07-15_planificacion_pasantias.md, 2026/07/2026-07-29_ensamblaje_red_monitorizacion_shm.md
- **streaming**:
  - @Milton: 2026/06/2026-06-16_ring_buffer_acelerografo.md, 2026/06/2026-06-17_ring_buffer_fase4_event_extractor.md, 2026/06/2026-06-23_correccion_rotacion_ring_buffer.md, 2026/06/2026-06-30_shm_preprocesador_gpd.md, 2026/07/2026-07-02_worker_inferencia_gpd_fase3.md, 2026/09/2026-09-02_resiliencia_pipeline_adquisicion.md
- **supervisor**:
  - @Milton: 2026/09/2026-09-01_diagnostico_adquisicion_cha01.md
- **streamlit**:
  - @Milton: 2026/07/2026-07-24_event_analyzer_fase1_streamlit.md, 2026/08/2026-08-06_seleccion_calendario_downsampling_event_analyzer.md
- **systemd**:
  - @Milton: 2026/07/2026-07-23_correlador_eventos_regionales_mqtt.md, 2026/08/2026-08-27_respaldo_influxdb_multiservidor_fase5.md, 2026/09/2026-09-02_resiliencia_pipeline_adquisicion.md, 2026/09/2026-09-03_sincronizacion_ntp_systemd.md
- **tailscale**:
  - @Milton: 2026/04/2026-04-23_configuracion_tailscale_estaciones.md
- **telegraf**:
  - @Milton: 2026/05/2026-05-14_correccion_datos_duplicados_grafana.md
- **telemetria**:
  - @Milton: 2026/02/2026-02-03_refactorizacion_mqtt_coordinator.md, 2026/04/2026-04-21_resolucion_bug_mqtt_coordinator.md, 2026/05/2026-05-11_extraccion_remota_mqtt.md, 2026/05/2026-05-15_comandos_broadcast_mqtt.md, 2026/06/2026-06-16_ring_buffer_acelerografo.md, 2026/07/2026-07-07_configuracion_pruebas_fase5_gpd.md
- **ubuntu**:
  - @Milton: 2026/05/2026-05-22_quiosco_grafana_seguro.md, 2026/07/2026-07-31_despliegue_nuevo_servidor_ubuntu.md
- **ui**:
  - @Milton: 2026/05/2026-05-12_nodered_dashboard_layout.md, 2026/05/2026-05-15_estabilizacion_nodered_dashboard.md, 2026/07/2026-07-22_zona_horaria_nodered.md
- **unificacion**:
  - @Milton: 2026/06/2026-06-03_unificacion_configuracion.md
- **ultrasonico**:
  - @Milton: 2026/04/2026-04-23_migracion_sensor_ultrasonico.md, 2026/07/2026-07-28_planificacion_pasantia_sensor_ultrasonico.md
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
- **zona-horaria**:
  - @Milton: 2026/07/2026-07-22_zona_horaria_nodered.md

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
- ADR-013: decisiones/013_downsampling_trazas_y_estado_reactivo_event_analyzer.md (event-analyzer, streamlit, plotly, dsp, downsampling, performance) — Diezmado dinámico de trazas (max 3000 pts) y selección reactiva por calendario en Event Analyzer para evitar congestión de WebSockets y demoras UI.
- ADR-014: decisiones/014_resampling_dinamico_alta_resolucion_plotly_resampler.md (event-analyzer, plotly-resampler, downsampling, sismologia, performance, docker) — Resampling dinámico de alta resolución mediante plotly-resampler para picada exacta de fases sísmicas P y S.
- ADR-015: decisiones/015_indice_eventos_sismicos_influxdb_clasificacion_mqtt.md (influxdb, mqtt, telegraf, event-analyzer, nodered, correlador, streaming) — Índice centralizado de eventos sísmicos en InfluxDB, ingesta desacoplada MQTT QoS 1 con Telegraf, Lazy Loading de trazas MiniSEED y ciclo cerrado de clasificación.
- ADR-016: decisiones/016_resolucion_global_trazas_y_determinacion_temporal_event_id.md (event-analyzer, correlador, miniseed, influxdb, mqtt, lazy-loading, consistencia-temporal, arquitectura) — Resolución global de trazas MiniSEED en Event Analyzer, aislamiento de datos de registro continuo y generación determinista del event_id a partir de dt_min en el Correlador Regional.
- ADR-017: decisiones/017_respaldo_influxdb_y_arquitectura_multiservidor_con_sesion_persistente_mqtt.md (influxdb, backup, restore, rclone, gdrive, systemd, multi-servidor, mqtt, sesion-persistente, clean_session, docker-compose, profiles, arquitectura) — Protocolo de respaldo híbrido de InfluxDB (snapshot + CSV), automatización portable con plantillas systemd y arquitectura multi-servidor con sesión persistente MQTT.
- ADR-018: decisiones/018_resiliencia_pipeline_adquisicion_acelerografo.md (acelerografo, resiliencia, adquisicion, systemd, dspic, named_pipe, watchdog, mqtt, supervisor) — Arquitectura de defensa en profundidad en 4 capas para auto-recuperación y desacoplamiento del pipeline de adquisición.

---

## Diagnósticos Técnicos

- **acelerografo**:
  - `2026-09-01` — Parada de adquisición CHA1: Fallo en cascada por named pipe tras reinicio y bloqueo de subidas a Google Drive.
    Estado: **resuelto** (2026-09-02). Produjo: ADR-018, blueprint 2026-09-02_plan_resiliencia_pipeline_adquisicion.md.
  - `2026-09-01` — Automatización y saneamiento del despliegue en estaciones acelerográficas (segregación main/develop y actualización OTA vía MQTT).
    Estado: **pendiente**.
    → [2026-09-01_diagnostico_automatizacion_despliegue.md](https://github.com/RedSismicaAustro/RSA-Acelerografo/blob/main/docs/analysis/2026-09-01_diagnostico_automatizacion_despliegue.md)
- **tig**:
  - `2026-09-02` — Ingesta, persistencia y alertamiento de la telemetría status/acquisition en el Stack TIG.
    Estado: **pendiente**.
    → [2026-09-02_diagnostico_ingesta_telemetria_status_acquisition.md](https://github.com/RedSismicaAustro/RSA-Intern-TIG-MQTT/blob/main/docs/analysis/2026-09-02_diagnostico_ingesta_telemetria_status_acquisition.md)


