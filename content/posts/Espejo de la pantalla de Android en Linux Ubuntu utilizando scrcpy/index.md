---
title: "Espejo de la pantalla de Android en Linux Ubuntu utilizando scrcpy"
date: 2020-07-21T11:52:00-0500
draft: false
type: "post"
showTableOfContents: true
tags: ["linux", "windows", "macos", "android", "scrcpy", "mobile"]
# description: "A blog post"
# image: "/path/to/image.png"
---

# Introducción

{{< figure src="scrcpy-screenshot.png#floatright" width="200" alt="Pantalla en espejo de un teléfono Android" >}}

`scrcpy` es una herramienta hecha por los mismos creadores de Genymobile y disponible para Linux, Windows y MacOS que permite visualizar desde el PC la pantalla del teléfono móvil que se encuentre conectado, de manera similar a como se experimentó [anteriormente con FFMpeg]({% post_url blog/2020-06-20-espejo-de-la-pantalla-de-Android-en-linux-ubuntu-utilizando-ffmpeg %}).  Adicionalmente este software incluye la posibilidad de interactúar con el teléfono desde el PC y la posibilidad de copiar & pegar.

Según los autores, los objetivos de diseño de la aplicación son los siguientes.
 
 - Liviano.
 - Desempeño (30~60fps).
 - Calidad (1920×1080 o superior).
 - Baja latencia (35~70ms).
 - Corto tiempo de inicio (~1 segundo para mostrar la primer imagen).
 - No intrusivo (no queda nada instalado en el teléfono).

# Prerequisitos

Es necesario contar con la siguiente configuración del teléfono.

 1. El teléfono debe tener por lo menos Android 5.0 (API 21) como sistema operativo.
 1. Activar *Depuración por USB* (*USB debugging*) bajo las *Developer Options*.  Hacer esto __antes__ de conectar el teléfono al PC.
 1. Aceptar en el teléfono la conexión desde el PC.

 Si esto se ha realizado exitosamente, el teléfono empieza a ser visto desde el PC.

```
$ adb devices

List of devices attached
ce091929fcde1a1804	device
```

Si aparece el mensaje `unauthorized` significará que aún no ha sido aprobado el acceso al teléfono a través de la conexión.

# Instalación

Para Ubuntu/Mint la instalación puede realizarse a través de dos diferentes tipos de medios. APT:

```
$ sudo apt install scrcpy
```

Y Snap:

```
$ sudo snap install scrcpy
```

La versión disponible en el repositorio de Snap parece estar mas actualizada.

# Forma de uso

Para utilizar la aplicación sólo es necesario ejecutarla.

```
$ scrcpy
```

Esta aplicación recibe varias opciones interesantes para ajustar su funcionamiento.  A continuación se describen algunas de las opciones mas útiles.

## Grabar la pantalla

| Opción | Descripción |
| --- | --- |
| `--record` file.mp4 | Graba en video lo mostrado en la pantalla del móvil.  El formato es determinado por la opción `--record-format` o la extensión del archivo destino |
| `--record-format` format | Determina el formato de la grabación, puede ser `mp4` o `mkv`. |
| `--no-display` | Desactiva la pantalla del teléfono (en conjunto con `--record`) |

## Configurar recursos

