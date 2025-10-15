---
title: RSA Metodologias
layout: home
nav_order: 1
---

# RSA Metodologias

Sitio de documentación oficial de la **Red Sísmica del Austro (RSA)** que proporciona metodologías, guías técnicas y mejores prácticas para el desarrollo, configuración y mantenimiento de la infraestructura y proyectos de la red.

## Acerca de la Red Sísmica del Austro

La Red Sísmica del Austro es un grupo de investigación adscrito al Departamento de Ingeniería Civil del Vicerrectorado de Investigación de la Universidad de Cuenca. Su labor se centra en la operación y mantenimiento de estaciones de adquisición de datos sísmicos distribuidas en la región austral del Ecuador.

El proyecto combina hardware especializado, sistemas de telemetría, procesamiento de datos en tiempo real y almacenamiento centralizado, con el propósito de monitorear y analizar la actividad sísmica regional y contribuir al entendimiento del comportamiento sísmico del territorio.

## Objetivo de este Sitio

Este repositorio centraliza la documentación técnica necesaria para:

- Configurar entornos de desarrollo para proyectos RSA
- Implementar flujos de trabajo estandarizados con Git y GitHub
- Configurar hardware para estaciones de campo (Raspberry Pi, sensores sísmicos)
- Gestionar infraestructura y despliegues
- Establecer protocolos de red para estaciones remotas

La documentación está orientada a desarrolladores, investigadores y personal técnico que colabora en los proyectos de RSA.

---

## Contenido de esta Documentación

### [Entornos](docs/entornos/)
Guías para la configuración de entornos de desarrollo, incluyendo instalación de WSL, configuración de Ubuntu, gestión de ambientes con conda/micromamba, y preparación de sistemas para desarrollo de software.

### [Git](docs/git/)
Metodologías de control de versiones específicas para proyectos RSA, incluyendo convenciones de commits, estrategia de branching, comandos esenciales y configuración de múltiples cuentas de GitHub en un mismo equipo.

### [Redes](docs/redes/)
Configuración de hardware de red y comunicaciones para estaciones de campo, incluyendo adaptadores WiFi para Raspberry Pi, configuración de interfaces de red y protocolos de telemetría.

---

## Proyectos Relacionados

Esta documentación da soporte a los siguientes proyectos de la Red Sísmica del Austro:

- **RSA-Acelerografo**: Sistema de adquisición de datos sísmicos basado en Raspberry Pi
- **RSA-Intern-TIG-MQTT**: Dashboard de monitoreo en tiempo real (stack TIG - Telegraf, InfluxDB, Grafana)
- **Server-RSA**: Servidores centrales de procesamiento y almacenamiento de datos

---

## Navegación

Utilizar el menú lateral para explorar las diferentes secciones de documentación. Cada guía incluye instrucciones paso a paso, ejemplos de código y casos de uso específicos para la infraestructura RSA.

## Contribuciones

Esta documentación es mantenida de forma colaborativa por el equipo de RSA. Para sugerir cambios o reportar errores, utilizar el enlace "Edit this page on GitHub" disponible en cada página de documentación, o abrir un issue en el repositorio correspondiente.

---

## Información Técnica

**Sitio generado con:** Jekyll + Just the Docs theme  
**Repositorio:** [RedSismicaAustro/RSA-Metodologias](https://github.com/RedSismicaAustro/RSA-Metodologias)  
**Despliegue:** GitHub Pages (actualización automática)