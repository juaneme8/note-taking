# Conceptos Básicos de CSS
> [w3.org - Conceptos Básicos de CSS](https://www.w3.org/community/webed/wiki/Es/Conceptos_b%C3%A1sicos_de_CSS)

CSS (Cascading Style Sheets) es una tecnología web utilizada para dar estilo y determinar la posición de los elementos que conforman el HTML. 
Existen 3 versiones de CSS y cada una incorpora nuevas propiedades y las de las versiones anteriores siguen funcionando.
El nombre hoja de estilos en cascada hace referencia a cómo el CSS afecta a los elementos del html.

Esto es llevado a cabo por un **sistema de reglas**

No se trata de un lenguaje de programación como JavaScript ni de un lenguaje de marcado como HTML.

## Reglas de Estilo
```css
selector {
  property1:value;
  property2:value;
  property3:value;
}
```
Lo que está dentro de las llaves se conoce con el nombre de **bloque de declaración** o property block*

Al igual que sucede en HTML los espacios en blanco son ignorados por lo que será lo mismo que:
```css
selector {property1:value;property2:value;property3:value;}
```

## Comentarios
```css
/* Comentario */
selector{
  property1:value;
}
```
Es posible añadir comentarios entre reglas o bien dentro del bloque de declaración (o *property block*)
## Agrupar Selectores
Si queremos aplicarle el mismo estilo a `h1` y a `p` podemos escribirlo del siguiente modo:
```css
h1, p {color: red;}
```


# Selectores Básicos
## Selector de Elemento
```css
p {}
```

## Selector de Clase
```css
.example {}
```
## Selector de ID
```css
#example {}
```
Recordar que los ID son *unique* dentro de cada página.

## Combinar Selectores
`p.warning {}` Selecciona los elementos `p` con la clase `warning`.
`div#example {}` Selecciona el `div` con el id `example`
`p.info, li.hightlight {}` Selecciona los `p` con la clase info y los `li` con la clase `hightlight`.

## Selectores Anidados
`#miCaja p{color: blue;}` Solo afecto a los elementos tipo `<p>` anidados en `#miCaja`

## Aplicar CSS al HTML
Existen tres formas de aplicar CSS a un docuemento HTML, estilos en línea (*inline*), implementación interna (*embedded*) o implementación externa (*external*)

### Inline Styles
Podemos aplicar estilos a un elemento específico usando el atributo `style`
```css
<p style="background:blue; color:white; padding:5px;">Paragraph</p>
```
La ventaja de usar inline styles es que el navegador se verá forzado a usar estos estilos, independientemente de lo definido mediante los otros dos métodos (en otras hojas de estilo o embebido).

El problema de los *inline styles* es que dificultan el mantenimiento y no se aprovecha la mayor ventaja de CSS que es la cascada (afectar a más de un elemento).

Los estilos en línea son muy utilizados a la hora de hacer **newsletters** dado que no admiten adjuntar una hoja de estilos mediante un archivo externo.
Los newsletters suelen hacer el **maquetado con tablas** y con width < a 600px. Además hay valores como el width y el height que se deben poner como atributos y no como propeidades de CSS ya que muchos servidores de correo electrónico no utilizan CSS. Debemos incluir siempre un enlace a la web por si el usuario no lo ve bien.


### Embedded styles
Los estilos embebidos se ubican en el `<head>` del documento dentro de un elemento `<style>`
```html
<style>
  p {
    color:white;
    background:blue; 
    padding:5px;
    }
</style>
```
Estos estilos si bien tienen la ventaja en cuanto a mantenimiento de estar en un único lugar, tienen la desventaja de estar limitados sólamente a un archivo.

### External style sheets
Consiste en colocar todas reglas de estilo en un archivo con la extensión `.css` y luego aplicarlo al documento HTML usando un elemento `<link>` dentro del `<head>` del documento.
```html
<link rel="stylesheet" href="styles.css" />
```
Tienen la ventaja de que pueden ser utilizados por varios archivos con un único lugar para hacer mantenimiento. 

## Importar Hojas de Estilo
Tanto al utilizar hojas de estilo externas como al usar estilos embebidos es posible importar reglas desde otra hoja de estilo, con lo cual **simplificaré el mantenimiento** respecto de tener que poner `<link rel="stylesheet" href="">` en cada archivo HTML para importar ese recurso además de nuestra hoja de estilosssimport soss``
## Ley de Herencia
Cuando le aplico a un contenedor un valor en una propiedad, los elementos que están anidados lo van a heredar. Sin embargo, si a uno de estos elementos le indico otro valor remplazará al heredado. Siempre lo más específico remplaza a lo más general. Sin embargo no todas las propiedades son heredables (`background` por ejemplo no lo es), ni tampoco aquellas heredables son heredadas por todos los elementos (`color` no será heredada por un elemento `<a>`)

### Herencia de `font-size`
Si defino un determinado `font-size` en el `<body>`, si bien este valor será heredado por parte de los elementos `<p>`, los `<h1>`-`<h6>` no heredarán ese valor sino un valor proporcional.

# Colores
Los colores se obtienen combinando RGB.

## Representación colores:

* **nombre** (140 colores): AliceBlue, AntiqueWhite, Aqua, Aquamarine, Azure, Beige, etc.

* **rgb** `rgb(RED, GREEN, BLUE)` donde cada parámetro define la intensidad del color de 0 a 255. Por ejemplo el rojo lo expresamos con `rgb(255,0,0)`

* **rgba** `rgb(RED, GREEN, BLUE, ALPHA)` el valor del canal alpha alpha va de 0 a 1 siendo 0 transparencia y 1 color sólido. 

* **hexa** #RRGGBB donde RR, GG, BB definen la intensidad del color de 00 a FF, por ejemplo el rojo lo expresamos con `#FF0000`

* **HSL** representación con hue saturation light. por ejemplo el color rojo lo expresamos con `hsl(0, 100%, 50%)`
1. HUE de 0 a 360°
0 rojo, 100 verde, 200 azul

2. SATURATION de 0 a 100%
0% grises, 100% color sólido

3. LIGHTNESS de 0 a 100%
0% negro, 100% blanco

* **HSLA** se suma el canal alpha a lo expresado anteriormente.

* **currentColor**: es un keyword que se refiere al valor de la propiedad color de un elemento, por ejemplo:
```css
#myDIV {  
	color:  blue;  /* Blue text color */  
	border:  10px solid currentcolor;  /* Blue border color */  
}
```

# Valores
## Keywords
* `none`: 
* `inherit`: el valor de una propiedad será heredado del elemento padre
* `initial`: restablece el valor de una propiedad al valor inicial.
* `unset`: restablece el valor de un elemento al valor heredado, si es que naturalmente hereda uno de su padre y a su valor inicial si no es así. Es decir se comporta como `inherit` en el primer caso (cuando la propiedad es heredable) y como `initial` cuando la propiedad no es heredable.

Ejemplo 1) `unset`
En este caso como `color` es heredable, utilizará el valor heredado es decir que `.bar p` será verde en lugar de lo dictado por la regla aplicada a `p`
```html
<p>This text is red.</p>
<div class="foo">
  <p>This text is also red.</p>
</div>
<div class="bar">
  <p>This text is green (default inherited value).</p>
</div>
```
```css
.foo {color: blue;}
.bar {color: green;}

p {color: red;}
.bar p {color: unset;}
```

