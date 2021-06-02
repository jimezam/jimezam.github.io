---
layout: post
title: Instalar Anaconda en Ubuntu 21
date: 2021-06-01 19:24 -0500
categories: Python
tags: linux ubuntu python anaconda conda
---

## Introducción

A continuación se describen los pasos necesarios para instalar la versión gratuita de Anaconda en Linux Ubuntu 21 y distribuciones derivadas.

## Instalación de los paquetes requeridos

Garantizar que se cuenta con los paquetes requeridos.

```
$ sudo apt-get install libgl1-mesa-glx libegl1-mesa libxrandr2 libxrandr2 libxss1 libxcursor1 libxcomposite1 libasound2 libxi6 libxtst6
```
## Descarga del paquete de instalación

Obtener el URL de la última versión disponible en la [página de descargas](https://www.anaconda.com/products/individual#Downloads).  En este caso, se está descargando la versión `2021.05` para Linux de 64 bits.

```
$ wget https://repo.anaconda.com/archive/Anaconda3-2021.05-Linux-x86_64.sh
```

## Verificación del paquete de instalación (opcional)

Para verificar el paquete que se descargó en el paso anterior, es necesario calcular su *checksum* de verificación de la siguiente manera reemplazando adecuadamente su nombre.

```
$ sha256sum Anaconda3-2021.05-Linux-x86_64.sh
```

Este comando generará un valor del *hash* que debe compararse con el especificado en la [página de hashes](https://docs.anaconda.com/anaconda/install/hashes/) de acuerdo con la versión de Python, el sistema operativo y la arquitectura utilizada.

## Instalación del software

El proceso consiste en ejecutar el *script* de *shell* recién descargado y responder a las preguntas que se muestran a continuación.

```
$ bash ./Anaconda3-2021.05-Linux-x86_64.sh
```
```
In order to continue the installation process, please review the license
agreement.
Please, press ENTER to continue
>>> 
===================================
End User License Agreement - Anaconda Individual Edition
===================================
```

```
Do you accept the license terms? [yes|no]
[no] >>> yes
```

```
Anaconda3 will now be installed into this location:
/home/educacion/anaconda3

  - Press ENTER to confirm the location
  - Press CTRL-C to abort the installation
  - Or specify a different location below

[/home/educacion/anaconda3] >>> 
```

```
Do you wish the installer to initialize Anaconda3
by running conda init? [yes|no]
[no] >>> yes
```

```
==> For changes to take effect, close and re-open your current shell. <==

If you'd prefer that conda's base environment not be activated on startup, 
   set the auto_activate_base parameter to false: 

conda config --set auto_activate_base false

- Close and open your terminal window for the installation to take effect, or you can enter the command source ~/.bashrc.
```

## Verificación de la instalación

```
$ conda --version

conda 4.10.1

$ anaconda-navigator --version

2.0.3
```

## Actualización del software

`conda` es el gestor de paquetes que utiliza internamente Anaconda, a través de él mismo puede ser actualizado el software mediante los siguientes comandos.

```
$ conda update conda

$ conda update anaconda
```

## Recursos

- Installing on Linux  
  https://docs.anaconda.com/anaconda/install/linux/
- Verifying your installation  
  https://docs.anaconda.com/anaconda/install/verify-install/
- Anconda Installers  
  https://www.anaconda.com/products/individual#Downloads