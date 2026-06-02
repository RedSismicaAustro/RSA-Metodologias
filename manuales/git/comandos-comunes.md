---
title: Comandos Comunes
parent: Git
nav_order: 1
---

# Comandos Comunes de Git

Este documento presenta los comandos más utilizados en Git para la gestión de proyectos en la Red Sísmica del Austro.

## Contenido
{: .no_toc }

- TOC
{:toc}

---

## Inicialización de Repositorio

### Crear un nuevo repositorio

```shell
git init
```

Inicializa un nuevo repositorio Git en el directorio actual. Git comenzará a monitorear los cambios en los archivos.

### Verificar el estado del repositorio

```shell
git status
```

Muestra el estado actual del repositorio, incluyendo archivos modificados, archivos en staging area y archivos sin seguimiento.

---

## Configuración de Usuario

Es necesario configurar la identidad del usuario antes de realizar commits. Esta configuración se realiza una sola vez por equipo.

```shell
# Configurar nombre de usuario
git config --global user.name "RSA-Usuario"

# Configurar correo electrónico
git config --global user.email usuario@ucuenca.edu.ec

# Habilitar colores en la interfaz
git config --global color.ui true
```

### Verificar configuración

```shell
git config --list
```

Este comando muestra toda la configuración actual de Git.

---

## Gestión de Cambios

### Agregar archivos al staging area

```shell
# Agregar todos los archivos modificados
git add -A

# Agregar un archivo específico
git add nombre_archivo.py

# Agregar todos los archivos de una carpeta
git add carpeta/
```

### Realizar commits

```shell
# Crear un commit con mensaje
git commit -m "FEAT: Implementada nueva funcionalidad"

# Modificar el mensaje del último commit
git commit --amend -m "FEAT: Implementada autenticación de usuarios"
```

**Nota:** Es necesario agregar los archivos con `git add` antes de ejecutar `git commit`, de lo contrario se producirá un error.

### Visualizar historial de commits

```shell
# Mostrar historial completo
git log

# Mostrar historial resumido
git log --oneline

# Mostrar historial con gráfico de ramas
git log --oneline --graph --all

# Exportar historial a archivo de texto
git log > commits.txt
```

### Navegar entre commits

```shell
# Moverse a un commit específico
git checkout <codigo_hash>

# Regresar al último commit de la rama actual
git checkout master
```

**Ejemplo:**
```shell
git checkout a3f5b2c
git checkout master
```

---

## Descartar Cambios Locales

### Para hacer pull sin conflictos

Cuando se necesita descartar todos los cambios locales y obtener la última versión del repositorio remoto:

```shell
# Deshacer todos los cambios locales
git reset --hard HEAD

# Eliminar archivos no rastreados
git clean -df
```

**Advertencia:** Estos comandos eliminarán permanentemente todos los cambios locales no guardados.

---

## Comandos de Borrado

### Deshacer commits

```shell
# Borrar commit pero mantener el código
git reset --soft <codigo_hash>

# Borrar staging area pero mantener working area
git reset --mixed <codigo_hash>

# Borrar todo (commit y código)
git reset --hard <codigo_hash>

# Restablecer al último commit rastreado
git reset --hard HEAD
```

**Ejemplo de uso:**
```shell
# Ver historial de commits
git log --oneline

# Salida:
# a3f5b2c (HEAD) WIP: Trabajo en progreso
# b2e4d1a FEAT: Nueva funcionalidad
# c1d3e2f FIX: Corrección de error

# Volver al commit b2e4d1a eliminando a3f5b2c
git reset --hard b2e4d1a
```

### Eliminar repositorio local

Para eliminar completamente el repositorio Git de un directorio:

```shell
rm -rf .git
```

Este comando elimina la carpeta `.git` que contiene todo el historial y configuración del repositorio.

---

## Gestión de Ramas

