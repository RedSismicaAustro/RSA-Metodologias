---
title: Despliegue
parent: Exocortex RSA
nav_order: 1
---

# Despliegue del Exocortex RSA

Esta guía cubre la instalación completa del sistema exocortex desde cero en un nuevo equipo de trabajo.

---

## Requisitos Previos

- Cuenta de GitHub en la organización **RedSismicaAustro** (cuenta RSA).
- Cuenta de GitHub institucional (ej. `milton-ucuenca`).
- [SSH configurado para múltiples cuentas de GitHub](../git/multiples-cuentas-github.md).
- IDE agéntico instalado: **Antigravity** (Google) o **Claude Code** (Anthropic).

---

## Paso 1: Crear la estructura de directorios

Crea el directorio raiz de trabajo `git/` con tres subcarpetas según la cuenta de GitHub:

```bash
mkdir -p ~/git/rsa
mkdir -p ~/git/institucional
mkdir -p ~/git/personal
```

Esta estructura permite al IDE agéntico acceder a todos los repositorios desde una sola raíz.

---

## Paso 2: Clonar los repositorios del exocortex

Clona los tres repositorios que conforman el exocortex:

```bash
# Toolkit (cuenta org RSA)
cd ~/git/rsa
git clone git@github.com-rsa:RedSismicaAustro/RSA-Agent-Toolkit.git

# Metodologías (cuenta org RSA)
git clone git@github.com-rsa:RedSismicaAustro/RSA-Metodologias.git

# Bitácora personal (cuenta institucional)
cd ~/git/institucional
git clone git@github.com-institucional:TuUsuario/RSA-Bitacora-LLM-Milton.git
```

> **Nota sobre los alias SSH**: Los alias `github.com-rsa` y `github.com-institucional` deben estar configurados en `~/.ssh/config`. Ver [Configuración de Múltiples Cuentas](../git/multiples-cuentas-github.md).

---

## Paso 3: Instalar el toolkit en el workspace raíz

Copia los archivos del toolkit al directorio `git/` para que el IDE los detecte automáticamente:

**En Windows 11 (PowerShell):**
```powershell
cd C:\Users\TuUsuario\Documents\git
Copy-Item -Recurse -Force rsa\RSA-Agent-Toolkit\.agents .agents
Copy-Item -Force rsa\RSA-Agent-Toolkit\AGENTS.md AGENTS.md
```

**En Linux / WSL (Bash):**
```bash
cd ~/git
cp -r rsa/RSA-Agent-Toolkit/.agents .agents
cp rsa/RSA-Agent-Toolkit/AGENTS.md AGENTS.md
```

La estructura resultante en `git/` debe ser:

```text
git/
├── .agents/
│   ├── rules/
│   │   ├── commits.md
│   │   ├── restriccion_sshfs.md
│   │   └── idioma.md
│   └── skills/
│       ├── volcado_bitacora.md
│       ├── consulta_historica.md
│       ├── generar_contexto.md
│       ├── extraer_adr.md
│       └── sincronizar_toolkit.md
├── AGENTS.md
├── rsa/
│   ├── RSA-Agent-Toolkit/
│   ├── RSA-Metodologias/
│   └── RSA-Acelerografo/  (y otros proyectos)
├── institucional/
│   └── RSA-Bitacora-LLM-Milton/
└── personal/
```

---

## Paso 4: Configurar el IDE agéntico

### Antigravity (Google)

1. Abre **Antigravity** y agrega `git/` como workspace.
2. El IDE detecta automáticamente `.agents/` y `AGENTS.md` en la raíz del workspace.
3. Las reglas y skills quedan activos para todas las conversaciones.

### Claude Code (Anthropic)

1. Abre el terminal en `git/`.
2. Inicia Claude Code: `claude`.
3. El IDE detecta automáticamente `AGENTS.md` en el directorio actual.

---

## Paso 5: Prompt de inicio de sesión

Al comenzar cada sesión de trabajo, indica al agente el directorio de trabajo:

```
Para esta sesión, nuestro directorio de trabajo exclusivo será [DIRECTORIO].
No modifiques ni busques archivos fuera de esta ruta a menos que te lo pida.
```

Ejemplo para trabajar en el acelerógrafo:
```
Para esta sesión, nuestro directorio de trabajo exclusivo será rsa/RSA-Acelerografo/.
No modifiques ni busques archivos fuera de esta ruta a menos que te lo pida.
```

---

## Verificación de la Instalación

Comprueba que todo está en orden pidiendo al agente:

```
¿Qué skills y reglas tienes disponibles?
```

El agente debe responder listando los 5 skills (`volcado_bitacora`, `consulta_historica`, `generar_contexto`, `extraer_adr`, `sincronizar_toolkit`) y las 3 reglas (`commits`, `restriccion_sshfs`, `idioma`).
