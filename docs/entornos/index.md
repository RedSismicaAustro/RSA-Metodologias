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
2. **Montar sistema remoto**: Usar SSHFS para acceder a archivos de RPi/servidor
3. **Desarrollar localmente**: Editar código con IDE local (VS Code, PyCharm)
4. **Probar remotamente**: Ejecutar código en el dispositivo objetivo vía SSH
5. **Versionar cambios**: Commit y push desde el montaje SSHFS
6. **Desplegar**: Actualizar código en producción

### Desarrollo híbrido

1. **Prototipo local**: Desarrollo inicial en workstation con datos de prueba
2. **Validación remota**: Pruebas con hardware real en Raspberry Pi
3. **Integración**: Conectar con servicios centrales (InfluxDB, MQTT)
4. **Producción**: Despliegue en estaciones de campo

---

## Herramientas Comunes

### Gestión de entornos Python

**Conda/Micromamba**
```shell
# Crear entorno para proyecto RSA
micromamba create -n rsa-dev python=3.11
micromamba activate rsa-dev
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

# Python
pip install -r requirements.txt
```

---

## Configuraciones Recomendadas

### Para desarrollo en Python

```shell
# Instalar herramientas esenciales
sudo apt install python3-pip python3-venv python3-dev
pip install ipython jupyter numpy pandas matplotlib scipy

# Configurar Jupyter
jupyter notebook --generate-config
```

### Para desarrollo embebido (Raspberry Pi)

```shell
# En la workstation
sudo apt install gcc-arm-linux-gnueabihf gdb-multiarch

# Herramientas de desarrollo para RPi
pip install RPi.GPIO adafruit-blinka
```

### Para análisis de datos sísmicos

```shell
# Librerías especializadas
pip install obspy pandas numpy scipy matplotlib seaborn
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
    ├── RSA-Intern-TIG-MQTT/
    └── RSA-Metodologias/
```

### Aislamiento de dependencias

1. **Usar entornos virtuales**: Un entorno por proyecto
2. **Documentar dependencias**: Mantener actualizado `requirements.txt`
3. **Versionar configuraciones**: Incluir archivos de configuración de entorno en Git

### Sincronización de entornos

Para mantener consistencia entre desarrollo local y remoto:

```shell
# Exportar dependencias locales
pip freeze > requirements.txt

# Instalar en sistema remoto
scp requirements.txt rsa@rpi:/home/rsa/
ssh rsa@rpi "pip install -r requirements.txt"
```

### Respaldo de configuraciones

```shell
# Respaldar configuraciones de desarrollo
tar -czf ~/backups/dev-config-$(date +%Y%m%d).tar.gz \
    ~/.bashrc ~/.vimrc ~/.gitconfig ~/scripts/
```

---



## Recursos Adicionales

### Documentación relacionada

- [Git - Comandos Comunes](../git/comandos-comunes.md) - Control de versiones en entornos de desarrollo
- [Git - Múltiples Cuentas](../git/multiples-cuentas-github.md) - Configuración SSH para acceso remoto


### Guías futuras planificadas

- Instalación y configuración de Docker
- Configuración de entornos con Conda/Micromamba
- Setup de VS Code Remote Development
- Configuración de Jupyter para análisis remoto
- Cross-compilation para Raspberry Pi