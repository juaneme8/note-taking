# Unidades en CSS
### Unidades Absolutas:
Son aquellas fijas y atadas a una medición física: 
* `in` `(1in = 2.54cm = 96px)` *inches*
* `px` `(1px = 1/96th of 1in)` *pixels*

Sabiendo esto podemos obtener las equivalencias para: `cm`, `mm`, `Q`:
* `cm` (`1cm = 96px/2.54`)
* `mm` `(1mm = 1/10 de*th of 1cm)`
* `Q` `(1Q = 1/4 de0th of 1mcm)` *Q= quarter-millimeters*

* `pt` `(1inpc = 72pt1/6th of 1in)` *points*
* `pc` `(1inpt = 6pc1/72th of 1in)` *picas*

### Unidades Relativas:
* `em`
* `rem`
* `vw`
* `vh`
* `vmin`
* `vmax`

Ver lista completa en: [https://www.w3.org/TR/css3-values/](https://www.w3.org/TR/css3-values/)

# em 
La unidad `em` establece el tamaño de la propiedad en base al tamaño del padre.

Dado el siguiente html template, analizaremos el `font-size` de `<h1>` y `<p>` en distintos escenarios:
```html
<body>
	<div class="grid">
		<div class="col">
			<h1>Titulo</h1>
			<p>Lorem ipsum</p>
		</div>
	</div>
</body>
```
1) 
```css
h1{font-size:2.5em;}
p{font-size:1em;}
```
Como `.col` no tiene un `font-size` definido y tampoco lo tiene `.grid` "sube" hasta el `<body>` que tiene el valor default del navegador de `16px`, por lo que para `<h1>` `font-size` valdrá `16px*2.5=40px` y para `<p>` valdrá `16px`.

2) 
```css
.col{font-size:10px;}
```
Para `<h1>` `font-size` valdrá `2.5x10px=25px` y para `<p>` `10px`.

3)
```css
.grid{font-size:2em;}
.col{font-size:2em;}
.col h1 {font-size:2.5em;}
```
Para `<h1>` `font-size` valdrá `16px*2*2*2.5=160px`

Como podemos ver las unidades `em` tienen el llamado *compunding problem* donde los valores de los hijos se ven afectados en cascada por los valores de los padres. Las unidades `rem` vienen a solucionar este problema.

# rem
rem significa *root em*, es una unidad que no depende del padre sino del elemento raíz del documento, el elemento `html` 

```css
::root{
	font-size: 16px;
}
```
O lo que es lo mismo:
```css
html{
	font-size: 16px;
}
```
```css
.grid{font-size:2rem;}
.col{font-size:2rem;}
.col h1 {font-size:2.5rem;}
```
Para `<h1>` `font-size` valdrá `16px*2.5=40px` por lo que vemos que es más consistente dado que no le afecta el tamaño de los padres.

## em vs rem
Las unidades em y rem además de para `font-sizes` pueden ser usadas para establecer `margin`, `padding`, `width`, `height` pero en estos casos tienen un comportamiento distinto:

```css
.col h1{
	font-size:2.5em;
	margin-bottom: 1em;
	margin-top: 1rem;
}
```
En este caso a la hora de establecer el `margin-bottom` se fija en el `font-size` del propio elemento, no del padre mientras que para `margin-top` se fija en el root (**rem siempre se fija en el root**).

Decimos que em ofrece **adaptabilidad** y rem ofrece **consistencia**. 

Supongamos que tenemos el siguiente html:

```html
<div class='btn'>BOTÓN</div>

<div class='btn btn-small'>BOTÓN CHICO</div>
```

```css
.btn{
	font-size: 2em;
	padding: 1em 2em;
	margin: 0 1rem;
}
.btn-small{
	font-size: .5em;
}
```
Con `.btn-small` estamos cambiando el `font-size` por lo que el  padding definido en `.btn` se ajustará automáticamente en función de ese valor. Esto da cuenta de la **adaptabilidad ofrecida por em**.

