---
layout: post
title:  "Animación CSS3 Sunrise"
lang: es
date: 2018-06-12 13:20:44 +0200
categories: articulo css3
---

## Día. Animación del sol y la luna usando CSS3 

CSS3 es una referencia muy extensa. Es tan grande que es casi imposible dominar todos sus aspectos. Una de las novedades que trajo  fue la posibilidad de realizar animaciones lo que abre, desde mi punto de vista, infinitas posibilidades.

Multitud de efectos para los que, anteriormente, necesitábamos otras tecnologías (flash, javascript etc...) pueden lograrse ahora de manera más o menos sencilla usando simplemente CSS3.



Para ilustrar cómo funciona vamos a realizar la siguiente escena, el transcurso de un día:

La estructura del html será la siguiente:

```html
<!DOCTYPE html>
<html lang="es">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Sunrise CSS3 Animation by @pekechis </title>
    <link rel="stylesheet" href="./styles.css">
</head>

<body>
    <div class="container">
        <div id="mountain">
            <img src="./img/mountain.png">
        </div>
        <div id="moon">
            <img src="./img/luna.png">
        </div>
        <div id="rays">
            <img src="./img/rayos.png">
        </div>
    </div>

</body>

</html>
```

En primer lugar iremos posicionando los distintos elementos de la siguiente manera:

```css

.container {
    animation-duration: 24s;
    animation-iteration-count: infinite;
    animation-name: skycolor;
    margin: 100px auto;
    position: relative;
    width: 300px;
    height: 300px;
}
```

El contenedor general lo centramos con la propiedad margin, le damos las dimensiones adecuadas, position

en la que veremos tres animaciones:

* El sol que saldrá y se pondrá coincidiendo brevemente con la luna.
* La luna que saldrá y se pondrá coincidiendo brevemente con la luna.
* El color de fondo que irá cambiando a lo 