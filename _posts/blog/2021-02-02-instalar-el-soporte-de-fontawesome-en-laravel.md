---
layout: post
title: Instalar el soporte de FontAwesome en Laravel
date: 2021-02-02 18:26 -0500
categories: Laravel
tags: laravel web icons
---

## Introducción

En el presente documento se describen los pasos necesarios para instalar el soporte a los [íconos de FontAwesome](https://fontawesome.com/icons?d=gallery) en un proyecto de Laravel.

## Procedimiento

Instalar la librería que da soporte a los íconos de FontAwesome.

```
$ npm install @fortawesome/fontawesome-free --save
```

Referenciar los estilos de la librería de FontAwesome, agregando el siguiente contenido al final del archivo de configuración.

```
$ vi resources/sass/app.scss

// FontAwesome
@import '~@fortawesome/fontawesome-free/scss/fontawesome';
@import '~@fortawesome/fontawesome-free/scss/solid';
@import '~@fortawesome/fontawesome-free/scss/brands';
@import '~@fortawesome/fontawesome-free/scss/regular';
```

Compilar los recursos con las nuevas referencias recién agregadas.

```
$ npm run dev
```

## Recursos

- Íconos de FontAwesome  
  [https://fontawesome.com/icons?d=gallery](https://fontawesome.com/icons?d=gallery)
- Install FontAwesome Using a Package Manager  
  [https://fontawesome.com/how-to-use/on-the-web/setup/using-package-managers](https://fontawesome.com/how-to-use/on-the-web/setup/using-package-managers)