Las ramas son líneas de tiempo independientes en el proyecto, utilizadas para desarrollar funcionalidades, corregir errores o experimentar sin afectar la rama principal.

### Operaciones básicas con ramas

```shell
# Crear una nueva rama
git branch nombre_rama

# Listar ramas locales
git branch

# Listar todas las ramas (locales y remotas)
git branch --all

# Listar solo ramas remotas
git branch -r

# Cambiar a una rama existente
git checkout nombre_rama

# Crear y cambiar a una nueva rama
git checkout -b nombre_rama

# Eliminar una rama
git branch -D nombre_rama
```

### Renombrar ramas

```shell
# Renombrar una rama específica
git branch -m <nombre_antiguo> <nombre_nuevo>

# Renombrar la rama actual
git branch -m <nombre_nuevo>
```

**Ejemplo:**
```shell
# Crear y trabajar en una nueva rama
git checkout -b feature/procesamiento-datos
git add archivo.py
git commit -m "FEAT: Agregado procesamiento de datos sísmicos"

# Volver a la rama principal
git checkout develop
```

### Vincular rama local con rama remota

Para trabajar en una rama remota existente:

```shell
git checkout -b <nombre_rama_local> origin/<nombre_rama_remota>
```

**Ejemplo:**
```shell
git checkout -b desarrollo origin/desarrollo
```

---

## Cambio de Rama con Cambios Pendientes

### Descartar cambios para cambiar de rama

```shell
# Remover archivos sin seguimiento (usar con precaución)
git clean -df

# Descartar cambios en archivos rastreados
git checkout -- .
```

El segundo método es más seguro ya que solo descarta cambios en archivos existentes, sin eliminar archivos nuevos.

### Guardar cambios temporalmente

Si se desea cambiar de rama sin perder los cambios actuales:

```shell
# Guardar cambios en stash
git stash

# Ver lista de stash guardados
git stash list

# Recuperar cambios del stash
git stash apply stash@{0}

# Recuperar y eliminar del stash
git stash pop

# Eliminar un stash específico
git stash drop stash@{0}
```

**Ejemplo de flujo de trabajo:**
```shell
# Trabajando en feature/analisis
git add analisis.py
git stash  # Guardar cambios temporalmente

# Cambiar a otra rama para corrección urgente
git checkout hotfix/error-critico
# ... hacer corrección ...
git commit -m "HOTFIX: Corregido error crítico"

# Volver a la rama original
git checkout feature/analisis
git stash pop  # Recuperar cambios guardados
```

---

## Fusiones (Merge)

Una fusión combina los cambios de una rama con otra, creando un nuevo commit de fusión.

### Procedimiento para fusionar ramas

```shell
# 1. Cambiar a la rama que absorberá los cambios
git checkout rama_destino

# 2. Fusionar la rama origen
git merge rama_origen
```

**Ejemplo:**
```shell
# Fusionar feature en develop
git checkout develop
git merge feature/nueva-funcionalidad
```

### Resolver conflictos de fusión

Cuando hay conflictos, Git pausará la fusión y marcará los archivos en conflicto:

```shell
# Ver archivos en conflicto
git status

# Después de resolver manualmente los conflictos
git add archivo_resuelto.py
git commit -m "MERGE: Resueltos conflictos de fusión"
```

---

## Cherry-pick

Permite copiar commits específicos de una rama a otra.

```shell
# 1. Copiar el código SHA del commit deseado
git log --oneline

# 2. Cambiar a la rama destino
git checkout rama_destino

# 3. Copiar el commit
git cherry-pick <codigo_sha>
```

**Ejemplo:**
```shell
git checkout develop
git log --oneline
# a3f5b2c FEAT: Nueva función de procesamiento

git checkout main
git cherry-pick a3f5b2c
```

### Copiar archivo desde otra rama

```shell
# 1. Cambiar a la rama destino
git checkout rama_destino

# 2. Copiar el archivo desde la rama origen
git checkout rama_origen nombre_archivo.ext
```

