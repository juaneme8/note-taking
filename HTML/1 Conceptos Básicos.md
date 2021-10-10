# Introducción
Tim Berners Lee fue el creador de HTML en 1991 y fundador de W3C (*World Wide Web Consortium*) en 1994 que supervisa el desarrollo continuo de la web.

WHATWG (*web hypertext application technology working group*) fue fundado en 2004 a raíz de un desacuerdo en un meeting de W3C al recibir el anuncio de la decisión de centrarse en XHTML como evolución del HTML. Fue formado por organizaciones como Apple, la Fundación Mozilla y Opera Software.

La W3C y WHATWG  se encargan de fijar standards de lenguajes y trabajaron de manera conjunta en HTML5. Seguir estos standards nos permitirá crear sitios profesionales y accesibles.

HTML (*HyperTex Markup Language*), no es un lenguaje de programación sino un lenguaje de marcas o etiquetas con las cuales le decimos al navegador qué debe mostrar en pantalla.

Para iniciar un documento html en Visual Studio Code ponemos `html:5 + TAB` si bien hay varias formas esta es la única que incluye `<!DOCTYPE html>`

# Estructura HTML
```html
<!DOCKTYPE html>
<html>
	<head>
		<title> Título de Página </html>
	</head>
	<body>
		...
	</body>
</html>
	
```
En el `<head>`incluimos información para los robots de búsqueda y vinculación con css.

## DOCTYPE:
El docktype es la declaración del tipo de documento, con lo cual indicamos al navegador que nuestro documento sigue la **estructura de un DTD** (*Document Type Definition*) concreto indicando con ello qué versión de html vamos a utilizar.
Es obligatorio y fundamental para la correcta interpretación de las etiquetas que utilicemos.
El docktype fue mutando en las distitnas versiones, logrando simplificarse notablemente en html5 donde basta con poner `<!DOCKTYPE html>`, la estructura inicial será:


## `<meta>`
Prove información adicional sobre el documento tanto para el navegador como para los robots de búsqueda. 

```html
<meta charset="UTF-8" />
<meta name="description" content="Free Web tutorials">  
<meta name="keywords" content="HTML, CSS, JavaScript">  
<meta name="author" content="John Doe">
<meta name="viewport" content="width=device-width, initial-scale=1.0" />
```

# Elementos Semánticos
A partir de HTML5 comenzaron a existir los contenedores semánticos: `<header>`,`<footer>`,`<main>`,`<section>`,`<article>`,`<aside>`,`<address>`, `<nav>`
En cambio `<div>` y `<span>` son contenedores no semánticos y se utilizan cuando no me interesa expresar qué tipo de contenido tengo.

Todos los elementos que tienen un valor semántico nos servirán a la hora de brindar accesibilidad y posicionamiento web. 

Debemos usar sólo un `<h1>` y `<h2>` por página. Esto será importante en términos de posicionamiento web y seo.

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
* Sitemap o mapa del sitio
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

## `<hr />`
El elemento `<hr>` en versiones anteriores se utilizaba para representar una línea horizontal (*horizontal rule*), en la actualidad tiene un **significado semántico** de un cambio de tema  o para separar contextos.
Si lo que queremos es una línea debemos usar el CSS apropiado

# Elementos HTML
Los elementos se construyen a partir de etiquetas de apertura `<h1>` y etiquetas de cierre `</h1>` o bien sólo de etiquetas de apertura `<br>`
Las etiquetas no son case sensitive pero por convención las pondremos en minúsculas. Todo contenido debe estar dentro de un elemento.

## Atributos
Los atributos aportan información complementaria sobre los elementos HTML. Determinados atributos se pueden aplicar a cualquier elemento `id`, `class`, `style` 
`<elemento atributo="valor">Contenido</elemento>`

## Comentarios
Podemos poner comentarios con `<!-- Comentario -->` o en Visual Studio Code con `CONTROL + /`

## `<br>`
`<br>` se trata de un elemento de salto de línea.

## `<h1>...<h6>`
Los enunciados `<h1>`,`<h2>`,`<h3>`,`<h4>`,`<h5>`, `<h6>` son elementos de bloque, `<h1>` lo utilizamos para el título más importante del a página y `<h6>` para el menos importante.

