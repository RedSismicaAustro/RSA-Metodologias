---
id: ADR-028
titulo: Unificación de Orquestación Docker Compose y Modularización de Servicios
estado: Aceptado
fecha: 2026-11-10
temas: [docker-compose, orquestacion, modularizacion, node-red, telegraf, influxdb, mqtt]
entorno: [RSA-Monitor-Sismico, server-ubuntu, dev-wsl2]
---

# ADR-028: Unificación de Orquestación Docker Compose y Modularización de Servicios

## Estado

**Aceptado** | Fecha: 2026-11-10

## Contexto

El repositorio `RSA-Monitor-Sismico` orquesta la plataforma central de observabilidad, ingesta de telemetría, correlación regional y visualización sísmica de la Red Sísmica del Austro. Tras la consolidación inicial, la arquitectura presentaba fragmentaciones operativas:

1. **Doble punto de entrada de ejecución**:
   - `services/docker-unified/docker-compose.yml` orquestaba InfluxDB, Telegraf, Grafana, Correlator, Event Analyzer y DB Sync.
   - `services/node-red/docker-compose.yml` orquestaba de forma aislada a Node-RED (`rsa-nodered`).
   - Esto obligaba a levantar contenedores en rutas separadas, propiciaba errores de resolución en la red Docker externa `rsa_network` y complicaba los despliegues en servidores remotos.
2. **Asimetría en la estructura de configuraciones**:
   - `telegraf.conf` residía embebido en `services/docker-unified/`, rompiendo la simetría modular frente a componentes como Grafana (`services/grafana/provisioning/`).
   - InfluxDB carecía de directorio modular (`services/influxdb/`) y de mecanismo automatizado para la creación desatendida del bucket secundario permanente `rsa_events` en despliegues desde cero.
   - En Node-RED se evidenciaron problemas de desconexión periódica ("Client Stealing") por colisión de `client_id` MQTT entre entornos locales y de producción.

## Opciones Evaluadas

### Opción A: Mantener orquestación desacoplada multi-compose
- **Ventajas:** Aísla el ciclo de vida del panel de control de Node-RED respecto al stack central TIG.
- **Desventajas:** Duplica la administración de proyectos Docker, incrementa la probabilidad de colisiones de puertos y nombres de contenedores (como ocurrió con `/rsa-nodered` en producción), y dispersa la gobernanza de variables de entorno.

### Opción B: Centralizar en orquestador maestro unificado y modularizar configuraciones (Elegida)
- **Ventajas:** 
  - Punto de control único: `docker compose up -d` despliega y supervisa todos los microservicios (`rsa-influxdb`, `rsa-telegraf`, `rsa-grafana`, `rsa-correlator`, `rsa-event-analyzer`, `rsa-nodered`).
  - Simetría estructural: cada componente posee su carpeta de configuración pura (`services/telegraf/`, `services/influxdb/`, `services/node-red/`, `services/grafana/`).
  - Auto-aprovisionamiento: InfluxDB incorpora script `init/01-init-buckets.sh` para crear `rsa_events` de forma idempotente en `/docker-entrypoint-initdb.d/`.
  - Aislamiento MQTT: parametrización de `NODE_RED_CLIENT_ID` diferenciando entornos de desarrollo y producción.
- **Desventajas:** Requiere actualizar montajes de volumen relativos en el compose maestro.

## Decisión

Se eligió la **Opción B**. Se integró el servicio `nodered` en `services/docker-unified/docker-compose.yml`, se eliminó el archivo redundante `services/node-red/docker-compose.yml` preservando el directorio como módulo de configuración pura (`flows.json`, `settings.js`, `package.json`), y se crearon los directorios modulares `services/telegraf/` (albergando `telegraf.conf` y su README) e `services/influxdb/` (albergando scripts de inicio e información de retención).

## Consecuencias

- **Positivas:**
  - Despliegue atómico, determinista y consistente tanto en desarrollo local (Windows 11 / WSL 2) como en producción (`server-ubuntu`).
  - Modularidad clara y documentada para cada microservicio del stack.
  - Eliminación de desconexiones por client stealing en Node-RED mediante `NODE_RED_CLIENT_ID`.
  - Creación automática del bucket `rsa_events` sin intervención manual en inicializaciones limpias de InfluxDB.
- **Negativas / Consideraciones:**
  - En servidores donde el contenedor `rsa-nodered` ya existía previamente bajo el compose anterior, se debe remover el contenedor viejo huérfano (`docker stop rsa-nodered && docker rm rsa-nodered`) antes del primer arranque del stack unificado.

## Referencias

- Blueprint: [2026-11-10_unificacion_docker_compose_y_reestructuracion_servicios.md](file:///home/rsa/git/rsa-proyectos/RSA-Monitor-Sismico/docs/blueprints/2026-11-10_unificacion_docker_compose_y_reestructuracion_servicios.md)
- Release oficial: `v1.1.0`
- Contexto técnico asociado: [services/docker-unified/docker-compose.yml](file:///home/rsa/git/rsa-proyectos/RSA-Monitor-Sismico/services/docker-unified/docker-compose.yml)