| Opción | Descripción |
| --- | --- |
| `--max-size` 1024 | Determina el tamaño de la imagen manteniendo automáticamente la proporción de la imagen.  (0 sin límite) |
| `--bit-rate` 2M | Tasa de bits transmitidos.  Por defecto 8M. |
| `--max-fps` 15 | Máximo permitido de *frames* por segundo. |
| `--render-driver` name | Solicita a SDL el uso de un *driver* específico.  Las [opciones disponibles](https://wiki.libsdl.org/SDL_HINT_RENDER_DRIVER) son `direct3d`, `opengl`, `opengles2`, `opengles`, `metal` y `software`. |

## Configurar orientación

| Opción | Descripción |
| --- | --- |
| `--rotation` value | Determina el estado inicial de rotación.  Los valores disponibles son `0` (rotación natural), `1` (90°), `2` (180°) y `3` (-90°) |
| `--lock-video-orientation` value | Bloquea la pantalla con una rotación específica. Los valores disponibles son `0`, `1`, `2` y `3`, cada valor aumenta 90° en contra de las manecillas del reloj.  Por defecto es `-1` (sin rotación). |

## Configurar visualización

| Opción | Descripción |
| --- | --- |
| `--window-title` 'My device' | Determina el titulo |
| `--window-x` 100 | Establece el valor inicial para la posición horizontal de la ventana.  Por defecto es "`auto`". |
| `--window-y` 100 | Establece el valor inicial para la posición vertical de la ventana.  Por defecto es "`auto`". |
| `--window-width` 800 | Establece el ancho inicial de la ventana.  Por defecto es `0`. |
| `--window-height` 600 | Establece el alto inicial de la ventana.  Por defecto es `0`. |
| `--always-on-top` | Evita que la ventana sea enviada hacia atrás por otra |
| `--fullscreen` | Inicia en modo *pantalla completa* |
| `--stay-awake` | Mantiene *siempre* encendida la pantalla |
| `--turn-screen-off` | Apaga la pantalla al inicio de la conexión | 

## Configurar interacción

| Opción | Descripción |
| --- | --- |
| `--no-control` | Desactiva la interacción con la pantalla desde el PC |
| `--show-touches` | Muestra la ubicación de los *taps* |

## Transferir archivos

Para transferir archivos (`adb push`) desde el PC hacia el teléfono sólo es necesario *arrastrar y soltar* el archivo sobre la ventana de la aplicación.

Si el archivo es un `*.apk`, éste se instalará en el teléfono, de lo contrario, el archivo se copiará en `/sdcard/`.

Es posible ajustar la ruta donde se almacenarán los archivos copiados (por defecto `/sdcard/`) a cualquiera bajo este directorio mediante el siguiente comando.

```
$ scrcpy --push-target /sdcard/foo/bar/
```

# Atajos de teclado

| Atajo | Linux | MacOS |
| --- | --- | --- |
 | Switch fullscreen mode                      | `Ctrl`+`f`                    | `Cmd`+`f`
 | Rotate display left                         | `Ctrl`+`←` _(left)_           | `Cmd`+`←` _(left)_
 | Rotate display right                        | `Ctrl`+`→` _(right)_          | `Cmd`+`→` _(right)_
 | Resize window to 1:1 (pixel-perfect)        | `Ctrl`+`g`                    | `Cmd`+`g`
 | Resize window to remove black borders       | `Ctrl`+`x` \| _Double-click¹_ | `Cmd`+`x`  \| _Double-click¹_
 | Click on `HOME`                             | `Ctrl`+`h` \| _Middle-click_  | `Ctrl`+`h` \| _Middle-click_
 | Click on `BACK`                             | `Ctrl`+`b` \| _Right-click²_  | `Cmd`+`b`  \| _Right-click²_
 | Click on `APP_SWITCH`                       | `Ctrl`+`s`                    | `Cmd`+`s`
 | Click on `MENU`                             | `Ctrl`+`m`                    | `Ctrl`+`m`
 | Click on `VOLUME_UP`                        | `Ctrl`+`↑` _(up)_             | `Cmd`+`↑` _(up)_
 | Click on `VOLUME_DOWN`                      | `Ctrl`+`↓` _(down)_           | `Cmd`+`↓` _(down)_
 | Click on `POWER`                            | `Ctrl`+`p`                    | `Cmd`+`p`
 | Power on                                    | _Right-click²_                | _Right-click²_
 | Turn device screen off (keep mirroring)     | `Ctrl`+`o`                    | `Cmd`+`o`
 | Turn device screen on                       | `Ctrl`+`Shift`+`o`            | `Cmd`+`Shift`+`o`
 | Rotate device screen                        | `Ctrl`+`r`                    | `Cmd`+`r`
 | Expand notification panel                   | `Ctrl`+`n`                    | `Cmd`+`n`
 | Collapse notification panel                 | `Ctrl`+`Shift`+`n`            | `Cmd`+`Shift`+`n`
 | Copy device clipboard to computer           | `Ctrl`+`c`                    | `Cmd`+`c`
 | Paste computer clipboard to device          | `Ctrl`+`v`                    | `Cmd`+`v`
 | Copy computer clipboard to device and paste | `Ctrl`+`Shift`+`v`            | `Cmd`+`Shift`+`v`
 | Enable/disable FPS counter (on stdout)      | `Ctrl`+`i`                    | `Cmd`+`i`

_¹ Double-click on black borders to remove them._  
_² Right-click turns the screen on if it was off, presses BACK otherwise._


# Recursos

 1. `scrcpy` de Genymobile  
    [https://github.com/Genymobile/scrcpy](https://github.com/Genymobile/scrcpy)
