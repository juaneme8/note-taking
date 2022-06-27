# Introducción al Testing

🧪 [Introducción al Testing con JEST ✅ (Bootcamp FullStack) - midudev](https://www.youtube.com/watch?v=_DzBez4qMi0)

La forma más simple de entender el testing es pensando que nos permitirá comprobar el correcto funcionamiento de un método.

Es importante que los tests sean predecibles, que no dependan de factores externos de modo tal que siempre que los ejecute obtenga el mismo resultado.

A la hora de crear tests es importante hacerlos fallar pues quizás nunca falle y eso lo convertiría en un test inútil.

Supongamos que tenemos:

```javascript
const suma = (a,b) => {
  return a-b;
}
```

Si bien en este caso podemos darnos cuenta fácilmente qué está mal, en ocasiones será mucho más difícil notarlo. 

La importancia de los tests está no sólo en saber si nuestro método funciona o no, sino que también **nos permite documentar estos métodos** .

## Test en RunJS

Para testear este método de manera manual, podemos hacerlo en un entorno controlado utilizando **RunJS** que actúa como banco de pruebas o playground. 

Bastará con ingresar `suma(1,2) === 3` y nos aparecerá `false` lo cual nos da la pauta de que no está funcionando correctamente.

Otra forma de plantearlo podría ser `if(suma(1,2)!==3) console.log('suma of 1 and 2 expected to be 3')` o bien tirando un error para hacerlo más semántico `if(suma(1,2)!==3) new Error('suma of 1 and 3 expected to be 3')`

Sin embargo puede que para otras condiciones el método sí funcione, por ejemplo si pusiéramos como argumentos 0 y 0 el `if(suma(0,0)!==0) new Error('suma of 0 and 0 expected to be 0')` no sería ejecutado ya que da el valor esperado.



## Test en Node

Para testear esto mismo también manualmente pero directo en el proyecto de Express creamos una carpeta `tests ` y un archivo `suma.test.js`. 

> También podría ser `.spec.js`.

Haremos uso del método `console.assert()` con el cual aseveramos que una cosa dará cierto valor y en caso de que no se cumpla mostraremos un mensaje de error. Si lo que le pasamos como primer parámetro es `false` saldrá el error que le pasemos como segundo parámetro.

```javascript
const suma = (a,b) => {
  return a-b
}

console.assert(
    suma(0,0)===3,
    'Suma of 0 and 0 expected to be 0'
)

console.assert(
    suma(1,2)===3,
    'Suma of 1 and 2 expected to be 3'
)
```



Luego para ejecutar este test 

```
node tests/suma.test.js
```

Nos aparecerá ese mensaje de error sólo en caso de que la aseveración falle.



Lo ideal sería probar mas casos y eso podemos hacerlo empleando un array.

```javascript
const suma = (a,b) => {
  return a-b
}

const checks = [
	{ a: 0, b:0, result:0},
	{ a: 1, b:2, result:3},
	{ a: -3, b:3, result:0},
];

checks.forEach(check => {
	const {a,b,result} = check;
	console.assert(
    suma(a,b)===result,
    `Suma of ${a} and ${b} expected to be ${result}`
)
})

console.log(`${checks.length}` checks performed...);
```

Si bien podríamos ampliar la información devuelta en el mensaje de error para aportar mas datos, esto no tiene sentido pues existen herramientas que se encargan de ejecutar una tanda de tests y entregarnos un error detallado que nos permita saber dónde están los fallos y solucionarlos.

Estas herramientas reciben el nombre de framework de testing que nos darán una serie de funciones y un test runner con el cual ejecutamos los tests y obtenemos un reporte de cuántos pasaron la prueba.
Los frameworks mas conocidos son **Jasmine**, **Mocha** y **Jest** 


# Jest 

Jest es un testing framework de JavaScript mantenido por Facebook. Inicialmente fue desarrollado para hacer testing en el navegador y luego evolucionó siendo posible utilizarlo en servidor.

```
npm install jest -D
```

> Mocha y AVA son otras alternativas que al igual que Jest son frameworks de testing que podríamos haber elegido.



Como vamos a probar un método en concreto vamos a desarrollar lo que se conoce como **test unitario**. Esto es así cuando tenemos una entrada y una salida y no produce **side effects**, ni llama a otras funciones.

```javascript
const palindrome = (string) => {
  if(typeof string === 'undefined') return;
  
  return string
    .split('')
    .reverse()
    .join('')
}

module.exports = {
  palindrome
}
```

## Configuración Jest 

En `package.json` debemos modificar el script `"test"` de modo que quede de la siguiente forma:

```json
"scripts":{
	"test": "jest --verbose --silent"
}
```

> Con `--verbose` le indicamos que queremos que nos entregue la máxima información posible.
>
> Con `--silent` le indicamos que no queremos que nos muestre los `console.log()`.

Jest como dijimos anteriormente está pensado para trabajar por defecto en el cliente y viene con la capacidad de simular aspectos del DOM. Esto no sólo no lo necesitamos sino que puede ser problemático. Es por eso que en `package.json`  vamos a colocar:

```json
"jest":{
	"testEnvironment": "node"
}
```

> Otra opción sería colocarlo en `jest.config.js` pero lo ideal es tenerlo todo en un mismo lugar así es más fácil de encontrar y con menos archivos.

> Es posible que en Windows experimentemos problemas si el nombre de la carpeta tiene espacios (por ejemplo si nuestro nombre de usuario los tiene).



### Jest con ESModules

Si queremos utilizar jest con import y export es probable que tengamos que configurar lo siguiente:

```bash
"test": "set NODE_OPTIONS=--experimental-vm-modules && jest"
```

En Linux directamente podremos ponerlo sin el `set` y sin el `&&`,  `NODE_OPTIONS=--experimental-vm-modules jest`



## Ejecución de Tests

Si ejecutamos ahora `npm t` o `npm test` o `npm run test` veremos "No test found" y también veremos el pattern usado por jest para encontrar los tests
`testMatch: **/__tests__/**/*.[jt]s?(x), **/?(*.)+(spec|test).[tj]s?(x)` Lo cual nos da una idea de que entre otras opciones los archivos terminados en `test.js` dentro de una carpeta `test` serán considerados tests.



> Jest por defecto buscará todos los archivos que terminen en `.test.js`
>
> No es necesario importar `jest` ya que una vez que encuentra este archivo sabe que debe utilizar esas dependencias.

## Testing de Funciones Externas

En `lib.js` tenemos la función `absolute()` :
```js
module.exports.absolute = function(number) {
  if (number > 0) return number; 
  if (number < 0) return -number; 
  return 0; 
}
```
El número de unit tests debe ser mayor o igual de posibles caminos de ejecución de manera tal que verifiquemos toda la lógica. En este caso tenemos 3 caminos posibles.

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


## Agrupando Tests
En la medida que el número de tests aumenta es importante tenerlos organizados para que el mantenimiento sea simple.
La función `describe()` nos permite agrupar los tests relacionados (con lo cual podremos simplificar el nombre de cada test) y también podemos reemplazar `test()` por `it()` . 

También es posible anidar bloques `describe`.

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
Los tests nos permiten hacer refactoring con la confianza de que si la nueva implementación pasa todos los tests funcionará. Si no tuvieramos tests para esta función, realizar cambios sería más engorroso ya que tendríamos que ejecutar la aplicación y navegar hacia la parte de la aplicación donde se usa la función.



## Testing Strings
Trabajaremos con la función `greet()` que recibe un nombre y entrega un string.
```js
module.exports.greet = function (name) {
	return 'Welcome ' + name;
};
```
Entonces para comprobar su funcionamiento creamos el siguiente test:
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

```js
describe('greet', () => {
	it('deberia retornar un mensaje', () => {
		const result = lib.greet('Juan');
		expect(result).toMatch(/Juan/);
	});
});
```

## Testing Arrays

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


## Testing Objects
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

Para chequear si una propiedad está presente en la respuesta podríamos usar también `expect(result.body.id).toBeDefined();`

## Testing Exceptions
Trabajamos con la función `registerUser()` podemos ver que si el `username` es *falsy* (`0`, `false`, `empty string`, `null`, `undefined`, `NaN`) tiramos una excepción en caso contrario retorna un objeto con las propiedades `id` y `username`. Tenemos dos *execution paths* por lo que necesitamos por lo menos dos unit tests.
```js
module.exports.registerUser = function (username) {
	if (!username) throw new Error('Username is required.');

	return { id: new Date().getTime(), username: username };
};
```

Como las excepciones no son valores devueltos por la función, no podemos almacenarlos en una variable como venimos haciendo hasta ahora sino que debemos hacer lo siguiente: `expect(callback).toThrow(); `
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



## Testing Función FizzBuzz 

Supongamos que tenemos en un archivo llamado `exercise1.js` la siguiente función `FizzBuzz`

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
## Simple Mock Functions
Hasta ahora usamos funciones sin dependencias en funciones externas, sin embargo esto no es así en la función `applyDiscount()` que podemos ver a continuación:

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

## Test de palindrome

En primer lugar creamos el archivo `palindrome.test.js` en la carpeta `test`.



```js
test('Primer Test', () => {

});
```
Si ejecutamos ahora `npm t` veremos que el test pasa la prueba exitosamente
```bash
✅ palindrome of juaneme8
```

Mientras que si generamos un error:
```js
test('Primer Test', () => {
	throw new Error('Algo falló');
});
```
Al ejecutar el test veremos qué archivo de tests falló, en particular qué test y también en qué línea de código.
```bash
✅ palindrome of juaneme8
```



```javascript
const {palindrome} = require('../utils/for_testing')

test ('palindrome of juaneme8', ()=> {
	const result = palindrome('juaneme8');
	
	expect(result).toBe('8emenauj');
})
test ('palindrome of empty string', ()=> {
	const result = palindrome('');
	
	expect(result).toBe('');
})
test ('palindrome of undefined', ()=> {
	const result = palindrome();
	
	expect(result).toBeUndefined();
})
```

Con `test()` estamos creando un test y lo que está dentro del *callback* será lo que ejecutará para comprobar dicha prueba.

Podemos de un vistazo utilizar los tests como documentación del método. En este caso estaremos analizando las condiciones de borde o *corner cases* como ser qué sucede si llamamos al método con un string vacío, `undefined`, etc.



En la terminal veremos una salida similar a la siguiente:

```
✅ palindrome of juaneme8
✅ palindrome of empty string
✅ palindrome of undefined
```

Cuando tengamos más de un archivo nos convendrá agrupar los tests de cada uno de ellos y esto lo hacemos con el método `describe()`

```js
const {palindrome} = require('../utils/for_testing')

describe('palindrome', ()=> {
    test ('of juaneme8', ()=> {
        const result = palindrome('juaneme8');

        expect(result).toBe('8emenauj');
    })
    test ('of empty string', ()=> {
        const result = palindrome('');

        expect(result).toBe('');
    })
    test ('of undefined', ()=> {
        const result = palindrome();

        expect(result).toBeUndefined();
    })
})
```

Ahora en cambio veremos

```
palindrome
    ✅ of juaneme8
    ✅ of empty string
    ✅ of undefined
```



## Métodos Jest

Podemos ver la lista completa de métodos en la [documentación](https://jestjs.io/docs/expect).

* `toBe()`
* `toBeNull()`
* `toBeUndefined()`
* `toEqual()`


## Configuración ESLINT

Para evitar que el linter nos marque como errores el uso de `test()` y `expect() ` debemos modificar el la configuración de eslint.

Es posible que ESLint lo tengamos configurado en `package.json` y en ese caso debemos modificar el objeto `eslintConfig` **agregando** lo siguiente:

```json
"eslintConfig":{
	"env":{
		"jest": true,
	}
}
```



Si tenemos un archivo `.eslintrc.js` debemos agregar `jest: true` dentro de:

```
module.exports = {
	env:{
		...
		...,
		jest: true
	}
}
```



## Testing API Rest

[🔴 Testing de Backend con Express usando Jest y Supertest - midudev](https://www.youtube.com/watch?v=_xxVJdGNMrs)



Como sabemos existen distintos tipos de tests: unitarios, de integración y *end to end*. 

En este caso queremos testear una API REST y lo más importante es testear a los endpoints y el efecto que generan en la base de datos por lo tanto decimos que son **tests de integración** (incluso se podría decir que son *e2e*). No utilizamos **tests unitarios** pues estos nos servirían para testear de manera aislada un método. Utilizando el paquete supertest haremos las peticiones http y luego jest se encargará de evaluar los resultados.



## Variables Entorno y Scripts

A la hora de testear el backend lo primero que debemos hacer es modificar el archivo `package.json` en particular los scripts para asegurarnos cargar con un valor distinto la variable de entorno `NODE_ENV` según ejecutemos estemos en *development* `npm run dev`, *production* `npm start` o *testing* `npm test`.

Si estamos utilizando Windows tendremos que utilizar el paquete `cross-env`.

```json
{ 
    "scripts": {
        "dev": "cross-env NODE_ENV=development nodemon index.js",
        "start": "cross-env NODE_ENV=production node index.js",
        "test": "cross-env NODE_ENV=test jest --verbose --silent"
	},
}
```

> También podríamos pasarle un puerto especial para que utilice a la hora de correr los tests en lugar del definido en el archivo `.env` así evitamos colisiones por tener abierto el mismo puerto.

```json
"test": "cross-env NODE_ENV=test PORT=1234 jest --verbose --detectOpenHandles",
```



Luego en función del valor de `NODE_ENV` será que usaremos uno u otro connection string para conectarnos a la base de datos de producción o a la de pruebas.

```javascript
const { NODE_ENV,MONGO_DB_URI,MONGO_DB_URI_TEST} = process.env;
const connectionString = NODE_ENV === 'test'
? MONGO_DB_URI_TEST
: MONGO_DB_URI;
```

> Las variables de entorno `MONGO_DB_URI` y `MONGO_DB_URI_TEST` debemos definirlas en un archivo `.env` . Para acceder a esos valores mediante `process.env` debemos tener instalado `dotenv` y colocar en `index.js` `require('dotenv').config()` en la parte de arriba. Si lo colocamos debajo de la conexión a Mongo `require('./mongo')` no obtendremos el connection string.
>
> El hecho de crear una base de datos para testing no es buena práctica y al estudiar CI/CD veremos que lo más conveniente sería mockearla.


## Test de Endpoints
Como nuestro propósito es testear los endpoints, utilizaremos la biblioteca Supertest para acceder a ellos. Esta herramienta envuelve nuestro servicio de backend y nos permite testear los endponts. 

```bash
npm install supertest -D
```

Creamos un directorio `test` y en el un archivo `notes.test.js` . En esta prueba buscamos verificar que las notas de una api sean devueltas en JSON. 



## Peticiones `GET`

Cuando devolvemos una nota entregamos un status code `200` que significa **OK**.

Debemos tener presente que se trata de una operación asíncrona por lo que debemos esperar a que esté el resultado disponible. Esto lo hacemos con `async` y `await`.

```js
const supertest = require('supertest');
const {app} = require('../index');

const api = supertest(app);

test('the result should be a json', async () => {
    await api
        .get('/api/notes')
        .expect(200)
        .expect('Content-Type', /application\/json/);
});
```



> Como debemos exportar `app` para luego importarlo en el test, quizás nos convenga trabajar con `app.js` con `module.exports = { app };` y con `index.js` que también importará este valor y ejecuta el `app.listen()`
>
> Notar que usamos una RegEx porque podemos recibir `application/json; charset=utf-8` Sino podríamos haber puesto `expect(res.header['content-type'].toBe('application/json'))`.



## Resolución de Errores

En la consola obtenemos un mensaje que nos dice que ha terminado el test pero tenemos cosas abiertas (por ejemplo el servidor). Además nos sugiere que agreguemos la opción `--detectOpenHandles`, hacemos esto por lo que el script nos queda:

```json
"test": "cross-env NODE_ENV=test jest --verbose --silent --detectOpenHandles"
```



Para cerrar el servidor primero lo almacenamos en `server` que luego exportamos:

```js
const server = app.listen('3000', () => console.log(`Listening on Port ${PORT}`));

module.exports = { app, server };
```



Luego en el test importamos el `server` y creamos un hook  `afterAll()` con un callback para que después de todos los tests y cierre la conexión del servidor.

```js
const { app, server } = require('../index');

// todos los tests
// ...

afterAll(() => {
    mongose.connection.close()
    server.close()
})
```



> Con la versión de mongoose `6.0.9` es posible tener problemas para ejecutar los tests y con `--detectOpenHandles` obtenía **Jest has detected the following 1 open handle potentially keeping Jest from exiting: ●  Timeout**. Haciendo el downgrade a la versión `5.13.9` esto se soluciona.



Una vez solucionados todos los mensajes de la consola podemos sacar el `--detectOpenHandles`

```
"test": "cross-env NODE_ENV=test jest --verbose --silent"
```



## Watch Mode

Para no tener que correr el test manualmente con `npm run test` agregamos un nuevo script que se quedará atento a los cambios que hagamos en los archivos y volviendo a ejecutar los tests cuando estos sucedan:

`"test:watch": "cross-env NODE_ENV=test jest --verbose --silent --watch"`



O lo que es lo mismo reutilizando el script original de test:

```
"test:watch": "npm run test -- --watch"
```

> Hacemos referencia al script anterior y queremos pasarle más parámetros. Usamos `--` y luego `--watch` porque queremos pasarle el `--watch` al comando del script `test` y no al `npm run test`



Luego con `npm run test:watch` ejecutamos los tests y a partir del próximo cambio se ejecutarán automáticamente.



## `skip()`

Si queremos que no ejecuten una serie de tests momentáneamente podemos hacer lo siguiente:

```js
describe.skip(()=> {
	test('...', () => {
		//...
	});
	
	test('...', () => {
		//...
	});
});
```



En caso de querer saltear un test individual también podemos hacerlo con:

```js
test.skip('...', () => {
		//...
});
```

 

> Es posible configurar una **regla del linter** que muestre warning en caso de que salteemos tests.



## `only()`  

Si sólo queremos correr un conjunto de tests podemos poner:

```
describe.only(()=> {
	test('...', () => {
		//...
	});
	
	test('...', () => {
		//...
	});
});
```

En caso de querer correr sólo un test:

```
test.only('...', () => {
		//...
});
```



## Testear Archivo Específico

En ocasiones **cuando estemos testeando un archivo y saltando los tests de otros archivos** puede resultar molesto ver en pantalla datos de los tests saltados. En esos casos podemos editar el script indicando explícitamente el archivo que queremos testear:

```bash
"test": "jest --verbose --silent tests/notes.test.js"
```



Si queremos ir más allá y testear un archivo específico y un test específico podemos hacerlo con:

```
npm run test -- -t 'the first note should be about Juan'
```

> No es necesario poner el nombre completo podríamos poner simplemente "juan" y en cualquier capitalización.



## Estado Inicial con `beforeEach`

Los tests deben ser **predecibles** de modo tal que siempre arrojen el mismo resultado. 

Primero verificamos que una ruta devuelve el **status code** y el **Content-Type** esperados. Sin embargo, si queremos verificar por ejemplo la cantidad de elementos devueltos por una ruta dependeríamos de un factor externo como que alguien haya agregado un elemento a la base de datos de testing y así el test fallaría.

Queremos tener la certeza de que el contenido de la base de datos es el esperado. Para ello antes de cada test borraremos todas las notas y luego agregaremos las de `initialNotes`. Esto lo hacemos utilizando el hook `beforeEach`

```js
const mongoose = require('mongoose');
const supertest = require('supertest');
const { app, server } = require('../index');
const Note = require('../models/Note');

const api = supertest(app);

const initialNotes = [
    {
        content: "Notas de Juan",
        important: true,
        date: new Date()
    },
    {
        content: "Nota de Paco",
        important: true,
        date: new Date()
    },
    {
        content: "Notas de Pedro",
        important: true,
        date: new Date()
    }
]

beforeEach(async () => {
    await Note.deleteMany({});

    const note1 = new Note(initialNotes[0])
    await note1.save()

    const note2 = new Note(initialNotes[1])
    await note2.save()

    const note3 = new Note(initialNotes[2])
    await note3.save()
})

describe('GET /api/notes', () => {
    test('there are three notes', async () => {
        const res = await api.get('/api/notes')
        expect(res.body).toHaveLength(initialNotes.length);
    });
})

afterAll(() => {
    server.close()
    mongoose.connection.close();
})
```

También podríamos haber verificado que devuelva un array con 

```
expect(res.body).toBeInstanceOf(Array);
```



## Otras Peticiones `GET`

Si queremos testear que el contenido de un elemento en particular tenga ciertas características:

```js
test('the first note should be about Juan', async () => {
const res = await api.get('/api/notes');

expect(res.body[0].content).toBe('Notas de Juan');
});
```

Sin embargo, muchas veces queremos verificar el contenido en cualquier posición del array devuelto por la ruta:

```js
test('there should be a note about Paco', async () => {
    const res = await api.get('/api/notes');

    const contents = res.body.map(note => note.content);

    expect(contents).toContain('Notas de Paco');
});
```



## Peticiones `POST`

Cuando creamos una nota devolvemos un status code `201` que significa **created**.

```js
test('a new note should be added', async () => {
    const newNote = {
    	content: 'Notas de De La Mar',
    	important: false,
    };

    await api
    .post('/api/notes')
    .send(newNote)
    .expect(201)
    .expect('Content-Type', /application\/json/);

    const res = await api.get('/api/notes');

    const contents = res.body.map(note => note.content);

    expect(contents).toContain('Notas de De La Mar');
    expect(res.body).toHaveLength(initialNotes.length + 1);
});
```



Luego de agregar el elemento a la base de datos estamos chequeando tanto que el contenido sea el deseado como que el número de elementos también sea coherente.



Debemos chequear las condiciones de borde, por ejemplo que no sea posible agregar un elemento si falta un campo requerido. 

```js
test('an empty note should not be added', async () => {
    const newNote = {
    	important: false,
    };
    await api.post('/api/notes').send(newNote).expect(400);

    const res = await api.get('/api/notes');
    expect(res.body).toHaveLength(initialNotes.length);
});
```



> En este caso verificamos obtener un error 400 y que no hayan sido agregados elementos a la base de datos.

  ## Peticiones `DELETE`

Cuando eliminamos una nota devolvemos un status code `204` que significa **no content**.

Los tests los agrupamos dentro de este bloque:

```js
describe('DELETE /api/notes', () => {

})
```



Por ejemplo para testear el borrado de una nota.

Veremos mas adelante que `getAllNotes` es un helper para obtener las notas.

```js
test('a note should be deleted', async () => {
		const response = await getAllNotes()

		const { body: notes } = response;

		const [noteToDelete] = notes;

		await api
			.delete(`/api/notes/${noteToDelete.id}`)
			.expect(204)

		const response2 = await api.get('/api/notes');
		const content = response2.body.map(note => note.content)

		expect(response2.body).toHaveLength(initialNotes.length - 1)
		expect(contents).not.toContain(noteToDelete.content)
	})
```



> Estamos verificando que la cantidad de notas se reduce en una y que la nota que quisimos borrar no forma parte de las restantes.



Mientras que para testear que no se realiza el borrado si el ID es incorrecto:

```js
test('a note should not be deleted', async () => {
		await api
			.delete(`/api/notes/123`)
			.expect(400)

		const response = await api.get('/api/notes');

		expect(response.body).toHaveLength(initialNotes.length)
	})
```


## Test Helpers

Cuando tenemos líneas de código que se repiten es conveniente crear un archivo dentro de la carpeta `test` llamado `helpers.js` donde colocamos esas funciones o constantes requeridas.

Se recomienda **no utilizar helpers para el cuerpo del test** ya que aunque ese código se repita nos ayudará a saber de dónde viene el problema en lugar de tener una abstracción que lo haría más dificil.

Al implementar esto pondremos de manifiesto la importancia de los tests, ya que al refactorizar podremos tener certeza de que no hemos roto nada si seguimos pasando los tests.

Por ejemplo estas líneas que usamos dos veces las convertimos en:

```js
const res = await api.get('/api/notes');
const content = res.body.map(note => note.content)
```



```js
const getAllContentFromNotes = async () => {
	const res = await api.get('/api/notes');
	return {
		res,
		contents: res.body.map(note => note.content)
	}
}
```

> Notar que en `getAllContentFromNotes` retornamos `res` y el array `contents` ya que necesitamos ambos para las comprobaciones que hacemos a continuación.

Luego para usarlo `const {res, contents} = await getAllContentFromNotes()` .




Otra forma es implementando dos métodos:

```js
const getAllNotes = async () => {
	return api.get('/api/notes');
}

const getAllContentFromNotes = (notes) => {
	return notes.map(note => note.content)
}
```



Luego para utilizarlos:

```js
const response = await getAllNotes();
const contents = getAllContentFromNotes(response.body);
```



### Refactorizar `beforeEach()`

Hasta el momento tenemos 

```js
beforeEach(async () => {
	await Note.deleteMany({});

	const note1 = new Note(initialNotes[0]);
	await note1.save();

	const note2 = new Note(initialNotes[1]);
	await note2.save();

	const note3 = new Note(initialNotes[2]);
	await note3.save();
});
```



Esto mismo no podemos efectuarlo utilizando `forEach()` ya que no funciona de manera correcta con `async/await`. 

Tenemos dos alternativas

* Usar `forOf`

```js
beforeEach(async () => {
	await Note.deleteMany({});

	for(const note of initialNotes){
		const noteObject = new Note(note);
		await noteObject.save();
	}
});
```

Este método trabaja secuencialmente por lo que nos aseguramos que los elementos sean guardados en el mismo orden que están en el array.

* Usando `map` y `Promise().all`

```js
beforeEach(async () => {
	await Note.deleteMany({});

	const notesObject = initialNotes.map(note => new Note(note))
	const promises = notesObject.map(noteObject => noteObject.save());
    await Promise.all(promises);
});
```

Este método tiene la ventaja que realiza el trabajo en paralelo y desventaja que puede que no incorpore  los elementos en orden.



## Tests de Administración de Usuarios

Los modelos y controladores vinculados a la administración de usuarios se ubican en el apartado **Administración de Usuarios** en caso de dudas consultar ese material.



Creamos el archivo `user.test.js` y nos proponemos testear una ruta `POST` de creación de usuarios . Para ello con `beforeEach` nos aseguramos borrar todos los usuarios y luego creamos uno y lo guardamos en la base de datos. 

En el test en sí lo primero que hacemos es obtener todos los usuarios en un array (sabemos que debería haber sólo uno pero de este modo lo dejamos prepado para si luego creamos más en el `beforeEach`) y después agregamos un nuevo usuario y por último verificamos tener uno mas que al inicio.

Con `afterAll` nos aseguramos cerrar el servidor y la conexión de mongoose.

```js
const bcrypt = require('bcrypt')
const User = require('../models/User')
const { api, getUsers } = require('./helpers')
const moongose = require('mongoose')
const { server } = require('../index')

describe('creating a new user', () => {
    beforeEach(async () => {
        await User.deleteMany({})

        const passwordHash = await bcrypt.hash('pswd', 10)
        const user = new User({ username: 'jnmroot', passwordHash })

        await user.save()
    })

    test('should create a new user with a fresh username', async () => {
        const usersAtStart = await getUsers()

        const newUser = {
            username: 'midudev',
            name: 'Miguel',
            password: 'tw1tch'
        }

        await api
            .post('/api/users')
            .send(newUser)
            .expect(201)
            .expect('Content-Type', /application\/json/)

        const usersAtEnd = await getUsers()

        expect(usersAtEnd).toHaveLength(usersAtStart.length + 1)

        const usernames = usersAtEnd.map(u => u.username)
        expect(usernames).toContain(newUser.username)
    })


    afterAll(() => {
        moongose.connection.close()
        server.close()
    })
})
```



Inicialmente sabemos que deberíamos tener un único usuario dado que así lo hemos determinado en el `beforeEach`, no obstante verificamos esto con:

```js
const usersAtStart = await getUsers()
```



Siendo `getUsers()` un helper con el siguiente contenido:

```js
const getUsers = async () => {
	const usersDB = await User.find({})
	return usersDB.map(user => user.toJSON())
}
```

Notar que estamos trabajando con el modelo ya que a diferencia de cuando queríamos todas las notas, no tenemos un endpoint para tal fin. Con `User.find({})` obtenemos los datos tal cual están en la DB como un array de objetos que tienen `_id` y `__v` es por eso que con un map los sometemos a un `toJSON()`.



## Test Driven Development

TDD son las siglas de *test driven development* que es una forma de desarrollo en la cual primero se realiza el test y luego se crea el código, esto nos permite desarrollar de manera más fiable.

Existen distintas formas de trabajo: Una forma sería ir paso a paso desde el comienzo creando tests y luego desarrollando las funcionalidades y otra forma es plantear directamente un caso de uso. 

Por ejemplo siguiendo esta última forma de trabajo, puede que queramos testear la condición de borde de qué sucede si intentamos crear un usuario cuyo `username` ya existe. La idea sería crear el test, que nos de rojo, luego desarrollar el código hasta que no de verde y finalmente refactorizar.

```js
test('should not create user if username is already taken', async () => {
    const usersAtStart = await getUsers()

    const newUser = {
    username: 'miduroot',
    name: 'Miguel',
    password: 'midutest'
    }

    const result = await api
    .post('/api/users')
    .send(newUser)
    .expect(409)
    .expect('Content-Type', /application\/json/)

    console.log(result.body)

    expect(result.body.error).toContain('expected `username` to be unique')

    const usersAtEnd = await getUsers()
    expect(usersAtEnd).toHaveLength(usersAtStart.length)
})
```

Esperamos un 409 que significa **conflict**. También esperamos un `Content-Type` de `application/json` ya que vamos a querer enviar el error al cliente. A su vez también chequeamos que el JSON devuelto tenga una propiedad `error` con el valor `expected username to be unique`



Por último con `usersAtEnd` buscamos verificar que no haya sido agregado el usuario a la base de datos a pesar de ya haber constatado todo lo anterior.

# Mocha

> Basado en [The Net Ninja - MondoDB Tutorial for Beginners](https://www.youtube.com/watch?v=9OPP_1eAENg&list=PL4cUxeGkcC9jpvoYriLI0bY8DOgWZfi6u)
> Basado en [Traversy Media - Intro To JavaScript Unit Testing With Mocha JS & Chai](https://www.youtube.com/watch?v=MLTRHc5dk6s)

## Introducción

Es el testing framework más popular pero para ciertas tareas require del uso de librerías como [Chai](https://www.chaijs.com/) y [Sinon](https://sinonjs.org/). Esto tiene la contra de tener que ver más de una documentación y dependiendo de la evolución de estas bibliotecas en un futuro algo puede dejar de ser compatible con mocha.

Si queremos usar la assertion library default:
```js
const assert = require('assert');
```

Nos permite testear la conexión con la db, creación, lectura, actualización y eliminación de documentos en una colección.

`npm install mocha`

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
const MONGO_DB_URI = 'mongodb://localhost/unit-testing';

mongoose.connect(MONGO_DB_URI, {
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

## Hooks 
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

## Encontrar Documentos
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

## Eliminar Documentos:
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


## Actualizar Documentos:
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

## Actualizar Documento con Update Operators
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

## Documentos Anidados
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
