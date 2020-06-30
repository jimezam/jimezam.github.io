---
layout: post
title:  "Problemas accediendo a la cámara web en Processing desde Linux"
date:   2020-06-10 18:55:00 -0500
categories: Development
tags: linux video processing v4l2src gstreamer
---

## Descripción del problema

Cuando se intentan acceder a la cámara web en Linux desde una aplicación en Processing, se obtiene el error `"No such Gstreamer factory: v4l2src"` y no es posible obtener imágenes de ella.

![Error de video en Processing](/assets/images/2020-06-10/error video processing.png "Error de video en Processing"){:width="100%" style=''}

Esto se debe a que la librería de `Video` que se descarga desde *Sketch > Import library...* requiere de versiones antiguas de los paqutes de la librería `GStreamer` que ya no se encuentran disponibles en las úlitmas versiones de distribuciones como Ubuntu y Mint.

## Solución propuesta

Descargar la última versión disponible de la librería de `Video` desde el repositorio oficial de la misma.

> [https://github.com/processing/processing-video/releases](https://github.com/processing/processing-video/releases)

Descomprimir el archivo obtenido (`video-x.y.zip`) y ubicar sus contenidos en el directorio `~/sketchbook/libraries` (o `%CSIDL_MYDOCUMENTS%\Processing\libraries` en Windows) reemplazando el contenido del directorio `video` que se encuentre allí previamente.

![Librería de video](/assets/images/2020-06-10/video library files.png "Librería de video"){:width="100%" style=''}

## Recursos

 1. Aplicación para acceder a la cámara web desde Processing  
    [https://gist.github.com/jimezam/ddb95cf57f19e1f42e5ffda5acec36b5](https://gist.github.com/jimezam/ddb95cf57f19e1f42e5ffda5acec36b5)
 1. Video by Daniel Shiffman  
    [https://processing.org/tutorials/video/](https://processing.org/tutorials/video/)
 1. Repositorio Git de la librería `Video` de Processing  
    [https://github.com/processing/processing-video](https://github.com/processing/processing-video)