---
layout: post
title: "'Unable to watch for file changes in this large workspace' en Visual Studio Code bajo Linux"
date: 2020-08-12 21:57 -0500
categories: Linux
tags: linux ubuntu mint visualstudio code
---

![Mensaje de advertencia](/assets/images/2020-08-12/error_visualtudio_code.png "Mensaje de advertencia"){:width="100%" style=''}

## Descripción del problema

Al iniciar Visual Studio Code bajo Linux aparece un mensaje de advertencia con el texto: ``Unable to watch for file changes in this large workspace.``.

## Solución propuesta

La solución de esta situación se basa en ampliar la cantidad de *detectores* (*watchers*) que Inotify puede disponer para detectar cambios en el sistema de archivos.

Para lograr esto, es necesario editar el archivo ``/etc/sysctl.conf`` y agregar o ajustar la variable ``fs.inotify.max_user_watches``.

```
$ sudo vi /etc/sysctl.conf

    ...
    fs.inotify.max_user_watches=524288
```

Actualiza la información de los parámetros del Kernel de acuerdo con lo encontrado en el archivo de configuración.

```
$ sudo sysctl -p
```

La efectividad de esta modificación puede verificarse comparando la cantidad de *watchers* obtenida con el siguiente comando.

```
$ cat /proc/sys/fs/inotify/max_user_watches
```

## Recursos

 - Visual Studio Code is unable to watch for file changes in this large workspace" (error ENOSPC)  
   [https://github.com/flathub/com.visualstudio.code/issues/29#issuecomment-477126012](https://github.com/flathub/com.visualstudio.code/issues/29#issuecomment-477126012)
 - Increasing the amount of inotify watchers  
   [https://github.com/guard/listen/wiki/Increasing-the-amount-of-inotify-watchers](https://github.com/guard/listen/wiki/Increasing-the-amount-of-inotify-watchers)
