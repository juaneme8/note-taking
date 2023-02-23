# CSS Grid
> [The Net Ninja - CSS Grid](https://www.youtube.com/playlist?list=PL4cUxeGkcC9itC4TxYMzFCfveyutyPOCY) - [Repositorio](https://github.com/iamshaunjp/css-grid-playlist)
>
> :link: CSS Grid - Wes Bos

Grid nos permite desarrollar layouts con menor esfuerzo y logrando un código más limpio que si los implementáramos con **floats** o **flexbox**.

Por ejemplo ante la necesidad de implementar el siguiente diseño:
```plain
+--------------+
| HEADER       |
+-------+------+
| ASIDE |      |
+-------+ MAIN |
| NAV   |      |
+-------+------+
|   FOOTER     |
+-------+------+
```
Para implementar una solución con **floats** tendríamos el siguiente markup:
```html
<div id="content">
	<header>Header</header>
	<div id="middle"><!-- extra markup -->
		<main>Main</main>
		<div id="sidebar"><!-- extra markup -->
			<aside>Aside</aside>
			<nav>Nav</nav>
		</div>
	</div>
	<footer>Footer</footer>
</div>
```
En el `.CSS` a `main` le aplicamos `float:right` y a `#sidebar` `float:left`. En tanto a `#middle` lo creamos para lograr fijar el `footer` pues float rompe el flow del documento, esto lo logramos con:
```css
#middle:after{
  content: '';
  display: block;
  clear: both;
}
```
Estamos agregando *extra markup* (`#sidebar` y `#middle`) con propósitos del layout lo cual no es muy aconsejable.

En cuanto a una solución con **flexbox** si bien evitamos tener que usar floats, también tiene sus desventajas: por ejemplo sólo podemos trabajar en una dirección a la vez *row direction/column direction* y también tendremos que colocar *extra markup* para anidar los elementos como en el caso anterior.


== completar solucion al estudiar flexbox ==
[https://github.com/iamshaunjp/css-grid-playlist/blob/lesson-1/flex.html](https://github.com/iamshaunjp/css-grid-playlist/blob/lesson-1/flex.html)

Utilizando CSS Grid bastará con crear un *wrapper* sin la necesidad de los containers para lograr el layout deseado. 
```html
<div id="content">
	<header>Header</header>
	<main>Main</main>
	<aside>Aside</aside>
	<nav>Nav</nav>
	<footer>Footer</footer>
</div>
```

CSS Grid trata a la página como un grid (una caja con filas y columnas) y a la hora de posicionarlo **no importa el orden** en que están escritos los elementos html.

Es posible pasar por ejemplo del layout de **desktop** mostrado anteriormente a uno de **mobile** que tenga el header y abajo el footer sin mucho trabajo, cosa que con flexbox o floats sería más compleja.

## Grid Container
Lo primero que debemos hacer es crear un *grid container*:

```html
<div>
	<p>one</p>
	<p>two</p>
	<p>three</p>
</div>
```
```css
div{
	display:grid;
}
```
De esta manera logramos tener un *grid container* y automáticamente todos los elementos anidados pasan a ser *grid items*.
Como el `<div>` es un elemento de bloque que tiene por default todo el ancho de la página (o el contenedor dentro del cual esté), el grid también lo tendrá. 

## grid-template-columns
Es posible tener la cantidad de columnas que queramos:
```css
#content{
	display:grid;
	grid-template-columns: 33.3% 33.3% 33.3%
}
```

Otras formas de expresar que queremos 3 columnas del mismo ancho:
* `grid-template-columns: 1fr 1fr 1fr` (`fr` representa a una fracción del espacio libre en el grid,)
* `grid-template-columns: repeat(3,1fr)` esto es particularmente útil para cuando son muchas columnas.

Cuando sabemos las medidas a ambos lados y queremos que la columna del medio tome todo el espacio: `grid-template-columns: 40px 50px auto 50px 40px;`

Si tuviéramos luego cuatro *grid items*, el cuarto ocuparía una nueva fila.

### Nombres de Columnas
Podemos darle nombre a estas líneas de la siguiente manera `grid-template-columns: [first] 40px [line2] 50px [line3] auto [col4-start] 50px [five] 40px [end];` 
Cada línea puede tener más de un nombre por ejemplo `[row1-end row2-start]`
Si no especificamos ningún nombre se le asignará a cada línea un valor positivo y negativo.

> Las DevTools de Firefox nos permiten ver las líneas que forman para de cada fila y columna del grid

## grid-template-rows / grid-auto-rows
Si queremos especificar el número de filas deseadas (y no que se vayan creando cuando tenemos el contenido) contamos con la propiedad `grid-template-rows` similar a la anterior (recordar que el wrapper debe tener altura para que esto se ponga de manifiesto).

```css
grid-template-rows: 200px 300px 400px 200px;
grid-template-rows: repeat(3, 200px);
grid-template-rows: repeat(3, minmax(200px, auto));
```

Si en cambio queremos establecer el alto de las filas y que la cantidad dependa de la cantidad de elementos con `grid-auto-rows:200px` establezco un alto de fila fijo, si en cambio quiero que el contenido que supere un cierto tamaño se adapte puedo usar `grid-auto-rows:minmax(200px,auto)`

## grid-gap
Si queremos una separación entre los elementos (tanto columnas como filas), podríamos recurrir al margin pero esto nos aplicaría un margen también en la parte externa del grid. Para solucionar esto podemos utilizar la propiedad `grid-gap`
```css
grid-column-gap:10px
grid-row-gap:10px
```
Esto es equivalente a poner `grid-gap:10px`

## grid-lines
Nos permite definir posiciones en un grid, lo que permite romper con el orden default que adquieren los elementos ocupando una tras otra las posiciones establecidas.

* Si tenemos X columns tendremos X+1 column lines.
* Si tenemos Y rows tendremos Y+1 row lines
* El punto (1,1) es arriba a la izquierda.

### grid-column
```css
grid-column-start: 1;
grid-column-end: 3
```
Es equivalente a poner `grid-column: 1/3` o bien `grid-column: span(2)` donde no especificamos el punto de inicio sino que este no se altera y varía sólo el ancho/alto.

### grid-row
```css
grid-row-start:2;
grid-row-end:4;
```
Es equivalente a poner `grid-row: 2/4`

## Grid anidados:
Es posible tener un grid adentro de otro grid:
```html
<div id="content">
	<div>1</div>
	<div>2</div>
	<div class="nested">
		<p>a</p>
		<p>b</p>
		<p>c</p>
	</div>
```

```css
.nested{
	display:grid;
	grid-template-columns: 1fr 1fr
}
```


## Alineación y Justificado de Items
Por default los elementos ocupan todo el alto de la celda pero es posible modificar eso, estableciendo en el contenedor `align-items` y `justify-items`
Para la **alineación** (arriba y abajo):
```css
align-items: start | end | stretch (default)| center;
```

Para la **justificación** (izquierda y derecha):
```css
justify-items: start | end | stretch (default)| center;
```

También es posible hacer esto a nivel celda:
```css
align-self: start | end | stretch | center;
justify-self: start | end | stretch | center;
```

También es posible alinear y justificar el grid completo, esto será útil cuando el tamaño del grid sea menor que el del contenedor, por ejemplo en caso de haber definido los grid-items con unidades no flexibles como px.

Para la **alineación** (arriba y abajo):
```css
align-content: start | end | center | stretch | space-around | space-between | space-evenly
```

Para la **justificación** (izquierda y derecha):
```css
justify-content: start | end | center | stretch | space-around | space-between | space-evenly
```

## Grid de 12 columnas
Cuando utilizamos CSS libraries como Bootstrap veremos que cuentan con con una grid de 12 columnas, realizaremos una implementación similar usando CSS Grid.
```css
#content{
	display:grid;
	max-width: 960px;
	margin: 0 auto; /*colocamos auto para lograr el centrado horizontal*/
	grid-template-columns:repeat(12,1fr);
	grid-auto-rows: (100px,auto);
}
```
Por default si tengo 3 grid items cada uno ocupará una celda y utilizando `grid-column: */*` y `grid-row: */*` (aunque no haya especificado la cantidad de filas) podré posicionar los elementos donde quiera.

Si queremos que el header ocupe las 12 columnas y esté en la segunda fila:
```css
header{
	grid-column:1/13;
	grid-row: 2;
}
```

## Grid Areas
Habiendo previamente definido `grid-template-columns` y `grid-auto-rows` en el wrapper establecemos `grid-template-areas`
```css
grid-template-areas:
	"header	header header header"
	"aside aside main main"
	"nav nav main main"
	"section section section section"
	"section section section section"
	"footer footer footer footer";
```
A cada elemento debemos asignarle el nombre:
```css
header{
	grid-area: header;
}
```

## Responsive Grid
Las grid areas son particularmente interesantes a la hora de hacer diseños responsivos, establecemos una configuración default y una específica para desktop.

```css
/* estilo default para resoluciones chicas*/
grid-template-areas:

/*estilo específico para desktop*/
@media screen and (min-width:760px){
	grid-template-areas:
}
```