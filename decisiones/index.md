---
title: Decisiones de Arquitectura
nav_order: 9
---

# Decisiones de Arquitectura (ADR)

Esta sección contiene los Architecture Decision Records (ADR) de la Red Sísmica del Austro.

Cada ADR documenta una decisión técnica importante: el contexto que la motivó, las opciones evaluadas, la decisión tomada y sus consecuencias.

## Cómo usar los ADRs

- Para **consultar** una decisión existente: busca por número o tema en el [Índice Temático](../indice/indice_tematico.md).
- Para **crear** un nuevo ADR: usa el skill `extraer_adr` del agente IA.

## ADRs Activos

- [ADR-001: Unificación de la Configuración del Acelerógrafo](001_unificacion_configuracion_acelerografo.md) (Aceptado, 2026-06-03)
- [ADR-006: Apertura del Named Pipe en Modo Lectura-Escritura (O_RDWR) para Evitar EOF](006_apertura_named_pipe_lectura_escritura.md) (Aceptado, 2026-06-24 / Actualizado, 2026-09-15)
- [ADR-016: Resolución Global de Trazas MiniSEED, Exclusión de Registro Continuo y Determinación Temporal del event_id](016_resolucion_global_trazas_y_determinacion_temporal_event_id.md) (Aceptado, 2026-08-21)
- [ADR-017: Protocolo de Respaldo Híbrido de InfluxDB, Automatización Portable y Arquitectura Multi-Servidor con Sesión Persistente MQTT](017_respaldo_influxdb_y_arquitectura_multiservidor_con_sesion_persistente_mqtt.md) (Aceptado, 2026-08-28)
- [ADR-018: Resiliencia y Desacoplamiento del Pipeline de Adquisición Acelerográfica](018_resiliencia_pipeline_adquisicion_acelerografo.md) (Aceptado, 2026-09-02)
- [ADR-019: Modelo Jerárquico de Alertas y Visualización Centralizada de Estaciones en Grafana](019_modelo_jerarquico_alertas_y_visualizacion_grafana.md) (Aceptado, 2026-09-14)
- [ADR-020: Telemetría Especializada, Cadencia Unificada a 5 Minutos y Parada Remota de Contingencia en Estaciones Acelerográficas](020_telemetria_especializada_cadencia_unificada_y_parada_seguridad_estaciones.md) (Aceptado, 2026-09-14 / Actualizado, 2026-09-15)
- [ADR-021: Sincronización Espejo del Registro de Google Drive con el Almacenamiento Local y Síntesis de Diagnóstico](021_sincronizacion_espejo_registro_drive_y_sintesis_diagnostico.md) (Aceptado, 2026-09-17)
- [ADR-022: Gobernanza del Ciclo de Vida de Eventos Extraídos, Resiliencia Store and Forward y Prioridad de Desalojo en Acelerógrafos](022_gobernanza_ciclo_vida_eventos_extraidos_y_store_and_forward.md) (Aceptado, 2026-09-17)
