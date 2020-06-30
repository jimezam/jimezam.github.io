---
layout: post
title: Instalación mínima de KVM en Ubuntu/Mint 20 para la aceleración de AVD
date: 2020-06-30 01:31 -0500
categories: Development
tags: linux kvm avd android mobile
---

## Introducción

En esta publicación se describen los pasos necesarios para instalar la infraestructura mínima de KVM en Linux Ubuntu/Mint 20 para, teniendo el debido soporte de virtualización en hardware, acelerar las máquinas virtuales de Android (*Android Virtual Device* o AVD).

## Requerimientos

 * En términos de software, el usuario que realiza la instalación debe contar con los permisos suficientes para realizar el procedimiento que se describe a continuación.
 * En términos de hardware, los procesadores Intel deben soportar *Virtualization Technology* (VT-x), *Intel EM64T* (Intel 64) y *Execute Disable Bit* (XD), mientras que los procesadores AMD deben soportar *AMD Virtualization* (AMD-V).

Verificar si el sistema cuenta con el hardware necesario para dar soporte a la virtualización se deberá ejecutar el siguiente comando y obtener un valor mayor a cero.

```
$ egrep -c '(vmx|svm)' /proc/cpuinfo

4
``` 

Verificar si el sistema es capaz de correr máquinas virtuales KVM aceleradas por hardware.

```
$ sudo apt-get install cpu-checker

$ kvm-ok

INFO: /dev/kvm exists
KVM acceleration can be used

```

## Procedimiento

Instalar los paquetes mínimos requeridos por KVM.  Se recomienda adicionalmente la instalación de `virt-manager` para la gestión gráfica de las máquinas virtuales.

```
$ sudo apt-get install qemu-kvm libvirt-daemon-system libvirt-clients bridge-utils ia32-libs
```

Inscribir a los usuarios (en este caso el usuario en sesión) que estarán encargados de gestionar las máquinas virtuales en los grupos `kvm` y `libvirt`.

```
$ sudo adduser `id -un` libvirt
$ sudo adduser `id -un` kvm
```

Es necesario cerrar y volver a iniciar sesión de usuario para que el cambio de haber sido agregado a los grupos tenga efecto o ejecutar los siguientes comandos para obtener tal efecto en la sesión actual del *shell*.


```
$ newgrp libvirt

$ newgrp kvm
```

Verificar que la instalación ha sido exitosa.

```
$ virsh list --all

 Id   Name   State
--------------------

```

## Recursos

 * Configure hardware acceleration for the Android Emulator  
   [https://developer.android.com/studio/run/emulator-acceleration?utm_source=android-studio#vm-linux](https://developer.android.com/studio/run/emulator-acceleration?utm_source=android-studio#vm-linux)
 * KVM/Installation  
   [https://help.ubuntu.com/community/KVM/Installation](https://help.ubuntu.com/community/KVM/Installation)