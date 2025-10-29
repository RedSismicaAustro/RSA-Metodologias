---
title: Configuración de Micromamba
parent: Entornos
nav_order: 3
---

# Configuración de Micromamba

Guía completa para instalar y configurar micromamba, un gestor de paquetes y entornos ligero y rápido compatible con conda.

## Contenido
{: .no_toc }

- TOC
{:toc}

---

## ¿Qué es Micromamba?

Micromamba es una implementación rápida y ligera de conda, diseñada para gestionar entornos Python y paquetes de manera eficiente. Es especialmente útil en sistemas con recursos limitados o cuando se necesita una instalación rápida.

**Ventajas principales:**
- **Velocidad**: Resolución de dependencias más rápida que conda tradicional
- **Ligereza**: Ejecutable único sin dependencias de Python
- **Compatibilidad**: Compatible con repositorios conda-forge y archivos conda
- **Eficiencia**: Menor uso de disco y memoria

---

## Instalación en WSL/Ubuntu

### Método recomendado (paso a paso)

Instalación manual con control completo del proceso:

```shell
# Actualizar sistema e instalar prerrequisitos
sudo apt update
sudo apt install -y bzip2 tar curl ca-certificates

# Descargar micromamba en directorio temporal
cd /tmp
curl -L https://micro.mamba.pm/api/micromamba/linux-64/latest | tar -xvj bin/micromamba

# Mover a directorio local del usuario
mkdir -p ~/.local/bin
mv bin/micromamba ~/.local/bin/
rmdir bin

# Añadir ~/.local/bin al PATH de forma persistente
grep -qxF 'export PATH="$HOME/.local/bin:$PATH"' ~/.bashrc || \
  echo 'export PATH="$HOME/.local/bin:$PATH"' >> ~/.bashrc

# Recargar configuración de bash
source ~/.bashrc

# Inicializar integración con bash
micromamba shell init --shell=bash
source ~/.bashrc

# Verificar instalación
micromamba --version
```

### Método automático (script oficial)

Instalación rápida usando el script oficial:

```shell
# Actualizar e instalar dependencias mínimas
sudo apt update && sudo apt install -y curl ca-certificates

# Ejecutar instalador oficial
curl -Ls https://micro.mamba.pm/install.sh | bash

# Recargar bash
source ~/.bashrc

# Verificar instalación
micromamba --version
```

---

## Creación de Entornos

### Entorno básico

Crear un entorno con una versión específica de Python:

```shell
# Crear entorno con Python 3.11
micromamba create -y -n mi-proyecto python=3.11

# Activar el entorno
micromamba activate mi-proyecto

# Verificar versión de Python
python --version
```

### Entorno con paquetes específicos

Crear un entorno con múltiples paquetes desde el inicio:

```shell
# Crear entorno con Python y paquetes científicos
micromamba create -y -n analisis-datos \
  python=3.11 \
  numpy pandas matplotlib scipy

# Activar y verificar
micromamba activate analisis-datos
python -c "import numpy; print(numpy.__version__)"
```

### Entorno con versiones específicas

Control preciso de versiones para reproducibilidad:

```shell
# Crear entorno con versiones exactas
micromamba create -y -n mi-entorno \
  python=3.9 \
  tensorflow=2.12 \
  keras=2.12 \
  "numpy<2.0" \
  h5py=3.8

# Instalar paquetes adicionales desde conda-forge
micromamba activate mi-entorno
micromamba install -y -c conda-forge obspy pandas python-dotenv
```

---

## Gestión de Paquetes

### Instalación básica

```shell
# Activar entorno
micromamba activate mi-entorno

# Instalar un paquete
micromamba install -y -c conda-forge nombre-paquete

# Instalar múltiples paquetes
micromamba install -y -c conda-forge paquete1 paquete2 paquete3
```

### Instalación segura (freeze-installed)

Para evitar que el solver modifique dependencias críticas ya instaladas:

```shell
# 1. Revisar el plan de instalación antes de ejecutar
micromamba install -c conda-forge nuevo-paquete --dry-run

# 2. Instalar sin modificar paquetes existentes
micromamba install -c conda-forge nuevo-paquete --freeze-installed
```

**¿Por qué usar --freeze-installed?**

El solver de micromamba puede intentar "optimizar" el entorno actualizando dependencias. Esto puede causar problemas si se tienen versiones específicas de paquetes críticos (como TensorFlow, PyTorch, etc.). La opción `--freeze-installed` le indica al solver que no toque los paquetes ya instalados.

