# Svelte

> Basado en el [Crash Course](https://youtu.be/3TVy6GdtNuQ) de Traversy Media (COMPLETO).
>
> Basado en el curso [Introducción a Svelte](https://www.youtube.com/playlist?list=PLV8x_i1fqBw2QScggh0pw2ATSJg_WHqUN) de midudev de 7 videos (COMPLETO).
>
> Basado en el curso [Svelte Tutorial for Beginners](https://www.youtube.com/playlist?list=PL4cUxeGkcC9hlbrVO_2QFVqVPhlZmz7tO) de The Net Ninja de 35 videos.

## Introducción

Svelte fue creado por Rich Harris y nos permite construir interfaces de usuario **reactivas** de la misma manera que React, Vue o Angular. Svelte es un **compilador** que genera un bundle de vanilla JavaScript optimizado. Es frecuente que se lo llame framework pero en realidad no lo es.

Nos permite crear una SPA (*single page application*) o bien controlar una porción pequeña de un sitio con componentes reutilizables de la misma manera que en React, Vue o Angular.

El funcionamiento de Svelte es bastante simple de entender (por ejemplo si lo comparamos con React con el Virtual DOM y esas conceptos). Simplemente escribiremos el código con la sintaxis de Svelte y este luego será compilado en JavaScript puro optimizado que será leido por el navegador.

Hablamos de **aplicaciones reactivas** ya que queremos que si tenemos un cierto dato en pantalla y este cambia de valor reflejarlo de manera inmediata.



## Características

* Nos permite [escribir menos código](https://svelte.dev/blog/write-less-code) que el que necesitaríamos en una aplicación de React por ejemplo.
* Como el código es compilado, nos permite detectar errores que de otra forma veríamos en run-time (aunque antes el linter nos ayudaría con alguno de ellos).
* Nos permite crear frontend UIs dinámicos
* Produce JavaScript optimizado
* 30% más rápido que otros frameworks.
* Animaciones y transiciones interesantes disponibles por defecto.
* Gran ecosistema.
  * **SvelteKit**: similar a lo que es Next.js para React aportando SSR, routing, etc.
  * **Svelte Native**
* Simplicidad de uso.



## Creación Proyecto

Existen distintas formas de crear un proyecto con Svelte (podríamos utilizar por ejemplo SvelteKit) pero utilizaremos la indicada en al documentación usando la herramienta de scafolding llamada **degit** (también creada por Rich Harris) que realizará una copia del último commit de un repositorio (en este caso del de svelte) sin descargar el historial completo de git.

```bash
npx degit sveltejs/template md-svelte
cd md-svelte
npm install
npm run dev
```

> Recordemos que `npx` es un comando de`npm` que nos permite ejecuctar un paquete, en este caso `degit` que de otra forma tendríamos que instalar como global. Mientras que en este caso lo instalará en una carpeta temporal y lo ejecutará al vuelo.

Obtendremos un servidor de desarrollo y utilizaremos rollup como module bundler (pero podríamos utilizar por ejemplo Vite). 



### Extensión VS Code

Se aconseja instalar la extensión Svelte for VS Code la cual nos aportará de formateado, autocompletado, nos permitirá el uso de emmet



### Scripts

En `package.json` podremos ver que tenemos los siguientes scripts:

```json
"scripts": {
    "build": "rollup -c",
    "dev": "rollup -c -w",
    "start": "sirv public --no-clear"
},
```

* Con `npm start` con `sirv`serviremos la carpeta `public` para producción.

* Con `npm run dev` iniciamos un servidor de desarrollo con live reload.

* Con `npm run build`para compilar el código Svelte en código JavaScript optimizado para producción.



### Estructura de Carpetas

* `scripts` contiene el archivo `setupTypescript.js` y si ejecutamos `node scripts/setupTypescript.js` podremos utilizar TypeScript como vemos en la [documentación](https://svelte.dev/blog/svelte-and-typescript). 

* `public` en ella el archivo `index.html` que será la única página de la SPA que será cargada por el navegador. Veremos que en el `<body>` no tenemos nada y que importamos un archivo `/build/bundle.js` (archivo y carpeta que serán creados cuando ejecutamos `npm run build`).

  En `global.css` podremos colocar los estilos globales y en `/build/bundle.css` los estilos de los componentes.

* `src` tenemos dos archivos `main.js` que es el entrypoint o punto de entrada en el cual hacemos el binding del componente principal `App` con `document.body` . Si en cambio en `index.html` tenemos por ejemplo un div donde queremos renderizar los componentes podemos poner `target: document.getElementById("root")`.

```js
import App from './App.svelte';

const app = new App({
	target: document.body,
	props: {
		name: 'world'
	}
});

export default app;
```

> Notar además que le podemos pasar props (en este caso le pasamos una llamada `name`) y luego en `App.svelte` la recibimos haciendo `export let name;` dentro de `<script></script>` y luego podremos mostrarla en pantalla como `{name}`. Obviamente si no lo necesitamos, podremos quitar esto.

## Componentes

Los componentes son piezas reutilizables de UI que tienen la extensión `.svelte` y encapsulan la salida (HTML), la lógica (JavaScript) y los estilos (CSS).

En `src` creamos una carpeta `components` donde almacenamos todos los componentes.

El modo de importar un componente en otro (por ejemplo en `App.svelte`) es idéntico al de React `import FeedbackList from './components/FeedbackList.svelte'` y luego lo usamos con `<FeedbackList/>`.

### Estructura de un componente

* Lógica escrita en JavaScript dentro de tags `<script>` allí podremos setear variables, crear funciones o valores reactivos.
* Salida de html en el medio donde podremos emplear llaves `{}` cuando queramos mostrar una variable o evaluar una expresión. También podremos utilizar condicionales o each loops.
* Estilos en CSS dentro de tags `<style>`
* Notar que no es necesario exportar el componente como sí sucede en React.



```s
<script>
	let numbers = [1,2,3,4];

	function addNumber(){
        numbers.push(numbers.length+1);
    }
	$: sum = numbers.reduce((t,n) => t+n,0);
<script>
    
<p>{numbers.join(' + ')={sum}}</p>

<button on:click={addNumber}>
    Add a Number
</button> 
```



### Pasaje de Props

Si queremos pasarle ciertos datos a un componente podemos hacerlo via props por ejemplo `<FeedbackList feedback={feedback}/>`. 

Luego en el componente `FeedbackList` accederemos a esa prop de la siguiente forma:

```
<script>
	export let feedback = [];
</script>
```

> Si bien lo recibiremos desde el componente padre le damos un **valor default** de un array vacío por si por algún motivo no lo recibimos.
>
> Si recibe más de una props podríamos poner `export let prop1, prop2, prop3`



* Con `<FeedbackList feedback={feedback}/>` como tenemos el mismo nombre de la prop que el de la variable que almacena el dato, podemos poner `<FeedbackList {feedback}/>`

* En ocasiones nos convendrá almacenar las props en un objeto y pasárselas todas al componente utilizando el spead operator.

  ```
  <script>
  	const commonProps = { maxCounter: 5}
  </script>
  
  <Counter {...commonProps}/>
  ```

  



### Manejo de Estado

Queremos que nuestros componentes estén dotados de **reactividad** lo que significa que manejan un cierto estado interno y cuando este cambie veamos reflejados esos cambios en la interfaz. 

Svelte automáticamente detecta aquello que es estado y que al cambiar debe volver a renderizarse el componente. En cambio en React necesitamos una gestión del estado particular mediante el uso del hook `useState`.

Si tenemos`let firstName = 'Juan'` y luego queremos modificarlo podemos hacerlo directamente con `firstName='Jota'`.

Esto justifica un poco lo anunciado respecto a que con Svelte escribimos menos código.



```vue
<script>
	let greeting='frontender';
	let prefix='Hola'
	setTimeout(function(){
		greeting='mundo'
	},2000);
</script>

<h1>{prefix} {greeting}</h1>
```

Cuando cambiamos el valor a la variable Svelte detecta el cambio y vuelve a renderizar el componente. Esto se conoce como **asignación reactiva**, veremos luego lo que son las **declaraciones reactivas**.



Svelte gracias a la compilación es capaz de diferenciar lo que es una variable `prefix` de lo que es el estado `greeting`. En las devtools en `bundle.js` es decir en el output JavaScript veremos que esa variable de estado aparece asociada a un método `$$invalidate `.



#### Estado Inicial

Es posible recibir por props el estado inicial de un componente y luego modificarlo internamente.

```vue
<script>
	export let initialCounter = 0;
	
    let counter = initialCounter;
    
    const handleClick = () => {
        counter++;
    }
</script>

<button on:click={handleClick}>Increment</button>
```

> Cuando invocamos este componente le pasamos el valor inicial con `<Counter initialCounter={9}/>` y en caso de no recibir ninguno valdrá 0 que es su valor default.



#### Modificar Estado

Si tenemos que modificar el estado agregando por ejemplo un elemento a un array de `users` no podemos hacerlo con `users.push()` sino que debemos reasignarlo a otro array, lo cual hacemos con `users = [..users, {id:'4',name:'Jen'}]`



### Life Cycle Methods

De manera similar a los componentes de clases en React tenemos métodos del ciclo de vida de los componentes en Svelte.

```
import {onMount, onDestroy} from 'svelte'

onMount(()=> {
	console.log('mounted')
	//http requests para obtener data de backend
})

onDestroy(()=> {
	console.log('destroy')
})
```



### Estilos

Si tenemos un componente con estilos.

```
<style>
	h1{
		color: #09f;
	}
</style>
```

Al visualizar el CSS output o al analizarlo con las DevTools veremos que se le ha asignado una clase especial con un hash único para evitar colisiones. Esto significa que los `h1` que tengamos en otros componentes de la aplicación no se verán afectados por estos estilos.



#### Estilos Variables

Si tenemos una variable `let color='blue';` y queremos asignarlo a un elemento podemos hacerlo con `<h1 style="color: {color}">Título</h1>`



## Fetching de Datos

Para el fetching de datos utilizaremos [OMDb API  - The Open Movie Database](http://www.omdbapi.com/) que debe usarse de la siguiente forma `http://www.omdbapi.com/?apikey=[yourkey]&s=` seguido del título de la película que queremos buscar.

Queremos que cada vez que se ingresa un caracter en el input se actualice el valor del estado `input` y esto lo hacemos mediante el evento `on:input`. Además queremos que la solicitud a la API sólo se realice cuando la cantidad de caracteres ingresados sea mayor a 2.

```vue

<script>
  let value = ''
  let response = []
  
  const handleInput = (event) => value = event.target.value
  
  $: if (value.length > 2) {
    fetch(`https://www.omdbapi.com/?s=${value}&apikey=422350ff`)
      .then(res => res.json())
      .then(apiResponse => {
        response = apiResponse.Search || []
      })
  }
    
</script>

<input value={value} on:input={handleInput} />
```

> Hemos colocado `apiResponse.Search || []` debido a que en ocasiones la API no devuelve el campo `Search` por ejemplos si no encontró ninguna película.



### `{#await}`

Lo implementado anteriormente usando promesas puede reemplazarse haciendo uso del `{#await}`.

```vue
</script>
$: if (value.length > 2) {
	response = fetch(`https://www.omdbapi.com/?s=${value}&apikey=422350ff`)
	.then(res => !res.ok() && new Error('Something bad happened with the fetching of movies'))
	.then(res => res.json())
	.then(apiResponse => {
		return apiResponse.Search || []
	})
}
</script>

{#await response}
  <strong>Loading...</strong>
{:then movies}
  {#each movies as {Title, Poster, Year}, index}
    <Movie
      index={index}
      title={Title}
      poster={Poster}
      year={Year}
    />
  {:else}
    <strong>No hay resultados</strong>
  {/each}
{:catch error}
  <p>❌ There has been an error</p>
{/await}
```

> Notar que en este caso en `response` en lugar de asignarle el resltado, guardamos la promesa (notar el agregado del `return`) y con `await` esperamos a que se complete y mientras tanto mostramos el **Loading...** sin la necesidad de manejar un estado para tal fin.
>
> En caso de que hubiera un error en la API o en la API Key, con `!res.ok()` revisamos que el status code devuelto no sea de 200 o similar y en ese caso tiramos un error que luego con el bloque `{:catch}` mostraremos en pantalla.



## Bind

Supongamos que queremos tener un componente con dos inputs asociados a los valores `a` y `b`. Esto lo hacemos colocando `bind:value={a}` de esta manera cuando cambiamos el valor de uno de los inputs será cambiado el valor de dicha variable.

```
<script>
	let a = 1;
	let b = 2;
</script>

<input type="number" bind:value={a}>
<input type="number" bind:value={b}>

<p>{a} + {b} = {a + b}</p>
```



## Declaraciones Reactivas

Como dijimos anteriormente Svelte actualiza automáticamente el DOM cuando el estado del componente cambia.

Supongamos que tenemos un componente `Counter` que consiste en un contador que se incrementa al presionar un botón. En este caso el valor de `{count}` se verá actualizado en pantalla cada vez que lo presionemos.

```vue
<script>
	export let initialCounter = 0;

	let count = initialCounter
	function handleClick () {
		count++
	}
</script>

<button on:click={handleClick}>Incrementar</button>
<span>{count}</span>
```

Las declaraciones reactivas son necesarias cuando algunas partes del estado deben ser calculadas a partir de otras partes.

Por ejemplo si en nuestro componente `Counter` queremos tener ahora un indicador `isEvenOrOdd`. Si tuviéramos `let isEvenOrOdd = contador % 2 === 0 ? 'Is Even' : 'Is Odd'` y luego mostramos `{isEvenOrOdd}` veríamos que el valor en pantalla no se actualiza cuando el contador es incrementado. 

Queremos que esa declaración se ejecute cada vez que cambie `count`. Los valores reactivos los definimos con `$:` seguido del nombre de la variable y el valor que queremos que tenga.

```vue
<script>
	export let initialCounter = 0;
	export let maxCounter = 9;
	let count = initialCounter
	function handleClick () {
		count++
	}
	// let isEvenOrOdd = count % 2 === 0 ? 'Is Even' : 'Is Odd'
	$: isEvenOrOdd = count % 2 === 0 ? 'Is Even' : 'Is Odd'
	$: if (count > maxCounter ) {
		console.log('limit contador')
		count = maxCounter
	}
</script>

<style>
	span {
		color: red;
	}
</style>

<button on:click={handleClick}>Incrementar</button>
<span>{count}</span>
<span>{isEvenOrOdd}</span>
```

Otra opción hubiera sido colocar en el renderizado `{count % 2 === 0 ? 'Is Even' : 'Is Odd'}` y el mensaje se hubiera actualizado sin la necesidad de la declaración reactiva.



En cuanto a la limitación que hemos hecho del contador mediante una declaración reactiva, podemos ver que tiene un comportamiento similar al de un efecto en React y en ese caso `count` sería la dependencia (lo cual es detectado automáticamente por Svelte). En tanto que se ejecutará el código siempre que cambie el valor de `count` (y también la primera vez cuando se monta el componente).

```
$: if (count > maxCounter ) {
	console.log('limit contador')
	count = maxCounter
}
```

En el caso anterior vimos una declaración reactiva con un condicional, pero también es posible hacerla sin el directamente con `{}`. Por ejemplo queremos hacer un `console.log()` de una variable de estado cada vez que esta se actualiza podríamos hacerlo de este modo:

```
$:{
	console.log(value);
}
```



Otro ejemplo sería si queremos conformar un `fullName` calculado a partir de `firstName` y `lastName`. Para esos casos debemos usar declaraciones reactivas pues de lo contrario si tuviéramos `let fullName= firstName + ' ' + lastName` cuando cambia `firstName` o `lastName`, `fullName` no cambiaría.

```vue
<script>
	let firstName = 'Juan';
	let lastName = 'Ocho';

	$: fullName = firstName + ' ' + lastName;
</script>

<p>{fullName}</p>
```

Veremos que si modificamos alguna de esas dos variables `fullName` reaccionará a esos cambios y mostrará el valor actualizado. Nuevamente podríamos haber puesto (en lugar de `<p>{fullName}</p>`) directamente `<p>{firstName} {lastName}</p>` y no hubiera hecho falta una declaración reactiva.





## Eventos

La forma de trabajar con eventos en Svelte es colocando `on:` seguido del nombre del evento, por ejemplo `on:click={}`.



### `on:click`

Si queremos tener un botón que al presionarlo cambie el color de un elemento (modificando el valor de una variable).

Una forma es hacerlo usando una expresión en línea:

```vue
<button on:click={()=> color='red'}>Click</button>
```

> El `on:click` es una directiva.



También podemos hacerlo mediante una función aparte:

```vue
<script>
const toggle = () => {
	color= color==='red'?'purple':'red';
}
</script>

<button on:click={toggle}>Click</button>
```



### `on:input`

```vue
<script>
  let text = ''
      
  const handleInput = () => {
  
  }
</script>

<input type="text" on:input={handleInput} bind:value = {text} placeholder="Tell us something">
```



### `on:change`

```vue
</script>
	let selected=10
	
    const onChange = (e) => {
   		selected = e.currentTarget.value
    	dispatch('rating-select', selected)
    }
</script>

<ul class="rating">
  <li>
    <input type="radio" id="num1" name="rating" value="1" on:change={onChange} checked={selected===1} />
    <label for="num1">1</label>
  </li>
  <li>
    <input type="radio" id="num2" name="rating" value="2" on:change={onChange} checked={selected===2} />
    <label for="num2">2</label>
  </li>
  .
  .
  .
</ul>	
  
```

> Notar que en `onChange` recibimos el valor del radio input presionado en `e.currentTarget.value`



### `on:submit`

```
<script>
const handleSubmit = () => {
    if(text.trim().length > min) {
      const newFeedback = {
        id: uuidv4(),
        text,
        rating: +rating
      }
      
      text = ''
    }
  }
</script>


<form on:submit|preventDefault={handleSubmit}>

</form>
```

> Notar el uso de *event modifiers* para evitar tener que poner `e.preventDefault` (otra opción podría ser `on:submit|once` y sólo podríamos hacer submit una vez).
>
> Como queremos que el rating sea un número utilizamos el **unary operator** `+rating`
>
> Para generar un id único utilizamos el paquete `uuid` para ello simplemente lo importamos con `  import {v4 as uuidv4} from 'uuid'` y luego lo llamamos donde queremos un id unique `uuidv4()`.



## Renderizado Condicional

En Svelte es posible realizar un renderizado condicional, es decir que mostrar o no el contenido de acuerdo a una condición.

Supongamos que queremos mostrar la cantidad de resultados obtenidos luego de una llamada a una API o en su defecto un cartel que indique que no encontramos ningún resultado. La forma mas sencilla de hacerlo es con el operador ternario:

```
{response.length>0?`${response.length} movies found`:`No movies found`}
```

Sin embargo, si en lugar de texto queremos mostrar condicionalmente elementos HTML de marcado, como por ejemplo si quisiéramos mostrar:

```
<strong>`${response.length} movies found`</strong>
```

Debemos hacerlo con `{#if condicion}` y finalizamos con `{/if}`  y también es posible trabajar con `{:else}`

```
{# if response.length>0}
<strong>{response.length} movies found</strong>
{:else}
No movies found
{/if}
```



Otro ejemplo es si queremos mostrar y ocultar un texto conforme apretamos un botón:

```vue
<script>
	let showText = false;
	
	const toggleText = () => {
		showText = !showText;
	}
</script>

{#if showText}
	<p>Paragraph hidden by default</p>
{/if}

<button on:click={toggleText}>Click</button>
```



### Condicionales Anidados

Supongamos ahora que tenemos un estado `loading` que ponemos en `true` justo antes de hacer el fetch a una API y en `false` cuando obtuvimos el resultado. Como queremos evitar estar viendo información de películas encontradas no actualizada, la ocultamos mientras estemos cargando y para ello utilizamos condicionales anidados.

```
{#if loading}
	<strong>Loading...</strong>
{:else}
	{#if response.length>0}
		<strong>{response.length} movies</strong>
	{:else}
		<strong>No movies<strong>
	{/if}
{/if}
```



## Renderizado de Listas

```vue
<script>
	let users = [
		{
			id:'1',
			name:'John'
		},
		{
			id:'2',
			name:'Sara'
		},
		{
			id:'3',
			name:'Bob'
		}
	]
</script>

{#each users as user (user.id)}
	<h3>{user.id}: {user.name}</h3>
{/each}
```

Con `(user.id)` estamos estableciendo una clave única para cada iteración (de manera similar a lo que hacemos en React con la prop `key`).



> Es posible trabajar también con un segundo parámetro `index`

```
{#each users as user,index (user.id)}
	<h3>{user.id}: {user.name}</h3>
{/each}
```



> Si así lo quisiéramos podríamos utilizar **object destructuring**:

```
{#each users as {id,name} (id)}
	<h3>{id}: {name}</h3>
{/each}
```



###  `{#each}` con`{:else}`

En ocasiones vamos a querer iterar todos los elementos de una lista y mostrarlos en pantalla pero en caso de que no tenga ningún elemento mostrar algún contenido en particular. En ese caso podremos recurrir al uso en conjunto de `each` y `else`.

```
{#each response as {Title, Poster, Year}, index}
    <Movie
      index={index}
      title={Title}
      poster={Poster}
      year={Year}
    />
  {:else}
    <strong>No hay resultados</strong>
  {/each}
```



## `<slot>`

De manera similar a cuando en React utilizamos la prop `prop.children` en Svelte utilizamos el tag `<slot>`

Por ejemplo si en un componente padre tenemos:

```
<Card>
	Hello
</Card>
```



Luego en `Card.svelte` podremos mostrarlo de esta forma:

```
<slot><slot/>
```



## Dispatch Custom Event

Supongamos que tenemos un componente principal `App` con un array de elementos y se los pasamos como props a un componente `List` que a su vez llama a un componente `Item` por cada uno de ellos.

Además suponemos que ese elemento tiene una cruz con la cual podrá eliminar de la lista. Necesitamos de alguna forma que esto llegue hasta el componete `App` que tiene dicho array.



En `Item.svelte`

```
</script>
import {createEventDispatcher} from 'svelte'

const dispatch = createEventDispatcher();

const handleDelete = (itemId) => {
	dispatch('delete-feedback',itemId)
}
</script>

<button class="close" on:click={() => handleDelete(item.id)}>X</button>
```



En `List.svelte`

```
<#each feedback as fb (fb.id)>
<Item item={fb} on:delete-feedback />
</each>
```

> Como en `List` tampoco tenemos acceso al array queremos que sea enviado hacia arriba a `App`.



En `App.svelte`

```
<script>
	const deleteFeedback= (e) => {
		//console.log(e.detail)
		const itemId = e.detail;
		feedback = feedback.filter(item => item.id!== itemId)
	}
</script>

<List on:delete-feedback={deleteFeedback} />
```

> En la propiedad `detail` recibimos el id del elemento a eliminar de la lista.
>
> Notar que estamos reasignando `feedback`



## Transiciones

```
 import { fade, scale } from 'svelte/transition'
 
 {#each feedback as fb (fb.id)}
 	<div in:scale out:fade="{{ duration: 500 }}">
 		<FeedbackItem item={fb} />
 	</div>
 {/each}
```

> En este caso estamos configurando una transición de entrada entrada (in) que será `scale` y una de salida (out) de `fade` a la cual además le establecemos propiedades de duración. 
>
> Si quisiéramos que fuera igual para entrada y salida podríamos haber puesto directamente `transition: fade`

## Stores

El uso de stores nos permite no tener que crear eventos custom para pasar props hacia arriba.

Creamos un archivo `stores.js`

```
import { writable } from 'svelte/store'

export const FeedbackStore = writable([
  {
    id: 1,
    rating: 10,
    text: 'Lorem ipsum dolor sit amet consectetur adipisicing elit. consequuntur vel vitae commodi alias voluptatem est voluptatum ipsa quae.',
  },
  {
    id: 2,
    rating: 9,
    text: 'Lorem ipsum dolor sit amet consectetur adipisicing elit. consequuntur vel vitae commodi alias voluptatem est voluptatum ipsa quae.',
  },
  {
    id: 3,
    rating: 8,
    text: 'Lorem ipsum dolor sit amet consectetur adipisicing elit. consequuntur vel vitae commodi alias voluptatem est voluptatum ipsa quae.',
  },
])
```

> Los stores pueden ser `readable` o `writable`



En lugar de tener la lista hardcodeada en `App` la tenemos en un store.



Una forma de obtener los datos del Store es suscribirnos y desuscribirnos en el método `onDestroy` del ciclo de vida.

```
let feedback = []

const unsuscribe = FeedbackStore.suscribe(data => feedback=data)

onDestroy(()=> {
	unsuscribe();
})

{#each feedback as fb (fb.id)}
<div in:scale out:fade="{{ duration: 500 }}">
  <FeedbackItem item={fb} />
</div>
{/each}
```



Otra forma más simple (la desuscripción será automática)

```
{#each $FeedbackStore as fb (fb.id)}
<div in:scale out:fade="{{ duration: 500 }}">
  <FeedbackItem item={fb} />
</div>
{/each}
```



### Eliminar Items a Store

Anteriormente vimos que para eliminar un elemento de la lista debíamos usar custom events para elevar las props.

Ahora esto no será necsario y podremos alterar el store desde el `Item`

```
<script>
const handleDelete = (itemId) => {
    FeedbackStore.update((currentFeedback) => {
      return currentFeedback.filter(item => item.id != itemId)
    })
  }
</script>

<button class="close" on:click={() => handleDelete(item.id)}>X</button>
```



> Veremos que `List.svelte` y `App.svelte` nos quedan ahora mucho mas limpios sin el `on:delete-feedback`



### Agregar Items a Store

Una forma de hacerlo es con eventos custom y pasando las props hacia arriba hasta llegar a `App` que tiene el array de elementos. Sin embargo, utilizando stores esto es mucho mas simple y podemos hacerlo desde el formulario en el cual tengamos el elemento a agregar.

```
  const handleSubmit = () => {
    if(text.trim().length > min) {
      const newFeedback = {
        id: uuidv4(),
        text,
        rating: +rating
      }

      FeedbackStore.update((currentFeedback) => {
        return [newFeedback, ...currentFeedback]
      })

      text = ''
    }
  }
</script>

<form on:submit|preventDefault={handleSubmit}>

</form>
```

