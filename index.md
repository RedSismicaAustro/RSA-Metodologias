---
title: Inicio
layout: home
nav_order: 1
---

# Wiki Técnica — Red Sísmica del Austro

Documentación oficial de procedimientos, metodologías y conocimiento técnico de la **Red Sísmica del Austro (RSA)** de la Universidad de Cuenca.

## Acerca de este Sitio

Este sitio funciona como la **Wiki técnica** del equipo RSA. Centraliza los procedimientos operativos, las guías de configuración del entorno de trabajo, la arquitectura técnica de los proyectos y el conocimiento acumulado a través de sesiones de desarrollo.

La documentación está orientada a desarrolladores, investigadores y personal técnico que colabora en los proyectos de la RSA.

---

## Secciones

### 🧠 [Exocortex RSA](manuales/exocortex/)
Sistema de memoria externa asistido por IA para el equipo RSA. Cubre el despliegue completo, la gestión de sesiones de trabajo, la consulta de historiales técnicos y la documentación de decisiones de arquitectura.

### 💻 [Entornos](manuales/entornos/)
Configuración de entornos de desarrollo: WSL, desarrollo remoto con SSHFS, gestión de ambientes con Micromamba.

### 🔀 [Git](manuales/git/)
Metodologías de control de versiones: comandos esenciales, estrategia de ramas, formato de commits, gestión de múltiples cuentas de GitHub.

### 🌐 [Redes](manuales/redes/)
Configuración de hardware de red para estaciones de campo: adaptadores WiFi para Raspberry Pi, protocolos de comunicación.

### 🔧 [PCB](manuales/pcb/)
Diseño y fabricación de circuitos impresos: prototipado rápido con impresora SLA.

### 📚 [Conocimiento Técnico](conocimiento/)
Índice de la arquitectura técnica de los proyectos RSA: acelerógrafo, stack TIG, dispositivos de borde.

### 📄 [Decisiones de Arquitectura](decisiones/)
Registro de las decisiones técnicas más importantes tomadas en el desarrollo de proyectos RSA (Architecture Decision Records).

---

## Proyectos Principales

| Proyecto | Descripción |
|----------|-------------|
| **RSA-Acelerógrafo** | Sistema de adquisición de datos sísmicos en Raspberry Pi |
| **RSA-Intern-TIG-MQTT** | Dashboard de monitoreo en tiempo real (Telegraf + InfluxDB + Grafana + Node-RED) |
| **Server-RSA** | Servidores centrales de procesamiento y almacenamiento de datos |

---

**Sitio generado con:** Jekyll + Just the Docs | **Repositorio:** [RedSismicaAustro/RSA-Metodologias](https://github.com/RedSismicaAustro/RSA-Metodologias) | **Despliegue:** GitHub Pages