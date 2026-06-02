---
title: Contexto Técnico
parent: Exocortex RSA
nav_order: 4
---

# Contexto Técnico de Proyectos

Los **contextos técnicos** son documentos estructurados que describen la arquitectura, el funcionamiento y las dependencias de un script o componente. Sirven como memoria semántica: permiten al agente entender cómo funciona el código sin necesidad de leerlo completo cada vez.

---

## Dónde Viven los Contextos Técnicos

Cada contexto técnico vive en el **repositorio del proyecto** que documenta, bajo `docs/context/`:

```text
RSA-Acelerografo/
└── docs/
    └── context/
        ├── mqtt_coordinator_context.md
        ├── binary_to_mseed_context.md
        └── ...
```

El índice temático en `RSA-Metodologias` mantiene una referencia de una línea + link a cada contexto, sin duplicar el contenido.

---

## Generación de un Contexto Técnico

Usa el comando:

```
genera el contexto de mqtt_coordinator.py
```

El agente ejecuta el skill `generar_contexto` y:

1. Lee el archivo fuente completo (`mqtt_coordinator.py`).
2. Analiza arquitectura, dependencias, funciones clave, configuración y limitaciones.
3. Genera `docs/context/mqtt_coordinator_context.md` en el proyecto.
4. Actualiza el índice temático en `RSA-Metodologias`.

---

## Ejemplo de Contexto Técnico

El siguiente es un extracto real del contexto de `mqtt_coordinator.py`:

```markdown
---
proyecto: acelerografo
tipo: contexto_tecnico
archivo: scripts/operation/mqtt/mqtt_coordinator.py
temas: [mqtt, telemetria, daemon, hardware]
generado: 2026-03-15
---
# mqtt_coordinator.py — Contexto para Agentes IA

> Agente reactivo MQTT que corre como daemon en Raspberry Pi. Publica
> telemetría (estado operacional + métricas de hardware), recibe comandos
> remotos y tiene placeholder para correlación regional de eventos sísmicos.

**Ruta**: `scripts/operation/mqtt/mqtt_coordinator.py`
**LOC**: 373 | **Lenguaje**: Python 3 | **Dependencias**: paho-mqtt, python-dotenv
**Proceso**: Daemon gestionado por Supervisor

## Arquitectura

[diagrama Mermaid del flujo de datos]

## Comandos Soportados
| Comando | Estado |
|---------|--------|
| `get_status` | ✅ Funcional |
| `extract_event` | ✅ Funcional (asíncrono) |
| `restart_acquisition` | ❌ TODO |

## Limitaciones Conocidas
- Sin TLS/SSL configurado.
- `HEALTH_INTERVAL` hardcoded (300s).
```

---

## Actualización del Índice (Opción C)

El agente actualiza el índice temático con una referencia de una línea:

```markdown
## Contextos Técnicos (Código y Arquitectura)

- **acelerografo**:
  - `mqtt_coordinator.py`: Agente MQTT daemon en Raspberry Pi →
    [RSA-Acelerografo/docs/context/mqtt_coordinator_context.md](
    https://github.com/RedSismicaAustro/RSA-Acelerografo/blob/main/
    docs/context/mqtt_coordinator_context.md)
```

Esto permite al agente conocer la existencia del contexto sin necesidad de tener el proyecto clonado localmente.

---

## Actualización de un Contexto Existente

Cuando el código cambia significativamente, regenera el contexto:

```
genera el contexto de mqtt_coordinator.py
```

El agente sobreescribirá el archivo existente y actualizará el campo `generado:` en el front matter.

> **Recomendación**: Regenera el contexto cada vez que hagas un cambio arquitectónico importante (nueva clase, cambio de protocolo, nueva dependencia). No es necesario regenerarlo por bugfixes menores.
