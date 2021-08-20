# Vidly
> Código disponible en `/mosh-node-vidly`
# Genres
Crear un servicio para manejar la lista de géneros de las películas. Debemos crear un endpoint para obtener todos, crear, actualizar y eliminar en `http://vidly.com/api/genres` esta información será utilizada luego en la aplicación por ejemplo en una *drop-down list*.

Creamos la carpeta `mosh-node-vidly` y en ella el archivo `app.js` 
```bash
cd mosh-node-vidly
code .

npm init -y

npm i express mongoose @hapi/joi
```

## `app.js`
```js
const mongoose = require('mongoose');
const genres = require('./routes/genres');
const express = require('express');
const app = express();

mongoose
	.connect('mongodb://localhost/vidly', {
		useNewUrlParser: true,
		useUnifiedTopology: true,
	})
	.then(() => console.log('Conectado a MongoDB'))
	.catch(() => console.error('Error de Conexión a MongoDB'));

app.use(express.json());

app.use('/api/genres', genres);

app.listen('3000', () => console.log('Escuchando en Puerto 3000'));

```
## `/routes/genres.js`
```js
const express = require('express');
const router = express.Router();
const { Genre, validate } = require('../models/genre');

router.get('/', async (req, res) => {
	const genres = await Genre.find().sort('name');
	res.send(genres);
});

router.post('/', async (req, res) => {
	const { error } = validate(req.body);
	if (error) return res.status(400).send(error.details[0].message);

	const { name } = req.body;

	let genre = new Genre({ name });
	try {
		genre = await genre.save();
		res.send(genre);
	} catch (exc) {
		console.log(exc);
	}
});

router.get('/:id', async (req, res) => {
	const genre = await Genre.findById(req.params.id);

	if (!genre)
		return res.status(404).send('The genre with the given ID was not found.');

	res.send(genre);
});

router.put('/:id', async (req, res) => {
	const { error } = validate(req.body);

	console.log(error);

	if (error) return res.status(400).send(error.details[0].message);

	const genre = await Genre.findByIdAndUpdate(
		req.params.id,
		{ name: req.body.name },
		{
			new: true,
		}
	);

	if (!genre)
		return res.status(404).send('The genre with the given ID was not found.');

	res.send(genre);
});

router.delete('/:id', async (req, res) => {
	const genre = await Genre.findByIdAndRemove(req.params.id);

	if (!genre)
		return res.status(404).send('The genre with the given ID was not found.');

	res.send(genre);
});

module.exports = router;
```
## `/models/genre.js` 
El nombre del archivo en singular al igual que el modelo `Genre` en este archivo tenemos schema/modelo de mongoose y también la función para la validación con joi.
```js
const mongoose = require('mongoose');
const Joi = require('@hapi/joi');

const genreSchema = new mongoose.Schema({
	name: {
		type: String,
		required: true,
		minlength: 5,
		maxlength: 50,
	},
});

const Genre = mongoose.model('Genre', genreSchema);

function validateGenre(genre) {
	const schema = Joi.object({
		name: Joi.string().min(5).max(50).required(),
	});

	return schema.validate(genre, { abortEarly: false });
}

module.exports.Genre = Genre;
module.exports.validate = validateGenre;

```

Con Postman testeamos cada uno de los endpoints creados:
|        |                 |
| :----: | :-------------: |
|  GET   |   /api/genres   |
|  POST  |   /api/genres   |
|  GET   | /api/genres/:id |
|  PUT   | /api/genres/:id |
| DELETE | /api/genres/:id |

# customers
El propósito es crear los siguientes endpoints:

|        |                    |
| :----: | :----------------: |
|  GET   |   /api/customers   |
|  POST  |   /api/customers   |
|  GET   | /api/customers/:id |
|  PUT   | /api/customers/:id |
| DELETE | /api/customers/:id |

El procedimiento general es muy similar a lo anterior, sólo agrego algunos comentarios salientes.

El Schema cuenta con las propiedades `name` (String), `phone` (String) y `isGold` (Boolean) 

Es conveniente establecer un standard de mínimos y máximos esperados en los Strings para no tener que estar pensándolo en función del tipo de campo que almacenaremos y que de manera intuitiva coincidan los valores indicados para mongoose y en joi.

## `app.js`

## `/models/customer.js` 

