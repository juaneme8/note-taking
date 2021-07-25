# Design System

Un Design System no es simplemente un UI Kit sino que es el resultado de la comunicación entre las distintas áreas de la organización.

Los sistemas de diseño no deben ser estáticos sino que deben ir evolucionando y sufriendo cambios con el tiempo.



## Typographic scale

El concepto de **typographic scale** como vemos en el [artículo de Kevin Powell](https://www.kevinpowell.co/article/typographic-scale/) nos ayuda a lograr el contraste entre los distintos tamaños de fuentes para lograr lo que se conoce como *rythm* del diseño. Se encarga de buscar tamaños fuentes que tengan armonía entre sí, como las notas de la escala musical. El primer paso es elegir el *base size* (que habitualmente es 16px) y luego la escala por la cual multiplicamos una y otra vez: `base`, `base*scale`, `base*scale*scale`

> En pantallas grandes es posible elegir el Golden Ratio (1.618) y en pantallas pequeñas 1.333

Existen distintos recursos como [type-scale](https://type-scale.com/) que nos permiten tener una previsualización.

> Más información en:
>
> https://spencermortensen.com/articles/typographic-scale/
>
> [Material Design Type scale](https://material.io/design/typography/the-type-system.html#type-scale)



> Para evitar tener diseños borrosos y trabajar con valores no enteros, podremos trabajar con múltiplos de 8 (y rellenar con otros múltiplos de 2 o 4 cuando haga falta) .

En Figma supongamos que generamos 4 elementos con el criterio dicho anteriormente, nos quedarán de este tamaño:

* base = 16px

* base x 1.62 = 25.92 (lo llevamos a 24)

* base x 1.62 x 1.62 = 41.99 (40)

* base x 1.62 x 1.62 x 1.62 = 68.02 (64)

  

*  Suponiendo que el tamaño base lo usamos para body, para la fuente más pequeña calculamos 16 / 1.62 = 9.87 pero esto es muy pequeño por lo que lo llevamos a 14.