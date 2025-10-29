---
title: Entornos
nav_order: 2
has_children: true
---

# Entornos

Guías de instalación y configuración de entornos de desarrollo para proyectos de la Red Sísmica del Austro.

## Contenido de esta sección

Esta sección contiene información sobre la configuración de entornos de desarrollo locales y remotos para trabajar con los diferentes proyectos de RSA:

### [Instalación de WSL y Ubuntu](instalacion-wsl.md)
Guía para instalar y configurar Windows Subsystem for Linux (WSL) con Ubuntu, estableciendo un entorno de desarrollo Linux completo en sistemas Windows.

### [Desarrollo Remoto con SSHFS](desarrollo-remoto-sshfs.md)
Configuración de SSHFS para montar sistemas de archivos remotos, permitiendo el desarrollo en Raspberry Pi y servidores remotos utilizando herramientas locales.

### [Configuración de Micromamba](configuracion-micromamba.md)
Guía completa para instalar y configurar micromamba, un gestor de paquetes y entornos ligero. Incluye instalación, creación de entornos, gestión de paquetes, replicación de entornos y mejores prácticas para proyectos reproducibles.

---

## Tipos de Entornos en RSA

La Red Sísmica del Austro utiliza diversos entornos de desarrollo según el tipo de proyecto:

**Entorno Local (Workstation)**
- Sistema operativo: Ubuntu 22.04 LTS o WSL2
- IDEs: VS Code
- Herramientas: Git, Docker, Python, compiladores
- Uso: Desarrollo de software, análisis de datos, scripts de procesamiento

**Entorno Remoto (Raspberry Pi)**
- Sistema operativo: Raspberry Pi OS Lite
- Acceso: SSH, SSHFS
- Hardware: Raspberry Pi 3B+
- Uso: Equipo de desarrollo de las estaciones de campo

**Entorno de Servidor (Server-RSA)**
- Sistema operativo: Ubuntu Server 22.04 LTS
- Servicios: InfluxDB, Grafana, Telegraf, MQTT Broker
- Acceso: SSH, SSHFS
- Uso: Procesamiento centralizado, almacenamiento, dashboards

---

## Flujo de Trabajo Típico

### Desarrollo local con despliegue remoto

1. **Configurar entorno local**: Instalar WSL/Ubuntu con herramientas de desarrollo
2. **Crear entorno virtual**: Usar Micromamba para aislar dependencias del proyecto
3. **Montar sistema remoto**: Usar SSHFS para acceder a archivos de RPi/servidor
4. **Desarrollar localmente**: Editar código con IDE local (VS Code, PyCharm)
5. **Probar remotamente**: Ejecutar código en el dispositivo objetivo vía SSH
6. **Versionar cambios**: Commit y push desde el montaje SSHFS
7. **Desplegar**: Actualizar código en producción

### Desarrollo híbrido

1. **Prototipo local**: Desarrollo inicial en workstation con datos de prueba
2. **Entorno reproducible**: Crear entorno con Micromamba documentando dependencias
3. **Validación remota**: Pruebas con hardware real en Raspberry Pi
4. **Integración**: Conectar con servicios centrales (InfluxDB, MQTT)
5. **Producción**: Despliegue en estaciones de campo

---

## Herramientas Comunes

### Gestión de entornos Python

**Micromamba (Recomendado)**
```shell
# Instalar micromamba
curl -Ls https://micro.mamba.pm/install.sh | bash

# Crear entorno para proyecto RSA
micromamba create -n rsa-dev python=3.11 numpy pandas matplotlib
micromamba activate rsa-dev

# Exportar para reproducibilidad
micromamba env export --explicit > environment.lock
```

**venv (Python nativo)**
```shell
# Crear entorno virtual
python3 -m venv rsa-env
source rsa-env/bin/activate
```

### Contenedores Docker

Para aislar dependencias y garantizar reproducibilidad:

```shell
# Ejemplo para proyecto de análisis
docker run -v $(pwd):/workspace -it python:3.11 bash
```

