# RSA-Metodologías

Sitio de documentación de metodologías, guías y mejores prácticas para la **Red Sísmica del Austro (RSA)** de la Universidad de Cuenca.

**🌐 Sitio web:** https://redsismicaaustro.github.io/RSA-Metodologias

---

## 📖 Descripción

Este repositorio contiene la documentación técnica y metodológica utilizada por el equipo de la Red Sísmica del Austro para el desarrollo, mantenimiento y operación de proyectos relacionados con:

- Sistemas de adquisición de datos sísmicos
- Infraestructura de monitoreo en tiempo real
- Servidores de procesamiento central
- Configuración de estaciones remotas

El sitio está construido con **Jekyll** utilizando el tema **Just the Docs** y se despliega automáticamente en **GitHub Pages**.

---

## 📚 Contenido

### Git
Guías completas sobre control de versiones y flujo de trabajo con Git y GitHub:

- **[Comandos Comunes](docs/git/comandos-comunes.md)**: Referencia exhaustiva de comandos Git desde básicos hasta avanzados, con ejemplos prácticos para cada operación
- **[Tipos de Ramas](docs/git/tipos-ramas.md)**: Estrategia de branching que define los tipos de ramas (`main`, `develop`, `feature/`, `bugfix/`, `hotfix/`, `release/`, `support/`) y su aplicación
- **[Formato de Commits](docs/git/formato-commits.md)**: Convenciones para mensajes de commit claros y consistentes usando prefijos estandarizados (`FEAT`, `FIX`, `DOCS`, etc.)
- **[Múltiples Cuentas de GitHub](docs/git/multiples-cuentas-github.md)**: Configuración detallada para trabajar con múltiples cuentas de GitHub usando llaves SSH, aliases y gestión automática de identidades

### Entornos
Configuración de entornos de desarrollo:

- **[Instalación de WSL](docs/entornos/instalacion-wsl.md)**: Guía para instalar y configurar Windows Subsystem for Linux (WSL) con Ubuntu

### Redes
Configuración de hardware y conectividad para estaciones remotas:

- **[Adaptador WiFi para Raspberry Pi](docs/redes/adaptador_wifi_rpi.md)**: Instalación y configuración del adaptador TP-Link TL-WN822N en Raspberry Pi 3B+

---

## 🏗️ Estructura del Repositorio

```
RSA-Metodologias/
├── index.md                      # Página principal del sitio
├── _config.yml                   # Configuración de Jekyll
├── README.md                     # Este archivo
├── CLAUDE.md                     # Guía para Claude Code
├── docs/                         # Documentación organizada por categorías
│   ├── entornos/                 # Guías de entornos de desarrollo
│   │   ├── index.md
│   │   └── instalacion-wsl.md
│   ├── git/                      # Guías de Git y GitHub
│   │   ├── index.md
│   │   ├── comandos-comunes.md
│   │   ├── tipos-ramas.md
│   │   ├── formato-commits.md
│   │   └── multiples-cuentas-github.md
│   └── redes/                    # Guías de redes y hardware
│       ├── index.md
│       └── adaptador_wifi_rpi.md
└── .github/workflows/
    └── jekyll-gh-pages.yml       # Despliegue automático a GitHub Pages
```

---

## 🚀 Despliegue

El sitio se despliega automáticamente a GitHub Pages mediante GitHub Actions:

- **Rama de despliegue:** `main`
- **Tiempo de publicación:** 1-5 minutos después del push
- **Flujo de trabajo:** Definido en [`.github/workflows/jekyll-gh-pages.yml`](.github/workflows/jekyll-gh-pages.yml)

Cualquier cambio realizado en la rama `main` se refleja automáticamente en el sitio web.

---

## 🛠️ Desarrollo Local

### Requisitos previos

- Ruby 2.7 o superior
- Bundler
- Jekyll

### Instalación

```bash
# Clonar el repositorio
git clone git@github.com-rsa:RedSismicaAustro/RSA-Metodologias.git
cd RSA-Metodologias

# Instalar dependencias
gem install bundler jekyll
bundle install

# Ejecutar servidor local
bundle exec jekyll serve

# El sitio estará disponible en:
# http://localhost:4000/RSA-Metodologias
```

### Agregar nueva documentación

1. Crear un archivo `.md` en la carpeta `docs/` correspondiente
2. Agregar el front matter de Jekyll:

```yaml
---
title: Título de la Página
parent: Categoría Padre
nav_order: 1
---
```

3. Escribir el contenido en Markdown
4. Hacer commit y push a la rama `main`
5. El sitio se actualizará automáticamente

---

## 📝 Guía de Estilo

### Mensajes de Commit

Seguir las convenciones documentadas en [Formato de Commits](docs/git/formato-commits.md):

```
TIPO: Descripción breve del cambio

Descripción detallada opcional...
```

**Tipos de commit:**
- `FEAT`: Nueva funcionalidad o contenido
- `FIX`: Corrección de errores
- `DOCS`: Cambios en documentación
- `STYLE`: Formato, estilo (sin cambios de código)
- `REFACTOR`: Refactorización de código
- `CHORE`: Tareas de mantenimiento

### Estructura de Documentos

- Usar encabezados jerárquicos (`#`, `##`, `###`)
- Incluir tabla de contenidos para documentos largos
- Agregar ejemplos prácticos con bloques de código
- Utilizar notas y advertencias cuando sea necesario

---

## 🔗 Proyectos Relacionados

Esta documentación da soporte a los siguientes proyectos de la RSA:

- **[RSA-Acelerografo](https://github.com/RedSismicaAustro/RSA-Acelerografo)**: Sistema de adquisición de datos sísmicos en Raspberry Pi
- **[RSA-Intern-TIG-MQTT](https://github.com/RedSismicaAustro/RSA-Intern-TIG-MQTT)**: Dashboard de monitoreo en tiempo real (stack TIG)
- **Server-RSA**: Servidores de procesamiento central de datos sísmicos

---

## 🤝 Contribución

Para contribuir a esta documentación:

1. Crear una rama desde `main` con el nombre apropiado según el tipo de cambio
2. Realizar los cambios en la documentación
3. Asegurarse de que el sitio se construye correctamente localmente
4. Crear un Pull Request describiendo los cambios realizados
5. Esperar la revisión y aprobación

---

## 📄 Licencia

Este proyecto es mantenido por la Red Sísmica del Austro - Universidad de Cuenca.

Copyright © 2025 Red Sísmica del Austro - Universidad de Cuenca

---

## 📞 Contacto

**Red Sísmica del Austro**
Universidad de Cuenca
Cuenca, Ecuador

- **GitHub**: https://github.com/RedSismicaAustro
- **Sitio web**: https://redsismicaaustro.github.io/RSA-Metodologias
