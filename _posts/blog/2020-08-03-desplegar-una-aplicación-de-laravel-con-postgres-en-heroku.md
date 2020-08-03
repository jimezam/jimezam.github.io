---
layout: post
title: Desplegar una aplicación de Laravel con Postgres en Heroku
date: 2020-08-03 00:14 -0500
categories: Laravel
tags: laravel postgresql database heroku web development paas cloud
---

![Laravel / Heroku](/assets/images/2020-08-03/laravel_heroku.png "Laravel / Heroku"){:width="100%" style='margin:auto; display:block'}

## Introducción

En esta publicación se describen los pasos necesarios para desplegar un proyecto web hecho en Laravel en el nivel gratuito de Heroku utilizando una instancia de PostgreSQL como base de datos.

## Cuenta de usuario

Crear una cuenta gratuita de usuario en [https://signup.heroku.com/dc](https://signup.heroku.com/dc).

## Herramientas CLI

Instalar el `Heroku CLI` según corresponda con su sistema operativo.

 - [https://devcenter.heroku.com/articles/heroku-cli](https://devcenter.heroku.com/articles/heroku-cli)

### Linux Ubuntu/Mint

Para estas distribuciones de Linux puede instalarse desde `snap`.

```
$ sudo snap install --classic heroku
```

O desde `apt` según se decida.

```
$ curl https://cli-assets.heroku.com/install-ubuntu.sh | sh
```

## Versionamiento Git

El proyecto Laravel deberá estar versionado utilizando Git, en caso de NO estar versionado, se debe crear el respectivo repositorio como se muestra a continuación.

```
$ cd DIRECTORIO

$ git init

$ git add -A

$ git commit -m "First commit"
```

## Modificaciones para Heroku

A continuación se muestran las modificaciones que se le deben realizar al proyecto Laravel para ajustarlo a Heroku.

Crear el archivo `Procfile` en el directorio raíz del proyecto.

```
$ echo "web: vendor/bin/heroku-php-apache2 public/" > Procfile
```

Cambiar el `driver` del canal `single` de `single` a `errorlog` en el archivo `config/logging.php`.

{% highlight php %}
    'channels' => [
        'stack' => [
            'driver' => 'stack',
            'channels' => ['single'],
            'ignore_exceptions' => false,
        ],

        'single' => [
            'driver' => 'errorlog',     // single
            'path' => storage_path('logs/laravel.log'),
            'level' => 'debug',
        ],
{% endhighlight %}

Versionar los cambios realizados anteriormente.

```
$ git add -A

$ git commit -m "Changes for Heroku"
```

## Proyecto Heroku

Continuando ubicados en el directorio del proyecto Laravel, iniciar sesión con el usuario de Heroku.

```
$ heroku login -i
```

Crear el correspondiente proyecto en Heroku, el `NOMBRE` del mismo no deberá haber sido registrado previamente en el servicio.

```
$ heroku create NOMBRE
```

Tomar nota de las URLs mostradas una vez se crea el proyecto en Heroku, las cuales corresponden a la dirección web del proyecto y el URL del repositorio Git en Heroku para la actualización del código fuente.

```
https://NOMBRE.herokuapp.com/ | https://git.heroku.com/NOMBRE.git
```

## Publicar Aplicación

En este paso se publica el código fuente de la aplicación, versionado en el repositorio local, hacia el repositorio de Heroku.

```
$ git push heroku master
```

## Variables de configuración

El paso anterior no incluye el archivo `.env` del proyecto, así que es necesario establecer una a una las variables de entorno que necesite el proyecto.

Para crear la *llave de cifrado* primero se debe generar localmente con el siguiente comando.

```
$ php artisan key:generate --show

base64:xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx=
```

Posteriormente se debe configurar la instancia de Heroku con el valor generado de la siguiente manera.


```
$ heroku config:set APP_KEY=base64:xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx=
```

Demás variables básicas de entorno del proyecto.

```
$ heroku config:set APP_NAME=NOMBRE

$ heroku config:set APP_ENV=production

$ heroku config:set APP_DEBUG=false

$ heroku config:set APP_URL=https://NOMBRE.herokuapp.com/
```

Los valores anteriores corresponden a una aplicación en *producción*, en caso de que se desee en configurar en *desarrollo* se deberán realizar los siguientes ajustes `APP_ENV=local` y `APP_DEBUG=true`.

Una vez finalizados los ajustes a las variables de entorno en Heroku, es posible verificar su estado final mediante el siguiente comando.

```
$ heroku config 

=== NOMBRE Config Vars
APP_DEBUG:            false
APP_ENV:              production
APP_KEY:              base64:xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx=
APP_NAME:             NOMBRE
APP_URL:              https://NOMBRE.herokuapp.com/
```

## Acceder a la aplicación

Para acceder a la aplicación web recién desplegada puede utilizarse el URL asignado durante la creación del proyecto en Heroku: `https://NOMBRE.herokuapp.com/` o es posible utilizar el siguiente comando directamente.

```
$ heroku open
```
## Base de datos PostgreSQL

A continuación se describen los pasos necesarios para crear, configurar y poblar una base de datos PostgreSQL en la cual se realizará la persistencia de los datos de la aplicación Laravel.

Para crear la instancia gratuita en Heroku es necesario ejecutar le siguiente comando. 

```
$ heroku addons:create heroku-postgresql:hobby-dev
```

De ser necesario, puede consultar su información de la siguiente manera.

```
$ heroku pg:info

=== DATABASE_URL
Plan:                  Hobby-dev
Status:                Available
Connections:           0/20
PG Version:            12.3
Created:               2020-08-03 04:44 UTC
Data Size:             7.9 MB
Tables:                0
Rows:                  0/10000 (In compliance)
Fork/Follow:           Unsupported
Rollback:              Unsupported
Continuous Protection: Off
Add-on:                postgresql-clean-72157
```

## Configuración Laravel

Es necesario indicarle a Laravel que utilizará una base de datos PostgreSQL en lugar de la habitual MySQL, para esto, es necesario agregar la siguiente variable de ambiente.

```
$ heroku config:set DB_CONNECTION=pgsql
```

No es necesario realizar mas modificaciones en el archivo `config/database.php` como se hacía anteriormente, ya que Laravel es capaz de tomar la información de conexión a la nueva base de datos a partir de la variable de ambiente `DATABASE_URL` generada automáticamente en el paso anterior.  Ver [Configuration Using URLs
](https://laravel.com/docs/7.x/database#configuration).

## Poblar datos

Una vez creada la base de datos y establecida la conexión desde la aplicación web, sólo resta crear las tablas a través de las *migraciones* y agregar los datos a través de los *seeders* de la siguiente manera.

```
$ heroku run php artisan migrate --seed
```

## Otros comandos útiles

Consultar los mensajes de registro de la instancia del proyecto en Heroku.

```
$ heroku logs -t
```

Consultar los mensajes de registro exclusivamente de la base de datos.

```
heroku logs -p postgres -t
```

Consultar el estado de la base de datos asociada al proyecto.

```
heroku pg:diagnose
```

Establecer una conexión `psql` con la base de datos asociada al proyecto.  Para esto es necesario que se tenga previamente instaladas de manera local las herramientas de `postgresql-client`.

```
$ heroku pg:psql
```

## Recursos

 - Build apps for free on Heroku  
   [https://www.heroku.com/free](https://www.heroku.com/free)
 - Getting Started with Laravel on Heroku  
   [https://devcenter.heroku.com/articles/getting-started-with-laravel](https://devcenter.heroku.com/articles/getting-started-with-laravel)
 - Add-on Heroku Postgres  
   [https://elements.heroku.com/addons/heroku-postgresql](https://elements.heroku.com/addons/heroku-postgresql)
 - Heroku Postgres  
   [https://devcenter.heroku.com/articles/heroku-postgresql](https://devcenter.heroku.com/articles/heroku-postgresql)
 - Add-on ClearDB MySQL  
   [https://elements.heroku.com/addons/cleardb](https://elements.heroku.com/addons/cleardb)
 - ClearDB MySQL  
   [https://devcenter.heroku.com/articles/cleardb](https://devcenter.heroku.com/articles/cleardb)

