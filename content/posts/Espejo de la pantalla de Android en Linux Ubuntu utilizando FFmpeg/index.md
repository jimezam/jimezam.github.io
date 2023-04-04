---
title: "Espejo de la pantalla de Android en Linux Utilizando FFmpeg"
date: 2020-06-20T18:55:00-0500 
draft: false
type: "post"
showTableOfContents: true
tags: ["linux", "android", "ffmpeg", "mobile"]
# description: "A blog post"
# image: "/path/to/image.png"
---

<!--
https://jekyllrb.com/docs/configuration/markdown/
https://kramdown.gettalong.org/syntax.html
-->

# Introducción

{{< figure src="Android_Launcher.png#floatright" width="130" alt="Launcher de Android" >}}

En esta publicación se describen los pasos necesarios para visualizar (*mirror*) la pantalla de un teléfono Android conectado a través del puerto USB a un computador con Linux Ubuntu.  Esta visualización es de sólo lectura, es decir, no permite interactuar con el teléfono desde el computador, solamente visualizar lo que en un momento determinado se encuentre desplegando en su pantalla.

# Prerequisitos

Se requiere la instalación del `Android Debug Bridge (adb)`, `FFmpeg` y de un cliente de video, ya sea `ffplay` (incluido en el paquete anterior) o `mplayer`.

```
$ sudo apt install android-tools-adb ffmpeg mplayer
```

Se debe iniciar el servidor de ADB.

```
$ adb start-server
```

Se debe activar la opción de *Depuración por USB* del teléfono Android a través del menú de *Herramientas del desarrollador*. 

{{< figure src="USB debugging menu option.png#floatright" alt="Depuración por USB" width="350" >}}

Se debe conectar el teléfono al computador a través del cable USB y finalmente se debe verificar que el computador ha reconocido al teléfono conectado.

```
$ adb devices

List of devices attached
ce080808fcd01a0900	device
```

# Procedimiento

La ejecución el comando se realiza de la siguiente manera.

```
$ adb shell "while true; do screenrecord --output-format=h264 --size=540x960 -; done" | ffplay -framerate 60 -probesize 32 -sync video -
```

O si se desea utilizar `mplayer` en lugar de `ffplay` sería de la siguiente forma.

```
$ adb shell "while true; do screenrecord --output-format=h264 --size=540x960 -; done" | mplayer -framedrop -fps 600 -demuxer h264es - 
```

Una copia de este *script* puede obtenerse mediante a través de este [gist](https://gist.github.com/jimezam/d64676aec76fe374e7809453cb47d292).
