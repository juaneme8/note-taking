# Código Asincrónico
El código asincrónico no ocupa lugar en el stack de ejecución de JavaScript por lo que continúa ejecutando otras líneas y pasado el tiempo de asincronía realiza la acción pendiente. 

La función `setTimeout()` nos permite ejecutar código no bloqueante o asincrónico. 


```jsx
setTimeout(()=> {
	console.log('Mensaje 1');
},1000);
console.log('Mensaje 2');
console.log('Mensaje 3');
```

En la consola veremos Mensaje 2, Mensaje 3 y un segundo después veremos Mensaje 1.

## Callback
Los callbacks son parte fundamental del código asincrónico o no bloqueante. Se trata de una función que será ejecutada cuando otra función termine de ejecutarse.

```jsx
function foo(item, callback){
	console.log(item);
	callback();
}
```

Siendo `callback()` por ejemplo la siguiente *function expression* `const callbck = () => {...}`

## Promesas
Si bien el uso de *callbacks* permite ejecutar código de manera asincrónica, cuando tenemos una sucesión de eventos asincrónicos el código se vuelve bastante engorroso. Como solución a esto surgen las promesas y se trata de operaciones que van a terminar en el futuro. Esto es muy útil a la hora de consumir datos de una API.

## AJAX
La mayor parte de las cosas que podemos lograr trabajando de manera asincrónica involucran una API llamada XHR más conocida como AJAX.

### HTTP
**HTTP** es un protocolo standard de la web para transferencia de información entre un servidor y un cliente.
El servidor por seguridad nos perdirá autenticación (nombre de usuario, password, token, etc) para chequear la identidad.

En las Devtools cuando la pestaña de Network veremos todos los recursos que estamos descargando para la correcta visualización del sitio a través del protocolo http.
En los mensajes además del **body** contamos con **headers** (request y response headers) y esta información podremos verla también las Devtools. 



# Promesas

## `Promise.all`

```js
 const p1 = 
       fetch("https://rickandmortyapi.com/api/character/?name=cowboy")
 .then((response) => response.json())
 .then((data) => data.results[0]);

   const p2 = 
         fetch("https://rickandmortyapi.com/api/character/?name=leader")
   .then((response) => response.json())
   .then( (data) => data.results[0]);

const p3 = 
      fetch("https://rickandmortyapi.com/api/character/?name=daron")
.then( (response) => response.json())
.then( (data) => data.results[0]);

Promise
  .all([p1, p2, p3])
  .then((allData) => console.log(allData));
```

En `allData` tendremos un array con los datos devueltos por cada promesa. Esto es particularmente útil en aquellos casos en que los datos están relacionados y basta con que una promesa falle para que no obtengamos datos a la salida.