Ejemplo 2) `unset`
En este caso como `border` no es heredable usará el valor inicial.

```html
<p>This text has a red border.</p>
<div>
  <p>This text has a red border.</p>
</div>
<div class="bar">
  <p>This text has a black border (initial default, not inherited).</p>
</div>
```
```css
div {border: 1px solid green;}
p {border: 1px solid red;}
.bar p {border-color: unset;}
```

Ejemplo 3)  `inherit`
```css
  h2 { color: green; }

  /* Sin embargo quiero que los h2 dentro de #sidebar tengan el valor por defecto */
  #sidebar h2 { color: inherit; }
```

## Obtener Colores Imagen:
Podemos obtener el color de una imagen en [color.adobe.com), iniciando sesión y luego yendo a Crear -> Extraer tema.

Otra opción es abrir la imagen en el navegador y con las DevTools, por ejemplo al body asignarle un `background-color` y luego hacer click en el cuadrado de color para acceder al *eye dropper*.  

# Propiedades
Las propiedades `color`, `background`, `background-color` son aplicables a cualquier elemento.

## Obtener Colores Imagen:
Podemos obtener el color de una imagen en color.adobe.com, iniciando sesión y luego yendo a Crear -> Extraer tema.
Otra opción es abrir la imagen en el navegador y con las DevTools y buscar algun elemento que tenga el cuadrado de color y hacer doble click para acceder al *eye dropper*.  

# `inherit`
`inherit` es un valor permitido para todas las propiedades de CSS, hace que el elemento tome el valor de su padre. 

```css
  h2 { color: green; }

  /* Sin embargo quiero que los h2 dentro de #sidebar tengan el valor por defecto */
  #sidebar h2 { color: inherit; }
```

# Elementos Semánticos
A partir de HTML5 comenzaron a existir los contenedores semánticos: `<header>`,`<footer>`,`<main>`,`<section>`,`<article>`,`<aside>`,`<address>`, `<nav>`
En cambio `<div>` y `<span>` son contenedores no semánticos y se utilizan cuando no me interesa expresar qué tipo de contenido tengo.

