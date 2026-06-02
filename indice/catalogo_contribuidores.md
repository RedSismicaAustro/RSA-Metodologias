---
tipo: catalogo
actualizado: 2026-06-02
published: false
---

# Catálogo de Contribuidores del Exocortex RSA

Este archivo define el mapeo entre usuarios y sus repositorios de bitácora personal.
Es utilizado por el skill `consulta_historica` para resolver rutas del formato `@Usuario: YYYY/MM/archivo.md`.

---

## Contribuidores Activos

| Alias en Índice | Nombre | Repositorio | Cuenta GitHub | Ruta en workspace |
|-----------------|--------|-------------|---------------|-------------------|
| @Milton | Milton R. | RSA-Bitacora-LLM-Milton | institucional | `institucional/RSA-Bitacora-LLM-Milton/sesiones/` |

---

## Cómo Agregar un Nuevo Contribuidor

1. Crear el repositorio `RSA-Bitacora-LLM-{Nombre}` en la cuenta institucional de GitHub.
2. Clonar el repo en el workspace bajo `git/institucional/`.
3. Agregar una fila a la tabla de este archivo.
4. El agente podrá resolver las referencias `@{Nombre}:` a partir de la próxima sincronización.

---

## Resolución de Rutas

Cuando el índice temático contiene:
```
- **acelerografo**:
  - @Milton: 2026/05/2026-05-11_extraccion_remota_mqtt.md
```

El agente lo resuelve como:
```
institucional/RSA-Bitacora-LLM-Milton/sesiones/2026/05/2026-05-11_extraccion_remota_mqtt.md
```
