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



### Scripts

En `package.json` podremos ver que tenemos los siguientes scripts:

```json
"scripts": {
    "build": "rollup -c",
    "dev": "rollup -c -w",
    "start": "sirv public --no-clear"
},
```

Con `sirv` lo que haremos será servir la carpeta `public` cuando estemos en producción.