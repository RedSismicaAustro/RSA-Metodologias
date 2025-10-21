---
title: Desarrollo Remoto con SSHFS
parent: Entornos
nav_order: 2
---

# Desarrollo Remoto con SSHFS

Esta guía describe la configuración y uso de SSHFS (Secure SHell File System) para montar sistemas de archivos remotos en el entorno de desarrollo local, facilitando el trabajo con dispositivos embebidos y servidores remotos de la Red Sísmica del Austro.

## Contenido
{: .no_toc }

- TOC
{:toc}

---

## Contexto

SSHFS (Secure SHell File System) es una herramienta que permite montar un sistema de archivos remoto en el entorno local mediante el protocolo SSH. Esta tecnología resulta particularmente útil cuando se requiere acceder, editar o gestionar archivos ubicados en una máquina remota, manteniendo la seguridad y cifrado que ofrece SSH.

### Casos de uso en RSA

En los proyectos de la Red Sísmica del Austro, SSHFS facilita:

- **Desarrollo en Raspberry Pi**: Editar código en dispositivos de campo sin necesidad de editores en terminal
- **Trabajo con servidores remotos**: Acceder a archivos de servidores centrales desde el entorno de desarrollo local
- **Integración con IDEs**: Usar editores locales (VS Code, PyCharm, etc.) para trabajar con código remoto
- **Depuración remota**: Acceder a logs y archivos de configuración de estaciones en tiempo real
- **Gestión de repositorios**: Ejecutar comandos Git locales sobre repositorios alojados en dispositivos remotos

### Ventajas de SSHFS

- **Sin servicios adicionales**: No requiere configurar Samba, NFS u otros protocolos de red
- **Seguridad nativa**: Utiliza cifrado SSH para todas las transferencias
- **Transparencia**: Los archivos remotos aparecen como directorios locales
- **Multiplataforma**: Compatible con Linux, macOS y WSL en Windows
- **Ligero**: Mínimo impacto en recursos del sistema remoto

---

## Objetivo

Configurar y utilizar SSHFS para montar directorios remotos alojados en Raspberry Pi o servidores de RSA dentro del entorno de desarrollo local (Ubuntu, WSL), permitiendo el acceso transparente a los archivos del sistema remoto y la ejecución de operaciones de desarrollo locales sobre ellos.

---

## Requisitos Previos

### Acceso SSH configurado

- Acceso SSH funcional hacia el dispositivo remoto
- Usuario y contraseña o llave SSH configurada
- Dirección IP o nombre de host del dispositivo remoto
- Puerto SSH (por defecto: 22)

### Permisos en el sistema remoto

- Permisos de lectura y escritura sobre el directorio a montar
- Cuenta de usuario activa en el sistema remoto

### Conectividad de red

- Conexión de red estable entre sistema local y remoto
- Acceso al puerto SSH (verificar firewalls y restricciones de red)

### Software local

- Sistema operativo: Ubuntu, WSL, o cualquier distribución Linux
- Paquete `sshfs` (se instalará en el procedimiento)
- Módulo FUSE habilitado en el kernel

---

## Instalación de SSHFS

### Actualizar el sistema

Antes de instalar SSHFS, actualizar la lista de paquetes del sistema:

```shell
sudo apt update
```

### Instalar el paquete SSHFS

```shell
sudo apt install sshfs
```

### Verificar la instalación

```shell
sshfs --version
```

**Salida esperada:**
```
SSHFS version 3.7.3
FUSE library version 3.14.0
using FUSE kernel interface version 7.31
fusermount3 version: 3.14.0
```

### Verificar soporte FUSE

```shell
ls -l /dev/fuse
```

**Salida esperada:**
```
crw-rw-rw- 1 root root 10, 229 Oct 21 10:30 /dev/fuse
```

Si el archivo no existe o no tiene permisos adecuados, puede ser necesario cargar el módulo:

```shell
sudo modprobe fuse
```

---

## Configuración Básica

### Crear directorio de montaje

Crear un directorio local que servirá como punto de montaje para el sistema remoto:

```shell
mkdir ~/montajes
cd ~/montajes
mkdir acelerografo
```

**Estructura recomendada para múltiples montajes:**
```
~/montajes/
├── acelerografo/      # Raspberry Pi - proyecto acelerografo
├── server-rsa/        # Servidor central RSA
└── rpi-estacion-01/   # Raspberry Pi - estación 01
```

