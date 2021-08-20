# Express Apps
Hasta ahora trabajamos con node crudo *(raw node)* pero cuando tengamos gran cantidad de rutas se volverá más complejo el callback, es por eso que utilizamos **Express**.

 Express es un **framework** rápido y liviano que nos ayudará a manejar el routing y la lógica del lado del servidor de un modo elegante y fácil de leer. 
 Otra ventaja de Express es la facilidad con la cual podemos utilizar un *View Engine* como veremos más adelante. 

## RESTful API o RESTful Services
La comunicación entre cliente y servidor ocurre utilizando **http requests** mediante APIs (**A** pplication **P** rogramming **I** nterface) para ello en que en el servidor exponemos una serie de **servicios** a través de endpoints.

Ciertas aplicaciones web como ser Youtube, Uber, Twitter, etc proporcionan una interfaz con la cual podemos interactuar con ellas a través de endpoints que nos darán una respuesta en formato JSON y podremos mostrarlos en nuestra app.

 Diremos que estos servicios son RESTful o que son RESTful APIs cuando son basados en la **convención REST** (**RE** presentation of **S** tate **T** ransfer).

### Características:
RESTful web service tienen estas características:
-   Permiten realizar **CRUD Operations** (**C** reate, **R** ead, **U** pdate, **D** elete).
-   Necesitan un endpoint y un verbo/método HTTP (GET, POST, PUT, DELETE).
-   Usualmente regresan la información en formato **JSON**.
-   Retornan **códigos de estado**, por ejemplo 200, 201, 404, etc

## Instalación y Ejemplo de Uso:
Internamente utiliza el módulo http de node.

> npm install express

Con express se suele trabajar con un archivo `app.js`
Con `app.get('/',callback)` respondemos a un request de tipo GET en la ruta '/' y en ejecutamos el callback cuando se produce (el cual toma el *request* y *response* object).
Podemos trabajar de manera similar a como lo hicimos anteriormente con `res.write('')` y `res.end()` o bien al trabajar con express podemos hacerlo con `res.send('')` que tiene las siguiente ventajas:
* Infiere el tipo de contenido y setea el *Content Type Header*.
* Infiere el status code (aunque en algunas casos tendremos que setearlo manualmente)
En el siguiente ejemplo al visitar `localhost:3000` veremos el html devuelto (que por el momento es un string y luego será una página) y si visitamos la pestaña *Network* veremos el *Content Type* configurado en `text/html` y el Status Code es 200.
```js
const express = require('express'); //retorna una función

//invocamos la función y creamos una instancia de la express app
const app = express(); 

// listen for requests (asume que estamos trabajando con localhost)
//retorna una instancia del server (si quisieramos reutilizarla por ejemplo para web sockets lo almacenaríamos en una constante).
app.listen(3000);

app.get('/', (req, res) => {
	res.send('<p>home page</p>');
});
```

Para manejar varios endpoints debemos crear get handlers para cada uno y  si bien podríamos continuar enviando respuesta mediante *strings de html*  lo ideal será enviar un *archivo html*. Esto lo hacemos con `res.sendFile()` en lugar de `res.send()`. El primer parámetro por default será una ruta absoluta, pero si queremos que sea relativa debemos especificar con un segundo parámetro respecto a qué lo es. Suponiendo que tengo el html en la carpeta views del proyecto: `res.sendFile('./views/index.html', {root: __dirname})`.
Otra forma de hacerlo sería con el **módulo path** (que forma parte de los built in core modules) para unir el path relativo con el `__dirname`.
En cuanto al re direccionamiento, podemos hacerlo con `res.redirect()` (el *status code* 301 se completa automáticamente) que fuerza un nuevo request en la url indicada.
Finalmente la página `404.html` la mostraremos haciendo uso del método `app.use()` con el cual estaremos creando middleware y ejecutando funciones middleware. En este caso setearemos el *status code* manualmente y enviaremos el archivo `404.html` lo cual hacemos con: `res.status(404).sendFile('./views/404.html', { root: __dirname });` Notar que podemos encadenar dado que `res.status(404)` devuelve el `res` object.
Esto funciona de la siguiente manera el método `app.use()` se dispara para todos request (notar que no tiene ninguna url especificada como si ocurre con `app.get()` por ejemplo) que alcancen esa zona del código. Ante un request si la url coincide con la del get handler, ejecutará el callback y enviará una respuesta al navegador, por lo que no continuará ejecutando el resto del código. Es por eso que lo ubicamos en la parte inferior de modo que sólo llegue ahí si es un endpoint con una url inválida 
```js
const express = require('express'); 

const app = express(); 

app.listen(3000);

app.get('/', (req, res) => {
	res.sendFile('./views/index.html', {root: __dirname})
});
app.get('/about', (req, res) => {
	res.sendFile('./views/about.html', {root: __dirname})
});

// redirects
app.get('/about-us', (req, res) => {
	res.redirect('/about');
});

// 404 page
app.use((req, res) => {
	res.status(404).sendFile('./views/404.html', { root: __dirname });
});
```