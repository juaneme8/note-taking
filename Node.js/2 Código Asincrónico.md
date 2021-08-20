# Código Sincrónico y Asincrónico
## Código Sincrónico
Los programas sincrónicos o bloqueantes son aquellos en los que la segunda línea de código debe esperar a que se ejecute la primera.
```js
console.log('mensaje 1');
console.log('mensaje 2');
```
## Código Asincrónico
Los programas asincrónicos no son bloqueantes, por lo que es posible que una línea ubicada más abajo se ejecute antes que una ubicada más arriba.
Tenemos que tener presente que asincrónico no singnifica multithread o concurrencia, sino que tenemos un único thread.

```js
setTimeout(() => {
	console.log('mensaje1');
}, 3000);
console.log('mensaje2');

//veremos en la consola:
// mensaje2
// mensaje1 (aparece 3 segundos después)
```
`setTimeout()` programa una tarea para ser ejecutada en el futuro. Una vez programada la tarea el control es retornado por lo que llegamos a `console.log('mensaje 2')` . Luego del tiempo estipulado en el segundo argumento que simula por ejemplo el tiempo que tardaríamos en obtener algo de una base datos, ejecuta el callback (función que le pasamos como primer argumento) por lo que llegamos a `console.log('mensaje 1')` .
Siguiendo con la misma línea con getUser simulados una lectura de datos de un usuario en una DB:
```js
const  getUser  = (id) => {
	setTimeout(() => {
		console.log('datos db');
		return { id: id, gitUser: 'juaneme8' };
	}, 2000);
};

console.log('mensaje1');
const  user  =  getUser(1);
console.log(user);
console.log('mensaje2');
```

En la consola veremos:
1) `mensaje1`
2) `undefined` como resultado del `console.log(user)` dado que el valor retornado está disponible dos segundos después.
3) `mensaje2`

**Aclaración:**
Si bien JavaScript es single thread esto hace referencia al código que escribimos nosotros pero ciertos requests son manejados por otros threads que corren fuera de JavaScript. Por ejemplo en funciones de lectura de archivos, se le pasa la ejecución a un thread separado encargado del acceso al *file system* y luego cuando culmina ejecuta el *callback*.

Existen tres **patrones** para lidiar con llamadas asincrónicas y lograr imprimir este valor en pantalla en lugar de `undefined`.

* callbacks
* promises
* async/await

# callback
Agregamos a la función `getUser()` un segundo parámetro llamado `callback`, se trata de una función que es llamada cuando el resultado de una operación asincrónica está listo.

```js
console.log('mensaje1');
getUser(1, (user) => {
	console.log(user);
});
console.log('mensaje2');

function  getUser(id, callback) {
	setTimeout(() => {
		console.log('datos db');
		callback({ id: id, gitUser: 'juaneme8' });
	}, 2000);
}
```
Ahora a la salida obtenemos: `mensaje1`luego `mensaje2` y dos segundos después `datos db` + `{ id: 1, gitUser: 'juaneme8' }` .

Supongamos ahora que una vez obtenido el usuario queremos obtener de la API de Github  la lista de repositorios de ese usuario y una vez obtenidos ellos obtener los commits asociados al primer repositorio. Nuevamente trata de operaciones asincrónica ya que demandan cierto tiempo.

```js
console.log('mensaje1');
getUser(1, (user) => {
	console.log(user);
	getRepositories(user.gitUser, (repos) => {
		console.log(repos);
		getCommits(repos[0], (commits) => {
			console.log(commits);
		});
	});
});
console.log('mensaje2');

function  getUser(id, callback) {
	setTimeout(() => {
		console.log('datos usuario id='  +  id);
		callback({ id: id, gitUser: 'juaneme8' });
	}, 2000);
}

function  getRepositories(gitUser, callback) {
	setTimeout(() => {
		console.log('repositorios usuario='  +  gitUser);
		callback(['repo1', 'repo2', 'repo3']);
	}, 2000);
}

function  getCommits(repo, callback) {
	setTimeout(() => {
		console.log('commits repositorio='  +  repo);
		callback(['commit1', 'commmit2', 'commmit3']);
	}, 2000);
}
```
Notar que al utilizar callbacks tenemos una **estructura anidada** que resulta compleja de leer en particular en aplicaciones donde el anidamiento es muy grande. Esto se conoce como **callback hell** o **christmas tree problem** debido a la apariencia de las indentaciones.

> Para aplanar esta estructura podemos usar **named functions** para los callbacks, conviene empezar desde el nivel más profundo de anidamiento. [+ info en Code with Mosh - The Complete Node.js Course - Video 68]

