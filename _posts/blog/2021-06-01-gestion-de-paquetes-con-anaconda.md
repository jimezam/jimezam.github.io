---
layout: post
title: Gestion de paquetes con Anaconda
date: 2021-06-01 23:22 -0500
categories: Python
tags: linux ubuntu python anaconda conda packages
---

## Introducción

A continuación se describen las principales tareas relacionadas con la gestión de paquetes utilizando `conda`, el gestor de paquetes incluido con Anaconda.

Es interesante tener en cuenta que `conda`, `python` e inclusive `anaconda` misma son a su vez paquetes de `conda`, así que pueden ser actualizados y manipulados utilizando los mismos comandos que se muestran en esta publicación. 

## Búsqueda de paquetes

Es posible buscar paquetes a través de conda a partir de su nombre, incluyendo el comodin asterisco para reemplazar fragmentos arbitrarios de la cadena.

```
$ conda search PAQUETE
```

También es posible especificar la versión exacta del paquete que se desea ubicar de la siguiente manera.

```
$ conda search PAQUETE=VERSIÓN
```

## Información de un paquete

Para consultar la información de un paquete específico, incluyendo las versiones disponibles y sus dependencias, es necesario utilizar la siguiente variación de `search`.

```
$ conda search PAQUETE --info
```

## Instalación de paquetes

La instalación de nuevos paquetes se realiza utilizando la opción `install`.  Por defecto se intenta instalar la versión mas alta disponible.

```
$ conda install PAQUETE
```

De la misma manera es posible instalar múltiples paquetes a través de la ejecución del mismo comando.

```
$ conda install PAQUETE1 PAQUETE2 ... PAQUETEN
```

Nótese que los paquetes se instalarán siempre en el ambiente virtual que se encuentre activo a no ser que se indique otro a través de la opción `--name`.

```
$ conda install --name NOMBRE PAQUETE
```

Entre las principales variaciones de este comando, es posible especificar la versión exacta del paquete que se desea instalar.

```
$ conda install PAQUETE=VERSIÓN
```

Así como el *canal* (repositorio) a través del cual se deberá obtener el paquete a instalarse.

```
$ conda install PAQUETE --channel CANAL
``` 
## Listado de paquetes instalados

Para listar los paquetes que se encuentran actualmente instalados en el ambiente virtual activo se debe utilizar el siguiente comando.

```
$ conda list
```

De ser necesario listar los paquetes en un ambiente virtual diferente del activo, es posible hacer esto de la siguiente manera.

```
$ conda list --name NOMBRE
```

Otra tarea muy común es la de verificar si un paquete se encuentra o no instalado en un repositorio determinado.

```
$ conda list PAQUETE
``` 

## Actualización de paquetes

Para actualizar un paquete previamente instalado en el ambiente virtual es necesario ejecutar el siguiente comando, el cual permite identificar si hay o no actualizaciones disponibles e instalarlas de acuerdo con la decisión del usuario.

```
$ conda update PAQUETE
```

Es importante notar que `conda update` buscando mantener la compatibilidad, actualiza los paquetes a la versión mas alta disponible dentro de la misma serie, por ejemplo, si se cuenta con `python 3.6` sería actualizado hasta la versión mas alta disponible dentro de `3.x`.


## Eliminación de paquetes

Para remover un paquete se debe utilizar el siguiente comando.

```
$ conda remove PAQUETE 
```

De manera similar a la instalación, es posible desinstalar múltiples paquetes utilizando el mismo comando.

```
$ conda remove PAQUETE1 PAQUETE2 ... PAQUETEN
```

Y especificar el ambiente virtual sobre el cual se desean desinstalar los paquetes, si este es diferente del ambiente activo.

```
$ conda remove --name NOMBRE PAQUETE
```

## Recursos

- Managing packages  
  [https://conda.io/projects/conda/en/latest/user-guide/tasks/manage-pkgs.html](https://conda.io/projects/conda/en/latest/user-guide/tasks/manage-pkgs.html)
- Command reference  
  [https://conda.io/projects/conda/en/latest/commands.html](https://conda.io/projects/conda/en/latest/commands.html)
- Cheat sheet  
  [https://conda.io/projects/conda/en/latest/user-guide/cheatsheet.html](https://conda.io/projects/conda/en/latest/user-guide/cheatsheet.html)
