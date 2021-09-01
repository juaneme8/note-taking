# `@media`
La regla `@media` es utilizada en *media queries* para aplicar distintos estilos para diferentes dispositivos.
Los *media queries* son utilizadas en el diseño responsivo para entregar una hoja de estilos a la medida del dispositivo.También es posible establecer estilos para documentos impresos o *screen readers*.
Los *media queries* se pueden especificar tanto en la hoja de estilos como al referenciar archivos externos: `<link href="estilos" rel="stylesheet" media="print">`


Si queremos tener un color `ligthblue` si la pantalla es menor a 400px, `ligthgreen` si es de más de 400px y menos de 800px y `lavander` si es de más de 800px.

```css
body {  
	background-color:  lightblue;  
}  
  
@media screen and (min-width: 400px) {  
	body  {  
		background-color:  lightgreen;  
	}  
}  
  
@media screen and (min-width: 800px) {  
	body  {  
		background-color:  lavender;  
	}  
}
```

## Viewport
El **viewport** es el área visible por el usuario de una página web. Es posible controlar el viewport utilizando el tag `<meta>` que debemos incluir en todas las páginas.

```html
<meta name="viewport"  content="width=device-width, initial-scale=1.0">
```

Con `width=device-width` establecemos el ancho de la página en función del ancho de la pantalla del dispositivo y `initial-scale=1.0` determina el zoom inicial.
En caso de que **no** tengamos este tag, veremos que el sitio aparece escalado de manera que entre toda la página en la pantalla.

