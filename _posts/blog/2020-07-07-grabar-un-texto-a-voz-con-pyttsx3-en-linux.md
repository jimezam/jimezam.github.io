---
layout: post
title: Grabar un texto a voz con pyttsx3 en Linux
date: 2020-07-07 00:57 -0500
categories: Python
tags: linux windows macos development python pyttsx3 text-to-speech mp3 wav
---

## Introducción

En esta publicación se describe el procedimiento necesario para almacenar en archivo de audio, la voz leyendo un texto específico utilizando pyttsx3.

Para acceder a la versión completa del código, consulte el [repositorio](https://github.com/jimezam/Examples-of-Python/blob/master/pyttsx3%2C%20basics/pyttsx3%20save%20speech.py) del mismo en el cual se puede apreciar además, el código necesario para obtener el texto a leer desde un archivo de texto plano.

 | **Advertencia** (07/07/2020). - la ejecución del método `save_to_file` parece no funcionar correctamente, algunas veces se genera el archivo de salida mientras que en otras no.  Consultar [incidencias](https://github.com/nateshmbhat/pyttsx3/issues) de la librería.

## Requerimientos

Bajo Linux se requiere contar con la instalación de [eSpeak]({% post_url 2020-07-04-iniciando-text-to-speech-con-pyttsx3-en-linux %}) y FFmpeg.

```
$ sudo apt install ffmpeg
```

## Incializar el motor

{% highlight python %}
import pyttsx3

engine = pyttsx3.init()
{% endhighlight %}

## Grabar voz a disco

{% highlight python %}
engine.save_to_file(message, 'output.mp3')
{% endhighlight %}

Es imperativo ejecutar el método `runAndWait`, de lo contrario es posible que la aplicación se termine antes de que finalice la ejecución del motor.

{% highlight python %}
engine.runAndWait()
{% endhighlight %}

## Recursos

 - Repositorio Github del proyecto pyttsx3  
   [https://github.com/nateshmbhat/pyttsx3](https://github.com/nateshmbhat/pyttsx3)
