# Svelte

> Basado en el [Crash Course](https://youtu.be/3TVy6GdtNuQ) de Traversy Media.



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

Los componentes son piezas reutilizables de UI que tienen la extensión `.svelte` y encapsulan la salida (HTML), la lógica (JavaScript) y los estilos (CSS)



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



## Características Svelte

### Valores Reactivos

Los valores reactivos los definimos con `$:` seguido del nombre de la variable y el valor que queremos que tenga. `$: name = firstName + ' ' + lastName;` luego lo utilizamos y veremos que si modificamos alguna de esas dos variables `name` reaccionará a esos cambios y mostrará el valor actualizado.

```vue
let firstName = 'Juan';
let lastName = 'Ocho';

$: name = firstName + ' ' + lastName;
```



### Manejo de Estados

Comparativamente con React donde si queremos emplear variables de estado debemos utilizar el hook `useState`, con Svelte podremos tener:

`let firstName = 'Juan'` y luego modificarlo directamente a `firstName='Jota'` sin la necesidad de usar un set.



### Estilos con variables

Si tenemos una variable `let color='blue';` y queremos asignarlo a un elemento podemos hacerlo con `<h1 style="color: {color}">Título</h1>`



### Eventos

La forma de trabajar con eventos en Svelte es colocando `on:` seguido del nombre del evento, por ejemplo `on:click={}`.

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



### Condicionales

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

