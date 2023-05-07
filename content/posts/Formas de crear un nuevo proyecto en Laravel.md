---
title: "Formas De Crear Un Nuevo Proyecto en Laravel"
date: 2023-05-07T00:13:33-05:00
draft: false
type: "post"
showTableOfContents: true
tags: ["laravel", "composer", "php", "docker"]
# description: "A blog post"
# image: "/path/to/image.png"
---

# Introducción

# Instalar Laravel localmente

Consiste en instalar los archivos del *framework* localmente como se muestra a continuación.

```bash
$ composer global require laravel/installer
```
Para crear con ellos a cualquier proyecto que se requiera.

```bash
$ laravel new example-app
```
Esta opción tiene la ventaja de que la creación de nuevos proyectos es muy rápida y no requiere del acceso a Internet, sin embargo el desarrollador sólo podrá crear proyectos de la versión de Laravel que tenga instalada.  Si se desea crear proyectos con una versión más nueva, tendrá que actualizar primero la versión local.

# Utilizando Composer

La segunda opción y más común, consiste en utilizar `composer` para crear los nuevos proyectos.

```bash
$ composer create-project laravel/laravel example-app
```

Esta opción requiere del acceso a Internet y toma algunos minutos mientras se descargan los paquetes requeridos.  La ventaja que tiene es que permite crear proyectos con la versión más actual de Laravel o con la que se desee.

# Utilizando Docker

Esta tercera opción permite crear un proyecto Laravel para que se ejecute directamente en un contenedor Docker.  

```bash
$ curl -s "https://laravel.build/example-app?with=mysql,mailpit,selenium" | bash
```

Esta opción tiene la ventaja de que no es necesario tener instalada ninguna herramienta de desarrollo en el equipo anfitrión además de Docker y Visual Studio Code (o cualquier IDE).  La ejecución de comandos en el proyecto se realiza a través de [Sail](https://laravel.com/docs/master/sail).