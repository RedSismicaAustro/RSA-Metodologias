# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

**RSA-Metodologias** is a documentation website for the **Red Sísmica del Austro (RSA)** that provides methodologies, guides, and best practices for working with RSA infrastructure and projects. The site is built with **Jekyll** using the **Just the Docs** theme and automatically deployed to GitHub Pages.

**Live site:** https://redsismicaaustro.github.io/RSA-Metodologias

## Repository Structure

The site follows Jekyll's Just the Docs theme conventions:

```
RSA-Metodologias/
├── index.md                      # Homepage (nav_order: 1)
├── _config.yml                   # Jekyll configuration
├── docs/                         # Documentation content
│   ├── entornos/                 # Development environment guides
│   │   ├── index.md
│   │   └── instalacion-wsl.md
│   ├── git/                      # Git workflow guides
│   │   ├── index.md
│   │   ├── comandos-comunes.md
│   │   ├── tipos-ramas.md
│   │   ├── formato-commits.md
│   │   └── multiples-cuentas-github.md
│   └── redes/                    # Network/hardware guides
│       ├── index.md
│       └── adaptador_wifi_rpi.md
└── .github/workflows/
    └── jekyll-gh-pages.yml       # Auto-deploy to GitHub Pages
```

## Jekyll Site Configuration

**Theme:** `just-the-docs/just-the-docs` (remote theme)
**Base URL:** `/RSA-Metodologias`
**Main URL:** `https://redsismicaaustro.github.io`

Key features enabled in [_config.yml](_config.yml):
- Search functionality (`search_enabled: true`)
- Remote theme via `jekyll-remote-theme` plugin
- Mermaid diagram support (version 10.6.1)
- "Edit this page" GitHub links for all documentation pages

## Content Organization

Documentation pages use Jekyll front matter to define structure:

```yaml
---
title: Page Title          # Display name
parent: Parent Category    # For nested pages (optional)
nav_order: 1              # Menu position
layout: home              # Only for index.md
---
```

**Current categories:**
- **Entornos**: Development environment setup (WSL, Ubuntu, etc.)
- **Git**: Git workflows, version control best practices, and GitHub configuration
- **Redes**: Network and hardware configuration (Raspberry Pi WiFi adapters, etc.)

## Working with Documentation

### Adding a New Guide

1. Create a markdown file in the appropriate `docs/` category directory
2. Add front matter with `title`, `parent`, and `nav_order`
3. Write content using standard markdown
4. The site rebuilds automatically on push to `main` branch

Example:
```markdown
---
title: Docker Installation
parent: Entornos
nav_order: 2
---

# Docker Installation
Guide content here...
```

### Creating a New Category

1. Create a new directory under `docs/` (e.g., `docs/devops/`)
2. Add documentation files with `parent: CategoryName` in front matter
3. The category appears automatically in the navigation menu

### Testing Locally

To preview the site locally with Jekyll:

```bash
# Install Ruby and Bundler first
gem install bundler jekyll

# In repository root
bundle install
bundle exec jekyll serve

# Site available at http://localhost:4000/RSA-Metodologias
```

## Deployment

The site uses GitHub Actions for automatic deployment:

- **Trigger:** Push to `main` branch or manual workflow dispatch
- **Workflow:** [.github/workflows/jekyll-gh-pages.yml](.github/workflows/jekyll-gh-pages.yml)
- **Process:** Build with Jekyll → Deploy to GitHub Pages
- **Permissions:** Repository must have Pages enabled with Actions as source

Changes pushed to `main` are live within 1-2 minutes.

## Content Guidelines

Documentation in this repository covers:
- Development environment setup (WSL, Linux, conda/micromamba)
- Git workflows specific to RSA projects
- Hardware configuration for field stations (Raspberry Pi, sensors)
- DevOps and infrastructure (Docker, deployment)
- Networking for remote seismic stations

**Tone:** Technical, precise, step-by-step instructions in Spanish.

## Related RSA Projects

This documentation supports multiple RSA projects:
- **RSA-Acelerografo**: Seismic data acquisition on Raspberry Pi devices
- **RSA-Intern-TIG-MQTT**: Real-time monitoring dashboard (TIG stack)
- **Server-RSA**: Central data processing servers

