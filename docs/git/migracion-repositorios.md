---
title: Migración de Repositorios
parent: Git
nav_order: 5
---

# Migración de Repositorios

Este documento describe el procedimiento para migrar repositorios de cuentas personales o de pasantes hacia la organización institucional Red-Sismica-del-Austro, preservando el historial completo, ramas, tags y referencias.

## Contenido
{: .no_toc }

- TOC
{:toc}

---

## Contexto

Durante el desarrollo de proyectos de investigación o prácticas pre-profesionales, los colaboradores temporales (pasantes, tesistas, becarios) suelen trabajar en repositorios alojados en sus cuentas personales de GitHub. Al finalizar el período de colaboración o al madurar el proyecto, es necesario transferir estos repositorios a la organización institucional para garantizar:

- **Continuidad del proyecto**: El código permanece accesible independientemente de cambios en el personal
- **Propiedad institucional**: Los desarrollos realizados bajo el marco de RSA quedan bajo custodia de la organización
- **Colaboración futura**: Facilita que nuevos colaboradores accedan y contribuyan al proyecto
- **Respaldo institucional**: Los repositorios se mantienen bajo las políticas de backup y seguridad de la organización

---

## Objetivo

Transferir el contenido completo de un repositorio desde una cuenta personal hacia la organización **Red-Sismica-del-Austro** en GitHub, preservando:

- Historial completo de commits
- Todas las ramas (locales y remotas)
- Tags y releases
- Referencias y metadatos
- Autoría original de los commits

El procedimiento utiliza clonación en espejo (`--mirror`) para garantizar una transferencia completa y fiel del repositorio original.

---

## Requisitos Previos

### Permisos necesarios

- Acceso de administrador a la organización **Red-Sismica-del-Austro** en GitHub
- Configuración SSH con alias `github.com-rsa` (ver [Múltiples Cuentas de GitHub](multiples-cuentas-github.md))
- Permisos de lectura en el repositorio origen

### Herramientas

- Git instalado y configurado
- Acceso SSH a GitHub configurado correctamente
- Espacio en disco suficiente para clonar el repositorio

### Información requerida

- URL del repositorio original: `https://github.com/USUARIO-PASANTE/NOMBRE_REPO.git`
- Nombre del nuevo repositorio institucional: `RSA-Intern-NombreProyecto`
- Información del autor original para atribución en README

---

## Procedimiento de Migración

### 1. Preparación del Entorno

Crear un directorio temporal para la operación de migración:

```shell
mkdir -p /home/rsa/tmp
cd /home/rsa/tmp
```

Este directorio temporal se utilizará únicamente para la clonación en espejo y será eliminado al finalizar el proceso.

---

### 2. Clonación en Modo Espejo

Clonar el repositorio original como un espejo bare (sin directorio de trabajo):

```shell
git clone --bare https://github.com/USUARIO-PASANTE/NOMBRE_REPO.git
cd NOMBRE_REPO.git
```

**Explicación de `--bare`:**
- Crea un repositorio sin directorio de trabajo (solo el contenido de `.git`)
- Incluye todas las ramas, tags y referencias
- Optimizado para transferencias completas entre repositorios

**Ejemplo con repositorio real:**
```shell
git clone --bare https://github.com/usuario-pasante/proyecto-telemetria.git
cd proyecto-telemetria.git
```

**Verificación de la clonación:**
```shell
# Listar todas las ramas
git branch -a

# Listar todos los tags
git tag -l

# Ver configuración del remoto
git remote -v
```

---

### 3. Creación del Repositorio Institucional

Acceder a GitHub y crear el nuevo repositorio en la organización:

1. Navegar a: `https://github.com/RedSismicaAustro`
2. Hacer clic en **New repository**
3. Configurar el repositorio:
   - **Owner**: Red-Sismica-del-Austro (organización)
   - **Repository name**: Seguir la convención de nombres institucional
   - **Description**: Descripción breve del proyecto
   - **Visibility**: Private o Public según políticas institucionales

**Importante**: No inicializar el repositorio con:
- README.md
- .gitignore
- LICENSE

El repositorio debe estar completamente vacío para recibir el mirror.

---

#### **Convención de Nombres**

Los repositorios institucionales siguen el formato estandarizado:

```
RSA-[Origen]-[Tipo]-[Nombre_Proyecto]
```