# Promesas
Una promesa es un **objeto** que almacena el eventual resultado de una operación asincrónica. Cuando esta operación concluye puede resolver tanto en un valor o en error.
Una promesa tiene 3 estados posibles:
* pending: cuando creamos un objeto promesa
* fulfilled: value
* rejected: error
```js
const  p  =  new  Promise(function (resolve, reject) {
const  sum  =  2  +  2;
//trabajo asincrónico
if (sum  ===  4) 
	resolve(1);
else  
	reject(new  Error('mensaje de error'));
	//es considerado una best practice pasar objeto de error y no un string
});

//consumo promesa
p
.then(result =>  console.log(result))
.catch(error =>  console.log(error));
```

Las promesas exponen el método `then()` y es posible encadenarlas para llevar a cabo una operación asincrónica compleja.

Las promesas nos permiten lograr una implementación con estructura plana (flat), para ello donde tengamos una función asincrónica con un callback debemos modificarla de modo que **retorne una promesa**. 

Si en la implementación que estamos haciendo no queremos rechazar la promesa, no incluimos el parámetro directamente.

```js
console.log("1");
//Consumir Promesa
getUser(1)
.then(user  =>  getRepositories(user.usuario))
.then(repos  =>  getCommits(repos[0]))
.then(commit  =>  console.log(commit))
.catch(err  =>  console.log(err.message));
console.log("2");

function  getUser(id) {
	return  new  Promise((resolve, reject) => {
	//Async Work
	setTimeout(() => {
		console.log("Datos DB ok!");
		resolve({ id: id, usuario: "jnm8" });
	}, 2000);
	});
}
 
function  getRepositories(us) {
	return  new  Promise((resolve, reject) => {
		setTimeout(() => {
			console.log("Datos GIT ok!");
			resolve(["repo1", "repo2", "repo3"]);
		}, 2000);
	});
}
 
function  getCommits(repo) {
	return  new  Promise((resolve, reject) => {
		setTimeout(() => {
			console.log("Datos Commit ok!");
			resolve(["com1", "com2", "com3"]);
		}, 2000);
	});
}
// 1
// 2
// Datos DB ok!
// Datos GIT ok!
// Datos Commit ok!
// [ 'com1', 'com2', 'com3' ]
```

## Settled Promises
En algunas oportunidades vamos a crear una promesa que ya está resulta (resolved), esto es útil cuando estamos realizando **unit tests**. Queremos simular una situación en la cual una operación asincrónica se completó exitosamente.

```js
const  p  =  Promise.resolve({ id: 1 });
p.then((value) =>  console.log(value));
```

De la misma manera en oportunidades queremos crear una promesa que fue rechazada (rejected):

```js
const  p  =  Promise.reject(new  Error('descripción error'));
p.catch((error) =>  console.log(error));
```

> En estos dos casos hablamos de promesas determinadas o settled promises.

## Parallel Promises
En oportunidades queremos ejecutar algunas operaciones asincrónicas en paralelo y cuando todas se completan hacer algo.

```js
const  p1  =  new  Promise((resolve) => {
	setTimeout(() => {
		console.log('operación 1');
		resolve(1);
	}, 1000);
});

const  p2  =  new  Promise((resolve) => {
	setTimeout(() => {
		console.log('operación 2');
		resolve(2);
	}, 5000);
});

Promise.all([p1, p2]).then((result) =>  console.log(result));
```

A `Promise.all()` le pasamos un *array de promesas* y obtenemos como resultado también un *array*. 
Tener en cuenta que no tenemos multithreading, seguimos lidiando con un único thread pero este está llevando a cabo múltiples operaciones asincrónicas. Primero comienza con la operación inicial, luego el thread es liberado para comenzar la segunda sin esperar el resultado de la primera.
Basta con que una promesa falle para que la promesa retornada por `Promise.all()` se considere *rejected* (el error lo veremos con el método `catch`), en ese caso debemos agregar el parámetro *reject* al crear `p1` y `p2` y llamarlo en lugar de *resolve*.

Cuando queremos hacer algo apenas una de las operaciones asincrónicas se completa:
```js
Promise.race([p1, p2]).then((value) =>  console.log(value));
```
En este caso en value obtenemos el valor de la primera en completarse y no un array.

# Async & Await
async y await nos permiten crear código asincrónico con una apariencia similar a la del código sincrónico, están construidos sobre la base de promesas (decimos que son *sintactically sugars*)
Cuando llamamos a una función que retorna una promesa, podemos poner *await* y luego almacenar el resultado como si fuera una función sincrónica.
 El **operador await** debe ser usado siempre dentro de una función decorada con el **modificador async**.
 Con async y await no tenemos el método catch por lo que debemos usar un **bloque try/catch** para obtener los errores.

```js
async  function  displayCommits() {
	try {
		console.log(1);
		const  user  =  await  getUser(1);
		const  repos  =  await  getRepositories(user.usuario);
		const  commit  =  await  getCommits(repos[0]);
		console.log(commit);
		console.log(2);
	} 
	catch (err) {
		console.log('Error', err.message);
	}
}
displayCommits();
```