---
title: Múltiples Cuentas de GitHub
parent: Git
nav_order: 4
---

# Configuración de Múltiples Cuentas de GitHub

Esta guía describe el procedimiento para configurar y trabajar con múltiples cuentas de GitHub en un mismo equipo, utilizando llaves SSH separadas y aliases para evitar conflictos de identidad.

## Contenido
{: .no_toc }

- TOC
{:toc}

---

## Objetivo

Configurar el acceso por SSH a múltiples cuentas de GitHub:

- **Cuenta Personal**: Para proyectos personales del usuario
- **Cuenta Institucional**: Para proyectos académicos o de investigación
- **Cuenta de Oficina**: Red-Sismica-del-Austro (proyectos oficiales de RSA)

Cada repositorio debe usar la cuenta correcta de forma automática, sin solicitar credenciales en cada operación.

---

## Estructura de Directorios Recomendada

La organización de proyectos en subdirectorios separados permite gestionar automáticamente las identidades de Git y las llaves SSH.

```
/home/usuario/git/
├── personal/        # Proyectos personales
├── institucional/   # Proyectos académicos/institucionales
└── rsa/            # Proyectos Red Sísmica del Austro
```

### Ventajas de esta estructura

1. **Separación lógica de identidad**: Permite aplicar configuraciones de nombre y correo distintos por carpeta
2. **Gestión automática de llaves SSH**: Cada subdirectorio contiene repositorios que apuntan a su alias específico
3. **Prevención de errores en commits**: Git utiliza automáticamente el correo correcto según el directorio
4. **Facilita automatización**: Permite ejecutar tareas por grupo de proyectos (backups, actualizaciones masivas, etc.)

### Crear la estructura

```shell
mkdir -p ~/git/{personal,institucional,rsa}
```

---

## Generación de Llaves SSH

Se requiere una llave SSH diferente para cada cuenta de GitHub.

### Generar las llaves

```shell
# Cuenta Personal
ssh-keygen -t ed25519 -C "usuario-personal" -f ~/.ssh/id_ed25519_personal -N ""

# Cuenta Institucional
ssh-keygen -t ed25519 -C "usuario-institucional" -f ~/.ssh/id_ed25519_institucional -N ""

# Cuenta Oficina (RSA)
ssh-keygen -t ed25519 -C "rsa-oficina" -f ~/.ssh/id_ed25519_rsa -N ""
```

**Nota:** Si alguna de estas llaves ya existe, omitir su generación y proceder directamente a añadirla al agente SSH y a GitHub.

### Visualizar llaves públicas

```shell
cat ~/.ssh/id_ed25519_personal.pub
cat ~/.ssh/id_ed25519_institucional.pub
cat ~/.ssh/id_ed25519_rsa.pub
```

### Añadir llaves públicas a GitHub

Para cada cuenta:

1. Acceder a **GitHub → Settings → SSH and GPG keys**
2. Seleccionar **New SSH key**
3. Copiar y pegar la llave pública correspondiente
4. Asignar un nombre descriptivo (ej. "Equipo Oficina - Personal", "Equipo Oficina - RSA")

### Establecer permisos correctos

```shell
chmod 700 ~/.ssh
chmod 600 ~/.ssh/id_ed25519_*
chmod 644 ~/.ssh/*.pub
```

---

## Configuración de SSH Config

El archivo `~/.ssh/config` define los aliases que permiten a SSH seleccionar la llave correcta para cada cuenta.

### Contenido del archivo `~/.ssh/config`

Reemplazar completamente el contenido del archivo:

```
# === BLOQUEAR GITHUB.COM PLANO ===
Host github.com
    HostName github.com
    User git
    IdentitiesOnly yes
    IdentityFile /dev/null
    # Esto evita que se use una llave por defecto si se olvida el alias
    # Cualquier git@github.com:... fallará explícitamente

# === CUENTA PERSONAL ===
Host github.com-personal
    HostName github.com
    User git
    IdentityFile ~/.ssh/id_ed25519_personal
    IdentitiesOnly yes

# === CUENTA INSTITUCIONAL ===
Host github.com-institucional
    HostName github.com
    User git
    IdentityFile ~/.ssh/id_ed25519_institucional
    IdentitiesOnly yes

# === CUENTA OFICINA (RSA) ===
Host github.com-rsa
    HostName github.com
    User git
    IdentityFile ~/.ssh/id_ed25519_rsa
    IdentitiesOnly yes
```

### Funcionamiento de la configuración