**Componentes:**
- **RSA**: Prefijo institucional (Red Sísmica del Austro)
- **[Origen]**: Fuente del proyecto
  - `Intern`: Proyectos desarrollados por pasantes o colaboradores temporales
  - Omitir para proyectos de infraestructura permanente
- **[Tipo]**: Clasificación técnica del proyecto
- **[Nombre_Proyecto]**: Nombre descriptivo en formato Snake_Case

---

#### **Códigos de Tipo de Proyecto**

| Código | Tipo de Proyecto | Descripción | Ejemplo |
|--------|------------------|-------------|---------|
| **SW** | Software | Aplicaciones, sistemas web, APIs | `RSA-Intern-SW-Seismic_Data_Visualizer` |
| **HW** | Hardware | Diseño de circuitos, PCBs, prototipos | `RSA-Intern-HW-Acelerografo_ESP32_SMD` |
| **FW** | Firmware | Código embebido para microcontroladores | `RSA-FW-Acelerografo_dsPIC` |
| **MX** | Mixto | Proyectos que integran software y hardware | `RSA-Intern-MX-Embedded_Seismic_Node` |
| **DS** | Data Science | Análisis de datos, procesamiento estadístico | `RSA-DS-Seismic_Analysis` |
| **ML** | Machine Learning | Modelos de aprendizaje automático | `RSA-ML-Generalized_Phase_Detection` |

---

#### **Ejemplos de Aplicación**

**Proyecto de pasante - Hardware:**
- Original: `acelerografo-esp32-smd`
- Institucional: `RSA-Intern-HW-Acelerografo_ESP32_SMD`

**Proyecto de pasante - Software:**
- Original: `dashboard-telemetria`
- Institucional: `RSA-Intern-SW-Dashboard_Telemetria`

**Proyecto de pasante - Mixto:**
- Original: `nodo-sismico-iot`
- Institucional: `RSA-Intern-MX-Nodo_Sismico_IoT`

**Proyecto de infraestructura - Firmware:**
- Original: `firmware-acelerografo`
- Institucional: `RSA-FW-Acelerografo_dsPIC`

**Proyecto de investigación - Machine Learning:**
- Original: `detector-fases-sismicas`
- Institucional: `RSA-ML-Generalized_Phase_Detection`

---

#### **Consideraciones para la Nomenclatura**

1. **Usar Snake_Case**: Separar palabras con guiones bajos (no espacios ni guiones)
2. **Nombres descriptivos**: El nombre debe indicar claramente la función del proyecto
3. **Evitar redundancia**: No repetir información ya contenida en el prefijo o tipo
4. **Consistencia de idioma**: Preferir nombres en inglés para proyectos técnicos
5. **Longitud razonable**: Máximo 50 caracteres totales para el nombre del repositorio

---

#### **Guía de Decisión Rápida para Tipo de Proyecto**

**¿Cómo clasificar el proyecto?**

```
┌─ ¿Incluye código que se ejecuta en microcontroladores/procesadores embebidos?
│  └─ SÍ → FW (Firmware)
│
├─ ¿El proyecto es principalmente diseño de circuitos/PCB/hardware físico?
│  └─ SÍ → HW (Hardware)
│
├─ ¿Combina desarrollo de hardware Y software de forma integral?
│  └─ SÍ → MX (Mixto)
│
├─ ¿Se enfoca en análisis de datos, estadística o procesamiento de datasets?
│  └─ SÍ → DS (Data Science)
│
├─ ¿Implementa modelos de ML/IA para predicción o clasificación?
│  └─ SÍ → ML (Machine Learning)
│
└─ ¿Es principalmente aplicación, API, sistema web o herramienta de escritorio?
   └─ SÍ → SW (Software)
```

**Casos ambiguos:**

| Situación | Clasificación Recomendada | Ejemplo |
|-----------|---------------------------|---------|
| Dashboard web que visualiza datos de hardware | **SW** (el foco es la interfaz) | `RSA-Intern-SW-Dashboard_Sismico` |
| Firmware con algoritmos ML embebidos | **FW** (se ejecuta en embedded) | `RSA-FW-Acelerografo_ML_Detection` |
| PCB con firmware simple de control | **HW** (el foco es el diseño físico) | `RSA-Intern-HW-Node_Seismic_v2` |
| Sistema completo: nodo hardware + gateway software | **MX** (integración crítica) | `RSA-Intern-MX-Sistema_Telemetria` |
| Pipeline de procesamiento de datos con ML | **ML** (el ML es la función principal) | `RSA-ML-Seismic_Event_Classifier` |