Todos los elementos que tienen un valor semántico nos servirán a la hora de brindar accesibilidad y posicionamiento web.

## `<header>`
`<header>`: contenedor con información introductoria. Es posible tener varios elementos `<header>` en una página. Sin embargo, no pueden ser ubicados dentro de un `<footer>`, `<address>` u otro `<header>`
Típicamente contiene:
* Uno o más elementos heading (`<h1>` - `<h6>`)
* Logo o ícono
* información de autor

```html
<article>  
	<header>  
		<h1>A heading here</h1>  
		<p>Posted by John Doe</p>  
		<p>Some additional information here</p>  
	</header>  
	<p>Lorem Ipsum dolor set amet....</p>  
</article>
```

## `<main>`
`<main>`: sirve para destacar la parte central del documento. Sólo debemos utilizarlo una vez y no puede ser descendiente de `<article>`, `<aside>`, `<footer>`, `<header>`, `<nav>`.

```html
<main>  
	<h1>Most Popular Browsers</h1>  
	<p>Chrome, Firefox, and Edge are the most used browsers today.</p>  
	  
	<article>  
		<h2>Google Chrome</h2>  
		<p>Google Chrome is a web browser developed by Google</p>  
	</article>  
	  
	<article>  
		<h2>Mozilla Firefox</h2>  
		<p>Mozilla Firefox is an open-source web browser</p>  
	</article>  
	  
	<article>  
		<h2>Microsoft Edge</h2>  
		<p>Microsoft Edge is a web browser developed by Microsoft</p>  
	</article>  
</main>
```

## `<section>`
`<section>` define una sección del documento, podemos pensarlo como una parte del todo.
```html
<section>  
	<h2>WWF History</h2>  
	<p>The World Wide Fund for Nature (WWF) is an international organization...</p>  
</section>  
  
<section>  
	<h2>WWF's Symbol</h2>  
	<p>The Panda has become the symbol of WWF...</p>  
</section>
```

## `<article>`
`<article>` es independiente del todo. 

```html
	<article>  
		<h2>Google Chrome</h2>  
		<p>Google Chrome is a web browser developed by Google</p>  
	</article>  
	  
	<article>  
		<h2>Mozilla Firefox</h2>  
		<p>Mozilla Firefox is an open-source web browser</p>  
	</article>  
	  
	<article>  
		<h2>Microsoft Edge</h2>  
		<p>Microsoft Edge is a web browser developed by Microsoft</p>  
	</article>  
```
Decimos que `<article>` es **independiente del todo**, por lo que lo usaremos para aquellos contenidos que no necesitan un contexto anterior para comprenderse, tranquilamente podrían difundirse en redes (posteo de un foro, artículo de un blog, revista, etc). 
`<section>` en cambio es **una parte del todo** y se asemeja a los capítulos de un libro, normalmente a notsuelen estar acompañados de un `<header>` que nos oriente sobre el contenido que vendrá después.

## `<nav>`
Con `<nav>` definimos una serie de links de navegación.
```html
<nav>  
	<a href="/html/">HTML</a> |  
	<a href="/css/">CSS</a> |  
	<a href="/js/">JavaScript</a> |  
	<a href="/python/">Python</a>  
</nav>
```
## `<footer>`
Un `<footer>` tipicamente contiene:
* Información del autor
* Información de copyright
* Información de contacto, debe ir dentro de un `<address>` tag.
*.
```css
@import 'custom.css';
```

## Ley de Herencia
Cuando le aplico a un contenedor un estilo (de tipografía por ejemplo), los elementos que están dentro lo van a heredar. Sin embargo, si a uno de estos le indico otro valor remplazará al heredado. Sitemap o mapa del sitio
* Navegación a los links superiores
* Documentos relacionados

```html
<footer>  
	<p>Author: Hege Refsnes</p>  
	<p><a href="mailto:hege@example.com">hege@example.com</a></p>  
</footer>
```

## `<address>`
El tag `<address>` contiene información de contacto del autor de un documento o artículo. Esta información puede ser un correo electrónico, URL, dirección física, teléfono.
```html
<address>  
	Written by <a href="mailto:webmaster@example.com">Jon Doe</a>.<br>  
	Visit us at:<br>  
	Example.com<br>  
	Box 564, Disneyland<br>  
	USA  
</address>
```

## `<aside>`
El elemento `<aside>` contiene información complementaria.
Usualmente estará ubicada como *sidebar*

```html
<p>My family and I visited The Epcot center this summer. The weather was nice, and Epcot was amazing! I had a great summer together with my family!</p>  
  
<aside>  
	<h4>Epcot Center</h4>  
	<p>Epcot is a theme park at Walt Disney World Resort featuring exciting attractions, international pavilions, award-winning fireworks and seasonal special events.</p>  
</aside>
```

