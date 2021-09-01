# Texto
Usando emmet:
`lorem15 + TAB` para obtener 15 palabras.
`lorem*5 + TAB` para obtener 5 líneas de texto. 
# Tipografías

## `font-family`
Con la propiedad `font-family` establecemos el tipo de fuente deseado. A la hora de elegir una fuente debemos saber de qué tipo de letra dispone el ordenador que se utiliza para acceder a nuestras páginas. No hay manera de saber esto en todos los casos pero contamos con las llamadas **fuentes seguras** o *safe fonts* disponibles en casi todos los sistemas operativos más utilizados.
Cuando utilizamos una fuente que el usuario no tiene instalada, el navegador utilizará su tipografía por defecto.

```css
font-family: Helvetica, Arial, sans-serif;
```
Esto se conoce como camino de fuentes y cuenta con varias fuentes que actúan como *fallback* para asegurar la máxima compatibilidad, si el navegador no cuenta una, probará con la siguiente y así sucesivamente. A lo último debemos indicar el nombre de la familia genérica (en este caso sans-serif).

Si la fuente tiene más de una palabra debemos usar comillas simples.

### Familias Genéricas:
- `serif`: times new roman
- `sans-serif`: arial (grupo genérico más usado)
- `cursive`: comic sans
- `monospace`: consolas

En el navegador es posible configurar la fuente default para cada familia.

### Importar Fuentes
Si queremos importar fuentes de Google Fonts podremos hacerlo con `<link>` en todos los htmls donde la usemos o con `@import` en el CSS, siendo este último el método más aconsejable por simplicidad futura en el mantenimiento.
En caso de utilizar la importación en el html hacerlo siempre antes de nuestra hoja de estilos.

Utilizar como máximo dos variantes de cada tipografía para no atentar contra el tiempo de carga del sitio y para no complicar la adaptabilidad a las distintas pantallas.

