# Código Asincrónico
El código asincrónico no ocupa lugar en el stack de ejecución de JavaScript por lo que continúa ejecutando otras líneas y pasado el tiempo de asincronía realiza la acción pendiente. De esta manera podemos dividir una tarea grande en otras más pequeñas y ejecutar cada una de esas de manera simultánea. Cuando trabajamos con código bloqueante en cambio debemos esperar que una tarea termine para ejecutar la siguiente, por lo que si   una se detiene se detienen todas.

En JavaScript existen tres formas de ejecutar código asincrónico:

* Callbacks
* Promesas
* Async / Await

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

Los callbacks son parte fundamental del código asincrónico o no bloqueante. Se trata de una función que será ejecutada desde adentro de otra función.

```jsx
const sum = (a,b) => a+b
const substraction = (a,b) => a-b

const operation = (a,b,callback) => {
  return callback(a,b)
}

operation(1,2,sum)
operation(1,2,substraction)
```

Esto adquiere particular sentido cuando se trata de tareas que llevan cierto tiempo de modo que queremos llamar a la función una vez completado eso.

Supongamos que tenemos una lista de tareas que deben realizarse de manera secuencial como vemos a continuación:

* Place order (2 seconds)
* Cut the fruit (2)
* Add water and ice (1)
* Start the machine (1)
* Select container (2)
* Select toppings (3)
* Serve the icecream (2)



Como dijimos que un callback se trata de una función que es ejecutada dentro de otra función, partimos de  `order` y `production`.

```jsx
const order = (call_production) =>{  
  setTimeout(() => {
    console.log('Order placed')
   	call_production();
  },2000)
};

let production = () =>{

  setTimeout(()=>{
    console.log("production has started")
    setTimeout(()=>{
      console.log("The fruit has been chopped")
      setTimeout(()=>{
        console.log("Liquids added")
        setTimeout(()=>{
          console.log("start the machine")
          setTimeout(()=>{
            console.log("Ice cream placed on container");
            setTimeout(()=>{
              console.log("Toppings added")
              setTimeout(()=>{
                console.log("serve ice cream")
              },2000)
            },3000)
          },2000)
        },1000)
      },1000)
    },2000)
  },0000)

};

order(production)
```



Como podemos ver si bien el uso de *callbacks* permite ejecutar código de manera asincrónica, cuando tenemos una sucesión de eventos asincrónicos el código se vuelve bastante engorroso.

Vemos que el código adquiere una forma de árbol de navidad, esta problemática recibe el nombre de **callback hell**, para evitar esto es que surgen las promesas que nos permiten tener un código visualmente mas limpio.



## Promesas

Como solución a la problemática de los callbacks surgen las promesas y se trata de operaciones que van a terminar en el futuro. Esto es muy útil a la hora de consumir datos de una API ya que quiero realizar una cierta acción una vez que estos datos esten disponibles.

Cuando hacemos una promesa su estado será **pending** por lo que podrá resolverse exitosamente (*resolve - fulfill*) o fallar (*reject*), esto se conoce como ciclo de la promesa.

Si se resuelve trabajamos con el handler `.then`, si es rechazada con `catch` y en cualquiera de los casos con el handler `finally`.

Es posible encadenar promesas.



Supongamos que tengo una función asincrónica `getResult` que debe ir a buscar un valor a una API. En este caso luego de unos segundos obtenemos el valor y ahi se resuelve la promesa. Como consecuencia de esto accedemos al valor en handler `.then`.

```tsx
let order = ( time, work ) => {

  return new Promise( ( resolve, reject )=>{
    if( is_shop_open ){
      setTimeout(()=>{
       // work is 👇 getting done here
        resolve( work() )
		// Setting 👇 time here for 1 work
       }, time)
    }

    else{
      reject( console.log("Our shop is closed") )
    }

  })
}

// step 1
order(2000,()=>console.log(`${stocks.Fruits[0]} was selected`))

// step 2
.then(()=>{
  return order(0000,()=>console.log('production has started'))
})

// step 3
.then(()=>{
  return order(2000, ()=>console.log("Fruit has been chopped"))
})

// step 4
.then(()=>{
  return order(1000, ()=>console.log(`${stocks.liquid[0]} and ${stocks.liquid[1]} added`))
})

// step 5
.then(()=>{
  return order(1000, ()=>console.log("start the machine"))
})

// step 6
.then(()=>{
  return order(2000, ()=>console.log(`ice cream placed on ${stocks.holder[1]}`))
})

// step 7
.then(()=>{
  return order(3000, ()=>console.log(`${stocks.toppings[0]} as toppings`))
})

// Step 8
.then(()=>{
  return order(2000, ()=>console.log("Serve Ice Cream"))
})

.catch(()=>{
  console.log("Customer left")
})

.finally(()=>{
  console.log("end of day")
})
```



### `Promise.all`

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



## Async / Await

```js
let is_shop_open = true;

function time(ms) {

   return new Promise( (resolve, reject) => {

      if(is_shop_open){
         setTimeout(resolve,ms);
      }

      else{
         reject(console.log("Shop is closed"))
      }
    });
}

async function kitchen(){
    try{
	await time(2000)
	console.log(`${stocks.Fruits[0]} was selected`)

	await time(0000)
	console.log("production has started")

	await time(2000)
	console.log("fruit has been chopped")

	await time(1000)
	console.log(`${stocks.liquid[0]} and ${stocks.liquid[1]} added`)

	await time(1000)
	console.log("start the machine")

	await time(2000)
	console.log(`ice cream placed on ${stocks.holder[1]}`)

	await time(3000)
	console.log(`${stocks.toppings[0]} as toppings`)

	await time(2000)
	console.log("Serve Ice Cream")
    }

    catch(error){
	 console.log("customer left")
    }
    
    finally{
      console.log("Day ended, shop closed")
    }
}

// Trigger
kitchen();
```

> Notar que también usamos `finally` al trabajar con async/await.



## FAQ

### AJAX

La mayor parte de las cosas que podemos lograr trabajando de manera asincrónica involucran una API llamada XHR más conocida como AJAX.

### HTTP

**HTTP** es un protocolo standard de la web para transferencia de información entre un servidor y un cliente

En las DevTools cuando la pestaña de Network veremos todos los recursos que estamos descargando para la correcta visualización del sitio a través del protocolo http.

En los mensajes además del **body** contamos con **headers** (request y response headers) y esta información podremos verla también las Devtools. 