### Sintaxis básica de montaje

```shell
sshfs [usuario]@[host]:[ruta_remota] [punto_montaje_local]
```

### Ejemplo de montaje

Montar el repositorio RSA-Acelerografo desde una Raspberry Pi:

```shell
sshfs rsa@10.22.147.227:/home/rsa/git/RSA-Acelerografo ~/montajes/acelerografo/
```

**Explicación de los parámetros:**
- `rsa`: Usuario en el sistema remoto
- `10.22.147.227`: Dirección IP de la Raspberry Pi
- `/home/rsa/git/RSA-Acelerografo`: Ruta del directorio remoto a montar
- `~/montajes/acelerografo/`: Punto de montaje local

### Verificar el montaje

```shell
ls ~/montajes/acelerografo/
```

Si el montaje fue exitoso, se visualizará el contenido del directorio remoto.

```shell
df -h | grep acelerografo
```

**Salida esperada:**
```
rsa@10.22.147.227:/home/rsa/git/RSA-Acelerografo  15G  8.2G  5.8G  59% /home/usuario/montajes/acelerografo
```

---

## Uso del Sistema Montado

### Navegar en el directorio montado

```shell
cd ~/montajes/acelerografo/
ls -la
```

Los archivos remotos se comportan como archivos locales. Es posible:

- Listar directorios
- Leer archivos
- Editar archivos
- Crear nuevos archivos
- Eliminar archivos (con los permisos apropiados)

### Operaciones con Git

```shell
cd ~/montajes/acelerografo/
git status
git log --oneline
git branch
```

Es posible ejecutar cualquier comando Git como si el repositorio fuera local.

**Ejemplo de flujo de trabajo:**

```shell
# Verificar estado
git status

# Crear rama de trabajo
git checkout -b feature/nuevo-sensor

# Los archivos se pueden editar con cualquier editor local
code .  # Abre VS Code en el directorio montado

# Después de editar
git add .
git commit -m "FEAT: Agregado soporte para sensor MPU6050"
git push origin feature/nuevo-sensor
```

### Edición con editores locales

#### VS Code

```shell
code ~/montajes/acelerografo/
```

VS Code abrirá el directorio montado como cualquier proyecto local.

#### Nano/Vim (en el sistema montado)

```shell
nano ~/montajes/acelerografo/config.py
vim ~/montajes/acelerografo/main.py
```

#### Operaciones de archivos

```shell
# Copiar archivos al sistema remoto
cp archivo_local.py ~/montajes/acelerografo/

# Copiar archivos desde el sistema remoto
cp ~/montajes/acelerografo/log.txt ~/descargas/

# Buscar archivos
find ~/montajes/acelerografo/ -name "*.py"

# Contar líneas de código
wc -l ~/montajes/acelerografo/**/*.py
```

---

## Desmontaje del Sistema

### Método estándar

Al finalizar la sesión de trabajo, desmontar el sistema de archivos para liberar recursos:

```shell
fusermount -u ~/montajes/acelerografo
```

### Método alternativo

En algunas distribuciones Linux:

```shell
umount ~/montajes/acelerografo
```

### Verificar desmontaje

```shell
df -h | grep acelerografo
```

Si no aparece ninguna salida, el sistema fue desmontado correctamente.

### Forzar desmontaje

Si el desmontaje normal falla (por ejemplo, por archivos abiertos):

```shell
fusermount -uz ~/montajes/acelerografo
```

La opción `-z` desmonta de forma "lazy", esperando a que se liberen los recursos.

---

## Opciones Avanzadas

### Reconexión automática

Para conexiones inestables o redes con interrupciones frecuentes:

```shell
sshfs -o reconnect rsa@10.22.147.227:/home/rsa/git/RSA-Acelerografo ~/montajes/acelerografo/
```

La opción `reconnect` permite que SSHFS intente reconectar automáticamente si se pierde la conexión.

### Usar llave SSH específica

```shell
sshfs -o IdentityFile=~/.ssh/id_ed25519_rsa rsa@10.22.147.227:/home/rsa/git/RSA-Acelerografo ~/montajes/acelerografo/
```

Útil cuando se tienen múltiples llaves SSH configuradas (ver [Múltiples Cuentas de GitHub](../git/multiples-cuentas-github.md)).

### Puerto SSH no estándar

Si el servidor SSH usa un puerto diferente al 22:

```shell
sshfs -p 2222 rsa@10.22.147.227:/home/rsa/git/RSA-Acelerografo ~/montajes/acelerografo/
```

### Montaje de solo lectura

Para prevenir modificaciones accidentales:

```shell
sshfs -o ro rsa@10.22.147.227:/home/rsa/logs ~/montajes/logs-remoto/
```

### Mejorar rendimiento

Para optimizar la velocidad en conexiones rápidas:

```shell
sshfs -o Ciphers=aes128-ctr -o Compression=no rsa@10.22.147.227:/home/rsa/git/RSA-Acelerografo ~/montajes/acelerografo/
```

**Opciones explicadas:**
- `Ciphers=aes128-ctr`: Usar cifrado más rápido pero igualmente seguro
- `Compression=no`: Desactivar compresión en redes rápidas

### Múltiples opciones combinadas

```shell
sshfs -o reconnect,IdentityFile=~/.ssh/id_ed25519_rsa,ServerAliveInterval=15,ServerAliveCountMax=3 rsa@10.22.147.227:/home/rsa/git/RSA-Acelerografo ~/montajes/acelerografo/
```

**Opciones adicionales:**
- `ServerAliveInterval=15`: Enviar keepalive cada 15 segundos
- `ServerAliveCountMax=3`: Desconectar después de 3 keepalives sin respuesta

---

## Configuración con Alias SSH

Si se tiene configurado un alias SSH (ver [Múltiples Cuentas de GitHub](../git/multiples-cuentas-github.md)), se puede simplificar el comando:

### Configuración en ~/.ssh/config

```
Host rpi-acelerografo
    HostName 10.22.147.227
    User rsa
    IdentityFile ~/.ssh/id_ed25519_rsa
    ServerAliveInterval 15
    ServerAliveCountMax 3
```

### Montaje usando el alias

```shell
sshfs rpi-acelerografo:/home/rsa/git/RSA-Acelerografo ~/montajes/acelerografo/ -o reconnect
```

Mucho más simple y legible.

---

## Automatización con Scripts

### Script de montaje

Crear archivo `~/scripts/mount-rpi.sh`:

```bash
#!/bin/bash

# Configuración
REMOTE_HOST="rpi-acelerografo"
REMOTE_PATH="/home/rsa/git/RSA-Acelerografo"
LOCAL_MOUNT="$HOME/montajes/acelerografo"

# Crear punto de montaje si no existe
mkdir -p "$LOCAL_MOUNT"

# Verificar si ya está montado
if mountpoint -q "$LOCAL_MOUNT"; then
    echo "Error: $LOCAL_MOUNT ya está montado"
    exit 1
fi

# Montar
echo "Montando $REMOTE_HOST:$REMOTE_PATH en $LOCAL_MOUNT..."
sshfs -o reconnect,ServerAliveInterval=15 "$REMOTE_HOST:$REMOTE_PATH" "$LOCAL_MOUNT"

if [ $? -eq 0 ]; then
    echo "✓ Montaje exitoso"
    ls -lh "$LOCAL_MOUNT"
else
    echo "✗ Error al montar"
    exit 1
fi
```

### Script de desmontaje

Crear archivo `~/scripts/umount-rpi.sh`:

```bash
#!/bin/bash

LOCAL_MOUNT="$HOME/montajes/acelerografo"

# Verificar si está montado
if ! mountpoint -q "$LOCAL_MOUNT"; then
    echo "Error: $LOCAL_MOUNT no está montado"
    exit 1
fi

# Desmontar
echo "Desmontando $LOCAL_MOUNT..."
fusermount -u "$LOCAL_MOUNT"

if [ $? -eq 0 ]; then
    echo "✓ Desmontaje exitoso"
else
    echo "✗ Error al desmontar. Intentando forzar..."
    fusermount -uz "$LOCAL_MOUNT"
fi
```

### Hacer ejecutables los scripts

```shell
chmod +x ~/scripts/mount-rpi.sh
chmod +x ~/scripts/umount-rpi.sh
```

### Uso de los scripts

```shell
# Montar
~/scripts/mount-rpi.sh

# Trabajar...

# Desmontar
~/scripts/umount-rpi.sh
```

---

## Montaje Automático con fstab

**Advertencia:** No se recomienda usar `/etc/fstab` para montajes SSHFS en sistemas de escritorio o desarrollo, ya que requiere que la conexión SSH esté disponible en el arranque y puede causar problemas si la red no está configurada.

