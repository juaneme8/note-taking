# Get, Post & Delete Requests
## Requests Types:
* **GET** nos permiten obtener un recurso del servidor (JSON, HTML, etc).
* **POST** nos permite crear nuevos datos (usualmente utilizando un webform al presionar submit)
* **DELETE** nos permite eliminar datos
* **PUT** nos permite actualizar datos

Por el momento tenemos un **get request** en la ruta `localhost:3000/blogs ` y queremos usar la misma ruta para un **post request** donde agregaremos nuevos blogs mediante un web form.

### Estructura típica de rutas:
Normalmente al desarrollar una **CRUD Application** tendremos una estructura similar a la siguiente para cada tipo de recurso (en este caso blogs).

| REQUEST |             URL             |
| :-----: | :-------------------------: |
|   GET   |    localhost:3000/blogs     |
|   GET   | localhost:3000/blogs/create |
|  POST   |    localhost:3000/blogs     |
|   GET   |  localhost:3000/blogs/:id   |
|   PUT   |  localhost:3000/blogs/:id   |
| DELETE  |  localhost:3000/blogs/:id   |

### POST Requests:
 A partir del contenido cargado en el formulario, al presionar submit queremos lanzar un post request con todos los datos que luego será manejado por el servidor para incorporar este nuevo elemento.
 El primer paso consiste en enviar el post request desde el formulario y podrá ser realizado de dos maneras:
 * Utilizando Fetch API u otra asynchronous library.
 * Directamente desde el formulario

Trabajaremos con esta última técnica en `views/create.ejs` donde teníamos simplemente `<form>` agregamos `<form  action="/blogs" method="POST">`. Con el atributo `action` establecemos donde queremos enviar el request y el atributo `method` especifica el tipo de request. Queremos asociar todos los datos del formulario al **body** del request para lo cual deben tener el atributo *name* todos los **input fields**

```html
<form action="/blogs" method="POST">
	<label for="title">Blog title:</label>
	<input type="text" id="title" name="title" required>
	<label for="snippet">Blog snippet:</label>
	<input type="text" id="snippet" name="snippet" required>
	<label for="body">Blog body:</label>
	<textarea id="body" name="body" required></textarea>
	<button>Submit</button>
</form>
```

Para poder acceder a los datos dentro del handler debemos utilizar un middleware que parseará los datos (*urlencoded  data*) y los adjuntará al *request object* (serán accesibles en `req.body`). Se trata de middleware que viene con express. Sin este middleware `req.body` valdría `undefined`.
```js
app.use(express.urlencoded({  extended: true  }));
```
Lo ponemos debajo del middleware para acceder a archivos estáticos.

```js
app.post('/blogs', (req, res) => {
	// console.log(req.body);
	const blog = new Blog(req.body);
	blog.save()
	.then(result => {
		res.redirect('/blogs');
	})
	.catch(err => {
		console.log(err);
	});
});
```
> Notar que a `new Blog()` le paso `req.body` ya que tiene las mismas propiedades que el Schema, por lo que no hace falta poner `new Blog({title:req.body.title,...}`

> En este caso estoy redirigiendo pero sino por convención debería poner `res.send(blog)` de modo de retornar el recurso creado por si el cliente necesita saber el id asignado y el status code 201 (resource created) `res.status(201).send(blog)`

### Route Parameters:
Los route parameters son la parte de la ruta que puede cambiar, por ejemplo en `localhost:3000/blogs/:id`,  `id` es un route parameter. 
El trabajo consiste en extraer los route parameters para luego de determinar si es un id válido hacer un *query* del documento y finalmente enviarlo al usuario.

En la vista `index.ejs` donde recibo *blogs* coloco un link de modo que al hacer click en cada blog vayamos a `localhost:3000/blogs/:id` y esto lo hacemos de la siguiente manera:

```html
<% if (blogs.length > 0) { %>
	<% blogs.forEach(blog => { %>
		<a class="single" href="/blogs/<%= blog._id %>">
			<h3 class="title"><%= blog.title %></h3>
			<p class="snippet"><%= blog.snippet %></p>
		</a>
	<% }) %>
<% } else { %>
	<p>There are no blogs to display...</p>
<% } %>
```
Luego en el handler para dicha ruta accedemos al parámetro con `req.params.id` y utilizaremos el método `findById()`
```js
app.get('/blogs/:id', (req, res) => {
	const id = req.params.id;
	Blog.findById(id)
	.then(result => {
		res.render('details', { blog: result, title: 'Blog Details' });
	})
	.catch(err => {
		console.log(err);
		res.render('404', { title: 'Blog not found' });
		});
});
```