# `margin`
Nos permite crear espacio en blanco alrededor de los elementos por fuera de sus bordes. Podemos setearlo utilizando las propiedades de los lados individuales: `margin-top`, `margin-rigth`, `margin-bottom` y `margin-left` o el shorthand `margin`:
* Si especifico cuatro valores el orden es top, right, bottom, left. ==Regla Mnemotécnica:== Palabra TRouBLe
* Si sólo especifico dos valores el primero es `top/bottom` y el segundo `right/left`
* Si sólo especifico tres valores el primero es `top`, el segundo `left/rigth` y el tercero `bottom`
* Si sólo especifico un valor son los 4 lados iguales.

`margin: _length_|auto|initial|inherit;`

*  `<body>` por defecto tiene 8px de margin.
* En caso de utilizar un porcentaje este será con respecto al **ancho** del elemento contenedor.
* Con `margin: auto` podremos centrar horizontalmente un elemento dentro de su contenedor.
* Es posible trabajar con un valor de margen negativo.

Es posible trabajar con un margin negativo.
En caso de especificarlo en **porcentaje** es con respecto al `width` del bloque que lo contiene, es decir el padre.

# `padding`
Es el espacio entre el borde y el contenido. El padding se suma al ancho y alto. A diferencia del `margin`, al trabajar con `padding`no se permiten valores negativos.
En caso de especificarlo en **porcentaje** es con respecto al `width` del bloque que lo contiene, es decir el padre.

## `box-sizing`
Desde el principio los modelos de caja de CSS se han medido sumando las medidas de  `width + padding + border`, o bien,  `height + padding + border`. Esto ocasionaba que se tuviera que ir sumando las medidas para calcular el tamaño exacto que queríamos que fueran nuestras cajas.

`box-sizing` apareció para resolver todo estos problemas que ocasionaba los modelos de caja en CSS. 
* `box-sizing: border-box` esto significa que el `width` será `border+padding+content`
* `box-sizing: content-box` esto significa que `anchowidthTotal` = `width+border+padding`

## `width`
Es una propiedad que sólo puede ser aplicada a los elementos de bloque o línea bloque (pero no a los de línea). El valor inicial es `width: auto`
En caso de expresarlo en porcentaje será respecto del contenedor.

## `height`
También una propiedad que sólo puede ser aplicada a los elementos de bloque y no a los de línea.
Cuando no lo establecemos se adapta al contenido, por lo que el valor inicial es `height: auto`

## `resize`
La propeidad `resize` determina si un elemento es redimensionable por el usuario y en caso de que lo sea en qué modo lo es.
Esta propiedad no aplica para elementos de linea o elementos de bloque con `overflow: visible` . 

`resize: none|both|horizontal|vertical|initial|inherit;`

```html
<div class="container">Lorem ipsum dolor sit amet consectetur adipisicing elit. Ullam neque, quod repudiandae sequi porro atque facere voluptate ab modi illo molestiae fugiat debitis recusandae? Quos atque molestiae ea cum quis.</div>
```

```css
.container{
  width: 100px;
  resize: both;
  overflow: auto;
}
```

## `overflow`
Si el contenido desborda al contenedor el hecho de que ese exceso se vea o no dependerá de la propiedad `overflow` y sólo actuará si el elemento de bloque tiene `height` especificada.
* `overflow: visible` el contenido aparece por fuera de la caja del elemento.
* `overflow: hidden` recorta el cotenido (*the overflow is clipped*)
* `overflow: scroll` coloca las dos barras de scroll sean o no necesarias
* `overflow: auto` coloca las barras de scroll solo si son necesarias. 

También es posible especificar esto en una dirección: `overflow-x: hidden` `overflow-y:scroll`

## `border`
`border` es el *shorthand* para establecer `border-width`, `border-style`, `border-color` ejemplo `border: 1px solid red`
Pero también podemos especificar las tres propiedades individualmente, las cuales admiten 4 valores diferentes (que serán top, rigth, bottom, left al igual que en margin/padding)

* `border-width` 

* `border-style` (**required**) Debe tomar alguno de los 10 valores o `initial`, `inherit`.
`border-style: none|hidden|solid|double|dotted|dashed|inset|outset|ridge|groove|initial|inherit;`

* `border-color` 

## `border-radius`
La propiedad `border-radius` nos permite definir qué tan redondeadas serán las esquinas. La redondez de cada esquina está definida usando uno o dos valores para el radio dependiendo de si es un círculo o una elipse.

* Si especifico cuatro valores el orden es `top-left` `top-right` `bottom-right` `bottom-left`
* Si sólo especifico tres valores el primero es `top-left` `top-rigth y bottom-left` `bottom-rigth`
* Si sólo especifico dos valores el primero es `top-left y bottom-right` `top-right y bottom-left` 
* Si sólo especifico un valor son los 4 bordes iguales.

