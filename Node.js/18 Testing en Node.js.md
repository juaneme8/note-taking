# Introducción al Testing

🧪 [Introducción al Testing con JEST ✅ (Bootcamp FullStack) - midudev](https://www.youtube.com/watch?v=_DzBez4qMi0)

La forma más simple de entender el testing es pensando que nos permitirá comprobar el correcto funcionamiento de un método.

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



Lo primero que podemos hacer es crear un proyecto sobre el cual trabajar:

```bash
mkdir testing
cd testing
npm init -y
```

Luego instalamos las dependencias 

```bash
npm i express
```



A la hora de testear el backend lo primero que debemos hacer es modificar el archivo `package.json` en particular los scripts para asegurarnos cargar con un valor distinto la variable de entorno `NODE_ENV` según ejecutemos estemos en *development* `npm run dev`, *production* `npm start` o *testing* `npm test`.

Si estamos utilizando Windows tendremos que utilizar el paquete `cross-env`.

```json
{ 
    "scripts": {
        "dev": "cross-env NODE_ENV=development nodemon index.js",
        "start": "cross-env NODE_ENV=production node index.js",
        "test": "cross-env NODE_ENV=test jest --verbose"
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



Como nuestro propósito es testear los endpoints, utilizaremos la biblioteca [Supertest](https://www.npmjs.com/package/supertest) para acceder a ellos. Nos permite probar servidores HTTP con microservicios que tengan endponts.

```bash
npm install supertest -D
```



Creamos un directorio `test` y en el un archivo `notes.test.js` . En esta prueba buscamos verificar que las notas de una api sean devueltas en JSON. 



Debemos tener presente que se trata de una operación asíncrona por lo que debemos esperar a que esté el resultado disponible. Esto lo hacemos con `async` y `await`.

```js
const supertest = require('supertest');
const app = require('../index');

const api = supertest(app);

test('notes are returned in json', async () => {
	 await api
     	.get('/api/notes')
    	.expect(200)
    	.expect('Content-Type', /application\/json/)
});
```



> Notar que debemos exportar `app` en`index.js`
>
> Notar que usamos una RegEx porque podemos recibir `application/json; charset=utf-8`



A la salida obtenemos un mensaje que nos dice que ha terminado el test pero tenemos cosas abiertas (por ejemplo el servidor). Además nos sugiere que agreguemos la opción `--detectOpenHandles`, hacemos esto por lo que el script nos queda:

```json
"test": "cross-env NODE_ENV=test jest --verbose --silent --detectOpenHandles"
```



Para cerrar el servidor primero lo almacenamos en `server` que luego exportamos:

```js
const server = app.listen('3000', () => console.log(`Listening on Port ${PORT}`));

module.exports = { app, server };
```



Luego en el test importamos el `server` y creamos un hook  `afterAll()` para que después de todo slos tests se cierre la conexión del servidor.

```js
const { app, server } = require('../index');

// todos los tests
// ...

afterAll(() => {
    server.close()
})
```

