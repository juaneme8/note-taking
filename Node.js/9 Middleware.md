# Middleware
Se llama **middleware** a todo el código que se ejecuta en el servidor entre que recibe un *request* y envía un *response*, por lo que podremos decir que una aplicación express no es otra cosa que un grupo de funciones middleware.

No necesariamente tenemos que tener un sólo middleware sino que podremos ejecutar varios hasta enviar una respuesta. Los mismos se ejecutan de arriba hacia abajo hasta que salgamos del proceso o enviemos una respuesta al navegador. 

El método `app.use(func)` es utilizado para montar la función middleware (también es posible especificar una ruta para que realice el montaje sólo si esta matchea). 

Lo mismo ocure para `app.METHOD(path, callback)` siendo `METHOD` GET, PUT, POST en minúsculas el método http para el cual se aplica la función middleware y `path` la ruta o via de acceso para la cual se aplicará,

`app.use(func)` se dispara para todos request (incluso los post requests)  mientas que `app.get(ruta,func)` debe coincidir la ruta del get request para que se ejecute. 

Una función middleware tiene acceso a `req`, `res` y `next`

## Built-In Middleware
```js
app.use(express.json());
app.use(express.urlencoded());
```
En este caso `express.json()` es una built-in middleware function que reconoce al request object como JSON Object.
`app.use(express.urlencoded());` lo usaremos cuando los datos provengan de formularios.

## Custom Middleware
```js
app.get('/', function(req,res,next){
	next();
})
```

### 404 Pages
Hemos usado custom middleware para manejar las 404 pages, la función se disparará para todos los requests  en la medida que no se haya enviado una respuesta anteriormente.
Debemos ubicarlo debajo de get handlers sino nunca los alcanzaría dado que estamos entregando una respuesta al navegador con lo cual no continuará ejecutando los restantes middleware. 
```js
 // 404 page
app.use((req, res) => {
	res.status(404).render('404', { title: '404' });
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

### Modularidad:
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

## 3rd Party Middleware:
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

## Static Files:
Por el momento si en `head.ejs` hacemos referencia a un archivo `.css`
```html
<link rel="stylesheet" href="/styles.css"/>
```
Veremos al ir por ejemplo a la home page `localhost:3000/` que en la pestaña Network de las Devtools obtuvimos un 404 para `styles.css`, dado que el servidor no nos permitió obtenerlo. Esto se debe a que el servidor protege de manera automática a todos los archivos
Para permitirle al navegador acceder a archivos estáticos (static assets) como **imágenes**, **css**, **archivos de texto**  debemos indicarle qué archivos deben ser públicos, esto podremos hacerlo con el middleware que viene con express llamado **static middleware**.

```js
// middleware & static files
app.use(express.static('public'));
```
Como consecuencia de esto todo lo que coloquemos en la carpeta **public** estará disponible como static file para el frontend. Si coloco allí al archivo `styles.css` los estilos serán cargados correctamente. Notar que no tenemos la dirección con `/public/styles.css` sino directamente `/styles.css` dado que hemos determinado a `public` como la carpeta pública. De la misma manera también podremos acceder a esta hoja de estilos yendo a `localhost:3000/styles.css` .
Ahora podremos poner en el archivo `styles.css` todos los estilos y sacarlos de `partials/head.ejs` donde los teníamos internamente.