Notar que si el `id` recibido es inválido por lo que se produce un error, redireccionamos a la página de 404 y también colocamos ese código de estado.

Ahora debemos crear la vista `details.ejs` y en ella recibiremos los objetos `blog` y `title`. Agregamos los 3 partials (`head.ejs`, `nav.ejs`, `footer.ejs`) como en los casos anteriores:

```html
<html lang="en">
<%- include("./partials/head.ejs") %>
<body>
	<%- include("./partials/nav.ejs") %>
	<div class="details content">
		<h2><%= blog.title %></h2>
		<div class="content">
			<p><%= blog.body %></p>
		</div>
	</div>
	<%- include("./partials/footer.ejs") %>
</body>
</html>
```

#### Múltiples Parámetros:
La url anterior `localhost:3000/blogs/:id` tiene un único parámetro mientras que es posible tener más de uno por ejemplo `localhost:3000/blogs/:year/:month` y luego podremos acceder a ellos con `req.params.year` y `req.params.month`

### DELETE Requests
La idea es incorporar un *delete button* en la vista individual de blogs recién creada, de modo que al hacer click en ese botón se envíe un delete request al server con ese **_id** y se elimine de la base de datos.
Al agregar el botón `<a class="delete" data-doc="<%= blog._id %>">delete</a>` utilizamos el *data attribute* `data-doc` y luego con un script de JavaScript (que correrá en el navegador, no en el server) le asociamos un event listener y con **fetch API** realizamos el delete request. En este script accedemos al *data attribute* del `_id` con `trashcan.dataset.doc`
```html
<div class="details content">
	<h2><%= blog.title %></h2>
	<div class="content">
		<p><%= blog.body %></p>
	</div>
	<a class="delete" data-doc="<%= blog._id %>">delete</a>
</div>

<script>
const trashcan = document.querySelector('a.delete');
trashcan.addEventListener('click', (e) => {
	const endpoint = `/blogs/${trashcan.dataset.doc}`;
	fetch(endpoint, {
	method: 'DELETE',
	})
	.then(response => response.json())
	.then(data => window.location.href = data.redirect)
	.catch(err => console.log(err));
});
</script>
```

Luego en `app.js` utilizaremos el método `findByIdAndDelete()`. 

Notar que en este caso enviamos JSON al navegador y no redireccionamos ya que no es posible cuando utilizamos un AJAX request, a diferencia de lo que ocurre cuando usamos un formulario web con post. En cambio retornamos json y usamos una propiedad llamada redirect y el redireccionamiento propiamente dicho lo haremos desde el frontend. En primer lugar en el frontend recibimos JSON y para convertirlo a un objeto de JavaScript usaremos `response.json()`. Luego el direccionamiento lo hacemos con `window.location.href = data.redirect`
```js
app.delete('/blogs/:id', (req, res) => {
	const id = req.params.id;
	Blog.findByIdAndDelete(id)
	.then(result => {
		res.json({ redirect: '/blogs' });
	})
	.catch(err => {
		console.log(err);
	});
});
```

### PUT Requests
Nos permiten realizar actualizaciones del contenido.
*(No fue implementado en el curso de the net ninja, pero sí en el de Mosh usando dummy data sin db)*
1) Buscar el elemento
2) Si no existe retornar 404 (not found)
3) Validar los datos recibidos, si no son válidos retornar 400 (bad request).
4) Actualizar el elemento y retornarlo.


### Query String Parameters
Se trata de parámetros que agregamos en la url luego del signo `?` a diferencia de los route parameters que son esenciales, los query string parameters son opcionales.
Para acceder a ellos `req.query`

### Postman:
Se trata de una extensión de Chrome para probar los endpoints.

Por ejemplo para un POST Request primero establecemos la url deseada, luego seleccionamos:
1) Body -> **x-www-form-urlencoded**
Completamos las columnas con los pares key/value y le damos send.
Para recibir correctamente los datos en `req.body` tenemos que tener el middleware: `app.use(express.urlencoded({  extended: true  }));`
2) Body -> Raw -> **JSON**
A continuación completamos con el contenido del request
```json
{
"name": "new course"
}
```
Para recibir correctamente los datos en `req.body` tenemos que tener el middleware: `app.use(express.json());`

Estos dos middleware serán útiles para POST y PUT requests.

**Nota:** La misma tarea realizada por estas dos porciones de middleware puede también ser llevada a cabo por el paquete **body-parser** que en versiones anteriores formaba parte de Express.

### Comentarios en Routes
A la hora de documentar las rutas podemos hacer lo siguiente:
```js
@route POST /api/url/shorten
@desc Create short URl
```