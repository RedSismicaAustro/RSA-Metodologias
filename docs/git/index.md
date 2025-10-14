---
title: Git
nav_order: 4
has_children: true
---

# Git

Guías y metodologías para el control de versiones en los proyectos de la Red Sísmica del Austro.

## Contenido de esta sección

Esta sección contiene información sobre las convenciones y mejores prácticas de Git utilizadas en RSA:

### [Formato de Commits](formato-commits.md)
Convenciones para escribir mensajes de commit claros y consistentes, utilizando prefijos estandarizados como `FEAT`, `FIX`, `DOCS`, etc.

### [Tipos de Ramas](tipos-ramas.md)
Estrategia de branching que define los tipos de ramas (`main`, `develop`, `feature/`, `bugfix/`, `hotfix/`, `release/`, `support/`) y su uso en el flujo de trabajo.

---

## ¿Por qué estas convenciones?

Seguir estas convenciones nos permite:

- **Mantener un historial claro**: Los commits con prefijos facilitan entender qué tipo de cambios se realizaron sin revisar el código.
- **Facilitar la colaboración**: Todos los miembros del equipo siguen las mismas reglas, reduciendo la confusión.
- **Automatizar procesos**: Los prefijos permiten filtrar commits y automatizar generación de changelogs.
- **Gestionar versiones eficientemente**: El modelo de branching separa claramente desarrollo, correcciones y producción.

---

## Aplicación en proyectos RSA

Estas metodologías se aplican a todos los repositorios de la Red Sísmica del Austro, incluyendo:

- **RSA-Acelerografo**: Sistema de adquisición de datos sísmicos
- **RSA-Intern-TIG-MQTT**: Dashboard de monitoreo en tiempo real
- **Server-RSA**: Servidores de procesamiento central
- **RSA-Metodologias**: Este sitio de documentación

Siguiendo estas prácticas aseguramos consistencia y calidad en todo nuestro código.