### Actualizar paquetes

```shell
# Actualizar un paquete específico
micromamba update nombre-paquete

# Actualizar todos los paquetes del entorno
micromamba update --all

# Actualizar con precaución (revisar plan primero)
micromamba update nombre-paquete --dry-run
micromamba update nombre-paquete
```

---

## Replicación de Entornos

### Exportar entorno funcional

Para garantizar reproducibilidad exacta en otra máquina:

```shell
# Activar el entorno a exportar
micromamba activate mi-entorno

# Exportar especificación exacta (lockfile)
micromamba env export --explicit > mi-entorno.lock
```

El archivo `.lock` contiene URLs directas a los paquetes exactos instalados, garantizando reproducibilidad binaria.

### Reconstruir entorno desde lockfile

```shell
# En la máquina destino, crear entorno desde lockfile
micromamba create -n mi-entorno -f mi-entorno.lock

# Verificar que el entorno es idéntico
micromamba activate mi-entorno
micromamba list
```

### Exportar para otras plataformas

Si se necesita compartir el entorno con diferentes sistemas operativos:

```shell
# Exportar entorno multiplataforma (YAML)
micromamba env export > mi-entorno.yml

# Reconstruir en otra máquina
micromamba env create -f mi-entorno.yml
```

**Nota:** El archivo YAML no garantiza versiones binarias exactas, solo versiones de paquetes.

---

## Gestión de Revisiones

Micromamba mantiene un historial de cambios del entorno, permitiendo volver a estados anteriores.

### Ver historial de revisiones

```shell
# Activar entorno
micromamba activate mi-entorno

# Listar todas las revisiones
micromamba list --revisions
```

Salida típica:
```
     0  2025-10-15 10:30:00  (rev 0)
     1  2025-10-15 11:45:00  +numpy, +pandas
     2  2025-10-16 09:15:00  +matplotlib
     3  2025-10-17 14:20:00  +tensorflow
```

### Volver a una revisión anterior

```shell
# Volver a la revisión 2
micromamba install --revision 2

# Verificar paquetes instalados
micromamba list
```

**Casos de uso:**
- Deshacer instalación problemática
- Volver a un estado funcional conocido
- Depurar conflictos de dependencias

---

## Comandos Útiles

### Gestión de entornos

```shell
# Listar todos los entornos
micromamba env list

# Información detallada del entorno activo
micromamba info

# Listar paquetes instalados
micromamba list

# Buscar un paquete en los repositorios
micromamba search nombre-paquete

# Eliminar un entorno completo
micromamba remove -n nombre-entorno --all

# Limpiar caché de paquetes
micromamba clean --all
```

### Activación y desactivación

```shell
# Activar entorno
micromamba activate nombre-entorno

# Desactivar entorno actual
micromamba deactivate

# Ver entorno activo
echo $CONDA_DEFAULT_ENV
```

---

## Mejores Prácticas

### Organización de entornos

**Un entorno por proyecto:**
```
~/micromamba/envs/
├── rsa-acelerografo/      # Proyecto de adquisición
├── rsa-analisis/          # Análisis de datos sísmicos
├── rsa-dashboard/         # Dashboard web
└── desarrollo-general/    # Entorno para desarrollo diverso
```

### Documentación de dependencias

Mantener siempre documentadas las dependencias del proyecto:

```shell
# En el directorio del proyecto
micromamba activate mi-proyecto

# Exportar ambiente.yml para compartir
micromamba env export > environment.yml

# Exportar lockfile para reproducibilidad exacta
micromamba env export --explicit > environment.lock

# Versionar ambos archivos en Git
git add environment.yml environment.lock
git commit -m "DOCS: Actualizado entorno de proyecto"
```

### Congelación de dependencias críticas

Para proyectos con dependencias sensibles (ML, procesamiento científico):

**1. Crear entorno base con versiones fijas:**
```shell
micromamba create -n proyecto-ml \
  python=3.9 \
  tensorflow=2.12 \
  keras=2.12 \
  "numpy<2.0"
```

**2. Exportar inmediatamente:**
```shell
micromamba env export --explicit > base.lock
```

**3. Instalar paquetes adicionales con precaución:**
```shell
# Siempre revisar plan primero
micromamba install -c conda-forge nuevo-paquete --dry-run

# Si es seguro, instalar con freeze
micromamba install -c conda-forge nuevo-paquete --freeze-installed
```

