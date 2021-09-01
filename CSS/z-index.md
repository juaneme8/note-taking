# `z-index`
La propiedad CSS `z-index` indica el orden de un elemento posicionado (con `position` distinto de static) y sus descendientes. Cuando varios elementos se superponen, los elementos con mayor valor `z-index` cubren aquellos con menor valor, es decir que determinamos **el orden de apilamiento**.

`z-index:  auto | <entero> | inherit`

Para una caja posicionada la propiedad  `z-index`  especifica:

1.  El nivel de apilamiento en el actual contexto de apilado.
2.  Si la caja establece un contexto de apilamiento local.



## Local Stacking Context
> Video de Kevin Powell que ilustra las posibles problemáticas - [CSS Z-Index and Stacking Context](https://www.youtube.com/watch?v=uS8l4YRXbaw)

El elemento `<html>` crea el *root stacking context* pero también determinadas propiedades pueden crear un *local stacking context*.

Cuando se crea un *local stacking context* el valor de `z-index` que tengan sus hijos permite calcular su posicionamiento pero ya no relativo al *root* sino a su padre.

Un contexto de apilamiento es formado, por:
* elemento raíz `<html>`
* elementos con `position: absolute` o `position: relative` con un valor `z-index` distinto de `auto`
* elementos con `position: fixed`
* flex items con un valor `z-index` distinto de `auto` 
* elementos con `opacity` menor de 1
* elementos con `transform` distinto de `none`
* elementos con `mix-blend-mode` distinto de `normal`
* elementos con `filter` distinto de `none`
* elementos con `perspective` distinto de `none`
* elementos con `isolation` igual a `isolate`