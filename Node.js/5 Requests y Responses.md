# Requests & Responses
Cada vez que hacemos un cambio tendríamos que detener el proceso `ctrl+c` y volver a ejecutarlo `node server.js`.



### Request Object

Dijimos que el objeto `req` contiene información acerca de la solicitud. Con `req.url` donde obtendremos la url visitada por el usuario y con `req.method` obtenemos el tipo de request realizado.
```js
const http = require('http');

const server = http.createServer((req, res) => {
	console.log(req.url, req.method); 
})

server.listen(3000, 'localhost', () => {
	console.log('listening for requests on port 3000');
});
```
Si ingresamos en `localhost:3000` donde dice `req.url` veremos `/` y donde dice `req.method` veremos `GET`. Mientras que si entramos en `localhost:3000/about` en `req.url` veremos `/about`.

### Response Object
De manera similar el server responde con response al cliente sino lo hacemos el navegador se quedará colgado cargando. El primer paso es establecer los *response headers son información extra como ser:
* Content-Type: * en los cuales indicamos al navegador el tipo de dato que estamos entregando en respuesta sea este texto, html, json.
* Status: 200 (ok), 404 (page not found) , etc

### Retornando Texto Plano:
Si deseamos enviar en respuesta texto utilizaremos `res.setHeader('Content-type','text/plain')` para establecer el tipo de contenido, `res.write('hello, ninjas')` con el mensaje en sí  y finalmente `res.end()` para enviar la respuesta al navegador.

```js
const http = require('http');
const server = http.createServer((req, res) => {
	res.setHeader('Content-Type', 'text/plain');
	res.write('hello, ninjas');
	res.end();
})

server.listen(3000, 'localhost', () => {
	console.log('listening for requests on port 3000');
});
```
En las devtools si vamos a la pestaña Network veremos que para el request de localhost en el apartado Headers varios response headers configurados automáticamente y el `Content-Type= 'text/plain'` configurado por nosotros.

### Retornando HTML:
Si en cambio queremos enviar html en respuesta debemos configurar: `Content-Type= 'text/html'` si queremos agregar varias líneas de código podremos poner varios `res.write('<h1>contenido</h1>')`

```js
const http = require('http');
const fs = require('fs');

const server = http.createServer((req, res) => {
	// set header content type
	res.setHeader('Content-Type', 'text/html');
	res.write('<p>hello, ninjas</p>');
	res.write('<p>hello again, ninjas</p>');
	res.end();
	});
});
server.listen(3000, 'localhost', () => {
	console.log('listening for requests on port 3000');
});
```
Si inspeccionamos los elementos devueltos veremos que el navegador agregó automáticamente los tags `<html>`, `<head>` y `<body>` pero podremos especificarlos para reemplazar el valor default:
Por ejemplo con `res.write('<head><link rel="stylesheet" href="#"></head>)`

Como podemos ver se vuelve bastante engorroso acumular `res.write()` para cada línea de `html`, por lo que debemos crear un archivo `.html` independiente el cual leemos con ayuda del módulo file-system y luego enviamos al navegador.

### Retornando páginas HTML:
Creamos una carpeta views y en ella un archivo index.html en el cual colocamos un dummy content.

```js
const http = require('http');
const fs = require('fs');

const server = http.createServer((req, res) => {
	res.setHeader('Content-Type', 'text/html');
	fs.readFile('./views/index.html', (err, data) => {
	if (err) {
		console.log(err);
		res.end();
	}
	res.write(data);
	res.end();
});

server.listen(3000, 'localhost', () => {
	console.log('listening for requests on port 3000');
});
```
Si sólo estamos llamando una vez a `res.write(data)`, podemos omitirlo y poner directamente `res.end(data)`.

### Retornando JSON
El método `JSON.stringify()` convierte un objeto o valor de JavaScript en una cadena de texto JSON
```js
const  http  =  require('http');
const  fs  =  require('fs');

const  server  =  http.createServer((req, res) => {
	res.setHeader('Content-Type', 'application/json');
	const  myObj  = {
		name: 'Ryu',
		job: 'Ninja',
		age: 32
	};
	res.end(JSON.stringify(myObj));
});

server.listen(3000, () => {
console.log('Escuchando Solicitudes en Puerto 3000');
});
```

