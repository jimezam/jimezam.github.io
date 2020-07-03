---
layout: post
title: Leer, mostrar y guardar imágenes con OpenCV en Python
date: 2020-07-02 23:55 -0500
categories: Python
tags: linux windows development python opencv image
---

## Introducción

A continuación se describe el proceso en Python utilizando OpenCV necesario para leer y guardar imágenes.

![Imagen a color y monocromática](/assets/images/2020-07-03/color-mono.png "Imagen a color y monocromática"){:width="66%" style='margin:auto; display:block'}

Para revisar la versión completa del código, revise su correspondiente [repositorio](https://github.com/jimezam/Examples-of-OpenCV-with-Python/tree/master/1.%20Load%20and%20save%20images).

## Leer imágenes de disco

{% highlight python %}
image = cv2.imread(filename, cv2.IMREAD_UNCHANGED)
{% endhighlight %}

La función `imread` recibe como segundo parámetro el modo con el cual se leerá la imagen.

{% highlight python %}
enum cv::ImreadModes {
  cv::IMREAD_UNCHANGED = -1,
  cv::IMREAD_GRAYSCALE = 0,
  cv::IMREAD_COLOR = 1,
  cv::IMREAD_ANYDEPTH = 2,
  cv::IMREAD_ANYCOLOR = 4,
  cv::IMREAD_LOAD_GDAL = 8,
  cv::IMREAD_REDUCED_GRAYSCALE_2 = 16,
  cv::IMREAD_REDUCED_COLOR_2 = 17,
  cv::IMREAD_REDUCED_GRAYSCALE_4 = 32,
  cv::IMREAD_REDUCED_COLOR_4 = 33,
  cv::IMREAD_REDUCED_GRAYSCALE_8 = 64,
  cv::IMREAD_REDUCED_COLOR_8 = 65,
  cv::IMREAD_IGNORE_ORIENTATION = 128
}
{% endhighlight %}

## Calcular la versión monocromática de una imagen

{% highlight python %}
gray = cv2.cvtColor(image, cv2.COLOR_BGR2GRAY)
{% endhighlight %}

## Mostrar imágenes

{% highlight python %}
cv2.imshow("Colored Image", image)

cv2.imshow("Monochrome Image", gray)
{% endhighlight %}

## Guardar imágenes

{% highlight python %}
cv2.imwrite("mono.png", gray)
{% endhighlight %}

## Cerrar todas las ventanas

{% highlight python %}
cv2.waitKey(0)

cv2.destroyAllWindows()
{% endhighlight %}

## Recursos

 1. OpenCV modules  
 [https://docs.opencv.org/master/](https://docs.opencv.org/master/)
 1. Image file reading and writing  
 [https://docs.opencv.org/master/d4/da8/group__imgcodecs.html](https://docs.opencv.org/master/d4/da8/group__imgcodecs.html)
 1. High-level GUI  
 [https://docs.opencv.org/master/d7/dfc/group__highgui.html](https://docs.opencv.org/master/d7/dfc/group__highgui.html)