Por otra parte vemos que el `margin` permanece invariante frente a este cambio de `font-size` debido a que sus unidades son rem, dando cuenta así de la **consistencia ofrecida por rem**.

# Accesibilidad
> Basado en [engageinteractive.co.uk/blog/em-vs-rem-vs-px](https://engageinteractive.co.uk/blog/em-vs-rem-vs-px)

El uso de pixels como unidad tiene como desventaja la **accesibilidad**. En la mayoría de los navegadores podremos configurar el tamaño de fuente para modificar *default font-size* (típicamente 16px), esto significa que si el usuario establece su valor en 20px todos los tamaños de fuente deberían escalarse acordemente. Sin embargo si el sitio configura sus tamaños de fuente en px, eso no sucederá y probablemente el usuario tendrá que recurrir a hacer zoom.

Mucha gente establece un `font-size:10px` en el root element para simplificar las cuentas a la hora de convertir de pixels a rem, pero como dijimos anteriormente estaremos atentando contra la accesibilidad. 

Para brindar buena accesibilidad debemos setear el tamaño de fuente del root element como un porcentaje (será entonces un porcentaje del font-size default del navegador). Una opción para simplificar las cuentas sería establecer `font-size:62.5%` de esta manera como el 62.5% de 16px es 10px. De esta manera si el usuario quiere tener fuentes más grandes podrá lograrlo.

# Viewport Units
Se trata de unidades de longitud relativas a la ventana del navegador.
## vh
La unidad `vh` significa **viewport height** y `vw` **viewport width**, al utilizarlas en las reglas de estilo podremos lograr que los elementos se adapten según el tamaño de la pantalla.

```css
* {
	margin: 0;
	padding: 0;
}
.hero{
	height:100vh; 
	padding: 10vh 0;
}
```
De esta manera estamos indicando que la altura corresponderá a toda la pantalla, a diferencia de cuando trabajamos con porcentajes que siempre son respecto al padre.

Para títulos puede resultar interesante trabajar con un tamaño de fuente establecido en `vw` de modo que  en la medida que agrandamos la pantalla el título se agrandará y al achicarla se achicará. De todos modos puede que para pantallas muy grandes o pantallas muy chicas debamos hacer *media queries*.
```css
.title{
	font-size: 8vw;
}
```
El uso de `vw` no es muy recomendable para párrafos ya que en ocasiones será difícil de leer, en estos casos es más conveniente usar `vh` ya que logramos una mejor lectura dado que ensanchar y achicar el ancho no nos afectará.

Al trabajar con viewport units debemos tener en cuenta que caso de tener barra de scroll vertical a la vista, `vw` toma en cuenta el espacio que hay debajo de la barra por lo que si establecemos `width: 100vw` habrá también scroll horizontal, es por eso que en el ejemplo anterior pusimos `width: 100%`
Para desactivar el scroll horizontal `body {overflow-x:hidden}`

## Accesibilidad con `vw` y `vh`:
Tener en cuenta que si una fuente establecida con `vh` o `vw` es muy pequeña, por más que el usuario intente hacer zoom esto no solucionará el problema. Por ejemplo en vez de poner `font-size: 2vw;` sería conveniente poner en cambio `font-size: calc(1vw + .5rem);` De manera tal que al hacer zoom como el valor `rem` aumenta también lo hará la letra.

## vmin y vmax
Cuando utilizamos `vmin` considerará el menor valor entre `vh` y `vw` mientras que cuando utilizamos `vmax` consideramos el mayor de ambos.
Estas unidades requieren de mucho testeo en diversas medidas para asegurarnos que se ven bien en todos los dispositivos.
```css
.title{
	font-size: 8vmin;
}
```
Si la pantalla es más ancha que alta usará `vh`, caso contrario `vw`.




## calc
Me permite hacer cálculos a la hora de especificar propiedades, posibilitando la mezcla de unidades. 

**Nota**: Es importante el espacio entre los números y los operandos.

### `calc()` para mantener padding constante
La función `calc()` puede servirnos si queremos no tener que configurar el padding en un contenedor. Si la pantalla es mayor a 1200px el contenedor ocupará esos 1200px caso contrario si es menor tendrá el ancho total de la pantalla menos 2em garntizándome 1em a cada lado.
```css
.container{
	margin: 0 auto;
	max-width: 1200px;
	width: calc(100vw - 2em);
}
```

### `calc/()` para títulos responsive
```css
.title{
	font-size: calc(5vw+1rem);
}
```
### `calc()` para imagen que ocupa toda la pantalla en contenedor menor
Imagen que ocupa toda la pantalla dentro de un contenedor menor. Supoongamos que la `<img>` con la clase `.big-image` se encuentra dentro de un `.container` con `width: calc(100vw - 6em)`  y queremos que la imagen no tenga esos 3em en blancos sino que cubra todo el ancho de la pantalla.
```css
.img{
	display: block;
	max-width: 100%;
}
.big-image{
	max-width: 100vw;
	width: 100vw;
	margin: 0 calc(-50vw + 50%); /*el % es respecto al padre*/
	max-heigth: 30vh;
	object-fit: cover; /*para evitar deformaciones*/
}
```
### `calc()`para texto que ocupa toda la pantalla en un contenedor menor
Este recurso también puede resultar útil para texto:
```css
.parrafo{
	width: 100vw;
	margin: 0 calc(-50vw + 50%)
	padding: 3em
}
```

### `calc()` para lograr margen fijo y container centrado
* Si queremos tener un margen fijo independientemente deel tamaño de la pantalla y a su vez centrado:
```css
.container{
	width: calc(100% - 10rem);
	max-width: 50rem; /*50x16px=800px*/
}
```

### `calc()` para eliminar salto de ventana
Una aplicación interesante es para eliminar el "windows jumping" cuando aparece la barra de scroll vertical.
```css
html{ 
	margin-left: calc(100vw - 100%);
}
```

### `calc()` para establecer tamaño mínimo de fuente
```css
h1{
font-size: calc(7vw + 2rem);
}
```
Al sumarle los `2rem` establezco un mínimo para cuando la pantalla de de un ancho muy pequeño y queremos que el título se vea grande.


Basado en el video de Kevin Powell  [min(), max(), and clamp()](https://www.youtube.com/watch?v=U9VF-4euyRo) 

## `min()`
La función `min()` elige el menor de una lista de valores separados por comas.
En ocasiones tendremos un `.container{ width: 70% y max-witdh:1200px}` lo cual significará que en pantallas chicas el contenedor sea de un 70% de la pantalla y en pantallas grandes de 1200px. Esto es equivalente a colocar `.container { min(1200px,70%);}`con lo cual elegiremos al menor de los dos valores.

## `max()`
La función `max()` elige el mayor de una lista de valores separados por comas.

Es posible colocar dentro expresiones sin la necesidad de usar `calc()` por ejemplo `min(500px + 10%,600px)`. 
También es posible anidar expresiones con `min()` y `max()` es decir que podremos tener `min(max(70%,600px),1020px)`

## `clamp()`
La función `clamp()` escoge un valor entre un límite superior e inferior. Recibe tres parámetros un valor mínimo, un valor preferido y un valor máximo.
Es usado frecuentemente con `font-size` de modo que la fuente crezca con el tamaño del viewport pero no sea inferior ni superior a los límites indicados.
Por ejemplo `font-size: clamp(2rem, 5vw, 5rem)` Cuando esté en pantallas chicas y `5vw` sea menor a `2rem` usaré `2rem` y cuando esté en pantallas grandes y `5vw` sea mayor a `5rem` dejaré fijo en `5rem`.