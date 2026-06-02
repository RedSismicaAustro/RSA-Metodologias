---
title: Decisiones de Arquitectura (ADR)
parent: Exocortex RSA
nav_order: 5
---

# Decisiones de Arquitectura (ADR)

Un **Architecture Decision Record (ADR)** es un documento que captura una decisión de diseño o arquitectura importante: el contexto que la motivó, las opciones evaluadas, la decisión tomada y sus consecuencias.

Los ADRs evitan que el conocimiento técnico crítico quede sepultado dentro de las bitácoras de sesión.

---

## ¿Cuándo Crear un ADR?

Crea un ADR cuando:

- Elegiste una tecnología o protocolo sobre otras alternativas.
- Tomaste una decisión que será difícil de revertir.
- Hay una razón no obvia detrás de cómo está implementado algo.
- Un miembro nuevo del equipo podría preguntarse *¿por qué se hizo así?*

No crees un ADR para decisiones rutinarias o fácilmente reversibles.

---

## Creación de un ADR

### Desde la sesión actual

```
extrae un ADR sobre la decisión de usar Wayland en lugar de X11 para el quiosco
```

### Desde una sesión pasada

```
extrae un ADR de la sesión 2026/05/2026-05-22_quiosco_grafana_seguro.md
```

El agente ejecuta el skill `extraer_adr` y:

1. Identifica el contexto, opciones y decisión tomada.
2. Asigna el siguiente número secuencial disponible en `decisiones/`.
3. Genera el archivo `NNN_titulo.md` en `rsa/RSA-Metodologias/decisiones/`.
4. Actualiza el índice temático.

---

## Estructura de un ADR

```markdown
---
id: ADR-001
titulo: Implementación de quiosco Grafana con sesión GNOME sobre Wayland
estado: Aceptado
fecha: 2026-05-22
temas: [grafana, ubuntu, wayland, kiosk]
entorno: tig
---

# ADR-001: Implementación de Quiosco Grafana con GNOME/Wayland

## Estado
**Aceptado** | Fecha: 2026-05-22

## Contexto
Se necesitaba mostrar un panel de Grafana en pantalla completa en un servidor
Ubuntu 24.04 con SeisComP3 instalado. La máquina se administra exclusivamente
por SSH, sin periferias locales permanentes...

## Opciones Evaluadas

### Opción A: Forzar X11 desactivando Wayland globalmente
- **Ventajas:** Compatibilidad total con aplicaciones legacy.
- **Desventajas:** Rompe la compatibilidad de SeisComP3 con Qt/Wayland.

### Opción B: Usar gestor de ventanas ligero (Openbox)
- **Ventajas:** Menor consumo de recursos.
- **Desventajas:** Requiere migrar SeisComP3 a nuevo entorno gráfico.

### Opción C: Autostart a nivel de usuario en GNOME/Wayland
- **Ventajas:** No invasivo, respeta el entorno existente de SeisComP3.
- **Desventajas:** Mayor complejidad en la configuración inicial.

## Decisión
Se eligió la **Opción C** porque es no invasiva y protege la estabilidad
de SeisComP3, que es software crítico de monitoreo sísmico.

## Consecuencias
- ✅ SeisComP3 mantiene su entorno gráfico original sin modificaciones.
- ✅ El quiosco se inicia automáticamente tras cada reinicio.
- ⚠️ La configuración requiere que el usuario esté con sesión gráfica activa.
- 🔜 Evaluar autologin de GNOME para eliminar la dependencia de sesión manual.

## Referencias
- Sesión de origen: `@Milton: 2026/05/2026-05-22_quiosco_grafana_seguro.md`
```

---

## Consultar ADRs Existentes

```
¿Qué ADRs existen sobre el stack TIG?
```

El agente busca en `## Decisiones de Arquitectura` del índice temático y devuelve los ADRs relacionados.

También puedes navegar directamente en la sección [Decisiones de Arquitectura](../../decisiones/) del sitio.

---

## Estados de un ADR

| Estado | Descripción |
|--------|-------------|
| **Propuesto** | En discusión, aún no implementado |
| **Aceptado** | Implementado y vigente |
| **Deprecado** | Ya no aplica, pero se mantiene por referencia histórica |
| **Reemplazado por ADR-NNN** | Sustituido por una decisión posterior |

> **Regla**: Los ADRs son **inmutables** una vez en estado *Aceptado*. Para cambiar una decisión, crea un nuevo ADR que la reemplace y actualiza el estado del anterior.
