# Unit Tests
>[The Net Ninja - MondoDB Tutorial for Beginners](https://www.youtube.com/watch?v=9OPP_1eAENg&list=PL4cUxeGkcC9jpvoYriLI0bY8DOgWZfi6u)
>[Traversy Media - Intro To JavaScript Unit Testing With Mocha JS & Chai](https://www.youtube.com/watch?v=MLTRHc5dk6s)

## Automated testing
Es la práctica de escribir **código para testear nuestro código** y luego ejecutar esos tests de manera automática.
En una aplicación si quisiéramos testear manualmente todas las funciones esto nos demandaría un **tiempo** muy grande, ya que para cada una de ellas sería necesario ejecutar la aplicación, loguearnos, navegar hasta cierto menú, llenar formularios, submit y ver el resultado en pantalla repitiendo esto para las distintas ramificaciones conocidas.
Con automated testing directamente invocamos a la función con diferentes **inputs** y verificamos que entregue el **output** esperado. Podremos ejecutar estos testeos cada vez que cambiamos el código, cada vez que hacemos un commit y **antes de hacer el deploy** de la aplicación (reduciendo así la cantidad de bugs en producción).
Otro beneficio es que nos permitirá hacer **refactoring** del código sin olvidarnos de probar nada luego de hacerlo.

## Tipos de tests:
* **Unit tests:** testeos de una unidad de la aplicación (puede ser una clase o varias) **sin** sus dependencias externas. Tienen la ventaja de ser fáciles de escribir y que se ejecutan rápido permitiéndonos verificar los *building blocks* de la aplicación. Sin embargo como desventaja no aportan mucha confianza al no estar usando las dependencias externas. 

* **Integration tests:** testeos de la aplicación **con** sus dependencias externas. Tardan más en ejecutar (normalmente involucran leer y escribir db) pero aportan una mayor confianza.

* **End-to-end:** testeos utilizando la UI de la aplicación, existen herramientas como selenium que nos permite grabar la interacción de un usuario con la aplicación y luego reproducir las mismas acciones para ver si obtenemos el resultado correcto. Estos tests aportan la mayor confianza pero son muy lentos y determinados mejoras en la aplicación o cambios mínimos en la UI pueden romper estos tests.

En nuestras aplicaciones debemos escribir los 3 tipos de tests esto se conoce como **pirámide de tests** y hace referencia a que la base deben ser los unit tests con mayor cantidad (rápidos y fáciles de escribir), también varios **integration tests** y unos pocos **end-to-end tests** (E2E) para las funciones centales de la aplicación, los casos de borde los atendemos con unit tests. Sin embargo en la práctica la cantidad de tests de cada tipo dependerá de la aplicación siendo recomendado usar los unit tests para testear las funciones con lógica compleja (y varios *paths* posibles) de manera rápida, los integration tests en aquellos casos de lectura/escritura de db.
Como criterio siempre que podamos hacer unit tests por sobre e2e debemos hacerlo

## Testing Framework
Para escribir tests necesitamos de un testing framework que nos dará una library con una serie de *utility functions* y un test runner con el cual ejecutamos los tests y obtenemos un reporte de cuántos pasaron la prueba.
Los frameworks mas conocidos son **Jasmine**, **Mocha** y **Jest** 

## Mocha
> Código en `/unit-testing-mocha`

 Es el testing framework más popular pero para ciertas tareas require del uso de libraries como ser [Chai](https://www.chaijs.com/) y [Sinon](https://sinonjs.org/). Esto tiene la contra de tener que ver más de una documentación y dependiendo de la evolución de estas bibliotecas en un futuro algo puede dejar de ser compatible con mocha.

Si queremos usar la assertion library default:
```js
const assert = require('assert');
```

Nos permite testear la conexión con la db, creación, lectura, actualización y eliminación de documentos en una colección.

`npm i mocha`

```js
const assert = require('assert');

describe('demo test', () => {
	it('sumar dos números', () => {
		assert(2 + 3 === 5);
	});
});
```

Dentro del bloque `describe()` colocamos los `it()` que consisten en tests individuales.

En `package.json` indicar a 
```json
"scripts": {
		"test": "mocha"
},
```
Luego al ejecutar `npm run test` (o la forma corta `npm test` o la aún mas corta `npm t`) veremos un tilde verde indicando que el resultado del test fue exitoso.

Para llevar esto al siguiente nivel, vamos a trabajar con bases de datos para ello primero creamos un archivo en `/test/connection.js` donde realizamos la conexión con la base de datos.
```js
const mongoose = require('mongoose');
const dbURI = 'mongodb://localhost/unit-testing';

mongoose.connect(dbURI, {
	useNewUrlParser: true,
	useUnifiedTopology: true,
});

const db = mongoose.connection;
db.once('open', () => console.log('Conectado a MondoDB'));
db.on('error', (err) => console.log(err));
```

Luego creamos `/models/marioChar.js` donde creamos el modelo `MarioChar` que importaremos luego en los tests.

```js
const mongoose = require('mongoose');

const mariocharSchema = new mongoose.Schema({
	name: {
		type: String,
		required: true,
		min: 3,
		max: 50,
	},
	weight: {
		type: Number,
	},
});

const Mariochar = mongoose.model('Mariochar', mariocharSchema);

module.exports = Mariochar;
```

Creamos el archivo `/test/saving_test.js`

```js
const assert = require('assert');
const Mariochar = require('../models/mariochar');

describe('Creacion Documentos', () => {
	it('Save', () => {
		const char = new Mariochar({
			name: 'Mario',
			weight: 80,
		});
		char.save().then(() => {
			assert(!char.isNew);
			done();
		});
	});
});
```

`char.isNew` es true cuando el objeto fue creado localmente pero no fue guardado en la base de datos.
`done()` lo utilizamos cuando hay operaciones asincrónicas, ya que necesito un modo de indicarle a mocha que ha terminado el test y podemos pasar al siguiente.

### Hooks 
Cuando ejecutamos `node run test` vemos lo siguiente:
```bash
	Creacion Documentos
Conectado a MondoDB
	    √ Save
```
Esto significa que antes de conectarse a la base de datos comenzó a realizar el test "Creación documentos" 
Utilizando mocha hooks podremos establecer el código que queremos ejecutar antes o después de un test. En este caso queremos ejecutar el código que realiza la conexión antes de los tests. 
Por lo tanto en `connection.js`:
```js
before((done) => {
	mongoose.connect(dbURI, {
		useNewUrlParser: true,
		useUnifiedTopology: true,
	});

	const db = mongoose.connection;
	db.once('open', () => {
		console.log('Conectado a MondoDB');
		done();
	});
	db.on('error', (err) => console.log(err));
});

```

Haciendo uso de los mocha hooks también podremos por ejemplo eliminar toda la colección antes de ejecutar cada testeo, por lo que en `connection.js` agregamos:
```js
beforeEach((done) => {
	mongoose.connection.collections.mariochars.drop(() => {
		done();
	});
});
```

Notar que es en el único lugar al que nos referimos a `mariochars` en plural ya que así se llama la colección.

### Encontrar Documentos
Creamos el archivo `/test/finding_test.js` y como en `connection.js` eliminamos todos los documentos de la colección antes de cada test, debemos crear un nuevo documento para luego buscarlo.
Para asegurarnos que el documento haya sido creado antes de buscarlo utilizamos el hook `beforeEach()`

```js
const assert = require('assert');
const Mariochar = require('../models/mariochar');

describe('Busqueda Documentos', () => {
	//Creacion de documento
	beforeEach((done) => {
		const char = new Mariochar({ name: 'Mario' });
		char.save().then(() => done());
	});

	//Lectura documento
	it('find', (done) => {
		Mariochar.findOne({ name: 'Mario' }).then((result) =>
			assert(result.name === 'Mario')
		);
		done();
	});
});

```
En este caso `findOne()` buscará al primer elemento con `name:'Mario'`. Supongamos por un momento que tenemos más de un elemento con el mismo nombre en ese caso vamos a querer buscar al que acabamos de crear y eso lo hacemos filtrando de acuerdo a su `_id` con el método findById. Para eso agregamos un nuevo test debajo del anterior:
```js
const assert = require('assert');
const Mariochar = require('../models/mariochar');

describe('Busqueda Documentos', () => {
	let myChar;
	//Creacion de documento
	beforeEach((done) => {
		const char = new Mariochar({ name: 'Mario' });
		myChar = char;
		char.save().then(() => done());
	});

	//Lectura documento
	it('find', (done) => {
		Mariochar.findOne({ name: 'Mario' }).then((result) =>
			assert(result.name === 'Mario')
		);
		done();
	});
	//Lectura documento por id
	it('find by id', (done) => {
		Mariochar.findOne({ _id: myChar._id }).then((result) =>
			assert(result._id.toString() === myChar._id.toString())
		);
		done();
	});
});

```
Debemos tener presente que `_id` no es un string sino un objeto por lo que para comparar si el valor leído es igual al esperado debemos convertir ambos a string tal como vemos en: `result._id.toString() === myChar._id.toString()`

### Eliminar Documentos:
A continuación creamos un archivo `/test/deleting_test.js` y primero vamos a crear un documento, luego lo eliminamos con `findOneAndRemove()` y por último con `findOne()` lo buscamos y hacemos llamamos a assert indicando que el resultado correcto es `null`.
```js
const assert = require('assert');
const Mariochar = require('../models/mariochar');

describe('Eliminacion Documentos', () => {
	//Creacion de documento
	beforeEach((done) => {
		const char = new Mariochar({ name: 'Mario' });
		char.save().then(() => done());
	});
	//Eliminacion de documento
	it('findOneAndRemove', (done) => {
		Mariochar.findOneAndRemove({ name: 'Mario' }).then(() => {
			Mariochar.findOne({ name: 'Mario' }).then((result) => {
				assert(!result);
				done();
			});
		});
	});
});

```

Para evitar el *DeprecationWarning* al usar findOneAndRemove() debemos agregar al options object de la conexión `useFindAndModify: false`


### Actualizar Documentos:
El modo de trabajo será similar al anterior primero creamos un nuevo test donde agregamos un nuevo documento, usamos `findOneAndUpdate()` para cambiar el nombre, hacemos `findOne()` para buscar el elemento recién actualizado de acuerdo a su `_id` y `assert()` con la propiedad modificada.

```js
const assert = require('assert');
const Mariochar = require('../models/mariochar');

describe('Actualizar Documento', () => {
	let char;
	//Creacion de documento
	beforeEach((done) => {
		char = new Mariochar({ name: 'Mario' });
		char.save().then(() => done());
	});

	//Actualización documento
	it('findOneAndUpdate', (done) => {
		Mariochar.findOneAndUpdate({ name: 'Mario' }, { name: 'Luigi' })
			.then(() => Mariochar.findOne({ _id: char._id }))
			.then((result) => {
				assert(result.name === 'Luigi');
				done();
			});
	});
});
```

### Actualizar Documento con Update Operators
De manera similar a lo realizado en el apartado anterior, creamos un nuevo personaje al cual le asignamos nombre y peso, a continuación con `updateOne()` y el uso de *update operators* incrementamos su peso en una unidad, luego lo buscamos por `_id` y llamamos a `assert()` con el nuevo peso esperado.
```js
const assert = require('assert');
const Mariochar = require('../models/mariochar');

describe('Incrementar Documento', () => {
	let char;
	//Creacion de documento
	beforeEach((done) => {
		char = new Mariochar({ name: 'Mario', weight: 50 });
		char.save().then(() => done());
	});

	//Actualización documento
	it('updateOne', (done) => {
		Mariochar.updateOne({ name: 'Mario' }, { $inc: { weight: 1 } })
			.then(() => Mariochar.findOne({ _id: char._id }))
			.then((result) => {
				assert(result.weight === 51);
				done();
			});
	});
});

```

En caso de querer actualizar más de un documento podríamos usar `updateMany()` en lugar de `updateOne()`:

```js
//Actualización documento
	it('updateOne', (done) => {
		Mariochar.updateMany({}, { $inc: { weight: 1 } })
			.then(() => Mariochar.findOne({ _id: char._id }))
			.then((result) => {
				assert(result.weight === 51);
				done();
			});
	});
```

### Documentos Anidados
Queremos crear una colección `authors` que cuente con las propiedades `name (String)`, `age (Number)` y a su vez `books` que será de tipo `[bookSchema]`.
La colección `books`  en tanto deberá contar con las propiedades `title`, `pages`.

Creamos el archivo `/models/author.js`

```js
const mongoose = require('mongoose');

const BookSchema = new mongoose.Schema({
	title: String,
	pages: Number,
});

const AuthorSchema = new mongoose.Schema({
	name: String,
	books: [BookSchema],
});

const Author = mongoose.model('author', AuthorSchema);

module.exports = Author;
```

Luego en `nesting_test.js` el proceso es similar al que venimos trabajando. Tenemos dos tests y antes de cada uno de ellos vamos a vaciar la colección `authors`.
El primer test consiste en crear un nuevo autor, guardarlo en la base de datos y luego buscar un autor con ese nombre y llamar a `assert()` preguntando si la cantidad de elementos del array `books` es 1.
En el segundo test nuevamente creamos un autor, lo guardamos y a continuación agregamos un nuevo libro al array `books`, luego lo buscamos (por el nombre del autor) y preguntamos si la cantidad de elementos del array `books` es 2.

```js
const assert = require('assert');
const mongoose = require('mongoose');
const Author = require('../models/author');

// Describe our tests
describe('Documentos Anidados', () => {
	beforeEach((done) => {
		// Drop the collection
		mongoose.connection.collections.authors.drop(() => {
			done();
		});
	});

	// Create tests
	it('Creates an author with sub-documents', (done) => {
		var pat = new Author({
			name: 'Patrick Rothfuss',
			books: [{ title: 'Name of the Wind', pages: 400 }],
		});-

		pat.save().then(() => {
			Author.findOne({ name: 'Patrick Rothfuss' }).then(function (record) {
				assert(record.books.length === 1);
				done();
			});
		});
	});

	it('Adds a book to an author', (done) => {
		var pat = new Author({
			name: 'Patrick Rothfuss',
			books: [{ title: 'Name of the Wind', pages: 400 }],
		});

		pat.save().then(() => {
			Author.findOne({ name: 'Patrick Rothfuss' }).then(function (record) {
				// add a book to the books collection
				record.books.push({ title: "Wise Man's Fear", pages: 500 });
				record.save().then(() => {
					Author.findOne({ name: 'Patrick Rothfuss' }).then(function (record) {
						assert(record.books.length === 2);
						done();
					});
				});
			});
		});
	});
});
```

### mocha + chai
Hasta el momento utilizamos la assertion library default:
```js
const assert = require('assert');
```
Si queremos usar chai: `npm i chai`
```js
const assert = require('chai').assert;
```

## Jest
> Código en `/unit-testing-jest`

Usado por Facebook para testear las aplicaciones React, es básicamente un wrapper de **Jasmine** y cuenta además con una built-in tool de code coverage.

En primer lugar creamos un proyecto Node.js nuevo `npm init -y` luego instamos jest con `npm i jest --save-dev`

En `package.json` modificar el objeto scripts colocando en la propiedad "test" el nombre del CLI que viene con jest para ejecutar los tests (lo encontramos en `/node_modules/.bin/jest`
```json
"scripts":{
	"test":"jest"
}
```

Si ejecutamos ahora `npm t` o `npm test` o `npm run test` veremos "No test found" y también veremos el pattern usado por jest para encontrar los tests
`testMatch: **/__tests__/**/*.[jt]s?(x), **/?(*.)+(spec|test).[tj]s?(x)` Lo cual nos da una idea de que entre otras opciones los archivos terminados en `test.js` dentro de una carpeta `test` serán considerados tests.

Como queremos testear el module`lib.js` creamos en la carpeta tests el archivo `lib.test.js`
```js
test('Primer Test', () => {

});
```
Si ejecutamos ahora `npm t` veremos que el test pasa la prueba exitosamente
```bash
 PASS  test/lib.test.js
  √ Primer Test (1 ms)
```

Mientras que si generamos un error:
```js
test('Primer Test', () => {
	throw new Error('Algo falló');
});
```
Al ejecutar el test veremos qué archivo de tests falló, en particular qué test y también en qué línea de código.
```bash
 FAIL  test/lib.test.js
  × Primer Test (2 ms)
```

### Testing Números
En `lib.js` tenemos la función `absolute()` :
```js
module.exports.absolute = function(number) {
  if (number > 0) return number; 
  if (number < 0) return -number; 
  return 0; 
}
```
El número de unit tests debe ser mayor o igual al número de *execution paths* de manera tal que verifiquemos toda la lógica. En este caso tenemos 3 caminos posibles.

El primer caso de testeo o primer execution path:
```js
const lib = require('../lib');
test('absolute - retornar >0 si entrada >0', () => {
	const result = lib.absolute(1);
	expect(result).toBe(1);
});
```

 `expect()` es una utility function y `toBe()` es una matcher function podemos ver la lista completa en:
 [https://jestjs.io/docs/en/using-matchers](https://jestjs.io/docs/en/using-matchers)

A la hora de testear números contamos con los siguientes matchers: `toEqual()` (en este caso funciona de la misma manera que `toBe()`) , `toBeGreaterThan()`, `toBeGreaterThanOrEqual()`, `toBeLessThan()`, `toBeLessThanOrEqual()`. 
En caso de tener números de punto flotante no podremos usar el matcher de *exact equality* `toEqual()` sino que usamos `toBeCloseTo()`

Completando el resto de los casos de testeo:
```js
const lib = require('../lib');
test('absolute - retornar >0 si entrada >0', () => {
	const result = lib.absolute(1);
	expect(result).toBe(1);
});
test('absolute - retornar >0 si entrada <0', () => {
	const result = lib.absolute(-1);
	expect(result).toBe(1);
});
test('absolute - retornar 0 si entrada 0', () => {
	const result = lib.absolute(0);
	expect(result).toBe(0);
});
```

Supongamos que en el último test en vez de `expect(result).toBe(0);`tuvieramos `expect(result).toBe(1);` el test fallaría y podríamos visualizar el valor esperado (1) y el valor recibido (0) junto con la línea donde se produjo la falla.


### Agrupando Tests
En la medida que el número de tests aumenta es importante tenerlos organizados para que el mantenimiento sea simple.
La función `describe()` nos permite agrupar los tests relacionados (con lo cual podremos simplificar el nombre de cada test) y también podemos reemplazar `test()` por `it()` 
> Si tuviéramos todo escrito en inglés veríamos `it('should return >0 if input >0')` es decir que lo veríamos como *plain english* "it should return..." 
```js
const lib = require('../lib');
describe('absolute', () => {
	it('deberia retornar >0 si entrada >0', () => {
		const result = lib.absolute(1);
		expect(result).toBe(1);
	});
	it('deberia retornar >0 si entrada <0', () => {
		const result = lib.absolute(-1);
		expect(result).toBe(1);
	});
	it('deberia retornar 0 si entrada 0', () => {
		const result = lib.absolute(0);
		expect(result).toBe(0);
	});
});
```
 Al comienzo dijimos que los tests nos permiten hacer refactoring con la confianza de que si la nueva implementación pasa todos los tests funcionará. Si no tuvieramos tests para esta función, realizar cambios sería más engorroso ya que tendríamos que ejecutar la aplicación y navegar hacia la parte de la aplicación donde se usa la función.

Supongamos que hacemos el siguiente refactoring:
```js
module.exports.absolute = function(number) {
  if (number >= 0) return number; 
  return -number; 
}
```
Luego ejecutamos los tests y vemos que la implementación es correcta. Lo mismo podríamos hacer ahora con el *ternary operator* logrando una nueva implementación verificada rápidamente.

### Testing Strings
Trabajaremos con la función `greet()` que recibe un nombre y entrega un string.
```js
module.exports.greet = function (name) {
	return 'Welcome ' + name;
};
```

```js
describe('greet', () => {
	it('deberia retornar un mensaje', () => {
		const result = lib.greet('Juan');
		expect(result).toBe('Welcome Juan');
	});
});
```
Si bien el test pasará, un mínimo cambio en el string de `greet()` por ejemplo `return 'Welcome ' + name + '!';` hará que el test falle. 
Los tests no deben ser ni muy **específicos** (se rompen fácilmente) ni muy **generales** (no brindarán confianza). Es por eso que a la hora de testear strings en vez de buscar exact equality podemos buscar ciertos patterns usando regular expressions y la función `toMatch(regEx)`. 
Otra alternativa sería usar el matcher `toContain()`.
A continuación ponemos los dos `expect()` uno debajo del otro.

```js
describe('greet', () => {
	it('deberia retornar un mensaje', () => {
		const result = lib.greet('Juan');
		expect(result).toMatch(/Juan/);
	});
});
```

### Testing Arrays
Trabajaremos con la función `getCurrencies()`:
```js
module.exports.getCurrencies = function () {
	return ['USD', 'AUD', 'EUR'];
};
```

```js
describe('getCurrencies', () => {
	it('deberia retornar las monedas aceptadas', () => {
		const result = lib.getCurrencies();
		//afirmación muy general: retornar por ejemplo un número pasaría el test
		expect(result).toBeDefined();
		expect(result).not.toBe(null);

		//afirmación muy específica:
		//un simple ordenamiento del array puede romper el test
		expect(result[0]).toBe('USD');
		expect(result[1]).toBe('AUD');
		expect(result[2]).toBe('EUR');
		//el agregado de una nueva moneda rompería le test
		expect(result.length).toBe(3);

		//Modo aceptable: con 3 assertions (o afirmaciones)
		expect(result).toContain('USD');
		expect(result).toContain('AUD');
		expect(result).toContain('EUR');

		//Modo ideal: más compacto
		expect(result).toEqual(expect.arrayContaining(['EUR', 'USD', 'AUD']));

	});
});

```

`expect.arrayContaining(arrayEspecificado)` matchea si el array recibido contiene todos los elementos del array especificado. En caso de de que en un futuro agreguemos más tipos de monedas seguirá funcionando.


### Testing Objects
Trabajamos con la función `getProduct()`

```js
module.exports.getProduct = function (productId) {
	return { id: productId, price: 10 };
};
```

```js

describe('getProduct', () => {
	it('deberia retornar el producto con el id entregado', () => {
		const result = lib.getProduct(1);

        //afirmación muy específica
        expect(result).toEqual({ id: 1, price: 10 });
        
        //afirmaciones correctas
		expect(result).toMatchObject({ id: 1, price: 10 });
		expect(result).toHaveProperty('id', 1);
	});
});

```

Nótese que no usamos `toBe()` ya que si pusiéramos `expect(result).toBe({id:1, price:10}` como se trata de dos objetos en distintas posiciones de memoria el test fallará, es por eso que debemos usar `toEqual()`.

Supongamos que `getProduct()` entrega varias propiedades además de `id` y `price`, en ese caso ya no puedo usar `expect(result).toEqual({ id: 1, price: 10 });` sino que tengo que cambiar `toEqual()` por `toMatchObject()`: `expect(result).toMatchObject({ id: 1, price: 10 });`
Una tercera opción es usando la función `toHaveProperty(key,value)` tener presente que el *value* debe ser del tipo esperado en este caso `Number`.

## Testing Exceptions
Trabajamos con la función `registerUser()` podemos ver que si el `username` es *falsy* (`0`, `false`, `empty string`, `null`, `undefined`, `NaN`) tiramos una excepción en caso contrario retorna un objeto con las propiedades `id` y `username`. Tenemos dos *execution paths* por lo que neceistamos por lo menos dos unit tests.
```js
module.exports.registerUser = function (username) {
	if (!username) throw new Error('Username is required.');

	return { id: new Date().getTime(), username: username };
};
```

Como las excepciones no son valores devueltos por la función, no podemos almacenarlos en una variable como venimos haciendo hasta ahora `expect(callback).toThrow(); `
```js
it('deberia tirar excepcion', () => {
		expect(() => {lib.registerUser(0);}).toThrow();
});
```
Tenemos que repetir esto mismo para cada uno de los valores falsy.  Es por eso que creamos un array con los argumentos que queremos pasar y luego un loop.

Sin embargo existe algo llamado *single assertion principle* (algunos lo interpretan como un único llamado a `expect()` en cada test), nosotros en cambio interpretamos que en este caso si bien estamos llamando varias veces a `expect()` lo estamos haciendo con la misma lógica (valores falsy). En este caso no habría mucha diferencia entre nuestra implementación y hacer distintos tests, pero en ocasiones el test será de muchas líneas y no queremos tener todo ese código repetido.

En cuanto al test para cuando el funcionamiento es correcto (happy path) y retorna un usuario, si bien colocamos dos expectations, afirmaciones o assertions con `expect()` (como en otras oportunidades ya hemos hecho) nuevamente ambos corresponden al mismo concepto lógico.
```js
describe('registerUser', () => {
	it('deberia tirar excepcion', () => {
		const args = [null, undefined, NaN, '', 0, false];
		args.forEach((a) => {
			expect(() => {
				lib.registerUser(a);
			}).toThrow();
		});
	});
	it('deberia retornar un usuario', () => {
		const result = lib.registerUser('mosh');
		expect(result).toMatchObject({ username: 'mosh' });
		expect(result.id).toBeGreaterThan(0);
	});
});
```

### Ejecutar Tests Continuamente
Hasta el momento para ejecutar los tests debemos ingresar `npm test`, para lograr que jest ejecute todos los tests cada vez que realizamos un cambio en el código debemos modificar en `package.json` en el objeto `scripts` la propiedad `test: jest` que pasará a ser `test: jest --watchAll` . A continuación ejecutamos `npm test` y a partir de ese momento cuando hagamos modificaciones se ejecutarán automáticamente todos los tests.

### Testing Función FizzBuzz 
```js

module.exports.fizzBuzz = function(input) { 
  if (typeof input !== 'number') 
    throw new Error('Input should be a number.');
    
  if ((input % 3 === 0) && (input % 5) === 0)
    return 'FizzBuzz';

  if (input % 3 === 0)
    return 'Fizz';

  if (input % 5 === 0)
    return 'Buzz'; 

  return input; 
}
```
```js
const exercise1 = require('../exercise1');

describe('FizzBuzz', () => {
	it('deberia tirar un error si la entrada no es un número', () => {
		expect(() => {
			exercise1.fizzBuzz('a');
		}).toThrow();
		expect(() => {
			exercise1.fizzBuzz(null);
		}).toThrow();
		expect(() => {
			exercise1.fizzBuzz(undefined);
		}).toThrow();
		expect(() => {
			exercise1.fizzBuzz({});
		}).toThrow();
	});
	it('deberia retornar FizzBuzz si es múltiplo de 3 y 5', () => {
		const result = exercise1.fizzBuzz(15);
		expect(result).toBe('FizzBuzz');
	});
	it('deberia retornar FizzBuzz si es múltiplo de 3 y no de 5', () => {
		const result = exercise1.fizzBuzz(3);
		expect(result).toBe('Fizz');
	});
	it('deberia retornar FizzBuzz si es múltiplo de 5 y no de 3', () => {
		const result = exercise1.fizzBuzz(5);
		expect(result).toBe('Buzz');
	});
	it('deberia retornar el número si no es divisible por 3 ni por 5', () => {
		const result = exercise1.fizzBuzz(1);
		expect(result).toBe(1);
	});
});
```
### Simple Mock Functions
Hasta ahora usamos funciones sin dependencias en funciones externas, sin embargo esto no es así en `applyDiscount()`

```js
const db = require('./db');

module.exports.applyDiscount = function (order) {
	const customer = db.getCustomerSync(order.customerId);

	if (customer.points > 10) order.totalPrice *= 0.9;
};
```

En `db.js` simulamos un llamado a MongoDB
```js
module.exports.getCustomerSync = function(id) { 
  console.log('Reading a customer from MongoDB...');
  return { id: id, points: 11 };
}
```
En los unit tests no debemos hablar con external resources, sino sería un integration test. El objetivo central de los unit tests es desacoplar los recursos externos pues estos pueden no estar disponibles a la hora de ejecutar los unit tests. Supongamos que en vez de obtener el customer de una db lo obtenemos de un servicio http remoto, en ese caso tendríamos que tener ese servicio corriendo para ejecutar los códigos o el test fallaría.

Veremos a continuación como aplicar unit tests a una función que directa o indirectamente habla con external resources. Debemos remplazar la implementación de `db.getCustomerSync()` por una fake o mock implementation que tome y entregue los mismos datos, pero que no hable con una db o un servicio http remoto. 

```js
const db = require('../db');
describe('applyDiscount', () => {
	it('deberia aplicar descuento si la cantidad de puntos es mayor a 10', () => {
		db.getCustomerSync = (customerId) => {
			console.log('Leyendo customer de mock function...');
			return { id: customerId, points: 20};
		};

		const order = { customerId: 1, totalPrice: 10 };
		lib.applyDiscount(order);
		expect(order.totalPrice).toBe(9);
	});
});
```
Importamos el módulo db `const db = require('../db');` y luego dentro del test remplazamos la función `db.getCustomerSync()` por una versión falsa que no habla a una db.
Al agregar esta función veremos que deja de aparecer el mensaje `'Reading a customer from MongoDB...'` y en cambio vemos `'Leyendo customer de mock function...'`
```js
```