### Gestión de paquetes

```shell
# Ubuntu/WSL
sudo apt update
sudo apt install build-essential python3-dev git

# Python con pip
pip install -r requirements.txt

# Python con micromamba (recomendado)
micromamba install -c conda-forge -f environment.yml
```

---

## Configuraciones Recomendadas

### Para desarrollo en Python

```shell
# Instalar herramientas esenciales
sudo apt install python3-pip python3-venv python3-dev

# Crear entorno con micromamba
micromamba create -n dev-tools python=3.11 ipython jupyter
micromamba activate dev-tools

# Instalar paquetes científicos
micromamba install -c conda-forge numpy pandas matplotlib scipy
```

### Para desarrollo embebido (Raspberry Pi)

```shell
# En la workstation
sudo apt install gcc-arm-linux-gnueabihf gdb-multiarch

# Herramientas de desarrollo para RPi con micromamba
micromamba create -n rpi-dev python=3.9
micromamba activate rpi-dev
pip install RPi.GPIO adafruit-blinka
```

### Para análisis de datos sísmicos

```shell
# Crear entorno especializado
micromamba create -n sismica python=3.11
micromamba activate sismica

# Librerías especializadas
micromamba install -c conda-forge obspy pandas numpy scipy matplotlib seaborn
```

---

## Mejores Prácticas

### Organización de proyectos

Mantener una estructura de directorios consistente:

```
~/git/
├── personal/          # Proyectos personales
├── institucional/     # Proyectos académicos
└── rsa/              # Proyectos RSA
    ├── RSA-Acelerografo/
    │   ├── environment.yml
    │   └── environment.lock
    ├── RSA-Intern-TIG-MQTT/
    │   ├── environment.yml
    │   └── environment.lock
    └── RSA-Metodologias/
        └── ...
```

### Aislamiento de dependencias

1. **Usar entornos virtuales**: Un entorno por proyecto
2. **Documentar dependencias**: Mantener `environment.yml` y `environment.lock` actualizados
3. **Versionar configuraciones**: Incluir archivos de configuración de entorno en Git
4. **Usar Micromamba**: Para resolución rápida de dependencias y reproducibilidad

### Sincronización de entornos

Para mantener consistencia entre desarrollo local y remoto:

```shell
# Exportar entorno local
micromamba env export --explicit > environment.lock

# Copiar a sistema remoto
scp environment.lock rsa@rpi:/home/rsa/mi-proyecto/

# Recrear en sistema remoto (si tiene micromamba)
ssh rsa@rpi
cd mi-proyecto
micromamba create -n mi-proyecto -f environment.lock

# Alternativa con pip
micromamba env export > requirements.txt
scp requirements.txt rsa@rpi:/home/rsa/mi-proyecto/
ssh rsa@rpi "cd mi-proyecto && pip install -r requirements.txt"
```

### Respaldo de configuraciones

```shell
# Respaldar configuraciones de desarrollo
tar -czf ~/backups/dev-config-$(date +%Y%m%d).tar.gz \
    ~/.bashrc ~/.vimrc ~/.gitconfig ~/scripts/ \
    ~/.local/bin/micromamba
```

---

## Recursos Adicionales

### Documentación relacionada

- [Git - Comandos Comunes](../git/comandos-comunes.md) - Control de versiones en entornos de desarrollo
- [Git - Múltiples Cuentas](../git/multiples-cuentas-github.md) - Configuración SSH para acceso remoto

### Guías disponibles

- **Instalación de WSL y Ubuntu**: Configuración del subsistema Linux en Windows
- **Desarrollo Remoto con SSHFS**: Montaje de sistemas de archivos remotos
- **Configuración de Micromamba**: Gestión de entornos Python reproducibles

### Guías futuras planificadas

- Instalación y configuración de Docker
- Setup de VS Code Remote Development
- Configuración de Jupyter para análisis remoto
- Cross-compilation para Raspberry Pi
- Integración continua con GitHub Actions