==Regla Menometécnica:== Comienza por `top-left` y voy en **sentido horario** y recorriendo las esquinas y si no tiene un valor será el mismo que el de la **diagonal opuesta**.

* `border-radius` es aplicable tanto a elementos de línea como a elementos de bloque.
* Si queremos lograr un círculo debemos utilizar `border-radius: 50%`

### `border-radius` con radio vertical
`border-radius: 1em / 2em` donde 1em es el radio horizontal y 2em el vertical.

En este caso tanto el primer radio como el segundo aceptan de 1 a 4 valores como vimos anteriormente.

### Propiedades individuales
Podemos utilizar:
`border-top-left-radius`
`border-top-rigth-radius`
`border-bottom-rigth-radius`
`border-bottom-left-radius`

```css
border-radius: 1em/5em;

/* es equivalente a: */

border-top-left-radius:     1em 5em;
border-top-right-radius:    1em 5em;
border-bottom-right-radius: 1em 5em;
border-bottom-left-radius:  1em 5em;
```

```css
border-radius: 4px 3px 6px / 2px 4px;

/* es equivalente a: */

border-top-left-radius:     4px 2px;
border-top-right-radius:    3px 4px;
border-bottom-right-radius: 6px 2px;
border-bottom-left-radius:  3px 4px;
```

### Problemas Comunes con `border-radius`
Cuando colocamos texto dentro de un contenedor con `border-radius`, para evitar que el texto quede afuera (*bleed out*) podemos hacer una de estas cosas:
Colocar un cierto padding en el texto para ser coherente con la curvatura del contenedor.
Utilizar `overflow: hidden` en el contenedor.


