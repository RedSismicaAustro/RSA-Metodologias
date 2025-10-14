---
title: Instalación de WSL y Ubuntu en Windows
parent: Entornos
nav_order: 1
---

# Instalación y Configuración de WSL y Ubuntu en Windows

## 1. Descarga de WSL y la distribución de Ubuntu

1. Acceder al sitio oficial de Ubuntu para WSL:
   [https://ubuntu.com/desktop/wsl](https://ubuntu.com/desktop/wsl)
2. Descargar el archivo comprimido de la versión deseada de Ubuntu (por ejemplo, `ubuntu-24.04.3-wsl-amd64.gz`).

> **Sugerencia:** Guardar el archivo en una ruta fácilmente accesible, como la carpeta *Descargas*.

---

## 2. Importación manual de la distribución en WSL

1. Abrir **PowerShell** con permisos de administrador.
2. Crear un directorio donde se almacenará la instalación de la distribución:

   ```powershell
   mkdir C:\WSL\Ubuntu2404
   ```
3. Ejecutar el siguiente comando para importar la distribución descargada:

   ```powershell
   wsl --import Ubuntu-24.04 C:\WSL\Ubuntu2404 C:\Users\<usuario>\Downloads\ubuntu-24.04.3-wsl-amd64.gz --version 2
   ```

   * Reemplazar `<usuario>` con el nombre del usuario de Windows correspondiente.
   * Asegurarse de que la ruta del archivo `.gz` coincida con la ubicación de descarga.

> **Nota:** Este método permite instalar WSL incluso si la Microsoft Store está bloqueada o no disponible.

---

## 3. Inicio de la distribución Ubuntu en WSL

Para iniciar la nueva distribución:

```powershell
wsl -d Ubuntu-24.04
```

---

## 4. Creación y configuración del usuario dentro de Ubuntu

1. Crear un usuario nuevo:

   ```bash
   adduser <nombre_usuario>
   ```
2. Agregar el usuario al grupo de administradores (sudo):

   ```bash
   usermod -aG sudo <nombre_usuario>
   ```
3. Salir de la sesión actual e iniciar sesión con el nuevo usuario:

   ```powershell
   wsl -d Ubuntu-24.04 -u <nombre_usuario>
   ```
4. Configurar el usuario como predeterminado:

   ```bash
   sudo nano /etc/wsl.conf
   ```

   Agregar el siguiente bloque:

   ```ini
   [user]
   default=<nombre_usuario>
   ```
5. Guardar los cambios y reiniciar WSL:

   ```powershell
   wsl --shutdown
   ```
6. Verificar el acceso con el usuario predeterminado:

   ```powershell
   wsl -d Ubuntu-24.04
   ```

> **Sugerencia:** Se recomienda asignar un nombre de usuario corto, sin espacios ni caracteres especiales.

---

## 5. Verificación de la instalación

* En PowerShell:

  ```powershell
  wsl --list --verbose
  ```

  Este comando mostrará las distribuciones instaladas y su versión de WSL (debe aparecer como `VERSION 2`).

* Dentro de Ubuntu:

  ```bash
  lsb_release -a
  ```

  Este comando confirmará la versión exacta del sistema Ubuntu instalada.

---

## 6. Sugerencias adicionales

* Para **actualizar los paquetes** del sistema:

  ```bash
  sudo apt update && sudo apt upgrade -y
  ```
* Si WSL no tiene conexión a Internet, verificar la configuración del archivo `/etc/resolv.conf` e incluir:

  ```bash
  nameserver 8.8.8.8
  ```
* Para detener y reiniciar completamente WSL:

  ```powershell
  wsl --shutdown
  ```
* Para acceder a los archivos de WSL desde Windows:
  Abrir el explorador y escribir en la barra de direcciones:

  ```
  \\wsl$\Ubuntu-24.04
  ```

---

## 7. Ejecución de Ubuntu desde WSL

Una vez configurado, Ubuntu puede ejecutarse de las siguientes formas:

* Desde PowerShell o CMD:

  ```powershell
  wsl -d Ubuntu-24.04
  ```
* Desde el menú de inicio de Windows, buscando **Ubuntu 24.04**.

> **Nota:** Al ejecutar por primera vez, WSL inicializa la red y los servicios base, lo que puede tardar algunos segundos.

---

### Instalación completada

El sistema Ubuntu en WSL está listo para ejecutar comandos de Linux, instalar entornos de desarrollo, y ejecutar herramientas científicas o de ingeniería directamente dentro de Windows.
