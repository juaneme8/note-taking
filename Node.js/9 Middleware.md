# Middleware
Se llama middleware a todo el código que se ejecuta en el servidor entre que recibe un *request* y envía un *response*, por lo que podremos decir que una aplicación express no es otra cosa que un grupo de funciones middleware.

No necesariamente tenemos que tener un sólo middleware sino que podremos ejecutar varios hasta enviar una respuesta. Los mismos se ejecutan de arriba hacia abajo hasta que salgamos del proceso o enviemos una respuesta al navegador. 

El método `app.use(func)` es utilizado para montar la función middleware (también es posible especificar una ruta para que realice el montaje sólo si esta matchea). 

Lo mismo ocure para `app.METHOD(path, callback)` siendo `METHOD` GET, PUT, POST en minúsculas el método http para el cual se aplica la función middleware y `path` la ruta o via de acceso para la cual se aplicará,

`app.use(func)` se dispara para todos requests mientas que `app.get(ruta,func)` se dispara para los `GET` en los que coincide la ruta con la especificada. 

Una función middleware tiene acceso a `req`, `res` y `next`

## Built-In Middleware
```js
app.use(express.json());
app.use(express.urlencoded());
```
En primer lugar `express.json()` reconoce al request object como un objeto JSON.
Mientras que `app.use(express.urlencoded());` lo usaremos cuando los datos provengan de formularios.

## Custom Middleware

Cuando utilizamos `app.use` se ejecutará siempre independientemente del tipo de endpoint. 

```js
app.use((req, res, next) => {
	console.log(req.method);
	console.log(req.path);
	console.log(req.body);
	console.log('-----');
    next();
})
```

Si queremos que continúe ejecutando los middlewares debemos colocar `next()` pues de lo contrario se quedará cargando.

Esto mismo podemos colocarlo en un módulo independiente, por ejemplo en `loggerMiddleware.js`

```js
const logger = (req, res, next) => {
	console.log(req.method);
	console.log(req.path);
	console.log(req.body);
	console.log('-----');
    next();
}

module.exports = logger
```

Luego en `index.js` lo importamos:

```
const logger = require('./loggerMiddleware')

app.use(logger)
```

En cambio si queremos que sólo se ejecucte para peticiones `GET` de una ruta determinada:

```js
app.get('/', (req,res,next) =>{
	next();
})
```

## Manejo de Errores
Hasta ahora hemos manejado los errores en el bloque `catch`, existe otra alternativa que es hacerlo en un middleware personalizado para el manejo de errores, para ello primero en `index.js` creamos ese middleware. Debemos ubicarlo debajo de todos los paths.

```js
app.use(express.json());
app.use('/api/genres', genres);

app.use((err, req, res, next) => {
	//console.error(err);
    console.log(err.name)
    if(error.name === 'CastError'){
        res.status(400).send({error: 'id used is malformed'});
    }
    else{//500 internal server error
    	res.status(500).end();    
    }	
});
```

En lugar de implementar los distintos  tipos de errores con un if anidado podemos hacemos lo siguiente:

```js
const ERROR_HANDLERS = {
  CastError: res =>
    res.status(400).send({ error: 'id used is malformed' }),

  ValidationError: (res, { message }) =>
    res.status(409).send({ error: message }),

  JsonWebTokenError: (res) =>
    res.status(401).json({ error: 'token missing or invalid' }),

  TokenExpirerError: res =>
    res.status(401).json({ error: 'token expired' }),

  defaultError: (res, error) => {
    console.error(error.name)
    res.status(500).end()
  }
}

module.exports = (error, request, response, next) => {
  const handler =
    ERROR_HANDLERS[error.name] || ERROR_HANDLERS.defaultError

  handler(response, error)
}
```



Luego en `catch()` del handler de la ruta en lugar de imprimir el error con `console.error(err)`  lo que hacemos es pasárselo al siguiente middleware que tenga como primer parámetro `err` 
```js
try {
    await genre.save();
    res.send(genre);
} catch (err) {
    next(err);
}
```



### Sentry

Sentry es una herramienta que nos permite conocer qué errores tenemos y  en qué archivo, línea, qué navegador fue utilizado, etc.

