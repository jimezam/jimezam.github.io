---
layout: post
title: Mostrar un video con OpenCV en Python
date: 2020-07-03 15:15 -0500
categories: Python
tags: linux windows development python opencv image video
---

## Introducción

A continuación se describe el proceso en Python utilizando OpenCV necesario para mostrar un video en pantalla, proveniente de un archivo o de una cámara web, y eventualmente procesar cada uno de sus fotogramas.

Para revisar la versión completa del código, revise su correspondiente [repositorio](https://github.com/jimezam/Examples-of-Python/blob/master/OpenCV%2C%20show%20video/show_video.py).  En esta versión se muestra el video a color y en blanco y negro, y permite el almacenamiento de los fotogramas cuando se presiona la letra 's'.

## Seleccionar el origen del video

El video puede provenir desde un archivo en un sistema de archivos local.

{% highlight python %}
capture = cv2.VideoCapture("/my/file/video.mp4")
{% endhighlight %}

O desde el flujo de imágenes de una videocámara conectada al computador.

{% highlight python %}
capture = cv2.VideoCapture(0)
{% endhighlight %}

Es prudente verificar que la interfaz de captura de video se haya podido establecer.

{% highlight python %}
if not capture.isOpened():
    print("ERROR, I could not establish video capture")
    exit()
{% endhighlight %}

## Captura de *frames*

{% highlight python %}
while True:
    (grabbed, frame) = capture.read()
{% endhighlight %}

`grabbed` es una variable booleana que indica si el *frame* pudo o no ser leído mientras que `frame` guarda la imagen efectivamente leída desde el video.

Es posible verificar si el video se ha terminado mediante el uso de `grabbed`.

{% highlight python %}
    if not grabbed:
        break
{% endhighlight %}

## Mostrar el *frame* del video

{% highlight python %}
    cv2.imshow("Video", frame)
{% endhighlight %}

## Liberar los recursos

Una vez finalizada la ejecución del video, se deberán liberar los recursos de la captura y las ventanas utilizadas.

{% highlight python %}
capture.release()

cv2.destroyAllWindows()
{% endhighlight %}