- **Bloqueo de `github.com` plano**: Si se olvida especificar el alias, Git fallará con *Permission denied* en lugar de conectarse a la cuenta incorrecta
- **Aliases específicos**: Cada alias (`github.com-personal`, `github.com-institucional`, `github.com-rsa`) usa su llave dedicada
- **IdentitiesOnly yes**: Asegura que SSH solo pruebe la llave especificada, sin intentar otras llaves disponibles

### Cargar llaves en el agente SSH

```shell
eval "$(ssh-agent -s)"
ssh-add ~/.ssh/id_ed25519_personal
ssh-add ~/.ssh/id_ed25519_institucional
ssh-add ~/.ssh/id_ed25519_rsa
```

### Verificar autenticación

Probar cada alias para confirmar que la autenticación funciona correctamente:

```shell
ssh -T git@github.com-personal
ssh -T git@github.com-institucional
ssh -T git@github.com-rsa
```

Cada comando debe responder con un mensaje de autenticación exitosa que incluye el nombre de usuario de GitHub asociado.

---

## Configuración de Identidad de Git por Carpeta

La configuración de Git permite establecer diferentes identidades (nombre y correo) según la ubicación del repositorio.

### Configuración global base

Archivo `~/.gitconfig`:

```
[includeIf "gitdir:~/git/personal/"]
    path = ~/.gitconfig-personal

[includeIf "gitdir:~/git/institucional/"]
    path = ~/.gitconfig-institucional

[includeIf "gitdir:~/git/rsa/"]
    path = ~/.gitconfig-rsa
```

### Configuraciones específicas por cuenta

**Archivo `~/.gitconfig-personal`:**
```
[user]
    name = Nombre Usuario
    email = email.personal@ejemplo.com
```

**Archivo `~/.gitconfig-institucional`:**
```
[user]
    name = Nombre Usuario (Institucional)
    email = email.institucional@universidad.edu.ec
```

**Archivo `~/.gitconfig-rsa`:**
```
[user]
    name = Nombre Usuario (RSA)
    email = contacto@redsismicaaustro.org
```

Git seleccionará automáticamente la identidad correcta según la carpeta del repositorio. Esta configuración no afecta la autenticación SSH, que es controlada por el alias y la llave correspondiente.

---

## Ajuste de Repositorios Existentes

Los repositorios existentes deben configurarse para usar el alias SSH correcto según la cuenta asociada.

### Cambiar la URL del remoto

```shell
# Repositorios personales
cd ~/git/personal/mi-proyecto
git remote set-url origin git@github.com-personal:usuario-github/nombre-repo.git

# Repositorios institucionales
cd ~/git/institucional/proyecto-investigacion
git remote set-url origin git@github.com-institucional:usuario-institucional/nombre-repo.git

# Repositorios RSA
cd ~/git/rsa/rsa-acelerografo
git remote set-url origin git@github.com-rsa:Red-Sismica-del-Austro/nombre-repo.git
```

### Verificar configuración

```shell
git remote -v
```

El comando debe mostrar el alias correcto (github.com-xxx) y el usuario u organización correspondiente.

**Nota:** Si algún repositorio utiliza HTTPS en lugar de SSH, es necesario cambiarlo a SSH para evitar solicitudes de usuario y token.

---

## Clonación de Repositorios

Al clonar nuevos repositorios, es fundamental especificar el alias correcto desde el inicio.

### Sintaxis de clonación

```shell
# Cuenta Personal
cd ~/git/personal
git clone git@github.com-personal:usuario-github/nombre-repo.git

# Cuenta Institucional
cd ~/git/institucional
git clone git@github.com-institucional:usuario-institucional/nombre-repo.git

# Cuenta Oficina (RSA)
cd ~/git/rsa
git clone git@github.com-rsa:Red-Sismica-del-Austro/nombre-repo.git
```

### Ejemplos específicos para RSA

```shell
# Clonar RSA-Acelerografo
cd ~/git/rsa
git clone git@github.com-rsa:Red-Sismica-del-Austro/RSA-Acelerografo.git

# Clonar RSA-Metodologias
cd ~/git/rsa
git clone git@github.com-rsa:Red-Sismica-del-Austro/RSA-Metodologias.git

# Clonar RSA-Intern-TIG-MQTT
cd ~/git/rsa
git clone git@github.com-rsa:Red-Sismica-del-Austro/RSA-Intern-TIG-MQTT.git
```

---

## Automatización con Bash Aliases

Es posible crear funciones en Bash para simplificar la clonación de repositorios.

### Configuración de `~/.bash_aliases`

Crear o editar el archivo `~/.bash_aliases`:

```bash
# === CLONAR POR CUENTA ===

gclone_personal() {
    if [ -z "$1" ]; then
        echo "Uso: gclone_personal OWNER/REPO.git"
        return 1
    fi
    git clone "git@github.com-personal:$1"
}

gclone_institucional() {
    if [ -z "$1" ]; then
        echo "Uso: gclone_institucional OWNER/REPO.git"
        return 1
    fi
    git clone "git@github.com-institucional:$1"
}

gclone_rsa() {
    if [ -z "$1" ]; then
        echo "Uso: gclone_rsa OWNER/REPO.git"
        return 1
    fi
    git clone "git@github.com-rsa:$1"
}
```

### Activar los aliases

```shell
source ~/.bash_aliases
```

Para que los aliases estén disponibles en cada nueva sesión, añadir al final de `~/.bashrc`:

```bash
if [ -f ~/.bash_aliases ]; then
    . ~/.bash_aliases
fi
```

### Uso de los aliases

```shell
# Clonar proyecto personal
cd ~/git/personal
gclone_personal usuario-github/mi-proyecto.git

# Clonar proyecto institucional
cd ~/git/institucional
gclone_institucional usuario-institucional/proyecto-tesis.git

# Clonar proyecto RSA
cd ~/git/rsa
gclone_rsa Red-Sismica-del-Austro/RSA-Acelerografo.git
```

---

## Flujo de Trabajo Típico

### Inicio de nuevo proyecto

```shell
# 1. Navegar a la carpeta correcta
cd ~/git/rsa

# 2. Clonar repositorio con alias correcto
git clone git@github.com-rsa:Red-Sismica-del-Austro/nuevo-proyecto.git

# 3. Entrar al repositorio
cd nuevo-proyecto

# 4. Verificar configuración
git config user.name
git config user.email
git remote -v
```

### Trabajo diario

```shell
# Actualizar repositorio
git pull

# Realizar cambios en archivos
# ...

# Preparar commit
git add .
git status

# Crear commit (usará el nombre/email configurado para esa carpeta)
git commit -m "FEAT: Nueva funcionalidad implementada"

# Enviar cambios (usará la llave SSH correcta automáticamente)
git push
```

### Cambio entre proyectos de diferentes cuentas

```shell
# Trabajar en proyecto RSA
cd ~/git/rsa/RSA-Acelerografo
git pull
# ... realizar cambios ...
git commit -m "FIX: Corregido error en captura de datos"
git push

# Cambiar a proyecto personal
cd ~/git/personal/mi-sitio-web
git pull
# ... realizar cambios ...
git commit -m "DOCS: Actualizada documentación"
git push

# Cada push usa automáticamente la llave y cuenta correcta
```

---

## Verificación de Configuración

### Checklist de verificación

1. **Llaves SSH generadas y añadidas a GitHub**
   ```shell
   ls -la ~/.ssh/id_ed25519_*
   ```

2. **Archivo `~/.ssh/config` configurado correctamente**
   ```shell
   cat ~/.ssh/config
   ```

3. **Autenticación SSH funcional**
   ```shell
   ssh -T git@github.com-personal
   ssh -T git@github.com-institucional
   ssh -T git@github.com-rsa
   ```

4. **Estructura de directorios creada**
   ```shell
   ls -la ~/git/
   ```

5. **Configuración de Git por carpeta**
   ```shell
   cat ~/.gitconfig
   cat ~/.gitconfig-personal
   cat ~/.gitconfig-institucional
   cat ~/.gitconfig-rsa
   ```

6. **Remotos configurados con alias SSH**
   ```shell
   cd ~/git/rsa/algún-repo
   git remote -v
   # Debe mostrar git@github.com-rsa:...
   ```

### Prueba de commit e identidad

```shell
# En cada carpeta, verificar identidad
cd ~/git/personal
git config user.email  # Debe mostrar email personal

cd ~/git/institucional
git config user.email  # Debe mostrar email institucional

cd ~/git/rsa
git config user.email  # Debe mostrar email RSA
```

---

## Solución de Problemas

### Error: Permission denied (publickey)

**Causa**: La llave SSH no está correctamente asociada o el alias es incorrecto.

**Solución**:
```shell
# Verificar que la llave está cargada
ssh-add -l

# Si no está, añadirla
ssh-add ~/.ssh/id_ed25519_rsa

# Verificar autenticación
ssh -T git@github.com-rsa
```

### Error: El commit usa el email incorrecto

**Causa**: El repositorio no está en la carpeta configurada en `includeIf` o la configuración no se cargó.