## `border`
Podremos configurar el valor de la propiedad `border` en una imagen con los siguientes valores `none`, `solid` (sólido), `dotted` (punteado), `dashed` (con guiones), `outset` (sombra hacia afuera,`inset` (sombra hacia adentro),`ridge` (similar a un relieve) ,`groove` (similar a un contrarelieve), `double` (doble)

## `border` vs `outline`
`border` es el borde que se ve todo el tiempo, mientras que el outline es el borde que aparece al hacer foco (de color celeste, que no debemos eliminar por cuestiones de accesibilidad).

## `outline`
Los contornos podemos pensarlos como **el borde después del borde** 

La propiedad `outline` es el *shorthand* para establecer en una sola declaración **una o más** de las siguientes propiedades (similares a las que establece el *shorthand* **border**)
* `outline-width`: podemos establecerlo en medidas (`px`, `em`, etc) o con`thin`, `medium`, `thick`
* `outline-style`: **(required)** alguno de los 10 valores (none, hidden, solid, double, dotted, dashed, inset, outset, ridge, groove)
* `outline-color`:


> Basado en video de Kevin Powel - [Trouble shooting trick and some fun effects with CSS outline](https://www.youtube.com/watch?v=aJml1TTB2Pg)

### Outline en Formularios
En los **formularios** al hacer foco sobre un elemento veremos el outline como un rectángulo de color celeste, contrariamente a lo que mucha gente hace es importante no deshabilitarlo `outline: none` por motivos de accesibilidad, a menos que lo remplacemos por otro *focus state* con lo cual manifestemos que hicimos click en el input, como podemos ver en el siguiente ejemplo.
```html
<input type="text" class="new">
```

```css
.new{
  outline: none;
  transition: border-bottom ease-in 150ms;
}

.new:focus{
  border-bottom-width: 3px;
}
```


### Outline para Debug
Los contornos **no ocupan espacio**, son dibujados por encima del elemento, por lo tanto no aportan al tamaño de la caja. Gracias a esto su aparición u ocultamiento no causa un redibujado de las cajas, lo cual puede ser particularmente útil a la hora de **debuggear**. 

#### Outline para Encontrar Horizontal Scroll
Cuando tenemos un scroll horizontal es común que le asignemos un borde a todos los elementos para detectar de donde proviene.
```css
*{
	border: 1px solid red;/
}
```

En este caso le estaremos sumando 2px de alto y 2px de ancho a cada elemento de nuestro diseño, lo cual ocasionará un corrimiento. Esto es así dado que el borde forma parte del ancho total (trabajando con `box-sizing: border-box` el ancho total será `border+padding+width`).
Para evitar este desplazamiento usamos outline que tiene un **efecto solamente visual** y no afecta las dimensiones:
```css
*{
	outline: 1px solid red;/
}
```

### Efectos con Outline
Supongamos que tenemos una imagen de fondo que ocupa toda la pantalla y sobre ella un div con fondo blanco, podemos obtener un recuadro translúcido (en el sentido que dejaría ver la imagen) con: `outline: 75px solid rgba(0,0,0,.5);`. 

Si intentamos lo mismo como `border` como el fondo blanco estaría por debajo del borde, ese color `rgba(0,0,0,.5)` se mezclaría con el blanco. Sin embargo, esto puede solucionarse con `background-clip: padding-box` con lo cual le diremos al fondo que termine en el padding.

Una tercera forma de hacer lo mismo sería con `box-shadow: 0 0 0 50px rgba(0,0,0,.5)`

### `outline-offset`
Esta propiedad nos permite desfasar un cierto valor el contorno respecto del elemento.

# Especificidad
La especificidad es la manera en la cual los navegadores deciden qué valores de una propiedad CSS son más relevantes para un elemento y por ende serán aplicados.
Entender este concepto nos permitirá

Los **estilos en línea tienen mayor jerarquía que los archivos de estilo externos**. Sin embargo, la modificación en línea no es muy recomendable ya que tiene un dificil mantenimiento.
En cuanto a los estilos en línea vs. los estilos internos, tendrán predominancia aquellos que se encuentre más abajo.

A igualdad de jerarquía en todos los casos se impone aquel estilo que está ubicado más abajo.

## Jerarquía de Especificidad
Cada selector tiene su lugar en la jerarquía de especificidad. Hay 5 que definen el nivel de especificidad de un selector:
|                                           |             |
| :---------------------------------------: | :---------: |
|                !important                 | (1,0,0,0,0) |
|               inline styles               | (0,1,0,0,0) |
|                    #id                    | (0,0,1,0,0) |
|          .class y pseudo clases           | (0,0,0,1,0) |
| etiquetas de elementos y pseudoelementos) | (0,0,0,0,1) |

> En Visual Studio Code en el CSS pasando el mouse por encima del selector podremos ver el nivel de especificidad los (últimos 3 números)

## Ejemplos:
* `a` (0,0,1)
* `p a` (0,0,2)
* `.class` (0,1,0)
* 
* `p a.whatever` (0,1,2)
* `.class.class` (0,2,0)
* `p.class a.class` (0,2,2)

1. (selector universal) \* { } 0: es el elemento con menor jerarquía
2. li { } 1 (un elemento HTML)
3. li:first-line { } (2 (un elemento HTML y un pseudoelemento)
4. ul li { } 2 (2 elementos HTML)
5. ul ol+li { } 3 (3 elementos HTML)
6. h1 + *[rel=up] { } 2 (un atributo y un elemento HTML)
7. ul ol li.red { } 13 (una clase y tres elementos HTML)
8. li.red.level { } 21 (dos clases y un elemento HTML)
9. style=“” 1000 (estilo en línea)
10. p { }  1 (un elemento)
11. div p { } 2 (dos elementos)
12. .class { } 10 (una clase)
13. div p.class { } 12 (una clase y dos elementos HTML)
14. #id { } 100 (un selector de id)
15. body #darkside .sith p { } 112 (un selector de id, una clase, dos elementos HTML)


```css
A: h1
B: #content h1
C: <div id="content">
<h1 style="color: #fff">Headline</h1>
</div>
```

Se aplicará la regla C pues esta tiene un mayor nivel de especificidad es (1,0,0,0) en caso de no existir esta se aplicaría la B (0,1,0,1) por sobre la A (0,0,0,1)

# `!important`
Cuando se emplea `!important` en una declaración de estilo, esta declaración sobrescribe a cualquier otra. Sin embargo el uso de `!important` es considerado una mala práctica y debería evitarse. Debemos buscar siempre trabajar con la especificidad antes de considerar su uso.

# `z-index`
La propiedad CSS `z-index` indica el orden de un elemento posicionado (cualquier valor distinto de `static) `y sus descendientes. Cuando varios elementos se superponen, los elementos con mayor valor `z-index` cubren aquellos con menor valor.


# `box-shadow`
La propiedad `box-shadow` nos permite añadir una o más sombras a un elemento.

En caso de querer añadir **más de una** sombra a un elemento, en ese caso debemos presentar una lista separadas por comas.

`box-shadow: none|[h-offset] [v-offset] [blur] [spread] [color] inset|initial|inherit;`

* `h-offset` offset horizontal (es **required**) un valor positivo coloca la sombra en el lado derecho de la caja.
* `v-offset` offset vertical (es **required**) un valor positivo coloca la sombra en la parte inferior de la caja.
* `blur` es el difuminado de la sombra. Si es valor es `0` la sombra será *sharp* y cuanto mayor sea este número será más borrosa.
* `spread` es la extensión de la sombra. El valor default es `0`.
* `color` por default tiene el color de la letra
* `inset` con este parámetro podemos cambiar de *outer shadow* a *innershadow*

En cuanto al **orden de los parámetros** como varios son del mismo tipo debemos respetar el siguiente:
1) `h-offset`
2) `v-offset`
3) `blur` 
4) `spread` 