## `/routes/customers.js`
En el Schema de mongoose establecimos que `isGold` tiene valor por default `false`, esto significa que si hacemos un `POST` y en `req.body` no le pasamos esta propiedad tendremos `isGold:false`. Sin embargo, si a la hora de actualizar hacemos algo así:
```js
const customer = await Customer.findByIdAndUpdate(
	req.params.id,
	{
		name: req.body.name,
		phone: req.body.phone,
		isGold: req.body.isGold,
	},
	{ new: true }
);
```
Y sólo le pasamos `name` y `phone` que son `required` veremos que pasamos a tener `isGold:null`, es por eso que debemos poner `isGold: req.body.isGold || false` de modo que si no le pasamos valor coloque `false`.

# movies
El propósito es crear los siguientes endpoints:

|        |                 |
| :----: | :-------------: |
|  GET   |   /api/movies   |
|  POST  |   /api/movies   |
|  GET   | /api/movies/:id |
|  PUT   | /api/movies/:id |
| DELETE | /api/movies/:id |

El Schema cuenta con las propiedades `title` (String) , `genre` (documento embebido de tipo `genreSchema`), `numberInStock` (Number) y `dailyRentalRate` (Number)

En la propiedad `genre` embebemos un documento (`_id` con la referencia y `name`) a pesar de que tenemos una colección `genres` pues queremos optimizar la performance de las queries utilizando una implementación híbrida. Esto se justifica también desde el punto de vista de que difícilmente se renombren los géneros (lo que demandaría modificar en más de un lugar para evitar inconsistencias de datos). Por otra parte la colección `genres` como ya dijimos nos servirá si por ejemplo queremos mostrar un *dropdown* de selección de géneros para tenerlos todos juntos.

## `/models/movie.js`
Notar que importamos `const { genreSchema } = require('./genre');` por lo que tendremos que exportarlo en `/models/genre.js`
Notar que en el mongoose Schema tenemos la propiedad `genreId` que es con la cual buscaremos el género para embeber los datos del documento.

## `/routes/movies.js`

Ponemos una ruta a modo de ejemplo:
```js
router.post('/', async (req, res) => {
	const { error } = validate(req.body);
	if (error) return res.status(400).send(error.details[0].message);

	const genre = await Genre.findById(req.body.genreId);

	if (!genre) return res.status(400).send('Invalid genre');

	let movie = new Movie({
		title: req.body.title,
		genre: {
			_id: genre._id,
			name: genre.name,
		},
	});
	try {
		movie = await movie.save();
		res.send(movie);
	} catch (exc) {
		console.log(exc);
	}
});
```
Notar que en relación a los archivos de modelos usamos los siguientes require:
```js
const { Movie, validate } = require('../models/movie');
const { Genre } = require('../models/genre');
```

Cuando creamos el objeto `movie` no hacemos `genre: genre` porque no queremos incorporar la propiedad relacionada con la versión `"__v"` y también porque la colección `genre` podría tener quizás 50 propiedades y sólo nos interesa `name` (ojo en ese caso en el schema `genre` no sería de tipo `genreSchema`). Además estamos estableciendo `_id: genre._id` de lo contrario le asignaría un valor aleatorio dado que dijimos que `genre` es de tipo `genreSchema`
```js
let movie = new Movie({
		title: req.body.title,
		genre: {
			_id: genre._id,
			name: genre.name,
		},
	});
```

# Rentals
El propósito es crear los siguientes endpoints:
|      |              |
| :--: | :----------: |
| GET  | /api/rentals |
| POST | /api/rentals |

El Schema cuenta con las propiedades: `customer` (definimos un nuevo schema con las propiedades más relevantes a mostrar cuando visualicemos los alquileres, **no** usamos `customerSchema`), `movie` (también usamos un custom schema con las propiedades esenciales y no `movieSchema`), `dateOut`, `dateReturned` y `rentalFee`.
Tener en cuenta que si bien no estamos guardando todas las propiedades en la API guardamos la referencia `_id` que nos permitirá en caso de necesitar algo adicional, hacer el query necesario.
Con joi sólo validaremos `customerId` y `movieId` las fechas y montos a pagar serán calculados en el server.

```js
router.post('/', async (req, res) => {
	const { error } = validate(req.body);
	if (error) return res.status(400).send(error.details[0].message);

	const customer = await Customer.findById(req.body.customerId);
	if (!customer) return res.status(400).send('Invalid customer.');

	const movie = await Movie.findById(req.body.movieId);
	if (!movie) return res.status(400).send('Invalid movie.');

	if (movie.numberInStock === 0)
		return res.status(400).send('Movie not in stock.');

	let rental = new Rental({
		customer: {
			_id: customer._id,
			name: customer.name,
			phone: customer.phone,
		},
		movie: {
			_id: movie._id,
			title: movie.title,
			dailyRentalRate: movie.dailyRentalRate,
		},
	});
	rental = await rental.save();

	movie.numberInStock--;
	movie.save();

	res.send(rental);
});
```

