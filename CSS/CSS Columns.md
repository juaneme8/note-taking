# CSS Columns
>[Kevin Powell - CSS Columns](https://www.youtube.com/watch?v=nJfrThH92WU)
>[Kevin Powell - Responsive CSS (min. 11)](https://www.youtube.com/watch?v=0ohtVzCSHqs)

Se trata de una propiedad de CSS con la cual podremos lograr layout de columnas de texto similar al de un periódico. Podremos indicar la cantidad de columnas deseadas pero como en ocasiones al reducir el tamaño de la pantalla puede que esa cantidad de columnas deseadas cambie es que también podemos especificar el ancho ideal. 
Esta propiedad nos permite lograr un diseño responsive sin escribir *media queries* y tiene muy buen soporte en los navegadores incluso en IE.

## `columns`, `column-width`, `column-count`
`columns: <column-width> | <column-count>`  acepta una propiedad, la otra o ambas.
 `columns`  el shorthand (notar que es en plural) sino podemos pasarlos individualmente:

`column-width: <column-width>`
`column-count: <column-count>` 

En caso de utilizar ambas propiedades `column-count` indica la cantidad máxima de columnas y `column-width` el ancho mínimo para cada columna. 

Si tengo `columns: 300px 2` pero la pantalla mide 500px voy a tener una sóla columna.

Un ejemplo simple de uso de **columns** podría ser el siguiente:Notar que estar reglas se aplican o bien sobre el contenedor que quiero dividir.
```html
<p class="lorem">
	...
</>
```

```html
<div> class="lorem">
	<p>
	<p>
	<p>
</>
```

El CSS en este cason ambos casos el CSS será:
```css
.lorem{
	columns:...
}
```
## `column-gap`
Nos permite especificar el gap entre columnas, por default el valor será `column-gap: 1em`

## Alineando párrafos
Es habitual observar problemas visuales de alineación en los párrafos esto se debe a que el primer párrafo al comenzar tiene un cierto valor de `margin-top`, luego si la segunda columna empieiza con un párrafo por la mitad, no tendrá este margen por lo que arrancará más arriba.
Para solucionar esto: `p {margin-top : 0}`

## `column-rule`
Nos permite definir el estilo de la línea entre columnas. De manera similar al shorthand de border especificamos `column-rule: width style color` por ejemplo `column-rule: 1px dotted #eee`

Si estuvieramos trabajando con flexbox para hacer esto tendríamos que utilizar *media queries* para saber si debemos mostrar o no el borde (si tenemos una sola columna no lo mostamos, si tenemos más si), mientras que esta propiedad lo hace automáticamente.

## `column-span`
Nos permite asignar en un hijo un comportamiento distinto al dictado por columns. Acepta dos posibles valores `column-span: all` con el cual ignora la división en columnas y `column-span: none` que nos permite detener el *spanning element*. Esto podría ser utilizado mediante media queries cuando quiero que un hijo vuelva a respetar las columnas. 