### Limpieza periódica

```shell
# Limpiar paquetes descargados
micromamba clean --all

# Eliminar entornos no utilizados
micromamba env list
micromamba remove -n entorno-viejo --all
```

---

## Solución de Problemas

### Error: "Solver no puede resolver dependencias"

**Problema:** Conflictos de versiones entre paquetes.

**Soluciones:**
1. Usar versiones más flexibles: `"numpy>=1.20,<2.0"` en lugar de `numpy=1.23.5`
2. Separar instalaciones en pasos: instalar paquetes base primero, luego adicionales
3. Usar `--freeze-installed` para no modificar paquetes críticos

### Error: "Command not found: micromamba"

**Problema:** PATH no configurado correctamente.

**Solución:**
```shell
# Añadir al PATH manualmente
export PATH="$HOME/.local/bin:$PATH"

# Hacer permanente
echo 'export PATH="$HOME/.local/bin:$PATH"' >> ~/.bashrc
source ~/.bashrc
```

### Entorno corrupto

**Problema:** El entorno no funciona correctamente después de una instalación.

**Soluciones:**
1. Volver a revisión anterior:
   ```shell
   micromamba list --revisions
   micromamba install --revision N
   ```

2. Recrear desde lockfile (si existe):
   ```shell
   micromamba remove -n mi-entorno --all
   micromamba create -n mi-entorno -f mi-entorno.lock
   ```

3. Recrear desde cero:
   ```shell
   micromamba remove -n mi-entorno --all
   # Seguir pasos de instalación inicial
   ```

---

## Comparación: Micromamba vs Conda

| Característica | Micromamba | Conda |
|---------------|------------|-------|
| Tamaño instalación | ~6 MB | ~500 MB |
| Velocidad | Muy rápida | Moderada |
| Resolución de dependencias | Libmamba (rápida) | Clásica |
| Compatibilidad | Repositorios conda | Repositorios conda |
| Gestión de Python base | No incluye Python | Incluye Python |
| Casos de uso | CI/CD, contenedores, recursos limitados | Uso general, principiantes |

---

## Integración con Proyectos RSA

### Estructura recomendada para proyectos

```
mi-proyecto-rsa/
├── environment.yml          # Especificación multiplataforma
├── environment.lock         # Lockfile para reproducibilidad exacta
├── src/                     # Código fuente
├── data/                    # Datos
├── notebooks/              # Jupyter notebooks
└── README.md               # Documentación incluye setup del entorno
```

### Template de environment.yml

```yaml
name: mi-proyecto-rsa
channels:
  - conda-forge
  - defaults
dependencies:
  - python=3.11
  - numpy>=1.24
  - pandas>=2.0
  - matplotlib>=3.7
  - scipy>=1.11
  # Paquetes específicos del proyecto
  - obspy>=1.4
  - python-dotenv
  # Desarrollo
  - jupyter
  - ipython
```

### Instrucciones de setup en README

```markdown
## Configuración del Entorno

### Usando Micromamba (recomendado)

\```shell
# Crear entorno desde environment.yml
micromamba env create -f environment.yml

# O desde lockfile para reproducibilidad exacta
micromamba create -n mi-proyecto-rsa -f environment.lock

# Activar entorno
micromamba activate mi-proyecto-rsa
\```
```

---

## Recursos Adicionales

### Documentación relacionada

- [Instalación de WSL y Ubuntu](instalacion-wsl.md) - Configuración inicial del sistema
- [Desarrollo Remoto con SSHFS](desarrollo-remoto-sshfs.md) - Trabajo en sistemas remotos

### Enlaces externos

- [Documentación oficial de Micromamba](https://mamba.readthedocs.io/en/latest/user_guide/micromamba.html)
- [Conda-forge](https://conda-forge.org/) - Repositorio principal de paquetes
- [Cheatsheet de comandos Conda/Micromamba](https://docs.conda.io/projects/conda/en/latest/user-guide/cheatsheet.html)

---

## Próximos Pasos

Después de configurar micromamba, puedes:

1. **Crear entornos para tus proyectos RSA**: Seguir las mejores prácticas documentadas
2. **Integrar con Git**: Versionar archivos de entorno
3. **Configurar VS Code**: Integración con entornos virtuales
4. **Explorar contenedores**: Usar micromamba en Docker para despliegue