Veremos en el navegador `{"name":"Ryu","job":"Ninja","age":32}`.

Mientras que si ponemos `res.end(JSON.stringify([1,2,3]));` veremos `[1,2,3]`

> Cuando utilicemos Express podremos retornar JSON directamente con `res.json(myObj)`

#### Retornando Stream con Pipe
Como res es un write stream podemos enviar todo aquello que recibamos 

```js
const  http  =  require('http');
const  fs  =  require('fs');

const  server  =  http.createServer((req, res) => {
	res.setHeader('Content-Type', 'text/plain');
	const  readStream  =  fs.createReadStream('./docs/blog1.txt', 'utf8');
	readStream.pipe(res);
});

server.listen(3000, () => {
	console.log('Escuchando Solicitudes en Puerto 3000');
});
```

### Ruteo Básico:
Hasta el momento ingresemos en la dirección que ingresemos obtenemos el mismo `html`. Debemos fijarnos la url y en función de eso entregar uno u otro archivo.
En la carpeta `view` creamos los archivos `about.html` y `404.html` (para cuando el usuario intenta ingresar a páginas que no existen).
En la variable `path` almacenaremos la ruta del archivo que deseamos leer y lo conformaremos en función de `req.url`.

### Status Codes:
Los códigos de estado describen el tipo de respuesta enviado al navegador y qué tan exitosa fue la solicitud. 

Los códigos más comunes son:

* 200 - OK
* 301 - Resource moved
* 404 - Not found
* 500 - Internal server error

Existen muchos otros y de acuerdo al rango en el que se encuentran podemos determinar de qué se trata:
* Rango de 100: informational responses
* Rango de 200: success codes
* Rango de 300: codes for redirects
* Rango de 400: client error codes
* Rango de 500: server error codes

El status code lo establecemos con `res.statusCode = 200` y podremos visualizarlo en las *devtools* dentro de la pestaña *Network* en la columna *Status*.



> En la página [http.cat](http://http.cat) podemos ver un listado de todos los status codes.

### Redirects:
En ocasiones puede que tengamos que redireccionar al usuario de una url a otra. Supongamos por ejemplo que solíamos tener la url `/about-me` y luego decidimos que fuera `/about`. Si bien en nuestro sitio cambiamos todas las referencias puede que alguien la tenga guardada e intente entrar a la url original, en ese caso no queremos mostrarles una página 404 sino que querremos redireccionarlos a la nueva.

En primer lugar creamos un `case 'about-us':` para esta nueva ruta, colocamos el status code en 301 (lo cual significa resource moved), setamos el header location encargado de configurar el re direccionamiento y luego de poner `res.end()` se realiza.
```js
const http = require('http');
const fs = require('fs');

const server = http.createServer((req, res) => {
	res.setHeader('Content-Type', 'text/html');
	let path = './views/'; //los archivos están en esa carpeta
	switch(req.url) {
		case '/':
			path += 'index.html';
			res.statusCode = 200;
			break;
			
		case '/about':
			path += 'about.html';
			res.statusCode = 200;
			break;
			
		case '/about-us':
			res.statusCode = 301;
			res.setHeader('Location', '/about');
			res.end();
			break;
			
		default:
			path += '404.html';
			res.statusCode = 404;
	}
	// send html
	fs.readFile(path, (err, data) => {
		if (err) {
			console.log(err);
			res.end();
		}
		//res.write(data);
		res.end(data);
	});
	});

});

server.listen(3000, 'localhost', () => {
	console.log('listening for requests on port 3000');
});
```

Este esquema de trabajo con `switch(req.url)` en sitios importantes se vuelve confuso y difícil de mantener especialmente cuando tenemos muchas endpoints y distintos tipos de requests (get, post, delete, update). Existe un third-party package llamado **Express** el cual nos ayudará a manejar todo esto de manera más simple y eficiente, de todos modos es importante haber aprendido lo que sucede *under the hood*.