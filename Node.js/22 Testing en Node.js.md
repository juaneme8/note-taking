🧪 [Introducción al Testing desde Cero con JEST ✅ (Bootcamp FullStack Gratuito) - midudev](https://www.youtube.com/watch?v=_DzBez4qMi0)

# Introducción al Testing

La forma más simple de entneder el testing es pensando que nos permitirá comprobar el funcionamiento de un método.

Supongamos que tenemos:

```javascript
const suma = (a,b) => {
  return a-b
}
```

Si bien en este caso podemos darnos cuenta fácilmente qué está mal, en ocasiones será mucho más dificil notarlo. 



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

Si bien podríamos ampliar la información devuelta en el mensaje de error para aportar mas datos, esto no tiene sentido pues como veremos mas adelante existen herramientas que se encargan de esto.



# Testing API Rest

Como sabemos existen distintos tipos de tests: unitarios, de ingregración y *end to end*. 

En este caso queremos testear una API REST y lo más importante es testear a los endpoints y el efecto que generan en la base de datos por lo tanto decimos que son **tests de integración** (incluso se podría decir que son *e2e*). No utilizamos **tests unitarios** pues estos nos servirían para testear de manera aislada un método.

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

> Las variables de entorno `MONGO_DB_URI` y `MONGO_DB_URI_TEST` debemos definirlas en un archivo `.env`.
>
> El hecho de crear una base de datos para testing no es buena práctica y al estudiar CI/CD veremos que lo más conveniente sería mockearla.



Como nuestro propósito es testear los endpoints, utilizaremos la biblioteca [Supertest](https://www.npmjs.com/package/supertest) para acceder a ellos. Nos permite probar servidores HTTP con microservicios que tengan endponts.

```bash
npm install supertest -D
```



Creamos un directorio `test` y en el un archivo `notes.test.js`
