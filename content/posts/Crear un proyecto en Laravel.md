---
title: "Crear Un Proyecto en Laravel"
date: 2023-04-18T01:13:33-05:00
draft: false
type: "post"
showTableOfContents: true
tags: ["linux", "laravel", "php", "project"]
# description: "A blog post"
# image: "/path/to/image.png"
---

# Introducción

Existen diferentes maneras de crear un proyecto Laravel.  En este artículo se revisan las tres principales formas.

# En un contenedor Docker

Para crear un proyecto directamente dentro de un contendor Docker es necesario ejecutar el siguiente comando.

```bash
$ curl -s "https://laravel.build/my-app?with=mysql,redis" | bash
```

Esto creará un directorio llamado `my-app` con los archivos iniciales de un proyecto Laravel dentro de un repositorio Docker y junto con los siguientes servicios especiificados a través de la variable `with` del *query string*.

- mysql *
- pgsql, 
- mariadb, 
- redis *
- mailpit *
- memcached, 
- meilisearch *
- minio, 
- selenium * 

Si no se utiliza ningún valor para la variable `with` como se muestra a continuación, se obtendrán los servicios por defecto, es decir, `mysql`, `redis`, `mailpit`, `meilisearch` y `selenium`.

```bash
$ curl -s "https://laravel.build/my-app" | bash
```

# Recursos

- Laravel Documentation | Installation
  https://laravel.com/docs/10.x/installation#getting-started-on-linux