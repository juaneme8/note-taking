# `display`
> Basado en el video de Kevin Powel - [Block, Inline, and Inline-Block explained](https://www.youtube.com/watch?v=x_i2gga-sYg)

## Línea vs Bloque
La gran mayoría de los elementos son de **bloque** y por default **los elementos de bloque se apilan uno arriba del otro** (agregan un salto de línea después del elemento). Los elementos de bloque por default tienen un `width: 100%` por lo que será el padre quién determine el tamaño.

Los elementos de **línea** (como por ejemplo los links) sólo toman el espacio del contenido. Los elementos de línea no pueden ser impactados por `height`, `width`, `margin-top`, `margin-bottom`, `padding-top`, `padding-bottom`. Esto es así ya que pensemos lo raro que sería si por ejemplo le pudiéramos dar padding o margin a un link y este quedara desfasado del texto del párrafo que lo contiene.

> Si a un elemento de línea le damos `margin-top` o `margin-bottom` no sucederá nada, mientras que si le damos `padding-top` o `padding-bottom` habrá cambios pero no del modo esperado sino de manera errática (no respeta el flow del documento).

Sin embargo, habrá ocasiones en las que vamos a necesitar asignarle por `height`, `width`, `margin` o `padding` a un elemento de línea (por ejemplo en el caso de que un link funcione como botón), en esos casos usaremos `display: inline-block`.

## Imágenes
Las imágenes técnicamente hablando son *inline elements* (replaced element) pero se comportan como *inline-block elements*