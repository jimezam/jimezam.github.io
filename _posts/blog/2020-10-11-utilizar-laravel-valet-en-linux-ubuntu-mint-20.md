---
layout: post
title: Utilizar Laravel Valet en Linux Ubuntu/Mint 20
date: 2020-10-11 18:09 -0500
categories: Laravel
tags: laravel valet linux nginx php-fm dnsmasq ngrok
---

![Mensaje de advertencia](/assets/images/2020-10-11/valet-linux-logo.png "Laravel Valet para Linux"){:width="100%" style=''}

## Introducción

**Laravel Valet** es una herramienta que pretende facilitar el desarrollo utilizando el framework de Laravel al manejar de manera automática dominios locales con el TLD de `.test` para cada proyecto entre algunas otras cosas.

A diferencia de **Laravel Homestead**, ésta sólo se hace cargo de la parte web de la infraestructura utilizando Nginx, no da soporte a otro software adicional como bases de datos.  Sin embargo se debe resaltar su bajo consumo de recursos en comparación con Homestead, ya que no utiliza una máquina virtual como este último.

La versión oficial de [Laravel Valet es para MacOS](https://laravel.com/docs/8.x/valet) únicamente, sin embargo además de la versión de Linux analizada en esta publicación, también existe una [versión para Windows](https://github.com/cretueusebiu/valet-windows).

## Instalación

### Requerimientos

Instalar los siguientes paquetes necesarios del sistema operativo.

```
$ sudo apt install network-manager libnss3-tools jq xsel
```

Instalar PHP y los siguientes paquetes de la versión que se desee, en este caso de ejemplo se utilizará la 7.4.

```
$ VERSION=7.4

$ sudo apt install php$VERSION-cli php$VERSION-curl php$VERSION-mbstring php$VERSION-xml php$VERSION-zip php$VERSION-sqlite3 php$VERSION-mysql php$VERSION-pgsql
``` 

Instalar `composer` para la gestión de paquetes PHP.

```
$ sudo apt install composer
```

### Valet

```
$ composer global require cpriego/valet-linux
```

Actualizar la variable de ambiente `PATH` en `~/.bashrc` para que el comando `valet` esté disponible.

```
$ vi ~/.bashrc

export PATH="$PATH:$HOME/.config/composer/vendor/bin"
```

### Advertencia de Nginx

Si se desea utilizar el uso de HTTPS con la opción `valet secure`, es posible que la versión de Nginx que se encuentra en el repositorio no soporte SSL.  Dado este caso, es necesario que se instale la versión del repositorio oficial de Nginix, para lograr esto es necesario registrar este repositorio en el sistema operativo de la siguiente manera.

```
$ sudo add-apt-repository -y ppa:nginx/stable
$ sudo apt-get update
```

Para verificar la versión de `nginx` que se encuenta finalmente instalada, utilizar el sigiuente comando.

```
$ sudo dpkg -l nginx
```

### Servicios de Valet

Instalar los siguientes servicios requeridos por Valet.

 - nginx
 - php-fpm
 - dnsmasq
 - valet-dns

```
$ valet install
``` 

### Actualizar el firewall

Si se desea que los sitios web puedan ser accedidos desde equipos en la misma red LAN, se debe ajustar a `ufw` para que permite este tráfico.

```
$ sudo ufw allow 'Nginx HTTP'

Rule added
Rule added (v6)

$ sudo ufw reload

Firewall reloaded

$ sudo ufw status
Status: active

To                         Action      From
--                         ------      ----
Nginx HTTP                 ALLOW       Anywhere                  
Nginx HTTP (v6)            ALLOW       Anywhere (v6)    
```

## Gestionar el servicio

Es posible **iniciar** el servicio de Valet (`valet-dns`, `php-fpm` y `nginx`) mediante el siguiente comando.

``` 
$ valet start
``` 

De igual manera es posible **detenerlo**.

```
$ valet stop
```

Y consultar su **estado**.

```
$ valet status

Php7.4-fpm is running...
Nginx is running...
```
### Carga de servicios al inicio del sistema

Por defecto, al inicio del sistema operativo (***boot***) los servicio `nginx` y `php-fpm` se inician por defecto, si no se desea este comportamiento se deben ejecutar los siguientes comandos para desactivarlos y sólo permitir su inicio manual a través de `valet start`.

``` 
$ sudo systemctl enable nginx

$ sudo systemctl enable php7.4-fpm
```

## Identificar directorio de proyectos

Para identificar el directorio en el cual se almacenerán los diferentes proyectos web a gestionarse con Valet, se debe crear esta ruta y ubicarse en ella.  En este caso se utilizará `~/code`.

```
$ mkdir ~/code

$ cd ~/code
```

Se ejecuta el comando `park` de Valet para marcar este directorio como ubicación de proyectos.

```
$ valet park
```

A partir de este momento, cualquier directorio ubicado sobre `~/code` puede accederse localmente utilizando el dominio ficticio `nombre-directorio.test`.

## Utilizar HTTPS para un proyecto

Por defecto, Valet provee acceso HTTP a los proyectos.  Si se desea que este acceso sea HTTPS, por ejemplo en el proyecto `demo`, se debe ejecutar el comando `secure`.

```
$ valet secure demo

Restarting php7.4-fpm...
Restarting nginx...
The [demo.test] site has been secured with a fresh TLS certificate.
```

Es posible verificar su estado mediante el comando `secured` y volver a un acceso HTTP mediante el comando `unsecure`.

```
$ valet unsecure demo

Restarting php7.4-fpm...
Restarting nginx...
The [demo.test] site will now serve traffic over HTTP.
```

## Cambiar la versión de PHP utilizada

A través de Valet es posible modificar la versión de PHP que se utiliza para un proyecto en particular mediante el comando `use` de la siguiente manera.

```
$ cd ~/code/demo

$ valet use php@7.2
```

## Acceso externo a un proyecto

Si se desea proveer acceso externo (desde Internet) para uno de los proyectos, es posible utilizar el comando `share` de Valet, el cual hace uso de la versión gratuita del servicio [ngrok](https://ngrok.com/) el cual provee un túnel desde el equipo de desarrollo hasta sus servidores con IP pública por un periodo máximo de 8 horas.

```
$ cd ~/code/demo

$ valet share
```

## Actualizar Valet

Para actualizar Valet es necesario ejecutar los siguientes comandos.  El primero de ellos verifica si hay o no una actualización vigente.

```
$ valet update
```

Los siguientes comandos realizan la actualización del paquete y realizan los ajustes que puedan ser necesarios para esta nueva versión.

```
$ composer global update

$ valet install
```

## Recursos

 - Laravel Valet para Linux  
   [https://cpriego.github.io/valet-linux/](https://cpriego.github.io/valet-linux/)
 - Requerimientos de Laravel Valet  
   [https://cpriego.github.io/valet-linux/requirements.html](https://cpriego.github.io/valet-linux/requirements.html)