---
title: "Uso de venv con Python3" 
date: 2021-02-20T23:06:00-0500 
draft: false
type: "post"
showTableOfContents: true
tags: ["linux", "venv", "pip", "python"]
# description: "A blog post"
# image: "/path/to/image.png"
---

# Introducción

Un ambiente virtual es un ambiente de trabajo de Python que separa al intérprete, librerías y *scripts* de manera independiente de otros proyectos.  Físicamente es un directorio en el sistema de archivos que almacena los archivos relacionados.

Cuando se instalan paquetes y el ambiente virtual se encuentra activo, los paquetes se almacenarán en el directorio del ambiente virtual en lugar de ir al ambiente `system` (buena práctica).  De la misma manera, si se ejecuta una aplicación estando activo un ambiente virtual, las librerías que serán tenidas en cuenta serán precisamente las contenidas en él.

# Creación del ambiente virtual

La sintáxis general para la creación de un ambiente virtual es la siguiente.

```
$ python -m venv /path/to/NOMBRE
```

Siendo `/path/to/` la ruta donde se almacenarán los diferentes ambientes virtuales y `NOMBRE` el identificador que se le asignará al ambiente que se está creando.

# Activación del ambiente virtual

Antes de poder utilizar el ambiente virtual es necesario activarlo.  Sólo podrá haber un ambiente activo a la vez.  Para activar el ambiente virtual en Bash/zsh es necesario ejecutar el siguiente comando.

```
$ source /path/to/NOMBRE/bin/activate
```

Otros *shells* o plataformas se pueden activar de la siguiente manera.

| Plataforma | Comando |
| --- | --- |
| fish | ```$ source /path/to/NOMBRE/bin/activate.fish``` |
| csh | ```$ source /path/to/NOMBRE/bin/activate.csh``` |
| Windows cmd | ```> \path\to\NOMBRE\Scripts\activate.bat``` |
| PowerShell | ```> \path\to\NOMBRE\Scripts\Activate.ps1``` |
| PowerShell Core | ```$ /path/to/NOMBRE/bin/Activate.ps1``` |

Una vez el ambiente virtual se encuentra activo, la variable `VIRTUAL_ENV` indicará la ruta del ambiente virtual activo y el *prompt* del *shell* reflejará su nombre.

```
(NOMBRE) educacion@silver:~$ 
```

# Instalación de librerías

Las librerías que se instalen mientras el ambiente virtual se encuentra activo, se almacenarán directamente en él.  Para instalar las librerías se debe utilizar el gestor de librerías de su preferencia, como Anaconda o Pip.

# Desactivar el ambiente virtual

Para desactivar el ambiente virtual se debe ejecutar el siguiente comando.

```
$ deactivate
```

# Remover el ambiente virtual

Para remover para siempre el ambiente virtual sólo se requiere eliminar el directorio en el cual fue instalado: `/path/to/NOMBRE`.

# Recursos

1. Virtual Environments and Packages  
[https://docs.python.org/3/tutorial/venv.html](https://docs.python.org/3/tutorial/venv.html)