**Ejemplo:**
```shell
git checkout develop
git checkout feature/procesamiento config.py
git commit -m "CHORE: Actualizada configuración desde feature"
```

---

## Integración con GitHub

GitHub es la plataforma utilizada para alojar repositorios remotos y facilitar la colaboración entre equipos.

### Clonar un repositorio

```shell
git clone <url_repositorio>
```

**Ejemplo:**
```shell
git clone https://github.com/RedSismicaAustro/RSA-Acelerografo.git
```

### Vincular repositorio local con remoto

```shell
# Agregar remoto
git remote add origin <url_repositorio>

# Verificar vinculación
git remote -v

# Remover vinculación
git remote remove origin
```

### Sincronización con repositorio remoto

```shell
# Enviar commits al repositorio remoto
git push origin <nombre_rama>

# Obtener cambios sin fusionar
git fetch origin

# Obtener y fusionar cambios
git pull origin <nombre_rama>

# Push de todas las ramas
git push --all origin
```

**Ejemplo de flujo completo:**
```shell
# Crear y configurar nuevo repositorio
git init
git add .
git commit -m "FEAT: Commit inicial"
git remote add origin https://github.com/usuario/proyecto.git
git push -u origin main

# Trabajo diario
git pull origin develop
# ... realizar cambios ...
git add .
git commit -m "FEAT: Nueva funcionalidad"
git push origin develop
```

---

## Descargas desde GitHub

### Descargar un archivo específico

```shell
curl -OL https://raw.githubusercontent.com/Usuario/Repositorio/rama/ruta/archivo.ext
```

**Ejemplo:**
```shell
curl -OL https://raw.githubusercontent.com/RedSismicaAustro/RSA-Acelerografo/main/config.py
```

### Descargar un directorio específico

```shell
curl https://codeload.github.com/[owner]/[repo]/tar.gz/[branch] | \
  tar -xz --strip=2 [repo]-[branch]/[folder_path]
```

**Ejemplo:**
```shell
curl https://codeload.github.com/RedSismicaAustro/RSA-Metodologias/tar.gz/main | \
  tar -xz --strip=2 RSA-Metodologias-main/docs/git/
```

### Descargar repositorio completo comprimido

```shell
wget -c https://github.com/[owner]/[repo]/archive/[branch].zip
```

**Ejemplo:**
```shell
wget -c https://github.com/RedSismicaAustro/RSA-Acelerografo/archive/main.zip
```

---

## Gestión de Commits Remotos

### Eliminar último commit remoto

```shell
# 1. Eliminar commit en repositorio local
git reset --hard HEAD^

# 2. Forzar actualización en repositorio remoto
git push origin -f
```

**Advertencia:** El uso de `git push -f` sobrescribe el historial remoto y puede afectar a otros colaboradores. Usar con precaución y comunicar al equipo.

**Alternativa segura:**
```shell
# Revertir el commit creando un nuevo commit
git revert HEAD
git push origin <rama>
```

---

## Configuración de Nuevo Usuario

Procedimiento para configurar Git en un equipo nuevo y colaborar en proyectos existentes.

### Pasos de configuración inicial

1. **Instalar Git** en el nuevo equipo

2. **Configurar usuario**
   ```shell
   git config --global user.name "RSA-Usuario-Equipo2"
   git config --global user.email usuario@ucuenca.edu.ec
   ```

3. **Clonar el repositorio**
   ```shell
   git clone <url_repositorio>
   cd nombre_repositorio
   ```

4. **Verificar ramas disponibles**
   ```shell
   # Ver solo rama actual
   git branch
   
   # Ver todas las ramas (incluidas remotas)
   git branch --all
   ```

5. **Vincular ramas remotas con locales**
   ```shell
   git checkout -b <nombre_rama> origin/<nombre_rama>
   ```

   **Ejemplo:**
   ```shell
   git checkout -b desarrollo origin/desarrollo
   git checkout -b feature/procesamiento origin/feature/procesamiento
   ```

