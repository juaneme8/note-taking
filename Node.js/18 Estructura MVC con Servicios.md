# Estructura de Código
Basada en el twitch de juanigallo en el clon de pinterest. Archivos en:
> pinterest-clone-api
> pinterest-clone

Basado en twitch de damian catanzaro sobre api. Archivos en:
> md-api-demo: 

Inicialmente tenemos en la carpeta `routes` un archivo llamado `userRoutes.js` con la siguiente ruta:
```js
router.get('/ping', (req, res) => {
	res.send('PONG');
});
```
Si bien podríamos dejar todo en un único archivo la mayoría de las veces esto hará que el código se vuelva engorroso por lo que es aconsejable separar las distintas tareas en distintos archivos.

1. El **request** llega a la ruta
2. La **ruta** llama al **controller** que es el encargado de hacer toda la **lógica de negocio** 
3. El **controller** llama al **service** para interacturar con la base de datos o la API que utilicemos 
4. El **service** le devuelve al **controller** la información 
5. El **controller** entrega la infromación al **usuario**.


De manera similar a lo realizado en el archivo en el CrashCourse de Node.js de The Net Ninja en el capítulo **"Estructura MVC"** lo que haremos será crear las carpetas  `controllers` y `services`

En esas carpetas creamos los archivos `UserController.js` y `UserService.js` que son clases que luego exportamos para importarlas en `userRoutes.js`.

Tanto en el controlador como en el servicio tendremos una función con el mismo nombre.

La clave está en `const UserInstance = new UserController(new UserService());` ya que con esa instancia accedemos los métodos del controlador y gracias al constructor de tenemos acceso a la instancia del servicio  que usaremos oportunamente.

`userRoutes.js` 
```js
const express = require('express');

const router = express.Router();

const UserService = require('../services/UserService');
const UserController = require('../controllers/UserController');
const { response } = require('express');
const UserInstance = new UserController(new UserService());

router.get('/', UserInstance.getUser);

module.exports = router;
```

`UserController.js` 
```js
class UserController {
	constructor(userService) {
		this.userService = userService;
	}

	getUser = (req, res) => {
		const user = this.userService.getUser();
		res.send(user);
	};
}

module.exports = UserController;
```

`UserService.js` 
```js
class UserService {
	//En el futuro será remplazado por una API o un fetch a una DB.
	getUser = () => {
		return {
			name: 'Juan',
			age: 33,
		};
	};
}

module.exports = UserService;

```

# Consumo de API
Utilizando la API de Star Wars `https://swapi.dev/` crearemos una nueva ruta `/api/planet/:id` con la cual obtendremos información sobre un planeta. 

Para realizar el consumo de la API utilizaremos para esto axios `npm i axios` y lo haremos como dijimos anteriormente en el Servicio.

`userRoutes.js`
```js
const express = require('express');

const router = express.Router();

const UserService = require('../services/UserService');
const UserController = require('../controllers/UserController');

const PlanetService = require('../services/PlanetService');
const PlanetController = require('../controllers/PlanetController');

const UserInstance = new UserController(new UserService());

const PlanetInstance = new PlanetController(new PlanetService());

router.get('/', UserInstance.getUser);
router.get('/api/planet/:id', PlanetInstance.getPlanet);

module.exports = router;

```

Al desarrollar el controller como primera instancia quizás nos convenga llamar a `res.json({})` para verificar que hasta ahí está funcionando todo bien y luego incorporamos el llamado al método de la instancia del servicio.

`PlanetController.js`

```js
class PlanetController {
	constructor(planetService) {
		this.planetService = planetService;
	}
	getPlanet = async (req, res) => {
		const id = req.params.id;
		const planet = await this.planetService.getPlanet(id);
		res.json(planet);
	};
}

module.exports = PlanetController;

```
 > Si tenemos instalada la extensión de Chrome **JSON Viewer** veremos el JSON devuelto con un formato más amigable.

`PlanetService.js`
En este archivo trabajaremos con `axios` tener presente que cuando hacemos `const planet = await axios.get('')`los datos estarán en `planet.data`
```js
const axios = require('axios');

class PlanetService {
	constructor() {
		this.url = 'https://swapi.dev';
	}
	async getPlanet(id) {
		const planet = await axios.get(`${this.url}/api/planets/${id}`);
		return planet.data;
	}
}

module.exports = PlanetService;

```


Hasta el momento estamos enviando desde el servicio todos los datos devueltos por la API al controlador. En caso de que sólo nos interesen alguno de ellos podríamos filtrarlos tanto en el servicio como en el controlador.

## Visualización HTML
Queremos que cuando el usuario ingrese a la dirección `/home` le mostremos un HTML que haga un fetch a la API que realizamos.
Para eso en el directorio raíz creamos un archivo `index.html` y luego creamos la siguiente ruta:

Recordemos que a `res.sendFile()` tenemos que pasarle la ubicación absoluta del archivo. A menos que le indiquemos la ubicación relativa  como primer argumento y ubicación absoluta como segundo.
Para confeccionar este segundo argumento hacemos uso del buil-in core module `path` y como queremos ir un nivel hacia atrás (estamos en `rotues`):

```js
console.log(__dirname); //C:\Users\JuaNeMe\Documents\Code\md-api-demo\routes
console.log(path.join(__dirname, '..')); //C:\Users\JuaNeMe\Documents\Code\md-api-demo
```

Por lo que ponemos `res.sendFile('index.html', { root: path.join(__dirname, '..') });`

Como estamos en la carpeta `/routes` necesitamos ir un nivel hacia atrás
```js
router.get('/home', (req, res) => {
	res.sendFile('index.html', { root: path.join(__dirname, '../') });
});

```