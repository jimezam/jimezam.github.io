---
layout: post
title: 'Problemas de autenticación con MySQL 8.x: ER_NOT_SUPPORTED_AUTH_MODE'
date: 2020-06-30 12:34 -0500
categories: MySQL
tags: linux windows mysql authentication nodejs development
---

## Descripción del problema

El problema sucede cuando se intenta realizar la conexión con una base de datos MySQL v8.x (o superior).  A continuación se muestra un código de ejemplo en NodeJS utilizando la librería [mysqljs/mysql](https://github.com/mysqljs/mysql);

```javascript
var mysql = require('mysql');    

var con = mysql.createConnection({
    host: "localhost",
    user: "me",
    password: "secret",
    database: "my_db",
    insecureAuth : true
});    

con.connect(function(err) {
    if (err) throw err;
        console.log("Connected!");
});
```

En lugar de una conexión, se obtiene un mensaje de error similar al siguiente.

```
Error: ER_NOT_SUPPORTED_AUTH_MODE: Client does not support authentication protocol requested by server; consider upgrading MySQL
```

## Causa del problema

El problema se debe a que MySQL a partir de su versión 8.0 cambió su método por defecto de autenticación (a `SHA2_PASSWORD`) y la librería cliente que se está utilizando aún usa el método utilizado en la 5.x (`SHA256_PASSWORD`).

## Solución propuesta en CLI

 * La solución obvia consiste en actualizar la librería cliente a una versión reciente en la cual incluya el soporte adecuado al *plugin* de autenticación `SHA2_PASSWORD`.
 * Sin embargo, si esto no es posible, existe otra alternativa que consiste en modificar la contraseña del usuario de conexión para que utilice el *plugin* antiguo (`SHA256_PASSWORD`).  Esto probablmente no sea muy recomendado en producción pero dependiendo del caso puede ser la única alternativa.

Para implementar esta última solución propuesta se debe ejecutar la siguiente sentencia DDL.

```sql
ALTER USER 'my_username'@'my_host' IDENTIFIED WITH mysql_native_password BY 'my_password';
```

En caso de que se vaya apenas a crear el usuario se puede utilizar la siguiente sentencia.

```sql
CREATE USER 'my_username'@'my_host' IDENTIFIED WITH mysql_native_password BY 'my_password';
```

Después de cualquiera de las dos opciones no olvidar ejecutar la siguiente sentencia para refrescar la información en memoria de los privilegios de los usuarios en la base de datos.

```sql
FLUSH PRIVILEGES;
```

## Solución propuesta en Docker

Esta situación sucede de igual manera cuando nos encontramos conectándonos con un contenedor Docker de MySQL v8.x.  Para solucionar el problema de conexión utilizando la estrategia mencionada en la sección anterior se debe agregar la especificación del *plugin* de autenticación (`--default-authentication-plugin`) en el momento de iniciar el contenedor de la siguiente manera.

```
$ docker run -d -p 3306:3306 \
             --name=my_container \
             -e MYSQL_ROOT_PASSWORD=my_root_password \
             -e MYSQL_ROOT_HOST=% \
             --default-authentication-plugin=mysql_native_password \
             my_image_mysql8
```

En caso de utilizarse *Docker Compose* para la gestión de los contenedores, se tendrá que agregar la siguiente instrucción al servicio de la base de datos en el archivo `docker-compose.yml`.

```
command: --default-authentication-plugin=mysql_native_password
```

## Recursos

 * Error: ER_NOT_SUPPORTED_AUTH_MODE with auth_socket #1507  
   [https://github.com/mysqljs/mysql/issues/1507](https://github.com/mysqljs/mysql/issues/1507)
 * Docker Container: MySQL 8  
   [https://davescripts.com/docker-container-with-mysql-8](https://davescripts.com/docker-container-with-mysql-8)