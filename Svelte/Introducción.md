# Svelte

> Basado en el [Crash Course](https://youtu.be/3TVy6GdtNuQ) de Traversy Media (COMPLETO).



## Introducción

Svelte es un **compilador** que genera código JavaScript mínimo y optimizado. 

Muchas veces nos referiremos a Svelte como un **frontend framework** ya que hace muchas de las cosas que hacen estos. Nos permite crear SPA (*single page application*) con componentes reutilizables de la misma manera que en React, Vue o Angular.

El funcionamiento de Svelte es bastante simple de entender (por ejemplo si lo comparamos con React con el Virtual DOM y esas conceptos). Simplemente escribiremos el código con la sintaxis de Svelte y este luego será compilado en JavaScript puro optimizado que será leido por el navegador.



## Características

* Nos permite crear frontend UIs dinámicos
* Produce JavaScript optimizado
* 30% más rápido que otros frameworks.
* Animaciones y transiciones interesantes disponibles por defecto.
* Gran ecosistema.
  * **SvelteKit**: similar a lo que es Next.js para React aportando SSR, routing, etc.
  * **Svelte Native**
* Simplicidad de uso.



## Instalación

Existen distintas formas de instalar Svelte (podríamos utilizar por ejemplo SvelteKit) pero la más simple es utilizar la herramienta de scafolding llamada **degit** que clonará el repositorio de svelte.

```bash
npx degit sveltejs/template md-svelte
cd md-svelte
npm install
npm run dev
```

Obtendremos un servidor de desarrollo y utilizaremos rollup como module bundler (pero podríamos utilizar por ejemplo Vite). 



### Extensión VS Code

Se aconseja instalar la extensión Svelte for VS Code la cual nos aportará de formateado, autocompletado, nos permitirá el uso de emmet



## Componentes

Los componentes son piezas reutilizables de UI que tienen la extensión `.svelte` y encapsulan la salida (HTML), la lógica (JavaScript) y los estilos (CSS).

En `src` creamos una carpeta `components` donde almacenamos todos los componentes.

El modo de importar un componente en otro (por ejemplo en `App.svelte`) es idéntico al de React `import FeedbackList from './components/FeedbackList.svelte'` y luego lo usamos con `<FeedbackList/>`.

### Estructura de un componente

* Lógica escrita en JavaScript dentro de tags `<script>` allí podremos setear variables, crear funciones o valores reactivos.
* Salida de html en el medio donde podremos emplear llaves `{}` cuando queramos mostrar una variable o evaluar una expresión. También podremos utilizar condicionales o each loops.
* Estilos en CSS dentro de tags `<style>`



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

Si queremos pasarle ciertos datos a un componente podemos hacerlo via props `<FeedbackList feedback={feedback}/>`. Además en este caso como tenemos el mismo nombre de la prop que el de la variable que almacena el dato, podemos poner `<FeedbackList {feedback}/>`

Luego en el componente `FeedbackList` accederemos a esa prop de la siguiente forma:

```
<script>
	export let feedback = [];
</script>
```

> Si bien lo recibiremos desde el componente padre le damos un **valor default** de un array vacío por si por algún motivo no lo recibimos.



## Life Cycle Methods

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



## Scripts

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



## Estructura de Carpetas

* `scripts` contiene el archivo `setupTypescript.js` y si ejecutamos `node scripts/setupTypescript.js` podremos utilizar TypeScript como vemos en la [documentación](https://svelte.dev/blog/svelte-and-typescript). 

* `public` en ella el archivo `index.html` que será la única página de la SPA que será cargada por el navegador. Veremos que en el `<body>` no tenemos nada y que importamos un archivo `/build/bundle.js` (archivo y carpeta que serán creados cuando ejecutamos `npm run build`).

  En `global.css` podremos colocar los estilos globales y en `/build/bundle.css` los estilos de los componentes.

* `src` tenemos dos archivos `main.js` que es el entrypoint en el cual hacemos el binding del componente principal `App` con `document.body` 

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

> Notar además que le estamos pasando una prop `name` y luego en `App.svelte` la recibimos con `export let name;` dentro de `<script></script>` y luego podremos mostrarla en pantalla como `{name}`. Obviamente si no lo necesitamos, podremos quitar esto.



## Valores Reactivos

Los valores reactivos los definimos con `$:` seguido del nombre de la variable y el valor que queremos que tenga. `$: name = firstName + ' ' + lastName;` luego lo utilizamos y veremos que si modificamos alguna de esas dos variables `name` reaccionará a esos cambios y mostrará el valor actualizado.

```vue
let firstName = 'Juan';
let lastName = 'Ocho';

$: name = firstName + ' ' + lastName;
```



## Manejo de Estados

Comparativamente con React donde si queremos emplear variables de estado debemos utilizar el hook `useState`, con Svelte podremos tener:

`let firstName = 'Juan'` y luego modificarlo directamente a `firstName='Jota'` sin la necesidad de usar un set.



## Estilos con variables

Si tenemos una variable `let color='blue';` y queremos asignarlo a un elemento podemos hacerlo con `<h1 style="color: {color}">Título</h1>`



## Eventos

La forma de trabajar con eventos en Svelte es colocando `on:` seguido del nombre del evento, por ejemplo `on:click={}`.



### `on:click`

Si queremos tener un botón que al presionarlo cambie el color de un elemento (modificando el valor de una variable).

Una forma es hacerlo usando una expresión en línea:

```vue
<button on:click={()=> color='red'}>Click</button>
```

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



## Condicionales

A la hora de escribir condicionales lo hacemos con `{#if condicion}` y finalizamos con `{/if}` . 

Por ejemplo si queremos mostrar y ocultar un texto conforme apretamos un botón:

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



También es posible trabajar con `{:else}`, por ejemplo supongamos que queremos que cuando no se muestre el texto anterior se muestre otra cosa:

```vue
<script>
	let showText = false;
	
	const toggleText = () => {
		showText = !showText;
	}
</script>

{#if showText}
	<p>Paragraph hidden by default</p>
{:else}
	<p>Another paragraph</p>
{/if}

<button on:click={toggleText}>Click</button>
```



## Loops

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
	<h3>{user.name}: {user.name}</h3>
{/each}
```

Con `(user.id)` estamos estableciendo una clave única para cada iteración (de manera similar a lo que hacemos en React con la prop `key`).



### Agregar elementos a lista

Si queremos agregar un elemento a la lista de `users` no podemos hacerlo con `users.push()` sino que debemos reasignarlo a otro array, lo cual hacemos con `users = [..users, {id:'4',name:'Jen'}]`



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



## Agregar Items a Store

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