# Mobile First
> [The Net Ninja - Mobile First](https://www.youtube.com/playlist?list=PL4cUxeGkcC9hH1tAjyUPZPjbj-7s200a4) - [Repositorio](https://github.com/iamshaunjp/responsive-css-grid-build)
> [Kevin Powell - Responsive CSS](https://www.youtube.com/watch?v=0ohtVzCSHqs)

## Filosofía
# Mobile First:
La implementación *mobile first* significa que comenzaremos desarrollando el sitio en primer lugar para pantallas de teléfonos móviles y luego pasaremos a pantallas más grandes como tablets, laptops y desktop. 
Esta filosofía es la opuesta a la que solía implementarse en el comienzo de los *mobile sites* ya que en ese entonces se contaba con *desktop sites* y lo que se hacía era apretar el contenido (ocultando cosas que no entraban en oportunidades), atentando contra la UX y logrando páginas diseñadas pobremente. Como el espacio es limitado en un teléfono debemos pensar bien qué contenido incorporar y dónde, es por eso que se adopta cada vez más esta metodología.

De todos modos no debemos perder de vista las métricas de nuestro sitio para así saber qué cantidad de tráfico móvil tenemos respecto del tráfico total. 

# Ejemplo The Net Ninja
Para esta solución utilizaremos Grid CSS en varios componentes por lo que por simplicidad les daremos la clase `.grid` para no tener que darles estilo individualmente.

Como es habitual utilizamos la extensión de Visual Studio Code `Live Server` para contar con un *local development server* para previsualizar el sitio en el navegador con *live refresh* de modo de ver los cambios sin la necesidad de refrescar.

## Template html
```html
 <nav class="site-nav grid">
    <h1>Cosmo Junkie</h1>
    <ul>
      <li><a href="#portfolio">Portfolio</a></li>
      <li><a href="#skills">Skills</a></li>
      <li><a href="#contact">Contact</a></li>
    </ul>
  </nav>
  <section id="welcome" class="grid">
    <div class="welcome-text">
      <h2>Space Enthusiast<br />& JavaScript Developer</h2>
      <p class="leading">Lorem ipsum dolor sit amet, consectetur adipiscing elit. In commodo consequat.</p>
      <a href="#portfolio" class="button">View my work</a>
    </div>
    <div class="welcome-img">
      <img src="assets/banner_image.png" alt="pic of planet">
    </div>
  </section>
  <section id="portfolio">
    <h3>Some of my Projects</h3>
    <div class="projects grid">
      <a href="#">
        <img src="assets/project_1.png" alt="space race image">
        <h4>Space Race Game</h4>
      </a>
      <a href="#">
        <img src="assets/project_2.png" alt="planet boy image">
        <h4>Planet Boy API</h4>
      </a>
      <a href="#">
        <img src="assets/project_3.png" alt="captain cosmo image">
        <h4>Captain Cosmo Blog</h4>
      </a>
    </div>
  </section>
  <section id="skills">
    <h3>Things I Can Do</h3>
    <ul class="grid">
      <li>
        <img src="assets/comet_1.svg" alt="comet">
        <h4>JavaScript</h4>
      </li>
      <li>
        <img src="assets/comet_2.svg" alt="comet">
        <h4>React</h4>
      </li>
      <li>
        <img src="assets/comet_3.svg" alt="comet">
        <h4>Firebase</h4>
      </li>
      <li>
        <img src="assets/comet_4.svg" alt="comet">
        <h4>Flutter</h4>
      </li>
    </ul>
    <p class="leading">Lorem ipsum dolor sit amet, consectetur adipiscing elit.</p>
  </section>
  <section id="contact">
    <h3>Get In Touch</h3>
    <p class="leading">Lorem ipsum dolor sit amet, consectetur.</p>
    <form>
      <input type="text" placeholder='NAME'>
      <input type="email" placeholder='EMAIL'>
      <textarea placeholder='YOUR MESSAGE'></textarea>
      <button class="button">Send Flare</button>
    </form>
  </section>
  <footer>
    <div class="grid">
      <p class="copyright">Copyright 2020 Cosmo Junkie</p>
      <ul class="social">
        <li><a href="#"><img src="assets/icon_fb.svg" alt="facebook"></a></li>
        <li><a href="#"><img src="assets/icon_tw.svg" alt="twitter"></a></li>
      </ul>
    </div>
  </footer>
```

## Clase Grid
Para este proyecto para todos los grid que tenemos utilizaremos un *8-column grid* es por eso fue que les asignamos a todos los contenedores la clase `.grid` a modo de reutilizar las reglas de estilo. 

```css
.grid{
	display: grid;
	grid-template-columns: repeat(8,1fr);
}
```

## Imágenes
Colocar todas las imágenes del proyecto en la carpeta `/assets` o `/img`

## Media Queries
Como normalmente para el *mobile layout* no son necesarios estilos muy avanzados e incluso sin CSS ya es responsive por default no utilizamos un *media query* específico sino que los ubicamos junto con los estilos genéricos.

```css
/* mobile styles*/

/*small tablets styles*/
@media screen and (min-width: 620px){

}
/*large tablets and laptops styles*/
@media screen and (min-width: 960px){
}

/*desktop styles*/
@media screen and (min-width: 1200px){
}
```

> Otro punto de vista: Algunos aconsejan utilizar como unidad para los media queries **em** en lugar de px o rem. Esto lo justifican con pruebas en distintos navegadores, haciendo zoom, cambiando el tamaño de fuente default del navegador, etc. Tener esto presente en caso de un comportamiento inesperado.

## Estilo Botones
```css
.button{
	background: none; /*si lo aplicamos sobre un <button> de form*/
	border: 2px solid var(--primary);
	color: var(--primary);
	padding: 6px 12px;
	border-radius: 20px;
	text-transform: uppercase;
	box-shadow: 1px 2px 3px rgba(0,0,0,0.6);
	display: inline-block; /*para que nos tome el margin-top*/
}
.button:hover{
	color: #222;
	background: var(--primary);
}
```
## Estilo Formulario 
```css
input,textarea{
  background: rgba(255,255,255,0.05);
  padding: 10px 16px;
  border-radius: 20px;
  border: 2px solid #9893D8;
  color: #f2f2f2;
}
```
## Estilo Títulos
```css
 h1,h2,h3,h4{
  font-weight: normal;
  line-height: 1.4em;
}
```
Por default los headings desde `<h1>` hasta `<h6>`  tiene un `font-weight:bold` (que mapea con un valor numérico 700), le ponemos `font-weight:normal` (equivalente a 400) ya que así es la fuente que estamos utilizando.
Recordemos que si la fuente tiene la versión bold o normal como parte de la familia, el navegador la usará. En cambio, si estas versiones no están disponibles el navegador imitará su propia versión bold o normal y esto es lo que queremos evitar.


## Fuentes Responsive
```css
p,a,li{
  color: #9893D8;
  line-height: 1.4em;
  font-size: 1em;
}
h1, h3{
  font-size: 1.2em;
}
h2{
  font-size: 1.6em;
}
h4{
  font-size: 1.1em;
}
.leading{
  font-size: 1.1em;
}
```

Establecemos los `font-size` en **em** debido a que luego para cada *media querie* vamos a cambiar el valor de dicha propiedad en el `<body>` y como em se fija en el padre (y ninguno de ellos tiene font-size configurado, sigue "subiendo" hasta llegar al `<body>` de donde toma el valor de referencia).

Desde el punto de vista de accesibilidad no está bueno definir font-size en px para el `<body>` dado que de esta manera estamos ignorando si el usuario aumentó la default font-size del navegador.

## Imagenes Responsive
Pará lograr que las imágenes se adapten de acuerdo a los distintos *media queries*, podemos colocarlas dentro de una contenedor grid (serán *grid items*). 
Supongamos que tenemos:
```html
<div class="projects grid">
	<a href="#">
		<img src="assets/project_1.png" alt="space race image">
		<h4>Space Race Game</h4>
	</a>
	<-- (+ proyectos...) --!>
</div>
```
Si creamos un *12-column grid*, veremos que si bien inicialmente cada *grid item* ocupa una celda, las imagenes están ocasionando el stretch de dichas celdas, para eso debemos asegurarnos que tengan como ancho máximo el ancho del contenedor.
```css
.projects img{
	max-width: 100%;
}
```

De la misma manera en ocasiones queremos que la imagen sea más grande que el contenedor para generar un efecto como que se escapa de la pantalla y en ese caso le pondremos  `max-width: 200%` acompañado de `overflow-x: hidden;` en el `<body>` para que no nos muestre la barra de scroll.

## Ubicación de Grid Items
Para posicionar a los elementos contenidos dentro del `<div class='projects grid'>` como sabemos que son 3 podemos recurrir a seleccionar cada uno de ellos usando la pseudo clase `:nth-child()`
```css
.projects a:nth-child(1){
  grid-column:  2/6;
  grid-row: 1;
}
.projects a:nth-child(2){
  grid-column:  8/12;
  grid-row: 1;
}
.projects a:nth-child(3){
  grid-column:  5/9;
  grid-row: 2;
}
```

## .site-nav
No queremos mostrar en los estilos de móvil los links de las distintas secciones sino que queremos que el usuario haga scroll:

```css
.site-nav ul{
	display:none;
}
```