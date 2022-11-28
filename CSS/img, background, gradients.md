# Imágenes:
## Centrado de Imágenes
Como las imágenes se comportan como elementos de línea bloque para centrarlos una forma es convertirlos en elementos de bloque con `display:block;` y luego `margin: auto`.

## Reseteos para Imágenes
```css
img{
  display:block;
	max-width: 100%;
	height: auto; /*cuando están dentro de un div c/ altura definida*/
}
```

## Hero Image
Una *hero image* es una imagen grande con texto, usualmente ubicada en la parte superior de la página.
```html
<div class="hero-image">  
	<div class="hero-text">  
		<h1>I am John Doe</h1>  
		<p>And I'm a Photographer</p>  
		<button>Hire me</button>  
	</div>  
</div>
```

```css
body, html {  
height:  100%;  
}  
 
.hero-image {  
/* Use "linear-gradient" to add a darken background effect to the image (photographer.jpg). This will make the text easier to read */  
background-image:  linear-gradient(rgba(0, 0, 0, 0.5), rgba(0, 0, 0, 0.5)), url("photographer.jpg");  
  

height:  50%;  
  
 
background-position:  center;  
background-repeat:  no-repeat;  
background-size:  cover;  
position:  relative;  
}  

.hero-text {  
text-align:  center;  
position:  absolute;  
top:  50%;  
left:  50%;  
transform:  translate(-50%, -50%);  
color:  white;  
}
```
## Máscara en imágenes:
Si al contenedor de una imagen le configuro un cierto ancho y alto, sumado a la propiedad `overflow:hidden` podré lograr un efecto de máscara.

## Image Sprites
Las **image sprites** consisten en una seguidilla de imágenes dentro de un único archivo. Son utilizadas en aplicaciones donde tenemos un número importante de imágenes y nos permiten **un mantenimiento más simple, reducir el tiempo de carga, evitar múltiples solicitudes al servidor y ahorrar ancho de banda**.
Luego para seleccionar la imagen deseada dentro del archivo, trabajaré con `background-position`
Una posible aplicación sería para trabajar con el `:hover` de un menú de redes sociales en el que quiero que al pasar el mouse por encima cambie el color de los íconos.


## Imágenes usando `background` 
Si tenemos un `<div>` con un cierto texto y queremos que tenga una imagen de fondo:
```html
<div class="box">
	<p> lorem ipsum </p>
```
Si pusiéramos debajo del `<p>` un `<img>` esta nos quedaría justamente debajo del párrafo. Si lo que queremos es que sea el fondo, debemos utilizar la propiedad `background` en css.

# Background
## `background-color`
Esta propiedad nos permite establecer el color de fondo para un elemento.

## `background-image`
Permite setear una o mas imágenes de fondo para un elemento. El valor predeterminado es `none`: `background-image: url('../img/imagen.jpg')` 
> Suponemos que el archivo `.css` con el que estamos trabajando está en la carpeta `css` por ese motivo en la ruta ponemos `../`

De acuerdo al tamaño del contenedor, veremos a la imagen incompleta o repetida. Debemos tener presetne que el tamaño del background de un elemento es igual a tu tamaño total (incluyendo el padding y el border, no así el `margin`). Es por esto que si queremos que aparezca una mayor porción de la imagen podemos aumentar el `padding`.

En ocasiones es interesante utilizar `background-color` y continuación `background-image` de modo que si por algun motivo no carga la imagen tenemos el color como *fallback*

## `background-repeat`
Hace referencia a cómo y en qué dirección se repite la imagen. 

`background-repeat: repeat|repeat-x|repeat-y|no-repeat|space|round|initial|inherit;`

* `repeat`: es el valor predeterminado con el cual obtenemos un mosaico (la última imagen se recortará si no entra entera), 
* `no-repeat` la imagen aparece una sola vez en la esquina superior izquierda
* `repeat-x` la imagen se repite en dirección al eje x
* `repeat-y` la imagen se repite en dirección al eje y.
* `space`: la imagen se repite lo más posible sin recortes. La primera y última imagen se ubican a ambos lados y se distribuye el espacio en blanco de manera pareja entre las imágenes.
* `round`: la imagen se repite lo más posible y se distorsiona de ser necesario de manera de ocupar todo el espacio disponible. 

