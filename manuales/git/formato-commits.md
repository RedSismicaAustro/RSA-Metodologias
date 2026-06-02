---
title: Formato de Commits
parent: Git
nav_order: 3
---

# Formato de Commits

Esta guía describe las convenciones para escribir mensajes de commit claros y consistentes en los proyectos de la Red Sísmica del Austro.

## Contenido
{: .no_toc }

- TOC
{:toc}

---

## Prefijos Comunes para Commits

### 1. WIP: Work in Progress

Indica que el trabajo aún no está completo, pero necesitas hacer un commit para guardar tu progreso.

**Ejemplo:**
```shell
git commit -m "WIP: Implementación parcial de la autenticación"
```

### 2. FIX: Corrección de errores (bugfix)

Se usa cuando el commit corrige un error o bug en el código.

**Ejemplo:**
```shell
git commit -m "FIX: Corregido error de validación de formulario en login"
```

### 3. FEAT: Nueva funcionalidad (feature)

Este prefijo se usa cuando estás añadiendo una nueva funcionalidad o característica al proyecto.

**Ejemplo:**
```shell
git commit -m "FEAT: Añadido soporte para autenticación con tokens JWT"
```

### 4. REFACTOR: Refactorización de código

Indica que has cambiado el código para mejorar su estructura o rendimiento, sin alterar su funcionalidad.

**Ejemplo:**
```shell
git commit -m "REFACTOR: Mejorada la legibilidad del módulo de autenticación"
```

### 5. DOCS: Cambios en la documentación

Utilizado para commits que solo afectan a la documentación del proyecto.

**Ejemplo:**
```shell
git commit -m "DOCS: Actualizado el README con instrucciones de autenticación"
```

### 6. STYLE: Cambios de estilo o formato

Se usa para indicar cambios que no afectan la lógica del código, como ajustes en la indentación, formato de código o nombres de variables.

**Ejemplo:**
```shell
git commit -m "STYLE: Formateado el código según el estándar PEP8"
```

### 7. CHORE: Tareas de mantenimiento o cambios menores

Este prefijo se utiliza para tareas rutinarias que no afectan la funcionalidad del código, como la actualización de dependencias, ajustes en scripts o cambios en la configuración del proyecto.

**Ejemplo:**
```shell
git commit -m "CHORE: Actualizadas las dependencias de npm"
```

### 8. TEST: Añadir o modificar tests

Se usa cuando el commit incluye cambios en las pruebas unitarias o de integración.

**Ejemplo:**
```shell
git commit -m "TEST: Añadidos tests unitarios para el módulo de autenticación"
```

### 9. PERF: Mejoras en el rendimiento

Indica que el commit mejora el rendimiento del código sin cambiar su funcionalidad.

**Ejemplo:**
```shell
git commit -m "PERF: Optimizado el procesamiento de datos en el servidor"
```

### 10. BUILD: Cambios en el sistema de build o en dependencias externas

Se usa para cambios que afectan el sistema de compilación o dependencias externas (configuración de CI, scripts de build, etc.).

**Ejemplo:**
```shell
git commit -m "BUILD: Actualizado script de despliegue automático"
```

### 11. CI: Cambios en la configuración de integración continua

Este prefijo es útil para identificar ajustes en el sistema de CI/CD, como cambios en los scripts de configuración de Jenkins, Travis, GitHub Actions, etc.

**Ejemplo:**
```shell
git commit -m "CI: Configurado GitHub Actions para ejecutar pruebas automáticas"
```

### 12. SECURITY: Cambios relacionados con la seguridad

Se usa para indicar que el commit incluye parches o mejoras de seguridad.

**Ejemplo:**
```shell
git commit -m "SECURITY: Corregida vulnerabilidad de inyección de SQL"
```

### 13. HOTFIX: Corrección urgente de un error en producción

Este prefijo se utiliza cuando hay que realizar una corrección crítica que debe ser aplicada rápidamente.

**Ejemplo:**
```shell
git commit -m "HOTFIX: Corregido error de autenticación que rompía la sesión de usuarios"
```

---

## Ejemplo de Flujo con Prefijos

En el siguiente ejemplo se implementa nueva funcionalidad de autenticación, seguido de un commit intermedio y una corrección de un bug:

```shell
git commit -m "WIP: Comienzo de la implementación de autenticación de usuarios"
git commit -m "FEAT: Añadida autenticación con tokens JWT"
git commit -m "FIX: Corregido error en la validación del token de autenticación"
```

---

## Beneficios de Utilizar Prefijos

1. **Claridad**: Los prefijos permiten identificar rápidamente el propósito de un commit.
2. **Historial limpio**: Facilita la revisión del historial de commits, ya que permite filtrar por tipos de cambios (por ejemplo, ver solo los commits de bugs o nuevas funcionalidades).
3. **Colaboración**: Si en el futuro más colaboradores se suman al proyecto, estos prefijos ayudan a todos los miembros del equipo a entender rápidamente qué tipo de cambios se han realizado sin necesidad de examinar todo el código.

---

## Filtrado de Commits por Tipo

Al seguir un esquema consistente de prefijos, se puede filtrar fácilmente los commits por tipo con el siguiente comando:

```shell
git log --grep="FIX"
```

Esto mostrará solo los commits con el prefijo `FIX`, por ejemplo.

En resumen, utilizar prefijos en los commits mejora significativamente la organización del historial del proyecto y facilita el seguimiento de los diferentes tipos de cambios realizados. 