# Responsive Navbar c/ Hamburger
> Los archivos de este proyecto están en `/responsive-navbar`> Los archivos de este proyecto están en `/responsive-navbar`

Se utilizó BEM como *naming convenition* sin embargo notar que cuando anidamos dos niveles conservamos el nombre del contenedor externo. Basado en las [FAQ de BEM](http://getbem.com/faq/) 
> According to BEM method, block structure should be flattened; you do not need to reflect nested DOM structure of the bloc

## `index.html`
```html
<nav class="navbar">
	<div class="navbar__brand">Metanoia</div>
	<a href="#" class="navbar__toggle">
		<span class="navbar__bar"></span>
		<span class="navbar__bar"></span>
		<span class="navbar__bar"></span>
	</a>
	<div class="navbar__links">
		<ul>
			<li><a href="#">Inicio</a></li>
			<li><a href="#">Nosotros</a></li>
			<li><a href="#">Servicios</a></li>
			<li><a href="#">Contacto</a></li>
		</ul>
	</div>
</nav>
```

## `style.css`
```css
@import url('https://fonts.googleapis.com/css2?family=Montserrat&display=swap');

:root {
	--primary-clr: #333;
	--secondary-clr: #555;
	--light-clr: #fff;
}

* {
	box-sizing: border-box;
}
body {
	margin: 0;
	padding: 0;
	font-family: 'Montserrat', sans-serif;
}
.navbar {
	display: flex;
	/* position: relative; */
	flex-direction: row;
	align-items: center;
	justify-content: space-between;
	background-color: var(--primary-clr);
	color: var(--light-clr);
}

.navbar__brand {
	font-size: 1.5em;
	text-transform: uppercase;
	margin: 0.5em;
}

.navbar__links ul {
	margin: 0;
	padding: 0;
	display: flex;
}

.navbar__links li {
	list-style: none;
}

.navbar__links li a {
	text-decoration: none;
	color: var(--light-clr);
	padding: 1rem;
	display: block;
}

.navbar__links li:hover {
	background-color: var(--secondary-clr);
}

.navbar__toggle {
	position: absolute;
	top: 0.75rem;
	right: 1rem;
	width: 30px;
	height: 21px;
	display: none;
	flex-direction: column;
	justify-content: space-between;
}

.navbar__bar {
	height: 3px;
	width: 100%;
	background-color: var(--light-clr);
	border-radius: 1em;
}

/*MOBILE STYLES*/
@media (max-width: 600px) {
	.navbar {
		flex-direction: column;
		align-items: flex-start;
	}
	.navbar__links {
		display: none;
		width: 100%;
	}
	.navbar__links.active {
		display: flex;
	}
	.navbar__links ul {
		flex-direction: column;

		width: 100%;
	}
	.navbar__links li {
		text-align: center;
	}

	.navbar__links li a {
		padding: 0.5rem;
	}

	.navbar__toggle {
		display: flex;
	}
}
```
### Comentarios:
`.navbar__links li a`: Ponemos `display: block` para que me tome el padding top y bottom, recordemos que en los elementos de línea ignora estos parámetros.

`.navbar__bar` `width: 100%`; porque queremeos que tenga el 100% del padre, es decir de navbar__toggle

Definimos los estilos para todas las pantallas y luego agregamos un media querie para pantallas de hasta 600px. En esa versión mostramos el `navbar_toggle` y ocultamos los `.navbar__links` a menos que tengan también la clase `.active`


# Hamburger Animation
> Los archivos de este proyecto están en `/hamburger-animation`

El propósito de este proyecto es diseñar un *burger button* que al hacer click se convierte en una cruz. A diferencia del proyecto anterior, en este no tendremos tres elementos `<span>` con la clase .bar sino que tendremos una sóla y agregaremos las restantes con pseudoelementos `:after` y `:before` 

En este caso queremos tener el botón en el medio de la pantalla, por lo que en `body` debemos definir `min-height: 100vh;` de lo contrario tendrá la altura dada por la altura de `.menu-btn`

`.menu-btn__burger::before,.menu-btn__burger::after `  trabajo con `position: absolute` porque necesito darle alto y ancho (y todos los elementos con `position: absolute` son automáticamente tratados como `display:block`). Como `.menu-btn` tiene `position:relative` si pongo `top:0` subiría hasta la parte de arriba del cuadrado del botón.

En cuanto a la animación cuando hacemos click queremos que la línea del medio desaparezca `background: transparent` y un desplazamiento hacia la izquierda `transform: translateX(-50px)`

Cuando utilizamos `transform: translate(50px) rotate(45deg);` debemos tener presente que el orden de ejecución es de derecha a izquierda es decir que primero rotará y luego transladará. Nótese que simplemente transladamos en la dirección contraria a la que lo hicimos en la barra del medio al desaparecerla.

Implementamos distintos tipos de efectos:
* Desplazamiento y desaparición de la barra central hacia la izquierda
* Achicamiento y desaparición de la barra central
* Achicamiento y conversión del contenedor en círculo:

# Sticky Navbar
> Los archivos de este proyecto están en `/sticky-navbar`> 

> Basado en mplementación de [OnlineTutorials](https://www.youtube.com/watch?v=6HFpw5fcaD8)


Si bien el elemento `<header>` tiene por default `display: block` como estamos poniendo `position: fixed` este es removido del flow del documento. Es por ese motivo que tendremos que especificar `width: 100%` o lo que es lo mismo `left:0` y `right:0`

En cuanto a los estilos de los textos tanto a `header .logo`  como a `header ul li a` le decimos `letter-spacing: 2px`

En el script de JavaScript queremos agregar la clase `sticky` si realizamos un scroll vertical, esto lo hacemos de la siguiente forma: `header.classList.toggle('sticky', window.scrollY > 0);` es decir que el primer cambio se producirá cuando `window.scrollY > 0` pase a ser `true` y luego volverá a cambiar cuando vuelva a ser `false`.

```html
<body>
	<header>
		<a href="#" class="logo">Metanoia</a>
		<ul>
			<li><a href="#">Inicio</a></li>
			<li><a href="#">Nosotros</a></li>
			<li><a href="#">Servicios</a></li>
			<li><a href="#">Contacto</a></li>
		</ul>
	</header>
	<script src="/js/main.js"></script>
</body>
```

```css
@import url('https://fonts.googleapis.com/css2?family=Poppins:wght@400;700&display=swap');

* {
	margin: 0;
	padding: 0;
	box-sizing: border-box;
	font-family: 'Poppins', sans-serif;
}
body {
	background-color: #000;
	min-height: 200vh;
}
header {
	position: fixed;
	top: 0;
	left: 0;
	right: 0;
	display: flex;
	justify-content: space-between;
	padding: 40px 100px;
	align-items: center;
	background-color: #270949;
	transition: all 0.5s ease-in-out;
}

header.sticky {
	padding: 5px 100px;
	background-color: #e6e9f5;
}

header.sticky .logo,
header.sticky ul li a {
	color: #270949;
}

header .logo {
	/* estilo fondo */
	/*background-image: url('../img/logo.svg'); 
	background-repeat: no-repeat;
	background-size: 100%;
	width: 50px;
	height: 50px;*/

	/* estilo texto */
	font-weight: 700;
	letter-spacing: 2px;
	font-size: 1.5em;
	text-transform: uppercase;
	text-decoration: none;
	color: #e6e9f5;
	/* ¿? */
	/* transition: all 0.5s ease-in-out; */
	/* position: relative */
}

header ul {
	list-style: none;
	display: flex;
	justify-content: center;
	align-items: space-between;
	/* ¿? */
	/* position: relative */
}

header ul li {
	list-style: none;
	/* ¿? */
	/* position: relative */
}

header ul li a {
	margin: 0 15px;
	text-decoration: none;
	letter-spacing: 2px;
	color: #e6e9f5;
	/* ¿? */
	/* position: relative */
	/* transition: all 0.5s ease-in-out; */
}
```
```js
window.addEventListener('scroll', () => {
	const header = document.querySelector('header');
	header.classList.toggle('sticky', window.scrollY > 0);
});

```

# Smoth Scroll
> Basado en video de Traversy Media

Utilizando CSS puro con `scroll-behavior` para suavizar el scroll y `scroll-snap` para hacer snap sobre la sección sobre la que nos encontremos mayoritariamente al scrollear. También es posible utilizar las flechas para desplazarnos de una sección a la otra.
El único inconveniente de este método es *browser compatibility* como podemos ver en [caniuse.com](https://caniuse.com/#search=scroll-behavior) pero aún usandolo en aquellos navegadores donde no es compatible, no se romperá el sitio sino que simplemente no se apreciará el efecto. 
Otra opción es en lugar de utilizar CSS puro, requirrir a alguna de las alternativas que utilizan javascript puro, jquery, o algun framework de css (como bootstrap), etc

== INCOMPLETO ==

# CSS-Only Accordion
> Basado en el video de [dcode - CSS-Only Accordion (Mobile Friendly) ](https://www.youtube.com/watch?v=pzy_QStQaqA)
> Proyecto completo en /css-accordion

Podemos hacerlo de dos modos: el que permite un múltiples secciones expandidas a la vez y también nos permite cerrarlo una vez expandida (basado en *checkboxes*) y el que permite sólo una sección a la vez y no es posible volver a una versión de todos los elementos cerrados luego (basado en *radio buttons*).

Con `max-width: 400px;` nos aseguramos que sea *responsive* intentará tener 400px y si la pantalla es pequeña se achicará.

Con `overflow: hidden` nos aseguramos que el `border-radius` se vea bien cuando tenemos *child elements* que podrían quedar fuera (*bleed out*) del contenedor (suponiendo que estos tengan un `width:100%`).

Establecemos el color de fondo al definir los estilos de `.accordion` y luego en `.accordion__label:hover` ponemos `background-color: rgba(0, 0, 0, 0.1);` con lo cual estaremos oscureciendo un poco ese color. De esta manera **evitamos tener que establecer dos colores primarios.**

Notar que en `.accordion__label::after` pusimos `right:20px` de modo que sea consistente con el `padding-right: 20px` establecido para `.accordion__content` y `accordion__label`

Como queremos lograr que si el radio button o el checkbox están checkedos se muestre el contenido en primer lugar lo ocultamos y luego con el selector `.accordion__input:checked ~ .accordion__content` ponemos `display: block`. Trabajamos con `~` los *general sibings* y es por eso que tenemos los elementos dentro de un `<div>` (de modo que no sean todos hermanos).


Cuando el *checkbox* está activo, queremos trabajar con el label `.accordion__input:checked ~ .accordion__label::after ` y rotar la flecha (logrando que permanezca centrada) `transform: translateY(-50%) rotate(0.5turn);`


```html
<div class="accordion">
			<div>
				<input type="checkbox" name="example_accordion" id="section1" class="accordion__input" />
				<label for="section1" class="accordion__label">Section #1</label>
				<div class="accordion__content">
					<p>
						Lorem ipsum dolor sit amet consectetur adipisicing elit. Dicta, cumque. Commodi repudiandae officia provident quisquam
						assumenda molestias corporis nulla ab facere iusto mollitia, veritatis dolor, ut quaerat atque perspiciatis possimus.
					</p>
				</div>
			</div>
			<div>
				<input type="checkbox" name="example_accordion" id="section2" class="accordion__input" />
				<label for="section2" class="accordion__label">Section #2</label>
				<div class="accordion__content">
					<p>
						Lorem ipsum dolor sit amet consectetur adipisicing elit. Dicta, cumque. Commodi repudiandae officia provident quisquam
						assumenda molestias corporis nulla ab facere iusto mollitia, veritatis dolor, ut quaerat atque perspiciatis possimus.
					</p>
				</div>
			</div>
			<div>
				<input type="checkbox" name="example_accordion" id="section3" class="accordion__input" />
				<label for="section3" class="accordion__label">Section #3</label>
				<div class="accordion__content">
					<p>
						Lorem ipsum dolor sit amet consectetur adipisicing elit. Dicta, cumque. Commodi repudiandae officia provident quisquam
						assumenda molestias corporis nulla ab facere iusto mollitia, veritatis dolor, ut quaerat atque perspiciatis possimus.
					</p>
				</div>
			</div>
		</div>
```

## style.css
```css
@import url('https://fonts.googleapis.com/css2?family=Poppins:wght@400;700&display=swap');
* {
	box-sizing: border-box;
	font-family: 'Poppins', sans-serif;
}
.accordion {
	max-width: 400px;
	box-shadow: 0 0 10px rgba(0, 0, 0, 0.2);
	border-radius: 5px;
	overflow: hidden;
	background-color: #f95b3d;
}
.accordion__label,
.accordion__content {
	padding: 14px 20px;
}

.accordion__label {
	display: block;
	color: #e6e9f5;
	cursor: pointer;
	position: relative;
	transition: background 0.1s;
}

.accordion__label:hover {
	background-color: rgba(0, 0, 0, 0.1);
}

.accordion__label::after {
	content: '';
	position: absolute;
	right: 20px;
	top: 50%;
	transform: translateY(-50%);
	height: 6px;
	width: 12px;
	background-image: url('data:image/svg+xml;utf8,<svg width="100" height="50" xmlns="http://www.w3.org/2000/svg"><polygon points="0,0 100,0 50,50" style="fill:%23FFFFFF99;" /></svg>');
	background-size: contain;
	transition: transform 0.4s;
}

.accordion__content {
	background-color: #e6e9f5;
	font-size: 0.85em;
	line-height: 1.6em;
	display: none;
}

.accordion__input {
	display: none;
}

.accordion__input:checked ~ .accordion__content {
	display: block;
}

.accordion__input:checked ~ .accordion__label::after {
	transform: translateY(-50%) rotate(0.5turn);
}

```

## Ejemplo Estructura Card:
```html
<div class="card card-important">
	<h3 class="card__title">Titulo</h3>
	<p class="card__price">Lorem ipsum</p>
	<div class="card__body">
		<p>parrafo 1</p>
		<p>parrafo 2</p>
		<a href="" class="button button--card">boton</a>
	</div>
</div>
```