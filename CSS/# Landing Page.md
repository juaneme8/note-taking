# Landing Page
> Basado en el video de Kevin Powell -  [Build a Modern Landing Page Website](https://www.youtube.com/watch?v=X1dz0xRbSJc)
> Los archivos de este proyecto podemos encontrarlos en la carpeta `md-landing`

El propósito de esta sección es recopilar algunos lineamientos para desarrollar una landing page con la metodología *mobile first*.
Tener presente que **este proyecto utiliza SASS**.

## Maquetado
La página cuenta con las siguientes secciones:
1. `<header>` con una *hero image* (que forma parte del `background-image` del `header`) y un `.container` con `h1`, `p` y un botón `a` 
2. `<main>` a su vez dividido en dos `<section>`:
	* La primera sección para los productos destacados con un `.container` y dentro un `h2` con el título de la sección y un `.split` con los productos en sí. Cada producto consiste en un `a` que tiene dentro un`img` y un `p` . Por último el `p` que tiene detalles del producto, tiene dentro un `span` con el precio del producto.
	* La segunda sección es para los otros productos, por lo que tenemos un título `h2`y cada uno de los productos es un `<article>`. Dentro de cada artículo tenemos un `h3`, `p`, `a`, `img`

## Reseteos
```css
*,
*::before,
*::after{
	box-sizing: border-box;
}
body{
	margin:0;
	line-height: 1.6;
}

h1,
h2,
h3,
p{
	margin: 0;
}
```

### Reseteos Imágenes
```css
img {
	max-width: 100%;
	display: block;
}
```
Al trabajar con imágenes si directamente las utilizamos `<img src="shoe-1.png">` y no colocamos ninguna regla la imagen tendrá su tamaño natural (`730px x 628px`) y no estará limitado por el tamaño del padre. Esto ocasionaría que dispositivos de pantalla pequeña haya un scroll horizontal. Para evitar esto establecemos `max-width: 100%` de modo tal que el ancho nunca supere al 100% del contenedor.
Para resoluciones más grandes, en cambio la imagen se verá en su resolución natural. Si en cambio la imagen fuera mucho más grande por ejemplo `5319x2708` en ese caso si bien el `max-width` evitará el scroll también en pantallas mas grandes, podría servirnos también establecer un valor de por ejemplo `width:1020px`.

Si quisiéramos centrar esta imagen podríamos hacerlo con `margin: 0 auto` (previo `display: block)`.

Al trabajar con diseño de imágenes responsive normalmente también tendremos `height:  auto;` pero en este caso no hace falta ya que no hemos establecido la altura de las imágenes explícitamente en el html.

`display: block` lo ponemos para evitar que aparezcan píxeles en blanco debajo de la imagen. 

> Este espacio en blanco extra (de aproximadamente 3px) aparece siempre que agregamos una imagen a un div. Ocurre debido a que como las imágenes son inline el navegador agrega espacio debajo de la *baseline* para ajustar otros elementos inline.


## Mobile First
Como utilizaremos la implementación mobile first lo primero que hacemos es en las DevTools pasar a un tamaño de dispositivo móvil (en Firefox `Control+Shift+M`) para empezar trabajar.

## `.container`
Comenzaremos con las clases que utilizamos varias veces en el proyecto.
En la clase `.container` establecemos el ancho con `width: 85%` con lo cual evitamos tener que trabajar con padding y el `max-width: 65em` (65x16px = 1040px)

```css
.container{
	width: 85%;
	margin: 0 auto;
	max-width: 65em;
}
```

## `.btn`
A la hora de dar estilo a `.btn` colocamos `display: inline-block` de modo tal que podamos darle `padding-top` y `padding-bottom`.
Le ponemos un `border-radius: .25em` de manera que sea sutil, lo cual se corresponde con los bordes redondeados de las zapatillas y el diseño basado en círculos sobre los que está el calzado.
A la hora de utilizar css *custom properties* también colocamos un *fallback* `background: var(--clr-accent, blue);` En este caso si la variable `--clr-accent` no está definida el color será `blue`

Si el tamaño de fuente en el root element es `16px` ese será el valor de `1rem`, por lo que `0,125rem` equivale a `2px` es decir que al poner `font-size: 1.125rem` será equivalente a `18px`.

Notar que al estar el padding especificado en `em` este será dependiente del font-size del elemento por lo que `padding: .5em 1.25em` por lo dicho anteriormente se traducirá en un valor de `9px` y `22.5px`. Si en cambio tuviera el valor en `rem` es decir `padding: .5rem 1.25rem` se traduciría en 8px y 20px

Para transiciones de opacidad se recomienda trabajar con `transition-timing-function: linear`

```css
.btn {
  display: inline-block;
  text-decoration: none;
  color: var(--clr-text, #fff);
  font-weight: 700;
  text-transform: uppercase;
  font-size: 1.125rem;
  padding: .5em 1.25em;
  background: var(--clr-accent, blue);
  border-radius: .25em;
  transition:
    transform 250ms ease-in-out,
    opacity 250ms linear;
}
```
Notar que no sólo trabajo con `:hover` sino también con `:focus` esto hará que perciba el efecto también si me estoy desplazando por el sitio con `TAB`.
```css
.btn:hover,
.btn:focus {
  transform: scale(1.1);
  opacity: .9;
}

```

# Tipografía Responsive
A la hora de establecer el tamaño de letra, primero ponemos `font-size: 4rem` para que nos sirva como default para aquellos navegadores que no soportan `clamp()` y luego lo establecimos utilizando esta función con `font-size: clamp(3rem, 5vw + 1rem, 4.5rem);` Notar que como segundo parámetro pusimos `5vw + 1rem` como indica la documentación sin el uso de `calc()`, pero en caso de utilizar LiveSass puede que nos tire error y haya que agregarlo.
```css
.primary-title {
  font-size: 4rem;
  font-size: clamp(3rem, 5vw + 1rem, 4.5rem);
  line-height: 1;
  text-transform: uppercase;
}
```

# Hero Image
Para lograr la *hero image* le daremos estilo a la clase `.hero` que es la que tiene el `<header>`
```html
<header class="hero">
	<div class="container spacing">
		<h1 class="primary-title">Amazing shoes at an amazing price</h1>
		<p>
			Lorem ipsum dolor sit amet consectetur adipisicing elit.
		</p>
		<a href="#" class="btn">See what we have</a>
	</div>
</header>
```
Notar que no defino la altura del `header` sino que simplemente asigno un padding importante con `padding: 15em 0` 
En cuando al fondo primero *fallback* con un color sólido `background: #222` y luego mediante la regla `@supports` si el navegador soporta el conjunto `propiedad:valor` `background-blend-mode: multiply` asigno una serie de estilos. 

Con `background-attachment: fixed;` nos aseguramos que la imagen permanezca fija cuando scrolleamos, lo cual es un rasgo característico de las *hero images*.
```css
.hero {
  color: white;
  text-align: center;
  padding: 15em 0;
  background: #222;

  @supports (background-blend-mode: multiply) {
    background: 
	    url(../img/shoe-3.png),
	    radial-gradient(#444, #111);
    background-blend-mode: multiply;
    background-attachment: fixed;
    background-repeat: no-repeat;
    background-position: center center;
  }
}
```

Notar que `@supports` lo anidamos dentro del estilo de la clase por estar utilizando SASS, pero en caso de utilizar CSS puro deberíamos haber puesto:
```css
@supports (background-blend-mode: multiply) {
	.hero {
		background: url(../img/shoe-3.png), radial-gradient(#444, #111);
		background-blend-mode: multiply;
		background-attachment: fixed;
		background-repeat: no-repeat;
		background-position: center center;
	}
}
```

## Espaciados Consistentes
Así como aplicamos el padding a `<header>` (a través de la clase `.hero`) le aplicaremos también padding a los dos `<section>`. Recordemos que `.container` (que será el hijo en los tres casos) no tiene padding.

## `.spacing`
Si tuvieramos que escribir el `<header>` con Zen Coding este sería:
`header.hero>.container.spacing>h1.primary-title+p+a.btn`

Para lograr un espaciado entre `h1`, `p` y `a` podríamos darle margen de manera individual, en cambio lo que haremos será utilizar una clase `.spacing` que la aplicamos al contenedor (`.container`) de esos tres elementos. Una de las formas de resolver este espaciado es asignando un `margin-top` a todos los elementos menos al primero.
```css
.spacing>*+*{
	margin-top: var(--spacer, 2rem);
}
```
Esto nos permitirá tener un espaciado uniforme, **recordemos que previamente hemos reseteado los márgenes de estos elementos** y nótese además que utilizamos variables css y un valor fallback de `2rem`.


# Featured Section
```html
<section class="featured">
  <div class="container">
    <h2 class="section-title">Featured products</h2>
    <div class="split">
      <a href="#" class="featured__item">
        <img src="img/shoe-4.png" alt="" class="featured__img">
        <p class="featured__details"><span class="price">$99</span>shoe name</p>
      </a>
      <a href="#" class="featured__item">
        <img src="img/shoe-5.png" alt="" class="featured__img">
        <p class="featured__details"><span class="price">$99</span>shoe name</p>
      </a>
      <a href="#" class="featured__item">
        <img src="img/shoe-6.png" alt="" class="featured__img">
        <p class="featured__details"><span class="price">$99</span>shoe name</p>
      </a>
    </div>
  </div>
</section>
```
## `.split`
`.split` es la clase del div que tiene los tres *featured products*. 

Queremos tener estos elementos apilados para resoluciones chicas y uno al lado del otro para resoluciones grandes. Como **no queremos recurrir al uso de media queries** para esto, hacemos lo siguiente:
* Para resoluciones chicas: `flex-wrap: wrap;` y luego en los *flex-items* tenemos  `flex-basis: 30%;` y `min-width: 15em;` con lo cual decimos que el ancho ideal es del 30% pero que como mínimo tiene que ser de `15em` es decir `240px`.
Resumiendo para pantallas chicas esto implicará items apilados y de mínimo `15em` de ancho.
* Para resoluciones más grandes. Los items estarán en fila y serán del `30%`

`gap: 1em;` no tiene soporte en Safari y en **caniuse** vemos que tampoco tiene un alto porcentaje global de soporte, probablemente deberíamos pensar otra opción por ejemplo: `.split > *:not(:first-child) {margin-left: 20px;}` Pero tener en cuenta que esto sólo funcionaría para cuando los elementos estén uno al lado del otro y no cuando estén uno arriba del otro.

Como estamos utilizando SASS al poner `&>* { }` será luego compilado como `.split > * { }`

```css
.split {
  display: flex;
  gap: 1em;
  flex-wrap: wrap;
  justify-content: center;

  &>* {
    flex-basis: 30%;
    min-width: 15em;
  }
}
```

## `.featured__item`
Con pseudoelementos quiero agregar un círculo y para asegurarme que sea perfecto, en lugar de trabajar con coordenadas top, bottom, left, rigth (lo cual daría lugar a una forma rectangular igual al contenedor y obtendría finalmente una elipse) trabajaré con padding y porcentajes. Recordemos que al trabajar con padding colocando el valor porcentual el mismo será con respecto al `width` del padre. `padding: 75% 75% 0 0;` luego con `border-radius: 50%` nos aseguramos obtener un círculo.

Notar que a pesar de estar usando `z-index: -1;` debido al local stacking context que genera el `transform: scale(.85);` hace que el círculo no se vaya detras del fondo gris `background: #eee;`
```scss
.featured {
  background: #eee;

  &__item {
    //display: block; //no hace falta ya que son flex items
    position: relative;
    transform: scale(.85);
    transition: transform 250ms ease-in-out;
    text-decoration: none;
    color: #333;
    text-align: center;
    line-height: 1.2;


    &:hover,
    &:focus {
      transform: scale(1);

      .featured__details {
        opacity: 1;
        text-shadow: 0 0 2em rgba(#fff, 1) //para lograr una mejor lectura sobre el círculo
      }
    }

    &::after {
      content: '';
      position: absolute;
      top: 10%; //podría haberlo centrado con:
      left: 10%; // top: 50%; left:50%; transform: translate(-50%,-50%) 
      padding: 75% 75% 0 0;
      border-radius: 50%;
      background: #2193b0;
      z-index: -1;
    }
  }

  &__details {
    opacity: 0;
    transition: opacity 250ms linear;

    span {
      display: block;
      font-weight: 700;
      font-size: 2.5rem;
    }
  }
}
```

# Products Section
```html
    <section class="our-products">
      <div class="container">
        <h2 class="section-title">Our products</h2>

        <article class="product shoe-red spacing">
          <img src="img/shoe-1.png" alt="" class="product__image">
          <h3 class="product__title">A really nice shoe</h3>
          <p class="product__description">Lorem ipsum dolor sit</p>
          <a href="" class="btn">Buy now</a>
        </article>
		<!--(más articles)  -->
      </div>
    </section>
```
Como podemos ver cada producto es un `<article>` con la clase `.product`.
En el `background` establecemos un gradiente radial con variables css y luego un color default por si estas no funcionan. Quizás lo ideal hubiera sido hacer al revés, unos colores default que sean redefinidos en caso de contar con las variables. 

En este caso como el contenedor es más grande utilizamos un `boder-radius` mas grande.

Notar que al realizar el gradiente radial tenemos un color interno que llamamos `---clr-inner` y uno externo `--clr-outer`

Queremos que en pantallas chicas la imagen del producto aparezca más arriba, es por eso que ponemos `margin: -5em 0 0 0;` para la clase `.product__image`

Utilizaremos media queries, lo cual no lo hacemos a una medida determinada sino de acuerdo al sitio donde el diseño lo demanda por ejemplo prestando atención a cuando se vuelven demasiado largas las líneas de texto, lo cual invitaría a tener columnas de dos elementos. En este caso eso lo observamos cerca de los 750px (750/16 = 46.875) es por eso que ponemos `@media (min-width: 45em)`.
**Es importante limitar la cantidad de media queries lo cual es beneficioso en términos de mantenimiento del sitio.**

Usaremos **floats** si bien es algo que no se utiliza mucho en la actualidad, recordemos que su propósito es para tener texto alrededor de cosas y es justamente lo que haremos. Establecemos `float: rigth` y `width: 65%;`
Como queremos que el texto se acerque a la imagen y no al rectángulo que la contiene colocamos `shape-outside: url(../img/shoe-1.png);` y  para dar un cierto margen `shape-margin: 1em;`. En el caso de las zapatillas donde en una imagen apunta para un lado y en otra apunta para el otro, no podremos usar la misma `url()`. 

Si hubieramos puesto `shape-outside: circle(50%)` hubieramos tenido un círculo alrededor del cual el texto wrapearía.
```scss
.product {
  background: radial-gradient(var(--clr-inner, limegreen), var(--clr-outer, purple));
  padding: 3em;
  border-radius: 1em;
  margin-bottom: 5em;
  text-align: center;

  &__title {
    font-size: clamp(3rem, calc(5vw + 1rem), 5.5rem);
    text-transform: uppercase;
    line-height: 1;
    color: #fff;
    text-shadow: 0 0 .2em rgba(#000, .2);
  }

  &__image {
    margin: -5em 0 0 0;
  }

  @media (min-width: 45em) {
    text-align: left;
    margin-bottom: 9em;

    &__title {
      margin: 0;
    }

    &__image {
      float: right;
      width: 65%;
      // outline: 2px solid red;
      shape-outside: url(../img/shoe-1.png);
      shape-margin: 1em;
      margin: 0 -5em 0 0;
    }

    &.shoe-left {
      .product__image {
        float: left;
        shape-outside: url(../img/shoe-2.png);
        margin: 0 0 0 -5em;
        shape-margin: 2em;
      }
    }
  }
}
```

## Personalización de Colores:
Notar que cada `article` tiene una clase como ser `.shoe-red`, `.shoe-white`, `.shoe-blue` que redefine las variables de modo de establecer la paleta para dicha clase. 

```scss
.shoe-red {
  --clr-inner: #faa700;
  --clr-outer: #e48d00;
  --clr-accent: #a1173c;
}

.shoe-white {
  --clr-inner: #fce4b3;
  --clr-outer: #eac886;
  --clr-accent: #2f4858;
}

.shoe-blue {
  --clr-inner: #6dd5ed;
  --clr-outer: #2193b0;
  --clr-accent: #008951;
}
```

Lo mismo hacemos para los colores del `.hero`

```scss
.hero {
	--clr-accent: #faa700;
}
```

== 1h 3==