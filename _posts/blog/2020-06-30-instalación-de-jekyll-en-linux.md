---
layout: post
title: Instalación de Jekyll en Linux
date: 2020-06-30 01:13 -0500
categories: Miscellaneous
tags: linux jekyll blog
---

## Introducción

En esta publicación se describen los pasos necesarios para instalar [Jekyll](https://jekyllrb.com/) en Linux Ubuntu o Mint 20.

## Requerimientos

 * Ruby (incluyendo sus cabeceras de desarrollo), versión 2.5.0 o superior.
 * RubyGems
 * GCC y Make

## Instalación

Instalar los preqrequisitos de software.

```
$ sudo apt-get install ruby-full build-essential zlib1g-dev
```

Actualizar las variables de ambiente para el acceso a las *gemas* de Ruby, para esto, agregar las siguientes líneas al final del archivo `~/.bashrc`.

```
$ vi ~/.bashrc

    # Install Ruby Gems to ~/gems
    export GEM_HOME="$HOME/gems"
    export PATH="$HOME/gems/bin:$PATH"
```

Utilizar los contenidos del archivo `.bashrc` recién modificados.  Esto sólo es necesario durante esta sesión para no tener que cerrar la terminal y volvera a abrir.

```
$ source ~/.bashrc
```

Instalar finalmente a Jekyll.

```
$ gem install jekyll bundler
```

## Recursos

1. Requierimientos de Jekyll  
   [https://jekyllrb.com/docs/installation/#requirements](https://jekyllrb.com/docs/installation/#requirements)
1. Procedimiento de instalación para Ubuntu Linux  
   [https://jekyllrb.com/docs/installation/ubuntu/](https://jekyllrb.com/docs/installation/ubuntu/)
