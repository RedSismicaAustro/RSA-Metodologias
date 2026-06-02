---
title: Gestión de Sesiones
parent: Exocortex RSA
nav_order: 2
---

# Gestión de Sesiones de Trabajo

Las sesiones son el mecanismo principal de la **memoria episódica** del exocortex: registros cronológicos de lo que se hizo, cómo se resolvió y qué se decidió en cada jornada de trabajo.

---

## ¿Cuándo hacer un volcado de bitácora?

Realiza un volcado al finalizar cualquier sesión de trabajo que haya producido:

- Un cambio de comportamiento en el código (bugfix, nueva función, refactorización).
- Una decisión de arquitectura (elegir una librería, cambiar un protocolo).
- Una configuración nueva de infraestructura (Docker, Tailscale, MQTT broker).
- Un procedimiento nuevo que necesitarás recordar (cómo desplegar, cómo depurar).

No es necesario hacer un volcado por conversaciones exploratorias o de análisis sin cambios.

---

## Volcado desde una Sesión Activa con el Agente

Al finalizar la sesión, escribe el siguiente comando al agente:

```
ejecutar el volcado de bitácora
```

El agente ejecutará automáticamente el skill `volcado_bitacora` y:

1. Analizará toda la conversación actual.
2. Identificará la fecha de inicio, temas y entorno.
3. Creará el archivo `.md` en `institucional/RSA-Bitacora-LLM-Milton/sesiones/YYYY/MM/`.
4. Actualizará el índice temático en `RSA-Metodologias/indice/indice_tematico.md`.
5. Confirmará las rutas y temas indexados.

### Ejemplo de salida del volcado

Suponiendo que la sesión fue sobre corregir un bug en el módulo MQTT:

```
✅ Bitácora actualizada:
   Ruta: institucional/RSA-Bitacora-LLM-Milton/sesiones/2026/06/2026-06-02_correccion_bug_mqtt.md
   Temas indexados: bugfix, mqtt, telemetria
   Entorno indexado: acelerografo
   Índice actualizado: rsa/RSA-Metodologias/indice/indice_tematico.md
```

---

## Estructura de un Archivo de Sesión

Cada archivo de bitácora tiene la siguiente estructura:

```markdown
---
fecha: 2026-06-02
temas: [bugfix, mqtt, telemetria]
entorno: [acelerografo]
autor: Milton
---

# Actividad del 2026-06-02

**Hitos de la jornada:**
Se identificó y corrigió un bug en `mqtt_coordinator.py` donde el dispatcher
no manejaba correctamente los payloads con campos opcionales ausentes...

**Decisiones y Cambios:**
- Se agregó validación de campos con `payload.get()` en lugar de `payload[]`.
- Se aumentaron los tests de integración para cubrir payloads incompletos.

**Scripts/Comandos relevantes:**
```python
def _cmd_extract_event(self, payload, client):
    params = {
        "start": payload.get("start"),
        "end": payload.get("end"),
        "station": payload.get("station", self.config["id"])
    }
```
---
```

---

## Volcado desde un Chat Externo (Sin Agente Activo)

Si la sesión ocurrió en un chat que ya no está disponible (ej. chat web de Claude o ChatGPT), usa la plantilla de volcado manual:

### Paso 1: Abre una nueva sesión con el agente

```
Para esta sesión, nuestro directorio de trabajo exclusivo será institucional/RSA-Bitacora-LLM-Milton/.
```

### Paso 2: Pega el prompt de extracción

Copia el siguiente prompt y reemplaza `[PEGA AQUÍ EL HISTORIAL DE CHAT]` con el texto del chat exportado:

```
Actúa como un analista técnico de sistemas. Analiza el siguiente historial de chat
y extrae el conocimiento técnico, las configuraciones creadas y las decisiones tomadas.
Ignora cualquier interacción trivial y céntrate exclusivamente en el valor de ingeniería.

Utiliza la fecha: [FECHA DE LA SESIÓN]
Ejecuta luego el volcado de bitácora con el resultado.

---
[PEGA AQUÍ EL HISTORIAL DE CHAT]
```

### Paso 3: Revisa y confirma

El agente generará el archivo de sesión y lo guardará automáticamente. Revisa el contenido antes de hacer commit.

---

## Actualización Manual del Índice Temático

El skill actualiza el índice automáticamente. Si necesitas corregir o añadir una entrada manualmente, el índice está en `rsa/RSA-Metodologias/indice/indice_tematico.md` y sigue este formato:

```markdown
## Sesiones por Entorno
- **acelerografo**:
  - @Milton: 2026/06/2026-06-02_correccion_bug_mqtt.md

## Sesiones por Tema
- **bugfix**:
  - @Milton: 2026/06/2026-06-02_correccion_bug_mqtt.md
- **mqtt**:
  - @Milton: 2026/06/2026-06-02_correccion_bug_mqtt.md
```

> **Importante**: Después de modificar `RSA-Metodologias`, recuerda hacer `git commit` y `git push` para que el índice quede sincronizado en la nube.