### Fuentes Propias
En este caso queremos utilizar una fuente específica, por ejemplo **Rubik** para ello vamos a [fonts.google.com](https://fonts.google.com/),buscamos la fuente y hacemos click en **Download family**, descromprimimos, elegimos el archivo deseado **Rubik-Regular.ttf** y la colocamos en la carpeta `/assets/fonts`

Como se trata de una fuente personalizada y no una *web-safe font* debemos usar `@font-face` con esta regla le especificamos un nombre y la dirección donde se encuentra.

```css
@font-face:{
	font-family:'Rubik Regular';
	src: url('assets/fonts/Rubik-Regular.ttf');
}
```
También hubiera sido posible especificar dos formatos de la misma fuente separados por coma, por ejemplo: `src: url(letra.ttf), url(letra.otf)`
Luego podemos utilizarla normalmente con: `body{ font-family: 'Rubic Regular}` 

## `font-size`
Nos permite establecer el tamaño de la fuente, el `body` tiene `font-size: 16px` por default.

### Proporcionalidad
La mayoría de los navegadores mostrarán el elemento `<h1>` y `<h2>` con los siguientes valores default:
```css
h1 {  
	display:  block;  
	font-size:  2em;  
	margin-top:  0.67em;  
	margin-bottom:  0.67em;  
	margin-left:  0;  
	margin-right:  0;  
	font-weight:  bold;  
}
h2 {  
	display:  block;  
	font-size:  1.5em;  
	margin-top:  0.83em;  
	margin-bottom:  0.83em;  
	margin-left:  0;  
	margin-right:  0;  
	font-weight:  bold;  
}
```
Headings restantes en: [https://www.w3schools.com/tags/tag_hn.asp](https://www.w3schools.com/tags/tag_hn.asp)

### `font-size` con palabras:
Podremos establecer `font-size` con palabras: 
`smaller``larger` (estas dos hacen referencia al padre), `small`, `large`, `xx-small`, `xx-large`

## `font-weigth`
La propiedad `font-weigth` nos permitirá especificar el peso o grueso de la letra. Algunos tipos de letra solo están disponibles en `normal` y `bold`

`normal`: Grueso normal de la fuente. Igual que 400.
`bold`: Grueso equivalente a negrita. Igual que 700.
`lighter`: Corresponde a un tipo de fuente menos grueso que el tipo del elemento padre (dentro de los valores disponibles).
`bolder`: Corresponde a un tipo de fuente más grueso que el tipo del elemento padre (dentro de los valores disponibles).

`100, 200, 300, 400, 500, 600, 700, 800, 900:` **Pesos numéricos** para los tipos de fuente que nos permiten más flexibilidad que  `normal - bold`. Si el peso especificado no existe en el tipo de fuente escogido, un valor entre 600 y 900 se presentará con el valor **más próximo disponible** para dar una letra más oscura. De igual forma, un valor entre 100 y 500 se presentará con el valor **más próximo disponible** para dar una letra más clara. Esto significa que para los tipos de letra que sólo dispongan de normal y bold, cualquier valor entre 100 y 500 será normal y entre 600 y 900, será bold.

## `font-style`
La propiedad `font-style` permite definir el aspecto de una familia tipográfica entre `normal`, `italic` (cursiva) y `oblique`

`normal`:Escoge un tipo de letra clasificado como  `normal`  familia de fuentes.

`italic`: Escoge un tipo de letra etiquetado como  `italic`, o, si una versión cursiva del tipo de letra no esté disponible, escoge un tipo de letra etiquetado como  `oblique`  en lugar de eso.

`oblique`: Escoge un tipo de letra etiquetado como  `oblique`, o, si una versión oblique del tipo de letra no esté disponible, escoge un tipo de letra etiquetado como  `italic`  en lugar de eso.

Si bien visualmente son similares `italic` y `oblique` debemos tener en cuenta que `oblique` es una fuente *roman* con skew de 8 a 12 grados, mientras que `italic` es una fuente con caracteres específicos para tal fin.

== Regla Mnemotécnica:== pensar en font-weight y font-style como los estilos clásicos de Word

## `font-variant`
Nos permite seleccionar entre los aspectos `normal` y `smallcaps` para la fuente. Al utilizar `smallcaps` todas las letras minúsculas se convierten en mayúsculas pero de un tamaño de fuente menor que las originalmente en mayúsculas.

`font-variant: normal|small-caps|initial|inherit;`

## `font`
Es el *shorthand* para `font-style`, `font-variant`, `font-weight`, `font-size` y `font-family`

El `font-size` y `font-family` son *required* y deben ir al final.

Son 5 propiedades, un ejemplo podría ser `font: bold italic small-caps 10px Arial`

`font: font-style font-variant font-weight font-size/line-height font-family_|caption|icon|menu|message-box|small-caption|status-bar|initial|inherit;`

## `line-height`
La propiedad `line-height` nos permite especificar el alto de una línea, entendiéndose como tal la distancia entre las *baselines* (líneas base) de dos líneas.
Es útil para centrar un texto verticalmente: si tengo un `line-height=height`

`line-height: normal|[_number]|[_|_length]_|initial|inherit;`

Podemos expresarlo de las siguientes formas:
* keyword *normal*: `line-height: normal`
* sin unidad: `line-height: 1.6` se usa esta cifra multiplicada por el tamaño de fuente del elemento.
* longitud: `line-height: 3em` 
* porcentaje: `line-height: 34%`
* 
```
/* Unitless values: usa esta cifra multiplicada por el tamaño de fuente
del elemento */
```

## `word-spacing`
La propiedad `word-spacing` nos permite incrementar o decrementar el *white space* entre palabras. Puede tomar valores negativos.

`word-spacing: normal|_length_|initial|inherit;`

## `letter-spacing`
La propiedad `letter-spacing` nos permite incrementar o decrementar el *white space* entre caracteres de un texto. También puede tomar valores negativos.

`letter-spacing: normal|_length_|initial|inherit;`

## `text-decoration`
Es el shorthand de `text-decoration-line` (requerido) `text-decoration-color` y `text-decoration-style` el valor default es `none currentcolor solid` por ejemplo `text-decoration: line-through red dashed;`

`text-decoration: _text-decoration-line_  _text-decoration-color_  _text-decoration-style_|initial|inherit;`

`text-decoration-line`: especifica el tipo de linea a utilizar `underline`, `overline`, `line-through`
`text-decoration-color`: especifica el color de línea
`text-decoration-style`: especifica el estilo de línea `solid`, `wavy`, `dotted`, `dashed`, `double`

==Regla Mnemotécnica:== Line, Color, Style podemos recordarlo como **LaCaSa**

## `text-transform`
Nos permite controlar la capitalización del texto:
`text-transform: uppercase` para convertirlo a mayúsculas, `lowercase` para minúsculas y `capitalize` para colocar la primera letra de cada palabra en mayúsculas.

`text-transform: none|capitalize|uppercase|lowercase|initial|inherit;`

## `text-align`
La propiedad `text-align` como su nombre lo indica nos permite especificar la alineación horizontal del texto dentro de un elemento.
`text-align: left|right|center|justify|initial|inherit;`

## `text-shadow`
La propiedad `text-shadow` nos permite agregar sombra al texto y acepta una lista de valores separados por coma de sombras.

`text-shadow: [h-shadow] [v-shadow] [blur-radius] [color]|none|initial|inherit;`

Tanto `[h-shadow]` como `[v-shadow]` son el desplazamiento vertical y horizontal de la sombra, son son **required** y pueden ser valores **negativos**.
Notar que a diferencia de lo que ocurre con la propiedad `box-shadow` si bien tenemos `blur` aca no tenemos `[spread]` ni `inset`

## `white-space`
La propiedad `white-space` determina como se comportan los espacios en blanco dentro de un elemento. En ocasiones como por ejemplo al insertar contenido de un tooltip como no queremos que el texto no ocupe varias líneas vamos a necesitar poner `white-space: nowrap`

`white-space: normal|nowrap|pre|pre-line|pre-wrap|initial|inherit;`