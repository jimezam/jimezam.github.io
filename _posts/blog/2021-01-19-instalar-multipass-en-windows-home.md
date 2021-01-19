---
layout: post
title: Instalar Multipass en Windows Home
date: 2021-01-19 10:38 -0500
categories: Linux
tags: virtualization linux ubuntu windows multipass
---

## Introducción

La instalación de Multipass en Windows es bastante simple, sin embargo si se cuenta con la versión Home se deben realizar un par de pasos adicionales ya que esta versión carece del sistema de virtualización Hyper-V.

Existen dos alternativas para instalar Multipass en Windows Home:

1. Instalar manualmente Hyper-V.
2. Instalar Multipass con soporte para Virtualbox.

En esta publicación se describirá el proceso para realizar la segunda opción.

## Requerimientos previos

Antes de iniciar el proceso de instalación de Multipass se debe instalar el siguiente software.

1. VirtualBox para Windows (*Windows hosts*)  
   [https://www.virtualbox.org/wiki/Downloads](https://www.virtualbox.org/wiki/Downloads)
2. VirtualBox Extension Pack (*All supported platforms*)  
   [https://www.virtualbox.org/wiki/Downloads](https://www.virtualbox.org/wiki/Downloads)

La instalación del *Extension Pack* se describe en este [documento](https://www.nakivo.com/blog/how-to-install-virtualbox-extension-pack/).

## Procedimiento

Descargar e instalar la última versión disponible de Multipass del siguiente repositorio.

* [https://github.com/canonical/multipass/releases/](https://github.com/canonical/multipass/releases/)

Activar VirtulBox con sistema de virtualización para Multipass (este paso sólo es requerido para Windows Home).

Para hacer esto se debe abrir el *Símbolo del sistema* (`cmd`) en **modo administrador** y ejecutar el siguiente comando.

```
> multipass set local.driver=virtualbox
```

## Recursos

* Installing Multipass for Windows  
  [https://multipass.run/docs/installing-on-windows](https://multipass.run/docs/installing-on-windows)