El uso de estos enunciados tiene una **importancia a nivel semántico** dado que para los robots de búsqueda aquello contenido dentro de `<h1>` será lo más importante de la página.

## `<p>`
Los párrafos `<p>` son elementos de bloque.

```html
<h1>Titulo Principal</h1>
<p>Contenido del párrafo</p>
<h2>Titulo Secundario</h2>
<p>Contenido del párrafo</p>
```
El primer párrafo es el más importante por estar debajo de `<h1>`

## `<strong>`, `<em>`, `<b>`, `<i>`
`<strong>`, `<em>`, `<b>`, `<i>` son elementos de línea ocupan sólo el espacio del texto que contienen.

`<strong>` y `<em>` tienen un valor semántico de importancia para el navegador y los robots de búsqueda. Sin embargo `<b>` e `<i>` ofrecen el mismo estilo que los primeros.  

El estilo debemos darlo utilizando CSS y no elementos como `<b>` e `<i>`

## `<ul>`
Las listas desordenadas `<ul>` son elementos de bloque

## `<ol>`
Las listas ordenadsa `<ol>` son elementos de bloque

## `<blockquote>`
El elemento `<blockquote>` sirve para utilizar en prólogos y citas. Veremos que tiene un margen izquierdo mayor que el del elemento `<p>`

# Imágenes
`<img src="/img/imagen.png' alt="descripcion imagen">` en el atributo `alt` colocamos una descripción para brindar accesibilidad, posicionamiento web y también es útil si la imagen no carga. Sin embargo, **si la descripción no aporta información, basta con poner el atributo vacío** `alt=""`, en lugar de poner por ejemplo `alt="logo"` que no tiene sentido que el que el screen reader diga logo.

* Decimos que los `<img>` son elementos de línea bloque, ya que por un lado se comportan como elementos de línea ya que pueden ir una al lado de la otra y por otra parte como elementos de bloque ya que se les puede especificar ancho y alto.  

## MapaTipos de Imágenes
Los **mapas de imágenes** permiten definir una serie de* `.gif`: 256 colores, admite canal alpha
* `.jpg`: 16 millones de colores
* `.png`: Admite canal alpha y una compresión sin pérdidas
* `.svg`: *scalable vector graphics*. es un formato de gráficos vectorialesen formato XML desarrolado por W3C.

## Mapas de Imágenes `<map>`
Los mapas de imagen nos permiten crear zonas clickeables dentro de una imagen yde modo tal que con cada una de ellstas áreas apuntaremos a una URL distinta. En la actualidad no son muy usados.url diferente.

Generador online: [http://www.image-maps.com/](http://www.image-maps.com/)

## `<iIframe>`
Antiguamente los iframes permitían cargar información sin la necesidad de refrescar la página.
En la actualidad para recargar información parcial de una página contamos con AJAX y los usamos para insertar mapas de google maps o videos de youtube.

### Insertar Mapa Google Maps
Buscar la dirección deseada, ir a Compartir y copiar el `<iframe>` que aparece en "Insertar Mapa"

### Insertar Video Youtube
De manera similar vamos a Compartir y luego incorporar un mapa y luego copiamos el `<iframe>`

## `<canvas>`
Se trata un contenedor sobre el cual podemos dibujar utilizando JavaScript.
```html
<canvas id="contenedor"></canvas>
<script>
	const miDibujo = document.getElementById('contenedor');
	const c = miDibujo.getContext("2d");
	c.moveTo(0,0);
	c.lineTo(200,100);
	c.stroke();
</script>
```

# Emoji Unicode
Los emojis parecen ser imágenes, pero no lo son. Se trata de letras del set de caracteres UTF-8 (unicode).

Podemos encontrar una tabla completa en [w3schools](https://www.w3schools.com/charsets/ref_emoji.asp)

Por ejemplo para mostrar un león podremos hacerlo con el decimal `&#29409` o con el hexadecimal `&#x1F981`
```html
<p style="font-size:100px">&#129409;</p>
<p style="font-size:100px">&#x1F981;</p>
```