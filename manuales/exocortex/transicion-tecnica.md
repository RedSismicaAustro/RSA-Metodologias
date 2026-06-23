---
title: Transición Técnica
parent: Exocortex RSA
nav_order: 6
---

# Transición Técnica de Sesiones

El archivo de **transición técnica** es un mecanismo clave de la memoria de desarrollo en el Exocortex RSA. Su objetivo es sistematizar los avances, el estado técnico de los entornos de desarrollo, las decisiones de código y los siguientes pasos al finalizar una sesión de trabajo con el agente de IA.

Esto asegura que cualquier agente subsecuente pueda reanudar el trabajo de forma inmediata y sin pérdida de contexto semántico.

---

## ¿Cuándo generar una Transición Técnica?

Ejecuta este flujo cuando finalices una sesión que contenga:
- Cambios significativos en la infraestructura o el entorno de desarrollo.
- Estructuración o creación de múltiples archivos/directorios.
- Lógica compleja que requiere continuidad inmediata por otro agente.
- Decisiones técnicas intermedias que no necesariamente ameritan un ADR formal pero sí documentar el porqué.

---

## Activación del Flujo con el Agente

Al finalizar la sesión, indica al agente uno de los siguientes comandos:

```text
crea un archivo de transición técnica
```
o bien:
```text
genera la transición técnica de esta sesión
```

El agente activará automáticamente el skill `crear_transicion_tecnica` y realizará los siguientes pasos:
1. **Identificación de rutas**: Determinará la raíz del proyecto y fijará el destino en `<raiz_del_proyecto>/docs/progress/`.
2. **Nombramiento**: Creará el archivo con el formato `YYYY-MM-DD_contexto-agente.md` basado en la fecha local.
3. **Extracción**: Analizará el historial del chat para estructurar hitos, entornos, decisiones, la estructura y los pasos pendientes.
4. **Escritura y Confirmación**: Creará el archivo con la plantilla estándar y sugerirá un formato de commit.

---

## Estructura Estándar del Archivo de Transición

El documento generado en `docs/progress/` seguirá exactamente esta estructura:

```markdown
# Resumen de Sesión: [Título descriptivo]

**Fecha**: YYYY-MM-DD  
**Repositorio**: `[nombre_del_repositorio]`  
**Agente de IA**: [Nombre del agente]  
**Usuario**: [Nombre del usuario]  

---

## 🎯 Objetivo de la Sesión
[Breve párrafo de 2-3 líneas describiendo la meta de la sesión de hoy].

---

## 📂 Estructura del Repositorio Implementada
[Diagrama de árbol de archivos y directorios creados o modificados].

```text
[Árbol de directorios]

---

## ⚙️ Configuración del Entorno Virtual (`[nombre_del_entorno]`)
[Detalle de entornos virtuales, dependencias, canales y soluciones mixtas pip/conda].

---

## 🛠️ Modificaciones de Código y Refactorización
[Detalle de cambios en archivos fuente, lógica depurada y simplificaciones].

---

## 📋 Pasos Sugeridos para el Siguiente Agente
[Listado secuencial y numerado de tareas pendientes].
```

---

## Formato de Confirmación del Agente

Una vez creado el archivo, el agente retornará una confirmación indicando:
- La ruta absoluta del archivo generado.
- Un breve resumen de los puntos clave.
- La recomendación de commit con el formato correcto en minúsculas (ej: `docs: agregar transición técnica de sesión`).