Si proporcionamos dos valores el primero corresponde a la dirección horizontal y el segundo a la dirección vertical. Si sólo hay un valor se aplica en ambas direcciones.

> Podemos descargar patrones sutiles de repetición de: [subtlepatterns.com](https://www.toptal.com/designers/subtlepatterns/)

## `background-size`
Nos permite establecer el tamaño de la imagen:
 `auto` es el valor default
`10px 120px`valor en horizontal y vertical.
`10px` valor horizontal (y el valor vertical será tal que mantenga la proporción)
 `contain` coloca la imagen entera sin deformar y esta ocupará al menos en una dirección (x o y) la totalidad del contenedor. Si hay espacio de sobra en la otra dirección habrá repetición.
 `cover` la imagen ocupará todo el contenedor, cropeandola si fuera necesario.

**NOTA:** Si la imagen es en formato SVG por más que intente deformarla conservará su proporcionalidad.

## `background-position`

`background-position` define la posición inicial de la imagen de fondo. 

* Podemos expresarlo en **palabras**:  `top`, `center`, `bottom` para el eje y, `left`, `center`, `right` para el eje x.
 *Podemos expresarlo en **porcentaje**: será primero el valor de x y luego el valor de y. Si sólo se especifica un valor entendederá que la posición horizontal con la vertical al 50%. 
* También se puede especificar un valor de **medida**(`px`, `em`, etc) en este caso si especificamos `background-position: 20px 30px` la **esquina superior izquierda de la imagen** estará a `20px` a la derecha y a `30px` hacia abajo respecto de la **esquina superior izquierda del contenedor**.

A continuación mencionaremos expresiones equivalente:
* `top left`, `left top` y `0% 0%`  es el valor por default 

* `top`, `top center` ,`center top`, `50% 0%` 
* `center`, `center center`, `50% 50%`
* `bottom`, `bottom center`, `center bottom`, `50% 100%`
 * `left`, `left center`, `center left`, `0% 50%`
* `right`, `right center`, `center right`, `100% 50%`
* 
* `top right`, `right top`, `100% 0%`
* `bottom left`, `left bottom`, `0% 100%`
* `bottom`, `bottom center`, `center bottom`, `50% 100%`
* `bottom right`, `right bottom`, `100% 100%`

## `background-attachment`
Con esta propiedad indicamos si la imagen se moverá o no al scrollear. Los valores posibles son `scroll` (valor por defecto) o `fixed`

## shorthand `background`
En vez de escribir:
```css
body {  
	background-color:  #ffffff;  
	background-image:  url("img_tree.png");  
	background-repeat:  no-repeat;  
	background-position:  right top;  
}
```
Podemos utilizar el shorthand y poner:
```css
body {  
	background:  #ffffff url("img_tree.png") no-repeat right top;  
}
```

Si una de las propiedades del *shorthand* es el `background-size` debemos usar un *slash* `/` para separarlo de `background-position`

```css
body {  
	background:  #ffffff url("img_tree.png") no-repeat right top/50% 50%;  
}
```

Si estamos utilizando múltiples `background-image`y también queremos un `background-color`, este debe ser el último de la lista.

##  Fondos Múltiples
`background-image: url("img1.png"), url("img2.png"), url("img3.png")`
En este caso `img1.png` quedará por encima del resto.

En cuanto a `background-repeat` si tengo `background-repeat: no-repeat` no repetirá ningún fondo, mientras que si quiero personalizarlo puedo poner: `background-repeat: no-repeat, repeat-x, no-repeat` 

En cuanto a `background-size` podemos poner: `background-size: 50px, 100px, 30%` o bien si queremos podemos poner `background-size: 50px, 100px` que será la medida de los fondos impares y pares respectivamente.

Más información en: [https://www.w3.org/TR/css-backgrounds-3/#layering](https://www.w3.org/TR/css-backgrounds-3/#layering)

### Shorthand Fondos Múltiples
`background: url("img1.png") no-repat center, url("img2.png") repeat-x center 20%, url("img3.png") no-repeat rigth-top ligthblue`
Notar que el color lo especificamos en último lugar.

Mientras que el `background-size` se hace por fuera del shorthand

## `background-clip`
La propiedad `background-clip` especifica si  el fondo  de  un elemento se extiende por debajo  su borde.
`background-clip: border-box|padding-box|content-box|initial|inherit;`

* `border-box` es el valor por default. El fondo se extiende por debajo del borde.
* `padding-box` el fondo llega hasta la parte interna del borde.
* `content-box` el fondo llega solo hasta el contenido.

## `background-origin`
La propiedad `background-origin` especifica el origen de la imagen de fondo.
`background-origin: padding-box|border-box|content-box|initial|inherit;`
* `padding-box`es el valor por default. La imagen comienza en la parte superior izquierda del padding.
* `border-box` la imagen comienza en la parte superior izquierda del borde.
* `content-box` la imagen comienza en la parte superior izquierda del contenido.

## `background-blend-mode`
La propiedad `background-blend-mode` define el modo de fusión de cada capa de fondo.
`background-blend-mode: normal|multiply|screen|overlay|darken|lighten|color-dodge|saturation|color|luminosity;`

```css
div {  
	width:  400px;  
	height:  400px;  
	background-repeat:  no-repeat, repeat;  
	background-image:  url("img_tree.gif"), url("paper.gif");  
	background-blend-mode:  screen;  
}
```

# Gradientes
Los gradientes se logran con la propiedad `background-image` en lugar de `background-color` que es lo que uno pensaría inicialmente.

## Herramientas
* [https://webgradients.com/](https://webgradients.com/)
* [https://piggment.co/](https://piggment.co/)
* [https://cssgradient.io/](https://cssgradient.io/)

En CSS contamos con dos tipos de gradientes **linear gradients** y **radial gradients**

## `linear-gradient`
Nos permiten lograr un gradiente lineal, para ello debemos definir al menos dos colores
`background-image: linear-gradient(_direction_, _color-stop1_, _color-stop2, ..._);`

* Si queremos la dirección default `top to bottom` (es lo mismo que `180deg`) podemos poner `linear-gradient(red, yellow);`
* Si queremos que vaya `left to right` ponemos`background-image:  linear-gradient(to right, red , yellow);`
* Si queremos que vaya de `top left a bottom right` ponemos `background-image:  linear-gradient(to bottom right, red, yellow);`
* Si queremos especificar un ángulo (del gradiente respecto de una línea horizontal) `background-image:  linear-gradient(-90deg, red, yellow);`
* Si queremos múltiples colores `linear-gradient(red, yellow, green);`
* Si queremos indicar la posición inicial/final de cada color: `linear-gradient(blue 50%, yellow 60%, blue)` Esto significa que desde la posición 0 (en este caso arriba) hasta el 50% será azul, luego de 50% a 60% pasará de azul a amarillo y de 60% a 100% pasará de amarillo a azul.
* Si queremos que los colores se distribuyan proporcionalmente: `linear-gradient(blue, yellow, blue)`

Como se trata en definitiva de una imagen de fondo, puedo colocar **más de un gradiente** en la medida que el que defino primero tenga una cierta transparencia para que se vea el de abajo.
```css
background-image: linear-gradient(...), linear-gradient(...);
background-size: 10px, 200px;
background-repeat: no-repeat, repeat-x;
```

## `radial-gradient`
En el gradiente radial, los colores son expresados del centro hacia afuera `0%` representa el centro.

`background-image: radial-gradient(_shape size_ at _position, start-color, ..., last-color_);`

* `shape` podrá ser `ellipse` (valor default) o `circle`
* `size` define el tamaño del gradiente -   `farthest-corner` (default), `closest-side`, `closest-corner`, `farthest-side`. En caso de colocar un valor numérico este servirá como radio del **círculo**, en caso de colocar dos valores serán los radios de la **elipse**. Si el valor ingresado es en porcentaje trabajaremos con elipses.
* `position`: Define la posición del centro del gradiente, el valor default es `center`, podemos poner por ejemplo `left`, `top right`, `60% 55%`, etc.
```css
background-image: radial-gradient(red,blue, yellow)
```