6. **Configurar remoto adicional** (si es necesario)
   ```shell
   git remote add upstream <url_repositorio>
   git remote -v
   ```

### Flujo de trabajo después de la configuración

```shell
# Actualizar rama antes de trabajar
git checkout desarrollo
git pull origin desarrollo

# Crear rama de trabajo
git checkout -b feature/nueva-funcionalidad

# Realizar cambios y commits
git add .
git commit -m "FEAT: Implementada nueva funcionalidad"

# Subir cambios
git push origin feature/nueva-funcionalidad
```

---

## Comandos Avanzados

### Ver diferencias

```shell
# Ver cambios no guardados
git diff

# Ver cambios en staging area
git diff --staged

# Comparar dos ramas
git diff rama1..rama2

# Ver cambios en un archivo específico
git diff archivo.py
```

### Búsqueda en el historial

```shell
# Buscar commits por mensaje
git log --grep="FEAT"

# Buscar commits por autor
git log --author="usuario"

# Ver commits que modificaron un archivo
git log --follow archivo.py

# Ver quién modificó cada línea de un archivo
git blame archivo.py
```

### Tags (Etiquetas)

```shell
# Crear tag
git tag v1.0.0

# Crear tag con mensaje
git tag -a v1.0.0 -m "Versión 1.0.0 - Release inicial"

# Listar tags
git tag

# Subir tags al remoto
git push origin v1.0.0
git push origin --tags
```

---

## Solución de Problemas Comunes

### Error: "Your local changes would be overwritten by merge"

```shell
# Opción 1: Guardar cambios temporalmente
git stash
git pull
git stash pop

# Opción 2: Descartar cambios locales
git reset --hard HEAD
git pull
```

### Error: "fatal: refusing to merge unrelated histories"

```shell
git pull origin main --allow-unrelated-histories
```

### Recuperar un archivo eliminado

```shell
# Ver historial del archivo
git log --all --full-history -- ruta/archivo.py

# Recuperar desde un commit específico
git checkout <codigo_hash> -- ruta/archivo.py
```

### Deshacer `git add` antes de commit

```shell
# Quitar archivo específico del staging
git reset HEAD archivo.py

# Quitar todos los archivos del staging
git reset HEAD
```

---

## Buenas Prácticas

1. **Realizar commits frecuentes**: Commits pequeños y frecuentes son más fáciles de revisar y revertir si es necesario.

2. **Usar mensajes descriptivos**: Seguir las convenciones de prefijos (FEAT, FIX, etc.) documentadas en la guía de formato de commits.

3. **Sincronizar regularmente**: Hacer `git pull` antes de comenzar a trabajar y `git push` al finalizar.

4. **No hacer push de trabajo incompleto a `main`**: Usar ramas de características para desarrollo y fusionar solo cuando esté completo y probado.

5. **Revisar antes de commit**: Usar `git status` y `git diff` para verificar qué cambios se están guardando.

6. **Mantener el repositorio limpio**: No incluir archivos generados, dependencias o configuraciones locales. Usar `.gitignore` apropiadamente.

7. **Comunicar al equipo**: Antes de usar comandos que reescriben historial (`git push -f`, `git rebase`), coordinar con el equipo.

---

## Referencias Rápidas

### Comandos esenciales diarios

```shell
git status          # Ver estado actual
git add .           # Agregar todos los cambios
git commit -m ""    # Guardar cambios
git pull           # Obtener cambios remotos
git push           # Enviar cambios remotos
git checkout -b    # Crear y cambiar de rama
git merge          # Fusionar ramas
```

### Comandos de emergencia

```shell
git reset --hard HEAD    # Deshacer todos los cambios locales
git clean -df            # Eliminar archivos no rastreados
git stash                # Guardar trabajo temporalmente
git reflog               # Ver historial completo (incluye commits eliminados)
```