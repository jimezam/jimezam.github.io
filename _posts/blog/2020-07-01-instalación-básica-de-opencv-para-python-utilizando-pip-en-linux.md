---
layout: post
title: Instalación básica de OpenCV para Python utilizando pip en Linux
date: 2020-07-01 15:55 -0500
categories: Python
tags: linux windows development python pip opencv 
---

![OpenCV en Ubuntu/Mint](/assets/images/2020-07-01/opencv-ubuntu.png "OpenCV en Ubuntu/Mint"){:height="80%" style='margin:auto; display:block'}

## Introducción

En esta publicación se describen los pasos necesarios para instalar OpenCV para Python 3 en Linux Ubuntu/Mint 20 utilizando `pip`.

Estos paquetes se consideran no oficiales ya que desde [OpenCV.org](https://opencv.org/releases/) se pueden descargar las fuentes del proyecto para su compilación personalizada.  A través de `pip` básicamente se pueden obtener cuatro versiones diferentes.

 - `opencv-python`: sólo incluye los módulos principales.
 - `opencv-contrib-python`: incluye módulos no libres además de los módulos principales. **(Recomendado)**
 - `opencv-python-headless`: sólo módulos principales pero sin funcionalidades de interfaz de usuario.
 - `opencv-contrib-python-headless`: módulos principales y módulos no libres sin funcionalidades de interfaz de usuario.

La versión recomendada es `opencv-contrib-python` ya que incluye la mayor cantidad de algoritmos disponibles para su uso.

## Procedimiento

Instalar Python 3.

```
$ sudo apt-get install python3
```

Instalar `pip`.

```
$ sudo apt-get install python3-pip
```

Instalar la librería de OpenCV.

```
$ pip3 install opencv-contrib-python
```

Instalar NumPY (opcional pero recomendado).

```
$ pip3 install numpy
```

## Verificación

```
$ python3 --version

Python 3.8.2
```

```
$ pip3 --version

pip 20.0.2 from /usr/lib/python3/dist-packages/pip (python 3.8)
```

```
$ python3 -c "import cv2; print(cv2.__version__)"

4.2.0
```

```
$ python3 -c "import numpy as np; print(np.__version__)"

1.19.0
```

## Recursos

 - OpenCV en pip  
   [https://pypi.org/project/opencv-contrib-python/](https://pypi.org/project/opencv-contrib-python/)
 - NumPY en pip  
   [https://pypi.org/project/numpy/](https://pypi.org/project/numpy/)