Si bien tanto`color` como `inset` pueden ir en cualquier orden, suponemos que los colocamos al final y así surge la ==regla mnemotécnica== `BautistaSabeComer` indicando `blur spread color`

# Etapas
Para el diseño y prototipado podemos recurrir a a AdobeXD o Figma.

* Sketch o mapa del sitio: bosquejo a mano de las partes que conforman la página.

* Wireframe : esquemas básicos de las estructuras sin estilos ni colores.

* Mockup: esquemas estáticos ya con todos los estilos y colores.

* Prototype: interactivo (AdobeXD, Figma)

# Maquetado
Al maquetar un diseño partiendo de una imagen (suponiendo que no contamos con el original de Figma o AdobeXD) debemos realizar un importante trabajo de **observación de detalles**.

Será necesario **descomponer** los diseños pensando en cómo se verian en HTML antes de ponernos a escribir código, yendo **de lo más grande a lo más pequeño**. Para ello debemos usar contenedores o etiquetas semánticas.

Un contenedor extra en ocasiones nos ahorrará repetir propiedades (por ejemplo *paddings*). También debemos usar **contenedores para separar contextos** de esta manera estaremos logrando escalabilidad. Un formulario que hoy usamos para una pantalla de Login con dos campos User y Password, el día de mañana puede sernos útil para una pantalla de registro.

Cuando trabajamos con diseños full-page utilizamos `min-heigth: 100vh` con esto nos aseguramos que si tengo más contenido que el que entra en la pantalla que no haya overflow.

Debemos tratar de que la altura de los contenedores siempre esté determinada de manera automática por el contenido, de esta manera lograremos **sistemas escalables** ya que si el día de mañana el contenido crece (porque se agrega texto a un párrafo por ejemplo), no tendremos overflow.

# Centrado Vertical
```html
<div class="box">
	<p> Texto a centrar </p>
</div>
```

## Método 1: `display: table`
```css
.box{
	display: table;
}
.box p{
	display: table-cell;
	vertical-align: middle;
}
```

## Método 2: `position:absolute`
```css
.box{
	position:relative;
}
.box p{
	position: absolute;
	top: 50%;
	width: 100%; /*del padre*/
	transform: translateY(-50%); /*propio*/
}

```

## Método 3: `display:flex`
```css
.box{
	display:flex;
	justify-content: center;
	align-items: center
}
```

## Método 4: `display:grid`
```css
.box{
	display:flex;
	justify-content: center;
	align-items: center
}
```
> Cuando tenemos `justify-content` y `align-items`  con el mismo valor podemos usar shorthand `place-items:center` 

## Método 5: `margin`
```css
.box{
	display: grid /*o flex*/
}
.box p{
	margin: auto;
}
```

# Listas
## `list-style-type`
La propiedad `list-style-type` se aplica tanto para `ul` como `ol` y define el estilo de la viñeta.

Acepta los valores: `disc`, `circle`, `square`, `decimal`, `decimal-leading-zero`, `lower-roman`, `upper-roman`, `lower-greek`, `lower-latin`, `upper-latin`, `armenian`, `georgian`, `lower-alpha`, `upper-alpha`

Si establecemos `margin: 0` y `padding: 0` en la lista, no veremos la viñeta.

