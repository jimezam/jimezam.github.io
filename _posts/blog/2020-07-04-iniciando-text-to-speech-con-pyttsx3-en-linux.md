---
layout: post
title: Iniciando text-to-speech con pyttsx3 en Linux
date: 2020-07-04 23:45 -0500
categories: Python
tags: linux windows macos development python pyttsx3 text-to-speech
---

## Introducción

pyttsx3 es una librería de Python 3 que permite la realización de *text-to-speech* (conversión de texto a voz) de manera local (*offline*) y portable entre diferentes idiomas y [plataformas](https://pyttsx3.readthedocs.io/en/latest/support.html).

 - Linux (eSpeak)
 - Windows (SAPI5)
 - MacOS (NSSpeechSynthesizer)
 - Android (eSpeak)
 
Es posible consultar el código fuente completo de este ejemplo en su correspondiente [respositorio](https://github.com/jimezam/Examples-of-Python/blob/master/pyttsx3%2C%20basics/pyttsx3%20hello.py).

## Instalacion

Bajo Linux requiere del soporte de eSpeak, el cual se instala de la siguiente manera.

```
$ sudo apt-get update && sudo apt-get install espeak speech-dispatcher-espeak
```

Posteriormente se instala la librería que recubre el motor tts bajo Python 3.

```
pip3 install pyttsx3
```

### Alternativa a eSpeak en Linux

Existe un *fork* del proyecto eSpeak llamado [eSpeak-ng](https://github.com/espeak-ng/espeak-ng/) que sugiere tener mejoras sobre el proyecto original.  Este sintetizador puede instalarse como se muestra a continuación.

```
$ sudo apt-get install espeak-ng espeak-ng-espeak libespeak-ng-libespeak1
```

Los paquetes `espeak-ng-espeak` y `libespeak-ng-libespeak1` proveen compatibilidad de aplicaciones y librerías respectivamente con el proyecto eSpeak.

## Hola Mundo

Importar la librería pyttsx3 que recubre al sintetizador de tts.

{% highlight python %}
import pyttsx3
{% endhighlight %}

Instanciar el motor de tts.  El método `init` recibe como argumento una cadena que define el tipo de sintetizador a utilizar de acuerdo con la plataforma en la que se va a ejecutar la aplicación: vacío (por defecto), `sapi5` para Windows, `nsss` para MacOS y `espeak` para Linux y Android.

{% highlight python %}
engine = pyttsx3.init()
{% endhighlight %}

Especificar el mensaje de texto que se va a reproducir con voz.

{% highlight python %}
engine.say("¡Hola a todos! ¿cómo están?")
{% endhighlight %}

Esperar a que se termine la ejecución de las tareas del motor.

{% highlight python %}
engine.runAndWait()
{% endhighlight %}

## Recursos

 - pyttsx3 (offline TTS or Text To Speech converter for Python 3)  
   [https://github.com/nateshmbhat/pyttsx3](https://github.com/nateshmbhat/pyttsx3)
 - Documentación oficial de pyttsx3  
   [http://pyttsx3.readthedocs.io/](http://pyttsx3.readthedocs.io/)
 - pyttsx3 en pip  
   [https://pypi.org/project/pyttsx3/](https://pypi.org/project/pyttsx3/)
 - Sitio oficial de eSpeak  
   [http://espeak.sourceforge.net/](http://espeak.sourceforge.net/)
 - Instalación de pyttsx bajo Windows  
   [https://carlosjuliopardoblog.wordpress.com/2018/05/14/motores-de-voz/](https://carlosjuliopardoblog.wordpress.com/2018/05/14/motores-de-voz/)
