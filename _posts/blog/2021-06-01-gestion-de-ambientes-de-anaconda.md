---
layout: post
title: Gestion de ambientes de Anaconda
date: 2021-06-01 19:48 -0500
categories: Python
tags: linux ubuntu python anaconda conda environments
---

## Introducción

Un ambiente virtual (*virtual environment*) es una herramienta que permite mantener las dependencias de uno o varios proyectos asociados, de manera aislada.  Existen varios [tipos de ambientes virtuales para Python](https://conda.io/projects/conda/en/latest/user-guide/concepts/environments.html), en general, los basados en la librería `venv` y los de `conda`.

A continuación se describen las principales opciones de manipulación de ambientes virtuales que provee `conda`.

## Creación de un ambiente

Para crear un nuevo ambiente virtual se utiliza la siguiente sintáxis la cual utiliza la misma versión de Python con que fue instalado Anaconda.

```
$ conda create --name NOMBRE
```

Por ejemplo:

```
$ conda create --name laboratorio
```

En caso de que se desee utilizar una versión diferente de Python, es posible especificarla durante el proceso de creación del ambiente virtual de la siguiente manera.

``` 
$ conda create --name NOMBRE python=3.8
```
## Creación de un ambiente a partir de un `spec-file`

Es posible especificar los paquetes que se instalarán por defecto en un ambiente virtual desde el mismo momento de su creación mediante el uso de un `spec-file`.  Esto se realizar a través de la siguiente sintáxis.

```
$ conda create --name NOMBRE --file spec-file.txt
```

El archivo `spec-file` se obtiene con la ejecución del siguiente comando sobre el repositorio que tiene los paquetes que se desean.

```
$ conda list --explicit > spec-file.txt
```

## Creación de un ambiente a partir de un `environment.yml`

El archivo `environment.yml` contiene una definición precisa de los elementos incluidos en un ambiente virtual.  Crear un ambiente a partir de este archivo garantiza que se tiene una copia exacta del mismo.  Para hacer esto es necesario ejecutar el siguiente comando.

```
$ conda env create -f environment.yml
```

Nótese como no es necesario especificar el nombre del ambiente ya que este se define en el archivo bajo la llave `name`.

## Clonación de un ambiente

Si se desea crear un nuevo ambiente que sea una copia exacta de otro se puede utilizar la misma opción `create` de la siguiente manera.

```
$ conda create --name NOMBRE_NUEVO --clone NOMBRE_ORIGINAL
```

Por ejemplo:

```
$ conda create --name pruebas --clone laboratorio
```

## Ambientes disponibles

Para listar los ambientes existentes en la estación de trabajo se debe utilizar cualquiera de los siguientes comandos equivalentes.

```
$ conda info --envs

$ conda env list
```

Al ejecutarse muestran un listado de los ambientes virtuales disponibles, señalando al ambiente activo con un asterisco.  Por ejemplo:

```
base                  *  /home/educacion/anaconda3
laboratorio              /home/educacion/anaconda3/envs/laboratorio
opencv                   /home/educacion/anaconda3/envs/opencv
pruebas                  /home/educacion/anaconda3/envs/pruebas
python-telegram-bot      /home/educacion/anaconda3/envs/python-telegram-bot
```

## Selección del ambiente activo

Para determinar cuál es el ambiente que se encuentra activo y por ende, al cual se tendrá acceso en todo momento, se utiliza el siguiente comando.

```
$ conda activate NOMBRE
```

Para desactivar el ambiente virtual activo y volver al `base`, se debe utilizar el siguiente comando.

```
$ conda deactivate
```

## Exportación de un ambiente

Para efectos de copia de seguridad o de compartir el ambiente activo, incluyendo no solo paquetes sino también variables, canales y otra información, es posible generar un archivo `YAML` con su definición de la siguiente manera.

```
$ conda env export > environment.yml
```

De ser necesario, es posible indicarle a `conda` que solamente incluya los paquetes que se han explícitamente solicitado, esto hace factible la recuperación en diferentes plataformas.  Esto se logra con la opción `--from-history` como se muestra a continuación.

```
$ conda env export --from-history > environment.yml
```

Para crear un nuevo ambiente virtual a partir del archivo `environment.yml` consultar la correspondiente [sección](#Creación-de-un-ambiente-a-partir-de-un-environment.yml).


## Restaurar una revisión del ambiente

`conda` guarda cada una revisión del ambiente virtual cada vez que se realiza una modificación a este.  Es posible consultar las revisiones del ambiente virtual activo a través del siguiente comando.

```
$ conda list --revisions
```

Es posible hacer retroceder (*rollback*) el ambiente a la revisión que se desee mediante el siguiente comando indicado el número de la revisión.

```
$ conda install --revision=REVNUM
```

## Eliminación de un ambiente

Para remover un ambiente virtual es necesario ejecutar el siguiente comando.

```
$ conda remove --name NOMBRE --all
```

## Recursos

- Managing environments  
  https://conda.io/projects/conda/en/latest/user-guide/tasks/manage-environments.html
- Command reference  
  https://conda.io/projects/conda/en/latest/commands.html
- Cheat sheet  
  https://conda.io/projects/conda/en/latest/user-guide/cheatsheet.html