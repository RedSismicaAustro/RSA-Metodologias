---
title: Adaptador WiFi TL-WN822N en Raspberry Pi
parent: Redes
nav_order: 1
---

# Configuración del Adaptador WiFi USB TL-WN822N en Raspberry Pi 3B+

Este documento describe el procedimiento realizado para instalar y configurar el adaptador WiFi USB **TP-Link TL-WN822N** en una **Raspberry Pi 3B+** con sistema operativo **Raspberry Pi OS (Bullseye)**.

---

## 1. Verificación del dispositivo

1. Conectar el adaptador WiFi TL-WN822N a un puerto USB del Raspberry Pi.

2. Verificar que el sistema detecta el dispositivo:

   ```bash
   lsusb
   ```
   Verificar que exista una linea similar a esta:
   ```bash
   Bus 001 Device 006: ID 2357:0108 TP-Link TL-WN822N Version 4 RTL8192EU
   ```

3. Comprobar la versión del kernel:

   ```bash
   uname -r
   ```

   En este caso: `6.1.21-v7+`

---

## 2. Instalación de dependencias

Se instalaron los paquetes necesarios para compilar el módulo del controlador:

```bash
sudo apt update
sudo apt install raspberrypi-kernel-headers build-essential dkms git -y
```

---

## 3. Descarga e instalación del controlador

Se utilizó el repositorio del controlador **rtl8192eu** para el chipset **Realtek RTL8192EU**, correspondiente al adaptador TL-WN822N.

```bash
git clone https://github.com/Mange/rtl8192eu-linux-driver.git
cd rtl8192eu-linux-driver
```
Es necesario modificar el Makefile para que el driver se compile para ARM (Pi):
```bash
CONFIG_PLATFORM_I386_PC = n
CONFIG_PLATFORM_ARM_RPI = y
```

Compilación e instalación mediante DKMS:

```bash
make
sudo make install
sudo dkms add .
sudo dkms install rtl8192eu/1.0
```

Durante la instalación, se obtuvo el siguiente mensaje indicando que el módulo ya existía:

```
Good news! Module version v5.6.4_35685.20191108_COEX20171113-0047 for 8192eu.ko.xz
exactly matches what is already found in kernel 6.1.21-v7+.
DKMS will not replace this module.
```

---

## 4. Carga del módulo

Cargar el modulo **8192eu** con el siguiente comando:

```bash
sudo modprobe 8192eu
```

Verificación del módulo cargado:

```bash
lsmod | grep 8192
modinfo -n 8192eu
```

---

## 5. Verificación de interfaces inalámbricas

Para confirmar que el adaptador fue reconocido correctamente:

```bash
sudo iw dev
```

Resultado obtenido:

```
phy#1 → wlan1 (adaptador USB TL-WN822N)
phy#0 → wlan0 (WiFi interno del Raspberry Pi 3B+)
```

Esto confirma que el adaptador USB fue detectado correctamente como **wlan1**.

---

## 6. Configuración de la conexión WiFi

Para simplificar la conexión, se configuró el adaptador **desde el entorno de escritorio del Raspberry Pi**, accedido mediante **VNC**.

1. Abrir el entorno gráfico vía VNC.
2. Seleccionar el icono de red en la barra superior.
3. Elegir la interfaz correspondiente al adaptador externo (**wlan1**).
4. Seleccionar la red WiFi deseada.
5. Ingresar la contraseña de acceso.
6. Confirmar la conexión y comprobar acceso a Internet.

---

## 7. Verificación final

Comprobación de conectividad y detección automática tras reinicio:

```bash
ping -c 4 8.8.8.8
```

Si se desea que el módulo se cargue automáticamente al iniciar el sistema:

```bash
echo 8192eu | sudo tee -a /etc/modules
```

---

## 8. Observaciones

* El adaptador se identificó correctamente como **wlan1**.
* El módulo **8192eu** viene incluido en el kernel 6.1.21-v7+.
* No fue necesario recompilar el driver manualmente, ya que DKMS detectó la versión instalada.
* La configuración mediante la interfaz gráfica fue suficiente para establecer la conexión WiFi de forma estable.

---

**Estado final:** Adaptador TL-WN822N operativo en Raspberry Pi 3B+ con conexión estable a red WiFi externa.
