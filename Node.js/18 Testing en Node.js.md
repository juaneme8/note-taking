# Introducción al Testing

🧪 [Introducción al Testing con JEST ✅ (Bootcamp FullStack) - midudev](https://www.youtube.com/watch?v=_DzBez4qMi0)

La forma más simple de entender el testing es pensando que nos permitirá comprobar el correcto funcionamiento de un método.

Es importante que los tests sean predecibles, que no dependan de factores externos de modo tal que siempre que los ejecute obtenga el mismo resultado.

A la hora de crear tests es importante verificar que fallan pues quizás nunca falle y eso lo convertiría en un test inútil.

Supongamos que tenemos:

```javascript
const suma = (a,b) => {
  return a-b;
}
```

Si bien en este caso podemos darnos cuenta fácilmente qué está mal, en ocasiones será mucho más difícil notarlo. 

La importancia de los tests está no sólo en saber si nuestro método funciona o no, sino que también nos permite documentar estos métodos .

## Test en RunJS

Para testear este método de manera manual, podemos hacerlo en un entorno controlado utilizando **RunJS** que actúa como banco de pruebas o playground. 

Bastará con ingresar `suma(1,2) === 3` y nos aparecerá `false` lo cual nos da la pauta de que no está funcionando correctamente.

Otra forma de plantearlo podría ser `if(suma(1,2)!==3) console.log('suma of 1 and 2 expected to be 3')` o bien tirando un error para hacerlo más semántico `if(suma(1,2)!==3) new Error('suma of 1 and 3 expected to be 3')`

Sin embargo puede que para otras condiciones el método sí funcione, por ejemplo si pusiéramos como argumentos 0 y 0 el `if(suma(0,0)!==0) new Error('suma of 0 and 0 expected to be 0')` no sería ejecutado ya que da el valor esperado.



## Test en Node

Para testear esto mismo también manualmente pero directo en el proyecto de Express creamos una carpeta `tests ` y un archivo `suma.test.js`.

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



## Testing con Jest 

Jest es un testing framework o *test runner* que inicialmente fue desarrollado para hacer testing en el navegador y luego evolucionó siendo posible utilizarlo en servidor.

```
npm install jest -D
```

> Mocha y AVA son otras alternativas que al igual que Jest son frameworks de testing que podríamos haber elegido.



Supongamos que tenemos una serie de métodos que queremos testear en `utils/for_testing.js`. Si tuviéramos que testearlos utilizando `console.assert()` esto sería complejo por lo que utilizaremos Jest. 

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

### Configuración Jest 

Jest como dijimos anteriormente está pensado para trabajar por defecto en el cliente y viene con la capacidad de simular aspectos del DOM. Esto no sólo no lo necesitamos sino que puede ser problemático. Es por eso que en `package.json`  vamos a colocar:

```json
"jest":{
	"testEnvironment": "node"
}
```

> Otra opción sería colocarlo en `jest.config.js` pero lo ideal es tenerlo todo en un mismo lugar así es más fácil de encontrar y con menos archivos.

> Es posible que en Windows experimentemos problemas si el nombre de la carpeta tiene espacios (por ejemplo si nuestro nombre de usuario los tiene).



### Utilización Jest

> Jest por defecto buscará todos los archivos que terminen en `.test.js`
>
> No es necesario importar `jest` ya que una vez que encuentra este archivo sabe que debe utilizar esas dependencias.



#### Test de palindrome

En primer lugar creamos el archivo `palindrome.test.js` en la carpeta `test`.



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



#### Métodos Jest

Podemos ver la lista completa de métodos en la [documentación](https://jestjs.io/docs/expect).

* `toBe()`
* `toBeNull()`
* `toBeUndefined()`
* `toEqual()`



### Configuración Script

En `package.json` debemos modificar el script `"test"` de modo que quede de la siguiente forma:

```json
"scripts":{
	"test": "jest --verbose --silent"
}
```

> Con `--verbose` le indicamos que queremos que nos entregue la máxima información posible.
>
> Con `--silent` le indicamos que no queremos que nos muestre los `console.log()`.

Luego ejecutamos los tests con `npm run test`



### Configuración ESLINT

Para evitar que el linter nos marque como errores el uso de `test()` y `expect() ` debemos modificar el la configuración de eslint.

Es posible que lo tengamos en `package.json` y en ese caso debemos modificar el objeto `eslintConfig` **agregando** lo siguiente:

```json
"eslintConfig":{
	"env":{
		"jest": true,
	}
}
```



# Testing API Rest

[🔴 Testing de Backend con Express usando Jest y Supertest - midudev](https://www.youtube.com/watch?v=_xxVJdGNMrs)



Como sabemos existen distintos tipos de tests: unitarios, de ingregración y *end to end*. 

En este caso queremos testear una API REST y lo más importante es testear a los endpoints y el efecto que generan en la base de datos por lo tanto decimos que son **tests de integración** (incluso se podría decir que son *e2e*). No utilizamos **tests unitarios** pues estos nos servirían para testear de manera aislada un método.



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

Debemos tener presente que se trata de una operación asíncrona por lo que debemos esperar a que esté el resultado disponible. Esto lo hacemos con `async` y `await`.

```js
const supertest = require('supertest');
const app = require('../index');

const api = supertest(app);

test('the result should be a json', async () => {
    await api
        .get('/api/notes')
        .expect(200)
        .expect('Content-Type', /application\/json/);
});
```



> Notar que debemos exportar `app` en`index.js`
>
> Notar que usamos una RegEx porque podemos recibir `application/json; charset=utf-8`



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
    moongose.connection.close()
    server.close()
})
```



> Con la versión de mongoose `6.0.9` tuve problemas para ejecutar los tests y con `--detectOpenHandles` obtenía **Jest has detected the following 1 open handle potentially keeping Jest from exiting: ●  Timeout**. Haciendo el downgrade a la versión `5.13.9` esto se soluciona.



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



## Saltar Tests

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



## Peticiones GET

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



## Peticiones POST

```js
test('a new note should be added', async () => {
    const newNote = {
    content: 'Notas de De La Mar',
    important: false,
    };

    await api
    .post('/api/notes')
    .send(newNote)
    .expect(200)
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

  

## Test Helpers

Como podemos ver hay líneas de código que se repiten bastante, es por eso que creamos un archivo dentro de `test` llamado `helpers.js`.