Sin embargo, si se desea configurar montaje automático:

### Configuración en fstab

Editar `/etc/fstab`:

```shell
sudo nano /etc/fstab
```

Añadir al final:

```
rsa@10.22.147.227:/home/rsa/git/RSA-Acelerografo /home/usuario/montajes/acelerografo fuse.sshfs noauto,x-systemd.automount,_netdev,users,idmap=user,IdentityFile=/home/usuario/.ssh/id_ed25519_rsa,allow_other,reconnect 0 0
```

**Opciones importantes:**
- `noauto`: No montar automáticamente en el arranque
- `x-systemd.automount`: Montar cuando se acceda al directorio
- `_netdev`: Esperar a que la red esté disponible
- `users`: Permitir a usuarios no root montar
- `allow_other`: Permitir acceso a otros usuarios

---
## Solución de Problemas

### Error: "fusermount: mount failed: Permission denied"

**Causa:** El usuario no tiene permisos para usar FUSE.

**Solución:**
```shell
# Añadir usuario al grupo fuse
sudo usermod -a -G fuse $USER

# Cerrar sesión y volver a iniciar sesión
# O recargar grupos
newgrp fuse
```

### Error: "read: Connection reset by peer"

**Causa:** Pérdida de conexión SSH o timeout.

**Solución:**
```shell
# Desmontar forzadamente
fusermount -uz ~/montajes/acelerografo

# Volver a montar con opciones de keepalive
sshfs -o reconnect,ServerAliveInterval=15 rsa@10.22.147.227:/home/rsa/git ~/montajes/acelerografo/
```

### Error: "Transport endpoint is not connected"

**Causa:** Montaje corrupto o conexión perdida.

**Solución:**
```shell
# Forzar desmontaje
fusermount -uz ~/montajes/acelerografo

# Si persiste
sudo umount -l ~/montajes/acelerografo
```

### Rendimiento lento

**Causas posibles:**
- Latencia de red alta
- Ancho de banda limitado
- Cifrado intensivo
- Compresión innecesaria

**Soluciones:**

```shell
# Desactivar compresión en redes rápidas
sshfs -o Compression=no rsa@host:/path ~/montaje/

# Usar cifrado más ligero
sshfs -o Ciphers=aes128-ctr rsa@host:/path ~/montaje/

# Incrementar tamaño de buffer
sshfs -o cache=yes,kernel_cache,large_read rsa@host:/path ~/montaje/
```

### El montaje no aparece en el explorador de archivos

**Causa:** Permisos de acceso.

**Solución:**
```shell
# Montar con allow_other
sshfs -o allow_other rsa@host:/path ~/montaje/
```

**Nota:** Requiere configuración en `/etc/fuse.conf`:
```shell
sudo nano /etc/fuse.conf
# Descomentar: user_allow_other
```

### Archivos con permisos incorrectos

**Solución:**
```shell
# Montar con mapeo de IDs
sshfs -o idmap=user,uid=$(id -u),gid=$(id -g) rsa@host:/path ~/montaje/
```

---

## Buenas Prácticas

### Gestión de montajes

1. **Siempre desmontar al finalizar**: No dejar montajes activos innecesariamente
2. **Usar nombres descriptivos**: Para puntos de montaje (`acelerografo`, no `mnt1`)
3. **Organizar por proyecto**: Estructura clara en `~/montajes/`
4. **Verificar antes de montar**: Evitar montajes duplicados

### Seguridad

1. **Usar llaves SSH**: Evitar autenticación por contraseña
2. **Permisos mínimos necesarios**: No montar como root si no es necesario
3. **Conexiones cifradas**: No desactivar cifrado en redes no confiables
4. **Auditar accesos**: Revisar logs SSH del servidor remoto

### Rendimiento

1. **Conexiones estables**: Usar opciones de reconexión en redes inestables
2. **Optimizar para el caso de uso**: Solo lectura cuando sea posible
3. **Caché apropiado**: Usar opciones de caché para lecturas frecuentes
4. **Evitar operaciones masivas**: Para transferencias grandes, usar `rsync` o `scp`

### Flujo de trabajo

