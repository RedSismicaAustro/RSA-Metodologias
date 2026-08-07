---
id: ADR-013
titulo: Diezmado Dinámico de Trazas Sísmicas y Selección Reactiva por Calendario en Event Analyzer
estado: Aceptado
fecha: 2026-08-06
temas: [event-analyzer, streamlit, plotly, dsp, downsampling, performance]
entorno: RSA-Intern-TIG-MQTT
---

# ADR-013: Diezmado Dinámico de Trazas Sísmicas y Selección Reactiva por Calendario en Event Analyzer

## Estado

**Aceptado** | Fecha: 2026-08-06

## Contexto

El sistema web `event-analyzer` expuesto en Streamlit (puerto 8501) permite la visualización e inspección multiestación de eventos sísmicos regionales. Durante el uso en caliente se presentaron dos problemas críticos de rendimiento y arquitectura de UI:

1. **Auto-renderizado prematuro**: La aplicación refrescaba y calculaba gráficos pesados automáticamente cada vez que el usuario cambiaba la opción de un desplegable o seleccionaba una estación, generando tiempos de espera innecesarios antes de confirmar la selección deseada.
2. **Exceso de payload WebSocket (`MessageSizeError > 200 MB`)**: Al graficar eventos multiestación densos (alta frecuencia de muestreo y registros prolongados), Plotly enviaba millones de puntos en formato JSON al navegador web. Esto excedió el límite estricto de WebSockets de Streamlit (200MB por defecto) y congelaba la memoria RAM del navegador.

## Opciones Evaluadas

### Opción A: Aumentar el límite `server.maxMessageSize` de Streamlit a 500MB+
- **Ventajas:** No requiere modificar la lógica de graficación en Python.
- **Desventajas:** No resuelve la congelación del navegador. Enviar 200MB+ de JSON sobre WebSockets satura los hilos JS del navegador, tardando minutos en renderizar un gráfico que físicamente solo tiene ~1920 píxeles horizontales en pantalla.

### Opción B: Filtrar eventos por fecha (Calendario) + Diezmado dinámico (Downsampling) en Plotly
- **Ventajas:** 
  - Al agrupar eventos por fecha y requerir la confirmación mediante un botón (`✅ Aplicar Selección de Eventos`), la UI no realiza cargas pesadas hasta que el usuario lo indique.
  - Al aplicar submuestreo por slicing eficiente en NumPy (`data[::step]`) acotado a un máximo de 3,000 puntos por traza (`MAX_POINTS_PER_TRACE`), el tamaño del payload enviado al navegador se reduce de >200MB a ~1-2MB.
  - Los cálculos de procesamiento DSP (Detrending, filtro pasabanda Butterworth y estimación de PGA Z) se mantienen sobre la señal cruda a máxima resolución.
- **Desventajas:** Ligera pérdida de detalle visual extremo en escalas de zoom hiper-profundas, aunque imperceptible a nivel de inspección regional.

## Decisión

Se eligió la **Opción B**. 

1. Se reestructuró la selección de eventos en `app.py` utilizando un widget de fecha `st.date_input` que filtra interactivamente la lista de eventos del día, manteniendo la UI en blanco hasta presionar `✅ Aplicar Selección de Eventos`.
2. Se implementó diezmado dinámico en `visualizer.py` limitando las trazas de Plotly a un máximo de `MAX_POINTS_PER_TRACE = 3000` puntos mediante slicing en NumPy.
3. Se añadió el montaje bind `../event-analyzer:/app` en `docker-compose.yml` para desarrollo y actualización en vivo.

## Consecuencias

- **Rendimiento instantáneo**: El tiempo de renderizado de gráficos multiestación pasó de varios minutos (o fallo con pantalla roja `MessageSizeError`) a menos de 2 segundos.
- **Eficiencia de CPU en Servidor**: La conversión de tiempos UTC (`times_utc_plot`) se realiza exclusivamente sobre los puntos diezmados (3,000 iteraciones en lugar de 100,000+).
- **Integridad Científica**: Los filtros DSP y las métricas de amplitud máxima (PGA) continúan calculándose sobre la traza cruda completa sin pérdida de precisión.

## Referencias

- Contexto técnico relacionado: `RSA-Intern-TIG-MQTT/docs/context/event_analyzer_context.md`
- Código modificado: `services/event-analyzer/app.py`, `services/event-analyzer/src/modules/visualizer.py`, `services/docker-unified/docker-compose.yml`
