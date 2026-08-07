---
id: ADR-014
titulo: Resampling Dinámico de Alta Resolución mediante Plotly-Resampler para Picada de Fases Sísmicas
estado: Aceptado
fecha: 2026-08-06
temas: [event-analyzer, plotly-resampler, downsampling, sismologia, performance, docker]
entorno: RSA-Intern-TIG-MQTT
---

# ADR-014: Resampling Dinámico de Alta Resolución mediante Plotly-Resampler para Picada de Fases Sísmicas

## Estado

**Aceptado** | Fecha: 2026-08-06

## Contexto

En el sistema `event-analyzer`, la implementación de diezmado estático (`[::step]`) resolvió el fallo de memoria `MessageSizeError` (>200 MB), pero introdujo una limitación sismológica: al hacer zoom en regiones de interés para picar los tiempos de llegada de las ondas P y S, el usuario observaba datos submuestreados (líneas rectas interpoladas) en lugar de las muestras crudas nativas (100-200 Hz).

Se requería un mecanismo que mantuviera la ligereza de la vista general inicial sin perder la fidelidad milimétrica de la traza original al ampliar la señal.

## Opciones Evaluadas

### Opción A: Diezmado Estático por Envolvente (Min-Max)
- **Ventajas:** No requiere comunicación bidireccional entre el navegador y el servidor.
- **Desventajas:** Mantiene un número de puntos fijo, por lo que un zoom profundo sigue mostrando picos aproximados sin la forma de onda continua exacta ni la resolución temporal nativa.

### Opción B: Integración de `plotly-resampler` con Servidor Asíncrono Dash (Puerto 8050)
- **Ventajas:**
  - Muestra una vista inicial diezmada a 3,000 puntos (carga instantánea y consumo bajo de ancho de banda).
  - Al realizar zoom, el navegador envía una petición asíncrona (AJAX/WebSockets) al servidor con el rango de tiempo seleccionado.
  - El servidor recalcula y devuelve únicamente los puntos de alta frecuencia para esa ventana, permitiendo ver el 100% de las muestras crudas originales al inspeccionar las ondas P y S.
- **Desventajas:** Requiere exponer un puerto secundario (`8050:8050`) en Docker y configurar el Hostname de la LAN (`RESAMPLER_HOST`).

## Decisión

Se eligió la **Opción B**.

1. Se añadió `plotly-resampler>=0.9.1` a las dependencias del proyecto.
2. Se inyectó `FigureResampler(sub_fig, default_n_shown_samples=3000)` en `visualizer.py` pasando las trazas crudas mediante `hf_x` y `hf_y`.
3. Se expuso el puerto `8050:8050` en `docker-compose.yml` e inyectó `register_plotly_resampler(mode="Dash", port=8050, host="0.0.0.0")` en `app.py`.

## Consecuencias

- **Precisión Sismológica**: Los sismólogos/operadores pueden picar inicios de fase P y S con la resolución original completa (100-200 Hz).
- **Rendimiento Protegido**: La vista inicial del evento completo se mantiene diezmada a ~3000 puntos (~2 MB), evitando cuelgues del navegador.
- **Infraestructura LAN**: Requiere el mapeo del puerto 8050 en el cortafuegos de la máquina host y la correcta configuración de `RESAMPLER_HOST` en redes locales.

## Referencias

- Contexto técnico relacionado: `RSA-Intern-TIG-MQTT/docs/context/event_analyzer_context.md`
- Código modificado: `services/event-analyzer/requirements.txt`, `services/docker-unified/docker-compose.yml`, `services/event-analyzer/app.py`, `services/event-analyzer/src/modules/visualizer.py`