## `list-style-image`
La propiedad `list-style-image` nos permite colocar una imagen en lugar de la viñeta.
Sin embargo es más personalizable utilizar `background-image` junto con `list-style-type:  none;` como podemos ver explicado en [este artículo](http://css.maxdesign.com.au/listutorial/master.htm)

`list-style-image: none|_url_|initial|inherit;`

```css
ul {  
	list-style-image: url('https://picsum.photos/100');
}
```

## `list-style-position`
La propiedad `list-style-position` especifica la posición de la viñeta.

`list-style-position: outside` la viñeta está en el padding.
`list-style-position: inside` la viñeta está dentro de la lista.


## `list-style`
La propiedad `list-style` es el shorthand de `list-style-type`, `list-style-image` y `list-style-position` . 
Las propiedades pueden ponerse en cualquier orden y si alguna de las propiedades no se especifica se usarán las default en su lugar: `disc outside none`

# Tablas
## `border`
Para especificar los bordes de una tabla en CSS podremos usar la propiedad`border`. Debemos tener en cuenta que tendremos bordes separados. Para tener todos los bordes debemos aplicarlo a `table`, `th`, `td`.

## `border-collapse` 
La propiedad `border-collapse` determina si el borde de la tabla debe colapsar en un único borde o estar separado como en standard HTML. Bastará con que lo apliquemos a `table`.

`border-collapse: separate|collapse|initial|inherit;` 

## `text-align`
La propiedad `text-align` nos permitirá alinear horizontalmente el contenido de `<th>` y `<td>`

## `vertical-align`
La propiedad `vertical-align` como su nombre lo indica nos permite alinear verticalmente el contenido de `<th>` o `<td>`.

## `empty-cells`
La propiedad `empty-cells` determina si muestra los bordes o no en las celdas vacías de una tabla.
El valor default es `empty-cells: show` y `hide` no tiene efecto si `border-collapse: collapse`
`empty-cells: show|hide|initial|inherit;`

# Contadores
Los contadores tienen un comportamiento similar al de variables que pueden ser incrementadas (en función de cuantas veces son usadas) por reglas de CSS.

## `counter-reset`
La propiedad `counter-reset` nos permite crear o resetear un contador haciendo que su valor sea 0. La colocamos en el `body` ya que queremos que sólo lo haga una vez.

## `counter-increment`
La propiedad `connter-increment` incrementa o decrementa la cuenta del contador. Si queremos decrementar lo hacemos con `counter-increment:  cnt -1;` Es decir nombre contador espacio y -1 todo junto.

## `counter()`
La función `counter()` retorna un string representando el valor actual del contador.
Acepta un segundo argumento que determina el tipo de contador utilizado por ejemplo podremos poner: `content: counter(chapter, lower-alpha);` para mostrar letras minúsculas en lugar de números. 

## Ejemplo Práctico:
Los contadores pueden ser útiles para sitios auto administrables donde quiero que aparezcan los números de elementos según la cantidad creada por el usuario.

Por ejemplo si queremos usar los contadores para mostrar un número de sección y de subsección podemos hacerlo del siguiente modo:
```css
body {
  /* Set "section" to 0 */
  counter-reset: section;
}

h1 {
  /* Set "subsection" to 0 */
  counter-reset: subsection;
}

h1::before {
  counter-increment: section;
  content: "Section " counter(section) ": ";
}

h2::before {
  counter-increment: subsection;
  content: counter(section) "." counter(subsection) " ";
}
```
* `border-width` 
* `border-style` (es la única de las tres que es *required*) valores posibles none, solid, dashed, dotted, inset, outset, groove.
* `border-color` si decimos `border-color: red green blue pink` es top, rigth, bottom, left (idem margin/padding en caso de especificar 1,2,3 o 4 valores)

# `outline`
Es el borde luego del borde. En los formularios al hacer foco sobre un elemento lo veremos de color celeste.

# `border` vs `outline`
`border` es el borde que se ve todo el tiempo, mientras que el outline es el borde que aparece al hacer foco (de color celeste, que no debemos eliminar por cuestiones de accesibilidad).

# Centrado Vertical
```html
<div class="box">
	<p> Texto a centrar </p>
</div>
```

## Método 1: `display: table`
```css
.box{
	display: table;
}
.box p{
	display: table-cell;
	vertical-align: middle;
}
```

## Método 2: `position:absolute`
```css
.box{
	position:relative;
}
.box p{
	position: absolute;
	top: 50%;
	width: 100%; /*del padre*/
	transform: translateY(-50%); /*propio*/
}

```

## Método 3: `display:flex`
```css
.box{
	display:flex;
	justify-content: center;
	align-items: center
}
```

## Método 4: `display:grid`
```css
.box{
	display:flex;
	justify-content: center;
	align-items: center
}
```
> Cuando tenemos `justify-content` y `align-items`  con el mismo valor podemos usar shorthand `place-items:center` 

## Método 5: `margin`
```css
.box{
	display: grid /*o flex*/
}
.box p{
	margin: auto;
}
```
pre lo más específico remplaza a lo más general. Sin emabrgo no todas las propiedades son heredables (`background` por ejemplo no lo es).

# `@supports`
Si bien CSS tiene un mecanismo natural de *fallback* haciendo uso de la cascada ya que si no entiende una `propiedad: valor` la ignorará y usará algo declarado previamente, adicionalmente contamos con la *feature querie* `@supports`.

La regla `@supports` nos permite testear si el navegador soporta una `propiedad` o la combinación `propiedad: valor` antes de aplicar un bloque estilos. 

```css
@supports (display: grid){  
	.main  {  
		display: grid;  
	}  
}
```
Tener en cuenta que si no sabe qué es `@supports` también saltará ese bloque, del mismo modo que si soporta `@supports` pero no esa `propiedad:valor`.

Si utilizamos SASS podremos anidar de manera directa en las reglas que estemos trabajando, por ejemplo:

```css
.container{
	color: #fff;
	background: #222;
	.
	@supports (display: grid){  
		display: grid;  
	}

}
```