---

### 4. Vinculación y Transferencia

Configurar el nuevo repositorio como destino y transferir todo el contenido:

```shell
# Cambiar la URL del remoto origin
git remote set-url origin git@github.com-rsa:RedSismicaAustro/RSA-Intern-NombreProyecto.git

# Verificar que el remoto apunta al destino correcto
git remote -v
# Salida esperada:
# origin  git@github.com-rsa:RedSismicaAustro/RSA-Intern-NombreProyecto.git (fetch)
# origin  git@github.com-rsa:RedSismicaAustro/RSA-Intern-NombreProyecto.git (push)

# Empujar el espejo completo
git push --mirror origin
```

**Explicación de `--mirror`:**
- Transfiere todas las referencias (branches, tags, notes)
- Mantiene la estructura exacta del repositorio origen
- Sobrescribe referencias existentes en el destino

**Salida esperada:**
```
Enumerating objects: 1234, done.
Counting objects: 100% (1234/1234), done.
Delta compression using up to 8 threads
Compressing objects: 100% (567/567), done.
Writing objects: 100% (1234/1234), 2.34 MiB | 1.23 MiB/s, done.
Total 1234 (delta 456), reused 1234 (delta 456)
To git@github.com-rsa:RedSismicaAustro/RSA-Intern-NombreProyecto.git
 * [new branch]      main -> main
 * [new branch]      develop -> develop
 * [new branch]      feature/telemetria -> feature/telemetria
 * [new tag]         v1.0.0 -> v1.0.0
```

---

### 5. Verificación de la Migración

Clonar el repositorio institucional en su ubicación definitiva y verificar la integridad:

```shell
# Navegar al directorio de proyectos RSA
cd /home/rsa/git/rsa

# Clonar el repositorio migrado
git clone git@github.com-rsa:RedSismicaAustro/RSA-Intern-NombreProyecto.git
cd RSA-Intern-NombreProyecto
```

#### **Verificar ramas**

```shell
git branch -a
```

**Salida esperada:**
```
* main
  remotes/origin/HEAD -> origin/main
  remotes/origin/main
  remotes/origin/develop
  remotes/origin/feature/telemetria
```

#### **Verificar tags**

```shell
git tag -l
```

**Salida esperada:**
```
v0.1.0
v0.2.0
v1.0.0
```

#### **Verificar historial de commits**

```shell
git log --oneline --graph --all
```

**Salida esperada:**
```
* a3f5b2c (HEAD -> main, origin/main) FEAT: Implementado sistema de alertas
* b2e4d1a REFACTOR: Optimizado procesamiento de datos
* c1d3e2f (tag: v1.0.0) FEAT: Primera versión estable
* d4e5f6a FIX: Corregido error en conexión MQTT
...
```

#### **Verificar autoría de commits**

```shell
git log --format="%an <%ae>" | sort -u
```

Confirmar que los autores originales se preservan correctamente.

---

### 6. Ajustes Post-Migración

#### **Renombrar rama principal (si es necesario)**

Si el repositorio original utiliza `master` como rama principal, renombrarla a `main`:

```shell
# Cambiar a la rama master
git checkout master

# Renombrar localmente
git branch -m master main

# Subir la nueva rama
git push -u origin main

# Eliminar la rama master remota
git push origin --delete master
```

**Actualizar rama por defecto en GitHub:**
1. Navegar a: `Settings → Branches`
2. En **Default branch**, seleccionar `main`
3. Hacer clic en **Update**
4. Confirmar el cambio

#### **Actualizar README.md**

Editar el archivo README para reflejar el nuevo contexto institucional:

```markdown
# RSA-Intern-NombreProyecto

Descripción del proyecto...

## Origen del Proyecto

Este proyecto fue originalmente desarrollado por **Nombre del Pasante** como parte de su trabajo de [práctica pre-profesional/tesis/investigación] en la Red Sísmica del Austro.

**Repositorio original:** [usuario-pasante/proyecto-original](https://github.com/usuario-pasante/proyecto-original)

## Desarrollo Actual

El proyecto es actualmente mantenido por el equipo de la Red Sísmica del Austro y forma parte de la infraestructura institucional.

## Contribuciones

Las contribuciones al proyecto deben seguir las guías de la organización disponibles en [RSA-Metodologias](https://github.com/RedSismicaAustro/RSA-Metodologias).
```

**Realizar commit y push:**

```shell
git add README.md
git commit -m "DOCS: Actualizado README para reflejar contexto institucional"
git push origin main
```