Analizando el POST podremos ver que primero guardamos el nuevo alquiler en la colección `rentals` y luego reducimos el stock de esa película afectando la colección `Movies`.
```js
rental = await rental.save();

movie.numberInStock--;
movie.save();
```
Necesitamos una forma de asegurarnos que ambas operaciones sean **atómicas** es decir que se ejecuten las dos o no se ejecute ninguna. Esto nos lo asegura una **transaction** que consiste en un grupo de operaciones que deben ser ejecutadas como una unidad, esto significa que o bien todas se ejecutan y cambian el estado de la DB o bien en caso de que se produzca una falla en el medio todas las operaciones ya realizadas efectuarán un *roll-back* y la DB volverá a su estado inicial.

Las transacciones fueron incorporadas en MongoDB 4.0 and Mongoose 5.2.0. Las transacciones funcionan gracias a MongoDB sessions, por lo que para iniciar una transaction debemos llamar a `startSession()` y luego llamar a `startTransaction()` de la sesión. Para ejecutar una operación en una transacción debemos pasarle la `session` como una opción.

> Mosh utiliza el paquete npm llamado fawn

## Object Id
Cuando guardamos un documento MondoDB crea la propiedad `_id` de manera automática con 24 caracteres, es decir de 12 bytes. 
* Los primeros 4 bytes representan un timestamp. Esto significa que no será necesario crear propiedades como `created_at` y también que si queremos ordenar por fecha de creación bastará con ordenar por `_id`.
* Los siguientes 3 bytes representan un identificador de la máquina.
* Los siguientes 2 bytes identifican el proceso
* Los últimos 3 bytes representan un contador. Si estamos en el mismo segundo, en la misma máquina y el mismo proceso el valor del contador será distinto.

Decimos que el object id es prácticamente unique pero no del todo, dado que como tenemos 3 bytes para el contador `2^8 = 16M` en caso de que en el mismo segundo, la misma máquina y el mismo proceso creemos más documentos que esa cantidad se producirá overflow con lo cual tendremos duplicados los `_id`.

En DBMS como SQL Server contamos con mecanismos que nos garantizan que en cada tabla tengamos un número autoincrementado que nos asegure la unicidad pero a expensas de escalabilidad.
En MondoDB El _id no es generador por el propio MongoDB sino que es generado por **MongoDB Driver** por lo que no tendrá que esperar a que MondoDB lo genere. Es por eso que aplicaciones que utilizan MongoDB son muy escalables, podremos tener varias instancias de MondoDB y no es necesario comunicarnos con un lugar central para obtener el _id.

Recordemos que mongoose es una abstracción sobre MondoDB Driver cuando creamos un nuevo documento mongoose habla con MondoDB Driver y genera un nuevo id.

También es posible generar explícitamente un id.

```js
const mongoose = require('mongoose');
const id = new mongoose.Types.ObjectId();
```

Si queremos obtener a partir del id el timestamp: 
```js
console.log(id.getTimestamp());
```
Si queremos validar un id podemos hacerlo con:
```js
console.log(mongoose.Types.ObjectId.isValid('1234')) //false
```

### Validar Object IDs
El POST request en `/api/rentals` recibe `movieId` y `customerId` queremos validar que el id recibido sea válido dado que sino cuando utilizamos el método `.findById('1234')` se producirá una excepción y en Postman se quedará colgado sin una respuesta. En cambio deberíamos devolver `400 (bad request)` acompañado de un mensaje que indique que el id es inválido.

Una opción sería incorporar las siguientes líneas de código antes de buscar los documentos determinados por los ids.
```js
if(!mongoose.Types.ObjectId.isValid(req.body.customerId)
	return res.status(400).send('Invalid customer');
if(!mongoose.Types.ObjectId.isValid(req.body.movieId)
	return res.status(400).send('Invalid movie');
```

Sin embargo para una mejor implementación debemos incorporar esta lógica adicional en `rental.js` en el método `validate()` donde validamos los datos enviados por el usuario usando joi (hasta ahora sólo nos interesaba que fuera un String). Para validar Object IDs con joi podemos instalar el paquete `joi-objectid` 
`npm i joi-objectid`

```js
const Joi = require('@hapi/joi');
Joi.objectId  =  require('joi-objectid')(Joi)

function validateRental(rental) {
	const schema = Joi.object({
		customerId: Joi.objectId().required(),
		movieId: Joi.objectId().required(),
	});
	return schema.validate(rental, { abortEarly: false });
}

```

113 completo