```shell
# 1. Montar al inicio de la jornada
~/scripts/mount-rpi.sh

# 2. Trabajar normalmente
code ~/montajes/acelerografo/
cd ~/montajes/acelerografo/
git status
# ... desarrollo ...

# 3. Commits y pushes desde el montaje
git add .
git commit -m "FEAT: Nueva funcionalidad"
git push

# 4. Desmontar al finalizar
cd ~
~/scripts/umount-rpi.sh
```

---

## Casos de Uso Específicos en RSA

### Desarrollo en Raspberry Pi de campo

**Escenario:** Modificar código del sistema de adquisición en una RPi remota.

```shell
# Montar
sshfs rsa@acel-devp-univ-01:/home/rsa/git/RSA-Acelerografo acelerografo/

# Probar cambios ejecutando en remoto (SSH separado)
ssh rsa@acel-devp-univ-01 "cd RSA-Acelerografo && python3 test_adquisicion.py"

# Commit desde el montaje
cd ~/montajes/estacion-03/
git commit -am "FIX: Corregida calibración del acelerómetro"
git push

# Desmontar
fusermount -u acelerografo/
```

### Acceso a logs de servidor central

**Escenario:** Analizar logs en tiempo real del servidor de procesamiento.

```shell
# Montar solo directorio de logs (lectura)
sshfs -o ro rsa@server-rsa:/var/log/rsa ~/montajes/logs-rsa/

# Monitorear con herramientas locales
tail -f ~/montajes/logs-rsa/processing.log
grep "ERROR" ~/montajes/logs-rsa/*.log

# Análisis con scripts locales
python3 ~/scripts/analyze-logs.py ~/montajes/logs-rsa/
```

### Sincronización de configuraciones

**Escenario:** Mantener configuraciones sincronizadas entre múltiples RPi.

```shell
# Montar configuración de RPi maestra
sshfs rsa@rpi-master:/home/rsa/config ~/montajes/config-master/

# Copiar configuración actualizada
cp ~/montajes/config-master/seismic.yaml ~/configs/seismic-v2.yaml

# Distribuir a otras estaciones
for station in rpi-01 rpi-02 rpi-03; do
    scp ~/configs/seismic-v2.yaml rsa@$station:/home/rsa/config/
done
```

---

## Alternativas y Comparación

### SSHFS vs Otros Métodos

| Característica | SSHFS | NFS | Samba | SCP/RSYNC |
|----------------|-------|-----|-------|-----------|
| Configuración servidor | No requerida | Compleja | Compleja | No requerida |
| Seguridad | SSH nativo | Requiere tunelado | Autenticación propia | SSH nativo |
| Montaje persistente | Sí | Sí | Sí | No |
| Rendimiento | Medio | Alto | Alto | N/A |
| Uso de recursos | Bajo | Medio | Alto | Bajo |
| Mejor para | Desarrollo remoto | Recursos compartidos | Redes Windows | Transferencias puntuales |

### Cuándo usar SSHFS

✅ **Usar SSHFS cuando:**
- Se necesita acceso frecuente a archivos remotos
- Ya se tiene acceso SSH configurado
- Se requiere mínima configuración del servidor
- Se trabaja con dispositivos embebidos (RPi)
- Se necesita cifrado sin configuración adicional

❌ **Evitar SSHFS cuando:**
- Se requiere máximo rendimiento (usar NFS)
- Se necesita compartir recursos en red local (usar Samba)
- Solo se requieren transferencias puntuales (usar SCP/RSYNC)
- La conexión de red es muy inestable

---

## Referencias

### Comandos clave

```shell
# Instalación
sudo apt install sshfs

# Montaje básico
sshfs usuario@host:/ruta/remota /ruta/local

# Montaje con opciones
sshfs -o reconnect,IdentityFile=~/.ssh/key usuario@host:/ruta /local

# Desmontaje
fusermount -u /ruta/local

# Desmontaje forzado
fusermount -uz /ruta/local
```

### Documentación relacionada

- [Instalación de WSL y Ubuntu](instalacion-wsl.md) - Configuración del entorno base
- [Múltiples Cuentas de GitHub](../git/multiples-cuentas-github.md) - Configuración de llaves SSH
- [Adaptador WiFi Raspberry Pi](../redes/adaptador_wifi_rpi.md) - Conectividad de estaciones

### Recursos externos

- [SSHFS GitHub Repository](https://github.com/libfuse/sshfs)
- [FUSE Documentation](https://www.kernel.org/doc/html/latest/filesystems/fuse.html)
- [SSH Config Manual](https://man.openbsd.org/ssh_config)