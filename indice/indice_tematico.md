---
tipo: indice_maestro
actualizado: 2026-06-17
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
  - `firmware`: Firmware del acelerógrafo (C) → [RSA-Acelerografo/docs/context/firmware_context.md](https://github.com/RedSismicaAustro/RSA-Acelerografo/blob/main/docs/context/firmware_context.md)
  - `gestor_archivos_acq.py`: Gestor de archivos de adquisición → [RSA-Acelerografo/docs/context/gestor_archivos_acq_context.md](https://github.com/RedSismicaAustro/RSA-Acelerografo/blob/main/docs/context/gestor_archivos_acq_context.md)
  - `mqtt_coordinator.py`: Agente MQTT daemon en Raspberry Pi → [RSA-Acelerografo/docs/context/mqtt_coordinator_context.md](https://github.com/RedSismicaAustro/RSA-Acelerografo/blob/main/docs/context/mqtt_coordinator_context.md)
  - `registro_continuo.py`: Registro continuo de datos sísmicos → [RSA-Acelerografo/docs/context/registro_continuo_context.md](https://github.com/RedSismicaAustro/RSA-Acelerografo/blob/main/docs/context/registro_continuo_context.md)
  - `mseed_event_extractor.py`: Extractor de eventos miniSEED → [RSA-Acelerografo/docs/context/mseed_event_extractor_context.md](https://github.com/RedSismicaAustro/RSA-Acelerografo/blob/main/docs/context/mseed_event_extractor_context.md)
  - `mseed_event_inspector.py`: Inspector de eventos miniSEED → [RSA-Acelerografo/docs/context/mseed_event_inspector_context.md](https://github.com/RedSismicaAustro/RSA-Acelerografo/blob/main/docs/context/mseed_event_inspector_context.md)
  - `web_context.md`: Entorno web de configuración (Flask/Vanilla JS) → [RSA-Acelerografo/docs/context/web_context.md](https://github.com/RedSismicaAustro/RSA-Acelerografo/blob/main/docs/context/web_context.md)
- **tig**:
  - `docker-compose-tig-mqtt`: Stack TIG con MQTT (Telegraf/InfluxDB/Grafana) → [RSA-Intern-TIG-MQTT/docs/context/docker-compose-tig-mqtt_context.md](https://github.com/RedSismicaAustro/RSA-Intern-TIG-MQTT/blob/main/docs/context/docker-compose-tig-mqtt_context.md)
  - `docker-compose-nodered`: Stack Node-RED → [RSA-Intern-TIG-MQTT/docs/context/docker-compose-nodered_context.md](https://github.com/RedSismicaAustro/RSA-Intern-TIG-MQTT/blob/main/docs/context/docker-compose-nodered_context.md)
- **edge-device**: (pendiente)

---

## Sesiones por Entorno

- **acelerografo**:
  - @Milton: 2026/01/2026-01-15_optimizacion_logging.md, 2026/01/2026-01-28_correcion_bug_subida_drive.md, 2026/02/2026-02-03_refactorizacion_mqtt_coordinator.md, 2026/02/2026-02-11_migracion_trixie_bullseye.md, 2026/02/2026-02-23_conectividad_remota_estaciones.md, 2026/02/2026-02-24_planeacion_migracion_entornos_virtuales.md, 2026/04/2026-04-21_resolucion_bug_mqtt_coordinator.md, 2026/04/2026-04-23_configuracion_tailscale_estaciones.md, 2026/04/2026-04-24_reversion_debouncing_mqtt_coordinator.md, 2026/04/2026-04-27_correccion_deploy_acelerografo.md, 2026/05/2026-05-11_extraccion_remota_mqtt.md, 2026/05/2026-05-15_comandos_broadcast_mqtt.md, 2026/06/2026-06-03_unificacion_configuracion.md, 2026/06/2026-06-04_panel_web_config_fase4.md, 2026/06/2026-06-04_wifi_ap_seguro.md, 2026/06/2026-06-11_diagnostico_registro_continuo.md, 2026/06/2026-06-16_ring_buffer_acelerografo.md
- **edge-device**:
  - @Milton: 2026/04/2026-04-23_migracion_sensor_ultrasonico.md
- **entornos-virtuales**:
  - @Milton: 2026/05/2026-05-26_micromamba_w11.md
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
  - @Milton: 2026/01/2026-01-28_correcion_bug_subida_drive.md, 2026/04/2026-04-21_resolucion_bug_mqtt_coordinator.md, 2026/04/2026-04-24_reversion_debouncing_mqtt_coordinator.md, 2026/05/2026-05-14_correccion_datos_duplicados_grafana.md
- **cloudflare**:
  - @Milton: 2026/02/2026-02-23_conectividad_remota_estaciones.md
- **configuracion**:
  - @Milton: 2026/06/2026-06-03_unificacion_configuracion.md, 2026/06/2026-06-04_panel_web_config_fase4.md
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
- **dnsmasq**:
  - @Milton: 2026/06/2026-06-04_wifi_ap_seguro.md
- **docker**:
  - @Milton: 2026/02/2026-02-20_consolidacion_tig_mqtt.md, 2026/05/2026-05-12_nodered_dashboard_layout.md, 2026/05/2026-05-15_estabilizacion_nodered_dashboard.md
- **documentacion**:
  - @Milton: 2026/02/2026-02-20_consolidacion_tig_mqtt.md, 2026/02/2026-02-24_planeacion_migracion_entornos_virtuales.md
- **drive**:
  - @Milton: 2026/01/2026-01-28_correcion_bug_subida_drive.md
- **dsp**:
  - @Milton: 2026/04/2026-04-23_migracion_sensor_ultrasonico.md
- **esp32**:
  - @Milton: 2026/04/2026-04-23_migracion_sensor_ultrasonico.md
- **flask**:
  - @Milton: 2026/06/2026-06-04_panel_web_config_fase4.md
- **flux**:
  - @Milton: 2026/02/2026-02-13_grafana_dashboard_persistence.md
- **frontend**:
  - @Milton: 2026/06/2026-06-11_diagnostico_registro_continuo.md
- **git**:
  - @Milton: 2026/02/2026-02-23_conectividad_remota_estaciones.md
- **gpo**:
  - @Milton: 2026/05/2026-05-26_micromamba_w11.md
- **grafana**:
  - @Milton: 2026/02/2026-02-13_grafana_dashboard_persistence.md, 2026/05/2026-05-14_correccion_datos_duplicados_grafana.md, 2026/05/2026-05-22_quiosco_grafana_seguro.md
- **hostapd**:
  - @Milton: 2026/06/2026-06-04_wifi_ap_seguro.md
- **influxdb**:
  - @Milton: 2026/02/2026-02-13_grafana_dashboard_persistence.md, 2026/05/2026-05-14_correccion_datos_duplicados_grafana.md
- **json**:
  - @Milton: 2026/06/2026-06-11_diagnostico_registro_continuo.md
- **kiosk**:
  - @Milton: 2026/05/2026-05-22_quiosco_grafana_seguro.md
- **logging**:
  - @Milton: 2026/01/2026-01-15_optimizacion_logging.md, 2026/01/2026-01-28_correcion_bug_subida_drive.md
- **micromamba**:
  - @Milton: 2026/05/2026-05-26_micromamba_w11.md
- **migracion**:
  - @Milton: 2026/02/2026-02-11_migracion_trixie_bullseye.md
- **mqtt**:
  - @Milton: 2026/02/2026-02-03_refactorizacion_mqtt_coordinator.md, 2026/02/2026-02-20_consolidacion_tig_mqtt.md, 2026/04/2026-04-21_resolucion_bug_mqtt_coordinator.md, 2026/04/2026-04-24_reversion_debouncing_mqtt_coordinator.md, 2026/05/2026-05-11_extraccion_remota_mqtt.md, 2026/05/2026-05-12_nodered_dashboard_layout.md, 2026/05/2026-05-15_estabilizacion_nodered_dashboard.md, 2026/05/2026-05-15_comandos_broadcast_mqtt.md
- **mseed**:
  - @Milton: 2026/01/2026-01-15_optimizacion_logging.md, 2026/05/2026-05-11_extraccion_remota_mqtt.md, 2026/06/2026-06-16_ring_buffer_acelerografo.md
- **node-red**:
  - @Milton: 2026/05/2026-05-12_nodered_dashboard_layout.md, 2026/05/2026-05-15_estabilizacion_nodered_dashboard.md
- **os**:
  - @Milton: 2026/02/2026-02-11_migracion_trixie_bullseye.md
- **plantillas**:
  - @Milton: 2026/06/2026-06-03_unificacion_configuracion.md
- **redes**:
  - @Milton: 2026/04/2026-04-23_configuracion_tailscale_estaciones.md, 2026/04/2026-04-24_reversion_debouncing_mqtt_coordinator.md
- **refactorizacion**:
  - @Milton: 2026/02/2026-02-03_refactorizacion_mqtt_coordinator.md
- **scripts**:
  - @Milton: 2026/04/2026-04-27_correccion_deploy_acelerografo.md, 2026/05/2026-05-11_extraccion_remota_mqtt.md
- **seguridad**:
  - @Milton: 2026/06/2026-06-04_wifi_ap_seguro.md
- **sensor**:
  - @Milton: 2026/04/2026-04-23_migracion_sensor_ultrasonico.md
- **shell**:
  - @Milton: 2026/05/2026-05-26_micromamba_w11.md
- **streaming**:
  - @Milton: 2026/06/2026-06-16_ring_buffer_acelerografo.md
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
  - @Milton: 2026/05/2026-05-26_micromamba_w11.md

---

## Decisiones de Arquitectura

- ADR-001: decisiones/001_unificacion_configuracion_acelerografo.md (configuracion, unificacion, plantillas, automatizacion) — Unificación de la configuración del acelerógrafo mediante plantillas y script de hidratación.
- ADR-002: decisiones/002_panel_web_configuracion_flask.md (web, configuracion, seguridad) — Selección de Flask sobre FastAPI para el servidor web de configuración del acelerógrafo por consumo de memoria.
- ADR-003: decisiones/003_wifi_ap_aislamiento_firewall.md (wifi, seguridad, redes) — Aislamiento del puerto 5000 en la interfaz eth0 mediante iptables durante el funcionamiento del WiFi AP.