**Solución**:
```shell
# Verificar la configuración actual del repositorio
git config user.email
git config user.name

# Si es incorrecta, verificar la ubicación del repo
pwd
# Debe estar en ~/git/rsa/ para proyectos RSA

# Verificar que ~/.gitconfig-rsa existe
cat ~/.gitconfig-rsa

# Corregir el último commit si es necesario
git commit --amend --author="Nombre Correcto <email@correcto.com>"
```

### Error: remote: Repository not found

**Causa**: El alias SSH no coincide con la cuenta que tiene acceso al repositorio.

**Solución**:
```shell
# Verificar la URL del remoto
git remote -v

# Si el alias es incorrecto, cambiarlo
git remote set-url origin git@github.com-rsa:Red-Sismica-del-Austro/repo.git

# Verificar acceso con el alias correcto
ssh -T git@github.com-rsa
```

### El sistema solicita contraseña constantemente

**Causa**: El repositorio está usando HTTPS en lugar de SSH.

**Solución**:
```shell
# Cambiar de HTTPS a SSH
git remote set-url origin git@github.com-rsa:Red-Sismica-del-Austro/repo.git

# Verificar
git remote -v
```

---

## Migración de Repositorios Existentes

### Procedimiento para reorganizar repositorios

```shell
# 1. Identificar qué repositorios pertenecen a cada cuenta
cd ~/proyectos-actuales

# 2. Mover cada repositorio a su carpeta correspondiente
mv proyecto-rsa ~/git/rsa/
mv proyecto-personal ~/git/personal/
mv proyecto-investigacion ~/git/institucional/

# 3. Para cada repositorio movido, actualizar el remoto
cd ~/git/rsa/proyecto-rsa
git remote set-url origin git@github.com-rsa:Red-Sismica-del-Austro/proyecto-rsa.git

cd ~/git/personal/proyecto-personal
git remote set-url origin git@github.com-personal:usuario/proyecto-personal.git

# 4. Verificar que la configuración es correcta
git config user.email  # Debe coincidir con la cuenta correspondiente
git remote -v          # Debe mostrar el alias SSH correcto
```

---

## Mantenimiento y Buenas Prácticas

### Prácticas recomendadas

1. **Siempre clonar en la carpeta correcta**: Utilizar los aliases de Bash para evitar errores
2. **Verificar identidad antes del primer commit**: Ejecutar `git config user.email` en cada repositorio nuevo
3. **Mantener las llaves SSH seguras**: No compartir llaves privadas entre equipos
4. **Usar nombres descriptivos para llaves**: Facilita identificar qué llave corresponde a cada equipo/contexto
5. **Documentar configuraciones personalizadas**: Mantener un registro de configuraciones específicas por proyecto

### Respaldo de configuraciones

Es recomendable mantener copias de respaldo de los archivos de configuración:

```shell
# Crear directorio de respaldo
mkdir -p ~/backup-git-config

# Respaldar archivos críticos
cp ~/.ssh/config ~/backup-git-config/
cp ~/.gitconfig ~/backup-git-config/
cp ~/.gitconfig-* ~/backup-git-config/
cp ~/.bash_aliases ~/backup-git-config/

# Opcional: crear archivo comprimido
tar -czf ~/backup-git-config-$(date +%Y%m%d).tar.gz -C ~/ .ssh/config .gitconfig .gitconfig-* .bash_aliases 2>/dev/null
```

### Sincronización de configuración entre equipos

Si se trabaja en múltiples equipos, es posible sincronizar las configuraciones SSH y Git:

```shell
# En equipo origen
scp ~/.ssh/config usuario@equipo-destino:~/.ssh/
scp ~/.gitconfig* usuario@equipo-destino:~/

# Nota: NUNCA copiar llaves privadas por red insegura
# Generar nuevas llaves en cada equipo y añadirlas a GitHub
```

---

## Resumen de Comandos Esenciales

### Configuración inicial

```shell
# Generar llaves SSH
ssh-keygen -t ed25519 -C "descripcion" -f ~/.ssh/id_ed25519_cuenta -N ""

# Cargar llave en agente
ssh-add ~/.ssh/id_ed25519_cuenta

# Probar autenticación
ssh -T git@github.com-alias
```

### Operaciones diarias

```shell
# Clonar con alias correcto
git clone git@github.com-alias:owner/repo.git

# Verificar identidad
git config user.email

# Verificar remoto
git remote -v
```

### Corrección de problemas

```shell
# Cambiar remoto a SSH con alias correcto
git remote set-url origin git@github.com-alias:owner/repo.git

# Verificar configuración de identidad
git config --list --show-origin | grep user

# Recargar configuración de Bash
source ~/.bashrc
```