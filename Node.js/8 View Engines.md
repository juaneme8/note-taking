# Templating Engines
Ante un request la respuesta puede ser un JSON Object o HTML.
Hasta el momento los datos devueltos por nuestra aplicación express  son **estáticos** (html) en muchas ocasiones vamos a querer que esos datos sean **dinámicos** como ser datos de una db (por ejemplo datos de usuario para ver su perfil, posteos de un blog, etc). Esto podemos lograrlo con un **View Engine** o **Template Engine**, que como su nombre lo indica no son más que **motores para generar plantillas dinámicas de html** que gracias a Express pueden ser usados fácilmente. Nos permiten escribir con una sintaxis de html pero a su vez incorporando datos dinámicos y lógica que luego le entregamos al navegador.

A la hora de desarrollar servicios RESTful como backend de aplicaciones clientes no son necearios los templating engines. Sino que son necesarios cuando tenemos **server side rendering**.

Existen distintas opciones:
* EJS
* Pug
* Handlebars
* Express Handlebars
* Jade
* Mustache

## EJS
```bash
npm install ejs
```
A continuación debemos especificar que queremos usar ejs como view engine lo cual lo hacemos con `app.set('view engine', 'ejs');`Los diferentes templates serán buscados por Express por default en la carpeta **views**. Sin embargo si quisiéramos colocarlos en otra ubicación por ejemplo en la carpeta `myViews` podríamos indicarlo agregando`app.set('views', './myViews')`.
Entonces en la carpeta `views` inicialmente creamos los archivos `index.ejs`, `about.ejs`,`404.ejs` y colocamos en ellos por el momento html puro.
Anteriormente ante un request en la ruta home, en el handler enviábamos el archivo `res.sendFile('./views/index.html', {root: __dirname})` ahora en cambio renderizaremos una view `res.render('index')` (notar que no ponemos el nombre de la extensión.
Notar que si queremos especificar el **status code** podemos hacerlo de manera similar a como lo hacíamos antes encadenando métodos: `res.status(404).render('404', { title: '404' });` 
Además agregamos el handler para la ruta `/blogs/create'` y la vista `create.ejs`

```js
const express = require('express');

// express app
const app = express();
// listen for requests
app.listen(3000);
// register view engine
app.set('view engine', 'ejs');

//en caso de querer una ubicación distinta a views
// app.set('views', 'myviews'); 

app.get('/', (req, res) => {
	res.render('index');
});

app.get('/about', (req, res) => {
	res.render('about');
	});
	
app.get('/blogs/create', (req, res) => {
	res.render('create');
});

// 404 page
app.use((req, res) => {
	res.status(404).render('404');
});
```
### Pasaje de Datos a las Vistas
Por el momento sólo estamos renderizando html puro, para incorporar datos dinámicos debemos usar **ejs tags** los cuales comienzan con `<%` y terminan con `%>` Por ejemplo: 
`<% const name = 'mario'%>`este código escrito en Vainilla JavaScript correrá en el server. Luego para mostrarlo en pantalla`<%= mario %>` notar que cuando se trata de un  valor a mostrar (*outputting a value*) debemos comenzar con `<%=` en lugar de `<%`.
Para pasarle datos desde el handler hacia la vista podemos hacerlo como un segundo parámetro del método `res.render()`este será un objeto y podremos acceder desde la vista a sus propiedades (de manera directa como si fueran variables, no hace falta hacer referencia al objeto).
Por ejemplo podríamos poner en el handler (en `app.js`):
```js
app.get('/', (req, res) => {
	res.render('index', { title: 'Home'});
});
```
Luego para utilizarlo en `index.ejs` por ejemplo para modificar el título:
```html
<title>Blog Ninja | <%= title %></title>
```
También es posible pasarle desde el handler a la vista un array objetos para recorrerlo (*cycle through)* luego desde la vista y mostrarlos en pantalla.
```js
const blogs = [
		{title: 'Yoshi finds eggs', snippet: 'Lorem ipsum'},
		{title: 'Mario finds stars', snippet: 'Lorem ipsum'},
		{title: 'How to defeat bowser', snippet: 'Lorem ipsum'},
	];
	res.render('index', { title: 'Home', blogs });
```
 En la vista en primer lugar debemos chequear que el array no esté vacío.
Luego vamos a recorrer los objetos del array con el método `blogs.forEach()` y para cada uno de ellos ejecutaremos el callback con el contenido como argumento.
```js
<% if (blogs.length > 0) { %>
	<% blogs.forEach(blog => { %>
		<h3 class="title"><%= blog.title %></h3>
		<p class="snippet"><%= blog.snippet %></p>
	<% }) %>
<% } else { %>
<p>There are no blogs to display...</p>
<% } %>
```

A continuación podemos ver el código completo de `app.js`
```js
const express = require('express');

// express app
const app = express();
// listen for requests
app.listen(3000);
// register view engine
app.set('view engine', 'ejs');

//en caso de querer una ubicación distinta a views
// app.set('views', 'myviews'); 

app.get('/', (req, res) => {
	const blogs = [
		{title: 'Yoshi finds eggs', snippet: 'Lorem ipsum'},
		{title: 'Mario finds stars', snippet: 'Lorem ipsum'},
		{title: 'How to defeat bowser', snippet: 'Lorem ipsum'},
	];
	res.render('index', { title: 'Home', blogs });
});

app.get('/about', (req, res) => {
	res.render('about', { title: 'About' });
	});
	
app.get('/blogs/create', (req, res) => {
	res.render('create', { title: 'Create a new blog' });
});

// 404 page
app.use((req, res) => {
	res.status(404).render('404', { title: '404' });
});
```

> Otra opción hubiera sido enviar html puro y desde el frontend acceder a los datos y renderizarlos dinámicamente.

### Funcionamiento EJS Templates:
Cuando queremos renderizar un **archivo .ejs**, este será pasado al **EJS view engine** que se encarga de buscar todo contenido dinámico, variables, loops, etc y generar el html resultante para esas partes y termina con un código html (basado en el template y los datos resultantes) que es retornado al navegador. Este proceso recibe el nombre de **Server Side Rendering**.

### Partials:
Cuando en varios archivos tenemos código repetido (algo que sucede a menudo con los navigation bar), esto se vuelve poco eficiente ya que en caso de tener que hacer una modificación deberíamos replicarla en todos estos archivos.

Esto lo podemos solucionar en Express y EJS haciendo uso de los **partial templates** que consiste en un único archivo con ese código que podrá ser importado en las distintas vistas para reutilizarlo. Como consecuencia de esto en caso de tener que hacer cambios sólo tendremos que modificar un archivo.
En `/views/` creamos una carpeta **partials**. En ella creamos un archivo `nav.ejs` donde pegamos el código que se repite.
```js
<nav>
	<div class="site-title">
		<a href="/"><h1>Blog Ninja</h1></a>
		<p>A Net Ninja Site</p>
	</div>
	<ul>
		<li><a href="/">Blogs</a></li>
		<li><a href="/about">About</a></li>
		<li><a href="/blogs/create">New Blog</a></li>
	</ul>
</nav>
```

Repetimos algo similar para `head.ejs` y `footer.ejs`.

Luego en la posición donde queremos importar este partial por ejemplo dentro de `index.ejs` hacemos lo siguiente:
```js
<%- include('./partials/nav.ejs')
```

Notar que si bien estamos mostrando un valor (*outputting a value*) usamos `<%-` (y no `<%=` ya en caso de hacerlo terminaríamos con un string y no html puro)

Finalmente el archivo `index.ejs` cuando importemos esos tres archivos nos quedará así:
```html
<html lang="en">
<%- include("./partials/head.ejs") %>
<body>
	<%- include("./partials/nav.ejs") %>
	<div class="blogs content">
		<h2>All Blogs</h2>
		<% if (blogs.length > 0) { %>
			<% blogs.forEach(blog => { %>
			<h3 class="title"><%= blog.title %></h3>
			<p class="snippet"><%= blog.snippet %></p>
		<% }) %>
		<% } else { %>
			<p>There are no blogs to display...</p>
		<% } %>
	</div>
	<%- include("./partials/footer.ejs") %>
</body>
</html>
```

### Agregado de CSS
Por el momento no podemos hacerlo mediante **archivos .css** dado que para eso debemos servir archivos estáticos (como archivos css e imágenes) cosa que no estamos haciendo aún. Por lo tanto si queremos agregar estilos debemos hacerlo de manera interna dentro del `<head>` (por lo que en nuestro caso será dentro de `partials/head.ejs`

```html
<head>
	<meta charset="UTF-8">
	<meta name="viewport" content="width=device-width, initial-scale=1.0">
	<title>Blog Ninja | <%= title %></title>
	<style>
		@import url('https://fonts.googleapis...');
		//...
		//...
		//...
	</style>
</head>
```
## pug
Al igual que lo visto anteriormente para utilizar el **templating engine** pug (`npm install pug`) no es necesario hacer un require sino que bastará con hacer `app.set('view engine','pug')` y express cargará el módulo internamente. También por default trabajaremos con la carpeta `views` y podremos cambiarla de la misma manera vista al estudiar el paquete ejs. 

En la carpeta `views` creamos el archivo `index.pug`, luego en él:
```pug
html
	head
		title=title
	body
		h1=message
```

Tanto `title` como `message` son valores seteados dinámicamente.

Luego en el route handler:
```js
app.get('/', (req,res)=>{
	res.render('index',{title:'Mi Aplicación', message:'Hola'});
});
```