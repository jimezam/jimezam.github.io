---
layout: post
title: Instalación de Processing 3.x en Linux Ubuntu/Mint 20
date: 2020-06-30 15:25 -0500
categories: Processing
tags: linux processing installation gnome cinnamon development
---

![Processing.org](/assets/images/2020-06-30/Processing.png "Processing.org"){:width="100%" style='margin:auto; display:block'}

## Introducción

En esta publicación se describen los pasos que sigo para instalar [Processing](https://processing.org/) en los escritorios *Cinnamon* y *GNOME* de Linux.

El procedimiento incluye tres pasos generales.

 1. Obtener e instalar el software.
 1. Crear el ícono del acceso directo para que Processing aparezca como programa en el menú.
 1. Asociar la extensión `.pde` con Processing para que se puedan abrir los códigos fuente haciendo doble clic sobre los archivos.

## Procedimiento

### Obtener e instalar el software

Descargar la última versión disponible del [sitio de descargas](https://processing.org/download/) en [Processing.org](https://processing.org/).

```
$ wget https://download.processing.org/processing-3.5.4-linux64.tgz
```

Descomprimir el paquete recién descargado.

```
$ tar zxvf processing-3.5.4-linux64.tgz
```

Crear un directorio donde se almacenarán las diferentes versiones de Processing instaladas (si no existe previamente).

```
$ mkdir ~/processing
```

Ubicar la nueva versión de Processing en el directorio recién creado.

```
$ mv processing-3.5.4 ~/processing/3.5.4
```

Remover el enlace a la versión actual (si existe actualmente).

```
rm ~/processing/current
```

Crear el nuevo enlace a la versión actual de Processing, la cual corresponderá con la que se está instalando.

```
$ ln -s ~/processing/3.5.4 ~/processing/current
```

Remover, si se desea, el paquete descargado.

```
$ rm processing-3.5.4-linux64.tgz
```

### Crear el ícono del acceso directo

Crear el archivo `processing.desktop` en el directorio `/usr/share/applications` con el contenido mostrado a continuación.

```
$ sudo vi /usr/share/applications/processing.desktop

[Desktop Entry]
Name=Processing
Exec=/home/educacion/processing/current/processing
Icon=/home/educacion/processing/current/lib/icons/pde-256.png
GenericName=Processing language
Comment=Integrated Development Environment
Terminal=false
Type=Application
Keywords=development;Processing;Java;IDE;platform;javase;
Categories=Development;IDE;Processing;Java;
StartupNotify=true
Encoding=UTF-8
Version=1.0
MimeType=text/x-processing
```

Deben ajustarse las rutas especificadas para las variables `Exec` y `Icon` de acuerdo con las ubicaciones utilizadas durante la instalación del software en la sección anterior.

### Asociar la extensión .pde con Processing

Crear el tipo MIME `text/x-processing` para los archivos de código fuente escritos en Processing (extensión `.pde`).

```xml
$ sudo vi /usr/share/mime/packages/processing.xml

<?xml version="1.0" encoding="UTF-8"?>
<mime-info xmlns="http://www.freedesktop.org/standards/shared-mime-info">
    <mime-type type="text/x-processing">
        <comment>Processing PDE sketch file</comment>
        <sub-class-of type="text/x-csrc"/>
        <glob pattern="*.pde"/>
    </mime-type>
</mime-info>
```

Agregar el nuevo tipo MIME a la base de datos (este paso puede tomar algunos segundos).

```
$ sudo update-mime-database /usr/share/mime
```

Finalmente se debe asociar el tipo MIME de Processing con el respectivo acceso directo de la aplicación que se encargará de abrirlo.  Para hacer esto, agregar al final del archivo `/usr/share/applications/defaults.list` la línea mostrada a continuación.

```
$ sudo vi /usr/share/applications/defaults.list

(al final)
text/x-processing=processing.desktop
```

## Recursos

 1. Installing Processing 3.x on Ubuntu Linux Systems Tutorial  
    [http://www.artsnova.com/processing/installing-processing-ubuntu-linux-tutorial.html](http://www.artsnova.com/processing/installing-processing-ubuntu-linux-tutorial.html)
 1. Adding MIME types en GNOME Developer  
    [https://developer.gnome.org/integration-guide/stable/mime.html.en](https://developer.gnome.org/integration-guide/stable/mime.html.en)