En la [página oficial](https://sentry.io/) debemos ir a Product para buscar Node y luego elegir Express y nos aparecerá un código de ejemplo

```
npm install @sentry/node
```



## Middleware 404

Creamos un middleware de 404 para aquellos casos en los que no hemos encontrado ningún controlador con la ruta que matchee con la petición recibida. Debemos colocarlo debajo de todos los controladores (o rutas).

```js
// 404 page
app.use((req, res) => {
	res.status(404).end();
});
```

Es una buena práctica tener un directorio `middleware` en el cual por ejemplo tendremos un archivo `notFound.js` con 



Luego en `index.js`

```js
const notFound = require(./middleware/notFound.js);

//...

app.use(notFount)
```



Lo mismo tendríamos que hacer para el manejo de errores con un archivo `handleErrors.js`



### 404 Pages

Vamos a utilizar un **custom middleware** para manejar las páginas 404, la función se disparará para todos los requests  en la medida que no se haya enviado una respuesta anteriormente.
Debemos ubicarlo debajo de los handlers sino nunca los alcanzará dado que estamos entregando una respuesta al navegador con lo cual no continuará ejecutando los restantes middleware. 

```js
 // 404 page
app.use((req, res) => {
	//res.status(404).render('404', { title: '404' });
    res.status(404).json({
        error: 'Not Found'
    })
});
```

### Datos Request y next()
Si colocamos en la parte superior este middleware personalizado:
```js
app.use((req, res, next) => {
	console.log('new request made:');
	console.log('host: ', req.hostname);
	console.log('path: ', req.path);
	console.log('method: ', req.method);
	next();
});
```
> Cuando usamos HTTP Module usamos req.url, al trabajar con Express también podremos usarla pero no es una propiedad nativa sino heredada de Node.

Es necesario invocar la función `next()` recibida mediante parámetros para dar paso a la ejecución del siguiente middleware sino el navegador se quedaría colgado debido a que Express no sabe cómo continuar.

Por lo tanto podremos agregar otra pieza de middleware personalizado y al terminar de ejecutar la primera ejecutará la segunda:

```js
app.use((req, res, next) => {
	console.log('new request made:');
	console.log('host: ', req.hostname);
	console.log('path: ', req.path);
	console.log('method: ', req.method);
	next();
});
app.use((req, res, next) => {
	console.log('in the next middleware');
	next();
});
```

### Modularidad
Cuando trabajemos con nuestras custom middleware functions queremos tenerlas a cada una en un módulo separado dentro de la carpeta `middleware`, por ejemplo creamos el archivo `logger.js`:

```js
function log(req,res,next){
	console.log('middleware');
	next();
}
module.exports = log;
```

Luego en `app.js`
```js
const logger = require('./middleware/logger');

app.use(logger)
```

## 3rd Party Middleware
Otra de las ventajas de usar Node y Express es hacer uso de las funciones middleware creadas por terceros, debemos utilizar sólo aquellos estrictamente necesarios pues repercutirán en la performance volviendo a la aplicación más lenta.
Podemos encontrarlos https://expressjs.com/en/resources/middleware.html

### helmet:
* **helmet**: middleware para seguridad
```bash
npm install helmet
```
Luego para utilizarlo:
```js
const helmet= require('helmet')
app.use(helmet());
```
### morgan:
Se trata de un logger que muestra en pantalla datos del request recibido.
```bash
npm install morgan
```
Luego para utilizarlo:
```js
const morgan = require('morgan')

app.use(morgan('dev'));  
//Ante un GET en /api/cursos veremos:
//GET /api/cursos 200 3.144 ms - 209
```
| format |                                                              |
| :----: | :----------------------------------------------------------: |
| `tiny` | :method :url :status :res[content-length] - :response-time ms |
| `dev`  | :method :url :status :response-time ms - :res[content-length] |


Como ya mencionamos habrá un impacto en la performance por lo que debemos evaluar si es conveniente utilizarlo en producción o si en cambio nos convedrá habilitarlo únicamente en el entorno de desarrollo.

* También existen middlewares para manejo de sesiones, cookies, validación, etc



## Archivos Estáticos

Por el momento si en cualquier parte del código hacemos referencia a archivos estáticos, por ejemplo en la vista a un `.css`
```html
<link rel="stylesheet" href="/styles.css"/>
```
Veremos al ir por ejemplo a la home page `localhost:3000/` que en la pestaña Network de las Devtools obtuvimos un 404 para `styles.css`, dado que el servidor no nos permitió obtenerlo. Esto se debe a que el servidor protege de manera automática a todos los archivos.

Lo mismo sucedería si creamos una carpeta `images` y colocamos en ella a `logo.png` si vamos a `localhost:3000/images/logo.png` no veríamos la imagen sino que también obtendríamos un 404.

Para permitirle al navegador acceder a archivos estáticos (static assets) como **imágenes**, **css**, **archivos de texto**  debemos indicarle qué archivos deben ser públicos, esto podremos hacerlo con el middleware que viene con express llamado **static middleware**.

```js
// middleware & static files
app.use(express.static('public'));
```
Como consecuencia de esto todo lo que coloquemos en la carpeta **public** estará disponible como static file para el frontend. Si coloco allí al archivo `styles.css` y la carpeta `images` los archivos serán accedidos correctamente. Notar que no tenemos la dirección con `/public/styles.css` sino directamente `/styles.css` dado que hemos determinado a `public` como la carpeta pública.



Si no queremos crear una carpeta `public` y queremos indicar que los archivos estáticos están en `/images` y colocamos allí el archivo `logo.png`

```js
app.use(express.static('images'));
```

Accederíamos como `localhost:3000/logo.png`

Mientras que si queremos acceder como `localhost:3000/images/logo.png` podríamos indicarle que sólo en esa ruta utilice ese middleware.

```js
app.use('/images', express.static('images'));
```



## Cors

Cuando tenemos el frontend corriendo en el puerto 3000 de localhost y la API en el 3001, son orígenes distintos, es por eso que en las dev tools veremos el siguiente mensaje:  **Access to XMLHttpRequest at http://localhost:3001/api/notes from http://localhost:3000 has been blocked by CORS policy**. Si bien normalmente no hay problema para obtener ciertos recursos de otro origen como ser imágenes, CDNs, para ciertos tipos de recursos como fuentes o JSON suele haber inconvenientes. En nuestro caso lo podemos solucionar fácilmente ya que trabajamos con el backend, en caso de que no tuvieramos acceso podríamos recurrir a otras técnicas como hacer un proxy.

Cuando diseñamos la API tenemos la potestad de decirle qué origenes pueden acceder a nuestros recurso. Cuando queremos crear una API pública que pueda ser usada por cualquier utilizamos el middleware llamado cors.

```
npm install cors -E
```

> Con `-E` nos aseguramos que nos instale la versión exacta (con `=` y no con `^`)



```js
const cors = require('cors');

app.use(cors());
```



Lógicamente para aportar seguridad deberíamos especificar los orígenes que aceptamos, pero esto no es una medida de seguridad super efectiva ya que puede ser engañado fácilmente. Otra opción es dejarlo así y delegar la seguridad en otros aspectos como tokens, autenticación.

