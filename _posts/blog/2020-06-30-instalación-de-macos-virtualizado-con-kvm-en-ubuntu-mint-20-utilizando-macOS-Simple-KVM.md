---
layout: post
title: Instalación de MacOS virtualizado con KVM en Ubuntu/Mint 20 utilizando macOS-Simple-KVM
date: 2020-06-30 02:26 -0500
categories: MacOS
tags: macos virtualization kvm qemu linux
---

![MacOS Catalina](/assets/images/2020-06-30/MacOS - Catalina - KVM.png "MacOS Catalina"){:width="100%" style='margin:auto; display:block'}

## Introducción

En esta publicación se describen los pasos necesarios para instalar distintas versiones de MacOS en una máquina virtual KVM en Linux Ubuntu/Mint 20 con el fin de explorar las capacidades de virtualización del sistema operativo.

Para esto se utilizan los *scripts* provistos por el proyecto [macOS-Simple-KVM](https://github.com/foxlet/macOS-Simple-KVM.git), los cuales facilitan en gran medida este proceso.

## Requerimientos

* Procesador Intel o AMD con capacidad de virtualización (SSE 4.2).  Se sugiere como un Ivy Bridge (o posterior), Xeon o Ryzen.
* Se requiere espacio en disco suficiente para almacenar la distribución de MacOS (~500MB comprimidos, 2GB descomprimidos), los archivos de la instalación (~6.5GB) y el disco duro que se cree para la máquina virtual (mínimo 20GB pero se recomiendan 64GB o más).

## Procedimiento

Instalar los paquetes necesarios del sistema operativo.

```
$ sudo apt-get install qemu-system qemu-utils python3 python3-pip
```

Clonar el repositorio de `macOS-Simple-KVM` el cual incluye los *scripts* de apoyo para realizar la instalación.

```
$ git clone https://github.com/foxlet/macOS-Simple-KVM.git

$ cd macOS-Simple-KVM/
```

Descargar los medios de instalación de MacOS, las opciones disponibles son `--high-sierra`, `--mojave` y `--catalina` además del reciente `--big-sur` el cual aún no está del todo disponible (ver [issue #1](https://github.com/foxlet/macOS-Simple-KVM/issues/250) y [issue #2](https://github.com/CloverHackyColor/CloverBootloader/issues/190)).

```
$ ./jumpstart.sh --catalina
```

Crear un disco duro virtualizado en el cual se instalará el sistema operativo y los archivos del usuario.  Ajustar el tamaño como se considere conveniente.

```
$ qemu-img create -f qcow2 MyDisk.qcow2 64G
```

Agregar el disco duro recién creado a la configuración de la máquina virtual, para hacer esto se deben agregar las siguientes líneas al final del archivo `basic.sh`.

```
$ vi basic.sh

    -drive id=SystemDisk,if=none,file=MyDisk.qcow2 \
    -device ide-hd,bus=sata.4,drive=SystemDisk \
```

Iniciar la máquina virtual para proceder con la instalación del sistema operativo en la misma.

```
$ ./basic.sh
```

## Recursos

 1. Proyecto macOS-Simple-KVM  
    [https://github.com/foxlet/macOS-Simple-KVM.git](https://github.com/foxlet/macOS-Simple-KVM.git)
 1. FAQ en macOS-Simple-KVM  
    [https://github.com/foxlet/macOS-Simple-KVM/blob/master/docs/FAQs.md](https://github.com/foxlet/macOS-Simple-KVM/blob/master/docs/FAQs.md)
 1. How To run macOS on KVM / QEMU  
    [https://computingforgeeks.com/how-to-run-macos-on-kvm-qemu/](https://computingforgeeks.com/how-to-run-macos-on-kvm-qemu/)
 1. 🤜 macOS-Simple-KVM 🤛  
    [https://www.youtube.com/watch?v=9vuLDVgeJKQ](https://www.youtube.com/watch?v=9vuLDVgeJKQ)
 1. KVM en ArchLinux  
    [https://wiki.archlinux.org/index.php/KVM](https://wiki.archlinux.org/index.php/KVM)
 1. QEMU en ArchLinux  
    [https://wiki.archlinux.org/index.php/QEMU](https://wiki.archlinux.org/index.php/QEMU)
 1. PCI passthrough via OVMF  
    [https://wiki.archlinux.org/index.php/PCI_passthrough_via_OVMF](https://wiki.archlinux.org/index.php/PCI_passthrough_via_OVMF)
 1. KVM Host/Guest Performance Optimizations  
    [https://nixhaven.com/index.php?threads/kvm-host-guest-performance-optimizations.14/](https://nixhaven.com/index.php?threads/kvm-host-guest-performance-optimizations.14/)