When documenting procedures, consider cross-project applicability.

## Git Configuration

The repository uses SSH key with host alias configuration:
- Remote: `git@github.com-rsa:RedSismicaAustro/RSA-Metodologias.git`
- This requires SSH config with `github.com-rsa` host alias

## Current State

**Active documentation:**

*Entornos (Development Environments):*
- WSL/Ubuntu installation and configuration

*Git (Version Control):*
- Comandos Comunes: Comprehensive Git command reference (initialization, configuration, commits, branches, merges, GitHub integration, troubleshooting)
- Tipos de Ramas: Branching strategy (main, develop, feature/, bugfix/, hotfix/, release/, support/)
- Formato de Commits: Commit message conventions with standardized prefixes (FEAT, FIX, DOCS, etc.)
- Múltiples Cuentas de GitHub: Multi-account GitHub setup with SSH keys, aliases, and automatic identity management per directory

*Redes (Networking):*
- TP-Link TL-WN822N WiFi adapter setup for Raspberry Pi 3B+

**Future sections planned:**
- DevOps guides (Docker, CI/CD, deployment)
- Additional hardware configuration guides
- Data processing workflows

The Git documentation section is now complete with comprehensive guides covering the entire workflow used by RSA projects.


## Git Commit Format

This project follows the Red Sísmica del Austro commit convention. All commits must include a prefix to indicate the type of change.

### Commit Prefixes

| Prefix | Purpose | Example |
|--------|---------|---------|
| **WIP:** | Work in Progress - incomplete work | `WIP: Implementación parcial de la autenticación` |
| **FIX:** | Bug fixes | `FIX: Corregido error de validación de formulario en login` |
| **FEAT:** | New features | `FEAT: Añadido soporte para autenticación con tokens JWT` |
| **REFACTOR:** | Code refactoring (no functionality change) | `REFACTOR: Mejorada la legibilidad del módulo de autenticación` |
| **DOCS:** | Documentation changes | `DOCS: Actualizado el README con instrucciones de autenticación` |
| **STYLE:** | Code style/formatting (no logic change) | `STYLE: Formateado el código según el estándar PEP8` |
| **CHORE:** | Maintenance tasks, dependency updates | `CHORE: Actualizadas las dependencias de npm` |
| **TEST:** | Adding or modifying tests | `TEST: Añadidos tests unitarios para el módulo de autenticación` |
| **PERF:** | Performance improvements | `PERF: Optimizado el procesamiento de datos en el servidor` |
| **BUILD:** | Build system or external dependencies | `BUILD: Actualizado script de despliegue automático` |
| **CI:** | CI/CD configuration changes | `CI: Configurado GitHub Actions para ejecutar pruebas automáticas` |
| **SECURITY:** | Security patches or improvements | `SECURITY: Corregida vulnerabilidad de inyección de SQL` |
| **HOTFIX:** | Critical production fixes | `HOTFIX: Corregido error de autenticación que rompía la sesión` |

### Commit Message Structure

```
<UPPERCASE PREFIX>: <Short description in Spanish>

<Optional longer description explaining the changes>

<Optional technical details, bullet points, etc.>

🤖 Generated with Claude Code

Co-Authored-By: Claude <noreply@anthropic.com>
```

### Examples

```bash
# Feature commit
git commit -m "FEAT: Agregar scripts de preprocesamiento para GPD con 1 componente (Z)"

# Bug fix
git commit -m "FIX: Corregir error en normalización de amplitud en preprocess_dataset.py"

# Documentation
git commit -m "DOCS: Actualizar guía de instalación con nuevas dependencias"

# Multiple changes
git commit -m "REFACTOR: Simplificar pipeline de validación

- Extraer funciones de plotting a módulo separado
- Mejorar manejo de errores en carga de datos
- Actualizar tests unitarios"
```

### Filtering Commits by Type

To view commits of a specific type:

```bash
# View all feature commits
git log --grep="FEAT"

# View all bug fixes
git log --grep="FIX"

# View all documentation changes
git log --grep="DOCS"
```

### Benefits

1. **Clarity**: Quickly identify the purpose of each commit
2. **Clean History**: Easy to review and filter commit history
3. **Collaboration**: Team members understand changes without examining code
4. **Automation**: Enables automatic changelog generation and release notes