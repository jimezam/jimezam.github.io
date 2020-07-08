---
layout: post
title: Activar la Snap Store en Linux Mint 20
date: 2020-07-07 00:57 -0500
categories: Linux
tags: linux mint snap
---

## Introducción

A partir de la versión 20.0, Linux Mint decidió no incluir por defecto la gestión de paquetes Snap promocionado por Canonical debido a diferencias conceptuales y posibles riesgos de seguridad.

Snap, a diferencia de otros tipos de repositorios como APT o Flatpack, depende únicamente de la *Snap Store*, no es posible agregar otras fuentes de paquetes ni medios de autenticación.

> This is a store we can’t audit, which contains software nobody can patch. If we can’t fix or modify software, open-source or not, it provides the same limitations as proprietary software.

Adicionalmente acusan a Canonical de haber roto la promesa de no reemplazar paquetes APT por paquetes Snap (ver caso [Chromium](https://linuxmint-user-guide.readthedocs.io/en/latest/chromium.html)), que inclusive ejecutan comandos como *root* sin consentimiento del usuario y realizan conexiones con la tienda operada de manera privada por Canonical.

A continuación se detalla el procedimiento necesario para activar la gestión de paquetes Snap bajo Linux Mint 20 o posterior.  Realice este procedimiento bajo su propio riesgo, considere el uso de otras fuentes de paquetes como APT y Flatpack.

## Procedimiento

Remover el archivo bandera del bloqueo a la *Snap Store*.

```
$ sudo rm /etc/apt/preferences.d/nosnap.pref
```

Actualizar el índice de paquetes disponibles.

```
$ sudo apt update
```

Instalar el paquete correspondiente a la *Snap Store*.

```
$ sudo apt install snapd
```

## Recursos

 - Snap Store en Linux Mint User Guide  
   [https://linuxmint-user-guide.readthedocs.io/en/latest/snap.html](https://linuxmint-user-guide.readthedocs.io/en/latest/snap.html)
 - Monthly News – May 2020 en el blog de Linux Mint  
   [https://blog.linuxmint.com/?p=3906](https://blog.linuxmint.com/?p=3906)
 - Monthly News – June 2020 en el blog de Linux Mint  
   [https://blog.linuxmint.com/?p=3766](https://blog.linuxmint.com/?p=3766)