#### **Configurar protecciones de rama**

Establecer protecciones para la rama principal:

1. Navegar a: `Settings → Branches → Branch protection rules`
2. Hacer clic en **Add rule**
3. Configurar:
   - **Branch name pattern**: `main`
   - ☑ Require pull request reviews before merging
   - ☑ Require status checks to pass before merging
   - ☑ Include administrators (opcional)

#### **Añadir colaboradores**

Gestionar acceso al repositorio:

1. Navegar a: `Settings → Collaborators and teams`
2. Añadir equipos de la organización con permisos apropiados:
   - **RSA Core Team**: Admin
   - **RSA Developers**: Write
   - **RSA Interns**: Read

---

### 7. Limpieza

Eliminar el directorio temporal utilizado para la migración:

```shell
rm -rf /home/rsa/tmp/NOMBRE_REPO.git
```

Verificar que no quedan archivos temporales:

```shell
ls -la /home/rsa/tmp/
```

---

## Gestión del Repositorio Original

### Opciones para el repositorio de origen

Después de una migración exitosa, existen varias opciones para gestionar el repositorio original:

#### **Opción 1: Archivar el repositorio**

Preservar el repositorio original como archivo histórico:

1. En GitHub, navegar al repositorio original
2. Ir a `Settings → General`
3. En **Danger Zone**, seleccionar **Archive this repository**
4. Confirmar la acción

**Ventajas:**
- Preserva el historial para referencia
- Claramente marcado como inactivo
- Enlaces históricos siguen funcionando

#### **Opción 2: Añadir aviso de redirección**

Actualizar el README del repositorio original:

```markdown
# ⚠️ REPOSITORIO MIGRADO

Este repositorio ha sido transferido a la organización institucional.

**Nueva ubicación:** [RedSismicaAustro/RSA-Intern-NombreProyecto](https://github.com/RedSismicaAustro/RSA-Intern-NombreProyecto)

Por favor, utilizar el nuevo repositorio para todas las contribuciones futuras.
```

#### **Opción 3: Eliminar el repositorio original**

**Advertencia:** Esta acción es irreversible.

Solo considerar si:
- La migración se verificó completamente
- No existen referencias externas críticas al repositorio original
- Se cuenta con backup completo del contenido

---

## Casos Especiales

### Migración con dependencias privadas

Si el repositorio tiene submódulos o dependencias de repositorios privados:

```shell
# Durante la clonación bare
git clone --bare --recurse-submodules https://github.com/USUARIO-PASANTE/NOMBRE_REPO.git

# Actualizar URLs de submódulos en el nuevo repositorio
cd /home/rsa/git/rsa/RSA-Intern-NombreProyecto
git submodule foreach 'git remote set-url origin git@github.com-rsa:RedSismicaAustro/$name.git'
git add .gitmodules
git commit -m "CHORE: Actualizadas URLs de submódulos"
git push
```

### Migración de repositorios muy grandes

Para repositorios con historiales extensos (>1GB):

```shell
# Usar opciones adicionales para optimizar la transferencia
git clone --bare --single-branch https://github.com/USUARIO-PASANTE/NOMBRE_REPO.git
cd NOMBRE_REPO.git

# Optimizar el repositorio antes del push
git gc --aggressive --prune=now

# Push con buffer aumentado
git config http.postBuffer 524288000
git push --mirror origin
```

### Preservar GitHub Issues y Pull Requests

**Limitación:** Git no transfiere issues ni PRs en la clonación.

**Alternativas:**
1. Exportar issues manualmente y recrearlos en el nuevo repositorio
2. Usar herramientas de terceros como `github-issue-mover`
3. Mantener el repositorio original archivado para referencia histórica

**Documentar issues importantes:**

En el nuevo repositorio, crear un archivo `MIGRATION.md`:

```markdown
# Notas de Migración

Este repositorio fue migrado desde `usuario-pasante/proyecto-original`.

## Issues Importantes del Repositorio Original

- Issue #12: Implementación de sistema de alertas
- Issue #23: Bug en procesamiento de timestamps
- PR #15: Optimización de consultas a base de datos

Para el contexto completo, consultar el [repositorio original archivado](https://github.com/usuario-pasante/proyecto-original).
```

---

## Verificación Final

### Comandos de verificación rápida

```shell
# Contar commits en ambos repositorios (deben coincidir)
cd /home/rsa/tmp/repo-original
git rev-list --all --count

cd /home/rsa/git/rsa/RSA-Intern-NombreProyecto
git rev-list --all --count

# Comparar último commit
cd /home/rsa/tmp/repo-original
git log -1 --format="%H %s"

cd /home/rsa/git/rsa/RSA-Intern-NombreProyecto
git log -1 --format="%H %s"
```

---

## Solución de Problemas

### Error: "Permission denied (publickey)"

**Causa:** Configuración SSH incorrecta o llave no asociada a la cuenta.

**Solución:**
```shell
# Verificar autenticación SSH
ssh -T git@github.com-rsa

# Si falla, verificar configuración
cat ~/.ssh/config | grep -A 5 "github.com-rsa"

# Añadir llave al agente si es necesario
ssh-add ~/.ssh/id_ed25519_rsa
```

### Error: "Repository not empty"

**Causa:** El repositorio destino tiene contenido previo.

**Solución:**
```shell
# Opción 1: Forzar la actualización (CUIDADO: sobrescribe contenido)
git push --mirror --force origin

# Opción 2: Eliminar el repositorio en GitHub y recrearlo vacío
```

### Error: "Large files detected"

**Causa:** El repositorio contiene archivos grandes que exceden límites de GitHub.

**Solución:**
```shell
# Identificar archivos grandes
git rev-list --objects --all | \
  git cat-file --batch-check='%(objecttype) %(objectname) %(objectsize) %(rest)' | \
  awk '/^blob/ {print substr($0,6)}' | sort -n -k 2 | tail -20

# Opción 1: Usar Git LFS
git lfs migrate import --include="*.zip,*.tar.gz" --everything

# Opción 2: Limpiar historial con BFG Repo-Cleaner
bfg --strip-blobs-bigger-than 100M NOMBRE_REPO.git
```

### Commits aparecen con autor incorrecto

**Causa:** Configuración de Git local sobrescribió autoría durante el proceso.

**Verificación:**
```shell
git log --format="%an <%ae>" | sort -u
```

**Corrección:**
```shell
# Si se realizó push sin --mirror, rehacer desde el bare clone
cd /home/rsa/tmp/NOMBRE_REPO.git
git push --mirror --force origin
```

---

## Buenas Prácticas

### Durante la migración

1. **Comunicar el proceso**: Notificar al autor original y colaboradores sobre la migración
2. **Documentar la migración**: Mantener registro de fecha, responsable y justificación
3. **Verificar exhaustivamente**: No eliminar el repositorio original hasta confirmar la migración
4. **Realizar en horarios de baja actividad**: Minimizar interrupciones a colaboradores activos

### Después de la migración

1. **Actualizar referencias**: Buscar y actualizar enlaces al repositorio original en documentación, wikis, etc.
2. **Configurar webhooks**: Si existían integraciones (CI/CD, notificaciones), reconfigurarlas
3. **Revisar permisos**: Asegurar que solo personal autorizado tiene acceso
4. **Establecer políticas**: Configurar branch protection y requerimientos de revisión

### Atribución y reconocimiento

1. **Preservar autoría**: Nunca modificar la autoría de commits existentes
2. **Reconocer contribuciones**: Documentar claramente el trabajo del autor original
3. **Mantener contacto**: Informar al autor sobre el nuevo hogar del proyecto
4. **Considerar licencias**: Respetar cualquier licencia original del proyecto

---

## Referencias

### Comandos clave

```shell
# Clonación en espejo
git clone --bare <URL_ORIGEN>

# Cambiar remoto
git remote set-url origin <URL_DESTINO>

# Push en espejo
git push --mirror origin

# Renombrar rama
git branch -m <nombre_antiguo> <nombre_nuevo>

# Eliminar rama remota
git push origin --delete <nombre_rama>
```

### Documentación relacionada

- [Comandos Comunes](comandos-comunes.md) - Referencia de comandos Git básicos
- [Múltiples Cuentas de GitHub](multiples-cuentas-github.md) - Configuración SSH para la organización
- [Formato de Commits](formato-commits.md) - Convenciones para nuevos commits post-migración

### Recursos externos

- [GitHub Docs: Duplicating a repository](https://docs.github.com/en/repositories/creating-and-managing-repositories/duplicating-a-repository)
- [Git Documentation: git-clone](https://git-scm.com/docs/git-clone)
- [GitHub Docs: Transferring a repository](https://docs.github.com/en/repositories/creating-and-managing-repositories/transferring-a-repository)