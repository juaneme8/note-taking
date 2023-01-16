# Svelte

> 2022 [Crash Course](https://youtu.be/3TVy6GdtNuQ) de Traversy Media (COMPLETO).
>
> 2022 - Curso [Introducción a Svelte](https://www.youtube.com/playlist?list=PLV8x_i1fqBw2QScggh0pw2ATSJg_WHqUN) de midudev de 7 videos (COMPLETO).
>
> 2022 - Curso [Svelte Tutorial for Beginners](https://www.youtube.com/playlist?list=PL4cUxeGkcC9hlbrVO_2QFVqVPhlZmz7tO) de The Net Ninja de 35 videos.
>
> 2023 - Actualizado de acuerdo al [tutorial oficial](https://learn.svelte.dev/)

## Introducción

Svelte fue creado por Rich Harris y nos permite construir aplicaciones web e interfaces de usuario **reactivas** de la misma manera que React, Vue o Angular. Svelte es un **compilador** que genera un bundle de vanilla JavaScript optimizado. 

En la [documentación](https://learn.svelte.dev/tutorial/welcome-to-svelte) se lo menciona como *user interface framework*.

En Svelte las aplicaciones están compuestas de uno o más **componentes**. Los componentes son bloques reutilizables que encapsulan HTML, CSS y JavaScript escritos en un archivo `.svelte`

Nos permite crear una SPA (*single page application*) o bien controlar una porción pequeña de un sitio con componentes reutilizables.

El funcionamiento de Svelte es bastante simple de entender (por ejemplo si lo comparamos con React con el Virtual DOM y esas conceptos). Simplemente escribiremos el código con la sintaxis de Svelte y este luego será compilado en un bundle de vanilla JavaScript puro optimizado que será leído por el navegador. Luego deployaremos este único archivo y no hará falta ninguna librería extra como sí sucede con React o Vue que si bien también generan el código *at build-time* aparte de este bundle requieren el intérprete de la librería de React o de Vue. 

Hablamos de **aplicaciones reactivas** ya que queremos que si tenemos un cierto dato o estado en pantalla y este cambia de valor reflejarlo de manera inmediata.

## Características

* Nos permite [escribir menos código](https://svelte.dev/blog/write-less-code) que el que necesitaríamos en una aplicación de React por ejemplo.
* Como el código es compilado, nos permite detectar errores que de otra forma veríamos en *run-time* (aunque antes el linter nos ayudaría con alguno de ellos).
* Nos permite crear UIs dinámicos.
* Produce JavaScript optimizado.
* 30% más rápido que otros frameworks.
* Animaciones y transiciones interesantes disponibles por defecto.
* Gran ecosistema.
  * **SvelteKit**: similar a lo que es Next.js para React aportando SSR, routing, etc.
  * **Svelte Native**
* Simplicidad de uso.



## Creación Proyecto

Si queremos probar código de Svelte online podemos usar el [Playground](https://svelte.dev/repl).

Existen distintas formas de crear un proyecto con Svelte (podríamos utilizar por ejemplo SvelteKit) pero en principio asumiremos que quermeos crear un simple proyecto de frontend por lo que utilizaremos Vite.

```
npm create vite@latest
```

Este comando lo ejecutamos desde el directorio padre de la futura carpeta del proyecto (o si ya creamos la carpeta cuando nos pregunte el nombre ponemos `.`)

```
√ Project name: ... md-vite
√ Select a framework: » Svelte
√ Select a variant: » JavaScript
```

```bash
cd md-svelte
npm install
npm run dev
```



### Extensión VS Code

Se aconseja instalar la extensión Svelte for VS Code la cual nos aportará de formateado, autocompletado, nos permitirá el uso de emmet.



### Scripts

En `package.json` podremos ver que tenemos los siguientes scripts:

```json
"scripts": {
    "dev": "vite",
    "build": "vite build",
    "preview": "vite preview"
 },
```



* Con `npm run dev` iniciamos un servidor de desarrollo con live reload.

* Con `npm run build`para compilar el código Svelte en código JavaScript optimizado para producción.

  

### Estructura de Carpetas

* `public` en ella el archivo `index.html` que será la página que será cargada por el navegador. Veremos que en el `<body>` no tenemos nada y que importamos un archivo `/build/bundle.js` (archivo y carpeta que serán creados cuando ejecutamos `npm run build`). 

  En `global.css` podremos colocar los estilos globales y en `/build/bundle.css` los estilos de los componentes.

* En `src` tenemos el archivo `main.js` que es el entrypoint o punto de entrada en el cual hacemos el binding del componente principal (root o top level component) `App.svelte` con el div principal que tenemos en `index.html`.

* ```jsx
  import './app.css'
  import App from './App.svelte'
  
  const app = new App({
    target: document.getElementById('app'),
    props: {
      name: 'juanocho'
    }
  })
  
  export default app
  ```

> Notar además que le podemos pasar props (en este caso le pasamos una llamada `name`) y luego en `App.svelte` la recibimos haciendo `export let name;` dentro de `<script></script>` y luego podremos mostrarla en pantalla como `{name}`. Obviamente si no lo necesitamos, podremos quitar esta propiedad.



# Componentes

Los componentes son piezas reutilizables de UI que tienen la extensión `.svelte` y encapsulan la salida (HTML), la lógica (JavaScript) y los estilos (CSS). Además nos permiten tener un código más legible que si tuviésemos todo en un único archivo `App.svelte`.

Tenemos distintas formas de organizar los componentes pero la mas frecuente es colocarlos dentro de una carpeta `components` en `src`. Sin embargo, en proyectos de magnitud es habitual una carpeta dentro de `components` para cada feature o sección.

Los nombres de los componentes deben siempre empezar en mayúsculas para diferenciarlos de los elementos HTML.

## Importar componentes

El modo de importar un componente en otro (por ejemplo en `App.svelte`) es idéntico al de React dentro de los tags `<script>` colocamos `import FeedbackList from './components/FeedbackList.svelte'` y luego lo usamos con `<FeedbackList/>`.

> Cuando en un componente utilizamos otro componente se dice que lo consumimos.



> :warning: Es importante planear el **árbol de componentes** con antelación para tener certeza de cómo van a fluir los datos para evitar dolores de cabeza.
>
> :warning: Las imágenes debemos colocarlas en la carpeta `assets` en `src`.
>
> :warning: Los componentes que serán reutilizables en distintas partes de la aplicación los colocamos en`/src/lib` por ejemplo `Button.svelte` (no dentro de `/src/components`)



## Estructura de un componente

* Lógica escrita en JavaScript dentro de tags `<script>` allí podremos setear variables, crear funciones o valores reactivos como veremos más adelante.
* Luego podemos colocar las etiquetas de marcado de html en el medio donde podremos emplear llaves `{}` cuando queramos mostrar una variable o evaluar una expresión. También podremos utilizar condicionales o iteraciones.
* Estilos de CSS dentro de tags `<style>`
* **Notar que no es necesario exportar el componente como sí sucede en React.**

## Mostrar variables

La mayor parte del tiempo vamos a querer mostrar datos en pantalla, luego veremos como crear componente personalizados pero la forma más rápida de hacerlo es editando `App.svelte`.

Definimos dentro de los tags de script una variable

```jsx
<script>
  let name='J8'
</script>
<h1>Welcome {name}!</h1>
```



## Atributos dinámicos

```
<img src={src} />
```

Svelte también se encarga de advertirnos de aspectos relacionados con la **accesibilidad**, es por eso que nos aparecerá el siguiente warning: `A11y: <img> element should have an alt attribute svelte (a11y-missing-attribute)`

### Shorthand

Frecuentemente tendremos atributos donde el nombre y el valor son los mismos como `src={src}` en esos casos podremos usar la forma abreviada `{src}`



## Tags HTML

Si queremos renderizar elementos HTML que tengamos dentro del contenido de una variable, no podremos hacerlo de manera directa ya que el contenido de ese string es insertado como texto plano.

```jsx
<script>
	let str = `this string contains some <strong>HTML!!!</strong>`;
</script>

<p>{str}</p>
```

Como consecuencia de esto a la salida veremos: `this string contains some <strong>HTML!!!</strong>`

En ese caso tendremos que usar en cambio el tag especial `{@html str}`

:warning: Tener presente que Svelte no realiza ninguna sanitización de la expresión antes de insertarlo en el DOM por lo que en caso de utilizar esta funcionalidad tendremos que tener cuidado con la exposición a ataques XSS.





# Estilos

## Estilos Locales

Los estilos locales son aquellos que tienen un scope a nivel componente, es decir que son específicos para los elementos que tiene dentro. Debemos hacerlo dentro de los tags `<style>` y veremos que Svelte le asigna un nombre de clase especial con un hash único para evitar colisiones. Esto podremos verificarlo en las DevTools o analizando el contenido de `dist/assets/index-(...).css`.

```vue
<h1>Styled Paragraph</h1>
<style>
	h1{
		color: purple;
	}
</style>
```

> Los `h1` que tengamos en otros componentes de la aplicación no se verán afectados por estos estilos.



## Estilos Condicionales

Si queremos que un estilo se aplique o no dependiendo de una expresión podemos hacerlo con con esta sintaxis `class:promo={isPromo}` esto significa que la clase `promo` se aplicará si `isPromo` es `true` (también podría ser una expresión, no sólo un booleano).  En cambio, la clase `backdrop` se aplicará siempre.

```jsx
<div class="backdrop" class:promo={isPromo}>
	<div class="modal">
		<p>Sign up for offers!</p>
	</div>
</div>
```



## Estilos en línea

Es posible asignarle a un elemento estilos en línea los cuales tendrán preponderancia sobre los locales.

```jsx
<p>This is a paragraph.</p>
<p style="color: red">This is a red paragraph</p>
<style>
	p {
		color: purple;
		font-family: 'Comic Sans MS', cursive;
		font-size: 2em;
	}
</style>
```



### Estilos dinámicos

Si tenemos una variable `let color='blue';` y queremos asignarlo a un elemento podemos hacerlo con `<h1 style="color: {color}">Título</h1>`

Si tenemos almacenado un porcentaje en una variable y queremos asignarlo como ancho de un elemento `style="width={percent}%"`



# Props

El pasaje de props (abreviatura de propiedades) nos permite que los componentes sean reutilizables y que reciban datos dinámicos.

Si queremos pasarle ciertos datos a un componente podemos hacerlo via props por ejemplo `<FeedbackList feedback={feedback}/>`. 

Luego en el componente `FeedbackList` accederemos a esa prop utilizando la *keyword* export.

```vue
<script>
	export let feedback = [];
</script>
```

> Si bien lo recibiremos desde el componente padre le damos un **valor default** de un array vacío por si invocamos el componente sin esa prop.
>
> Con `export` indicamos que se trata de un valor que recibiremos como props es decir que proviene de afuera del componente.
>
> Si recibe más de una props podríamos poner `export let prop1, prop2, prop3`



* :bulb: Cuando tenemos el mismo nombre de prop que el de la variable que almacena el dato como en `<FeedbackList feedback={feedback}/>` podemos usar el *shorthand* `<FeedbackList {feedback}/>`

  

* Cuando tenemos un objeto de propiedades nos va a convenir "spredearlos" en el componente en vez de especificarlos individualmente.

  ```vue
  <script>
  	const commonProps = { maxCounter: 5}
  </script>
  
  <Counter {...commonProps}/>
  ```

  Luego en el componente hijo los recibimos del modo habitual:
  
  ```vue
  <script>
  	export let maxCounter;
  </script>
  
  The counter value is: {maxCounter}
  ```
  
  



# Manejo de Estado

La **reactividad** es una característica central en Svelte y consiste en mantener el DOM sincronizado con el estado de la aplicación.

Esto significa que nuestros componentes manejan un cierto estado interno y cuando este cambie veremos reflejados esos cambios en la interfaz. 

Svelte automáticamente detecta aquello que es estado y que al cambiar debe volver a renderizar el componente. En cambio en React necesitamos una gestión del estado particular mediante el uso del hook `useState`.

Si tenemos`let firstName = 'Juan'` y luego queremos modificarlo podemos hacerlo directamente con `firstName='Jota'`.

Esto justifica un poco lo anunciado respecto a que con Svelte escribimos menos código.



A continuación mostramos un ejemplo en el que tenemos una variable de estado `greeting` que tiene un valor inicial y luego de cierto tiempo cambia a otro. Como consecuencia de esta nueva asignación y la reactividad propia de Svelte, el valor en pantalla se verá actualizado.

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



:earth_asia: Variable vs Estado

Svelte gracias a la compilación es capaz de diferenciar lo que es una variable `prefix` de lo que es el estado `greeting`. En las devtools en `bundle.js` es decir en el output JavaScript veremos que esa variable de estado aparece asociada a un método `$$invalidate `.



## Estado Inicial

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





## Declaraciones Reactivas

Los valores reactivos son aquellos que se actualizan automáticamente cuando cambian los datos de los cuales dependen.

Svelte actualiza el DOM cuando el estado de los componentes cambia (asignaciones reactivas). En ocasiones algunas partes del estado deben ser computadas a partir de otras partes.

Cuando tenemos un componente `Counter` que consiste en un contador que se incrementa al presionar un botón. En este caso el valor de `{count}` se verá actualizado en pantalla cada vez que lo presionemos.

```vue
<script>
	export let initialCounter = 0;

	let count = initialCounter;
	function handleClick () {
		count++
	}
</script>

<span>{count}</span>
<button on:click={handleClick}>Incrementar</button>
```

En este caso no fue necesario usar una declaración reactiva para ver el valor actualizado en pantalla.

Sino que las declaraciones reactivas son necesarias cuando algunas partes del estado deben ser calculadas a partir de otras partes.

Por ejemplo si en nuestro componente `Counter` queremos tener ahora un indicador `isEvenOrOdd`. Si tuviéramos `let isEvenOrOdd = count % 2 === 0 ? 'Is Even' : 'Is Odd'` y luego mostramos `{isEvenOrOdd}` veríamos que el valor en pantalla no se actualiza cuando el contador es incrementado. 

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

Podemos ver que tiene un comportamiento similar al de un efecto en React y en ese caso `count` sería la dependencia (lo cual es detectado automáticamente por Svelte). En tanto que se ejecutará el código siempre que cambie el valor de `count` (y también la primera vez cuando se monta el componente).



#### Otros casos de uso

Otro ejemplo clásico que se usa a la hora de estudiar **reactive values** es si queremos conformar un `fullName` calculado a partir de `firstName` y `lastName`. Para esos casos debemos usar declaraciones reactivas pues de lo contrario si tuviéramos `let fullName= firstName + ' ' + lastName` cuando cambia `firstName` o `lastName`, `fullName` no cambiaría.

```vue
<script>
	let firstName = 'Juan';
	let lastName = 'Ocho';

	$: fullName = firstName + ' ' + lastName;
</script>

<p>{fullName}</p>
<input type="text" bind:value={firstName}/>
<input type="text" bind:value={lastName}/>
```

Veremos que si modificamos alguna de esas dos variables `fullName` reaccionará a esos cambios y mostrará el valor actualizado. Nuevamente podríamos haber puesto (en lugar de `<p>{fullName}</p>`) directamente `<p>{firstName} {lastName}</p>` y no hubiera hecho falta una declaración reactiva.

## Sentencias Reactivas

No estamos limitados únicamente a la declaración de valores reactivos sino que podremos ejecutar sentencias reactivas (reactive statements) que son ejecutados cuando los datos dentro del statement cambian.

Por ejemplo si queremos loguear cada vez que cambia `count`:

```
<script>
	let count = 0;
	$: console.log(`the count is ${count}`);
</script>
```

También es posible **agrupar sentencias** 

```
$: {
	console.log(`the count is ${count}`);
	alert(`I SAID THE COUNT IS ${count}`);
}
```

También podemos tener reactive statements con **condicionales**:

```jsx
$: if (count > maxCounter ) {
	console.log('limit contador')
	count = maxCounter
}
```

O directamente poner un bloque de código:

```jsx
$:{
	console.log(value);
	console.log(isEvenOrOdd);
}
```

Este bloque de código se ejecutará cuando cambie `value` o `isEvenOrOdd` (que también es un valor reactivo).

## Actualizar arrays

Como la reactividad de Svelte es disparada por **asignaciones**, si usamos métodos como `push` o `splice` que mutan el array original (y no crean uno nuevo) no lograremos actualizaciones automáticas.



Si queremos tener una aplicación que partiendo de una array con `[1,2,3,4]` muestre la suma de esos números y cada vez que apretemos un valor agregue el número siguiente y presente la suma. Podríamos estar tentados a hacer `numbers.push(numbers.length+1);` pero como la reactividad es disparada por asignaciones no funcionaría.

```jsx
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

Una forma de solucionarlo es colocando abajo una asignación:

```jsx
numbers.push(numbers.length + 1);
numbers=numbers
```



Sin embargo otras formas más limpias de hacerlo son las siguientes:

```jsx
numbers = [...numbers, numbers.length+1]
```

```jsx
numbers = numbers.concat(numbers.length+1)
```

Si tenemos que modificar el estado agregando por ejemplo un elemento a un array de `users` no podemos hacerlo con `users.push()` sino que debemos reasignarlo creando uno nuevo lo cual hacemos con `users = [..users, {id:'4',name:'Jen'}]` o bien podríamos utilizar `users = users.concat({id:'4',name:'Jen'})` que también retorna un nuevo array.

Si en cambio queremos que el nuevo elemento sea agregado al comienzo podemos hacer `users = [{id:'4',name:'Jen'},..users]`



# Condicionales

HTML no tiene una forma de expresar lógica como condicionales y loops, mientras que en Svelte esto es posible.

En ocasiones vamos a querer realizar un renderizado condicional, es decir que mostrar o no el contenido de acuerdo a una condición. Esto podría ser útil por ejemplo si queremos mostrar un contenido si el usuario está logueado y otro distinto si no lo está. 

Supongamos que queremos mostrar la cantidad de resultados obtenidos luego de una llamada a una API o en su defecto un cartel que indique que no encontramos ningún resultado. La forma mas sencilla de hacerlo es con el operador ternario:

```jsx
{response.length>0?`${response.length} movies found`:`No movies found`}
```

Sin embargo, si en lugar de texto queremos mostrar condicionalmente elementos HTML de marcado, como por ejemplo si quisiéramos mostrar:

```vue
<strong>`${response.length} movies found`</strong>
```

Esto no funcionaría y debemos hacerlo en cambio con `{#if condicion}` y finalizamos con `{/if}`  y también es posible trabajar con `{:else}`

```vue
{#if response.length>0}
<strong>{response.length} movies found</strong>
{:else}
No movies found
{/if}
```

* `#` indica apertura del bloque.
* `:` indica continuación del bloque.
* `/` indica cierre del bloque.

>También podemos agregar una condición en el else utilizando `{:else if condicion}`

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



## Condicionales Anidados

Supongamos ahora que tenemos un estado `loading` que ponemos en `true` justo antes de hacer el fetch a una API y en `false` cuando obtuvimos el resultado. Como queremos evitar estar viendo información de películas encontradas no actualizada, la ocultamos mientras estemos cargando y para ello utilizamos condicionales anidados.

```vue
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



# Loops

Utilizamos loops cuando queremos recorrer un array y mostrar HTML para cada item que tiene.

## Bloque `#each`

```vue
<script>
	let users = [
    {id:'1',name:'John'},
    {id:'2',name:'Sara'},	
    {id:'3',name:'Bob'}
	];
</script>

{#each users as user}
	<h3>{user.id}: {user.name}</h3>
{/each}
```

:bulb: Tener presente que el `each` funciona tanto con arrays como con "array like objects" es decir aquellos que tienen una propiedad `.length` numérica.

:bulb: Es posible trabajar también con un segundo parámetro `index`

```vue
{#each users as user,index}
	<h3>{user.id}: {user.name}</h3>
{/each}
```



:bulb: Si así lo quisiéramos podríamos utilizar **object destructuring**:

```vue
{#each users as {id,name}}
	<h3>{id}: {name}</h3>
{/each}
```



### Bloque `each` con key

Si bien para listas estáticas no tenemos inconvenientes cuando pretendemos agregar o quitar elementos existe un problema que debemos solucionar. 

Supongamos que tenemos un componente `App.svelte` que muestra una lista de libros y cuenta con un botón para eliminar el primero de ellos.

```vue
<script>
	import Thing from './Thing.svelte';

	let things = [
		{ id: 1, name: 'apple' },
		{ id: 2, name: 'banana' },
		{ id: 3, name: 'carrot' },
		{ id: 4, name: 'doughnut' },
		{ id: 5, name: 'egg' }
	];

	function handleClick() {
		things = things.slice(1);
	}
</script>

<button on:click={handleClick}>
	Remove first thing
</button>

{#each things as thing}
	<Thing name={thing.name} />
{/each}

```

Luego en el componente `Thing.svelte`:

```vue
<script>
	const emojis = {
		apple: '🍎',
		banana: '🍌',
		carrot: '🥕',
		doughnut: '🍩',
		egg: '🥚'
	};
	// the name is updated whenever the prop value changes...
	export let name;
	// ...but the "emoji" variable is fixed upon initialisation of the component
	const emoji = emojis[name];
</script>

<p>
	<span>The emoji for {name} is {emoji}</span>
</p>
```

Veremos que inicialmente mostramos los datos correctos hasta que presionamos una vez el botón de eliminar. 

Esto es así debido a que eliminamos el primer componente `<Thing>` pero el último nodo del DOM y si bien luego actualiza el `name` en los nodos del DOM restantes, no hace lo mismo con el emoji (mostrando el que se fija durante la inicialización del componente).

El compotamiento deseado sería eliminar el primer componente `<Thing>` y el primer nodo DOM.

Para solucionar esto debemos introducir el campo `id`que debe ser único para cada elemento. El rol que tiene el id es ayudar a Svelte a diferenciar qué nodo del DOM debe cambiar cuando actualizamos un bloque each.

```jsx
<script>
	let users = [
    {id:'1',name:'John'},
    {id:'2',name:'Sara'},	
    {id:'3',name:'Bob'}
	];
</script>

{#each users as user (user.id)}
	<h3>{user.id}: {user.name}</h3>
{/each}
```

Con `(user.id)` estamos estableciendo una clave única para cada iteración (de manera similar a lo que hacemos en React con la prop `key`) para vincular cada elemento del DOM con cada ítem del array. En es una buena práctica ya que en caso de no hacerlo podríamos tener inconvenientes al manipular luego los elementos de la lista.



:bulb: Es posible utilizar cualquier objeto como clave ya que Svelte utiliza una estructura `Map` internamente (podríamos haber usado `thing` en lugar de `thing.id`). Sin embargo, el uso de strings o números es por lo general mas seguro. 



Si queremos agregar a esta lista la posibilidad de borrar elementos, es decir queremos pasar como argumento el id del elemento a borrar a un handler lo hacemos mediante **inline functions**.

```vue
<script>
	let users = [
    {id:'1',name:'John'},
		{id:'2',name:'Sara'},
		{id:'3',name:'Bob'}
	];

	const handleDelete = (id) =>{
		users = users.filter(user => user.id!==id)
	}	
</script>


{#each users as user (user.id)}
	<h3>{user.id}: {user.name}</h3>
	<button on:click={() => handleDelete(user.id)}>Delete</button>
{/each}
```



Si quisiéramos además recibir el evento `e` podríamos hacer `	<button on:click={(e) => handleDelete(e,user.id)}>Delete</button>` y luego lo recibimos `const handleDelete = (e, id){}`



##  Bloque `{#each}` con`{:else}`

En ocasiones vamos a querer iterar todos los elementos de una lista y mostrarlos en pantalla pero en caso de que no tenga ningún elemento mostrar algún contenido en particular. En ese caso podremos recurrir al uso en conjunto de `each` y `else`.

```vue
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



# Fetching de Datos

La mayoría de las aplicaciones deben lidiar con datos asincrónicos en algún punto. Svelte nos permite incorporar el await de promesas directamente en el marcado.



## Bloque `#await`

Supongamos que tenemos una aplicación que cada vez que hacemos click en un botón se dirige a https://svelte.dev/tutorial/random-number a fin de obtener un número random.

```vue
<script>
	async function getRandomNumber() {
		const res = await fetch(
			`https://svelte.dev/tutorial/random-number`
		);
		const text = await res.text();

		if (res.ok) {
			return text;
		} else {
			throw new Error(text);
		}
	}

	let promise = getRandomNumber();

	function handleClick() {
		promise = getRandomNumber();
	}
</script>

<button on:click={handleClick}> generate random number </button>

{#await promise}
	<p>...waiting</p>
{:then number}
	<p>The number is {number}</p>
{:catch error}
	<p style="color: red">{error.message}</p>
{/await}

```



Si sabemos que la promesa no puede ser rechazada, podemos omitir el bloque `catch` . También podemos omitir el primer bloque si no queremos mostrar nada hasta que se resuelva la promesa.

```vue
{#await promise then number}
	<p>The number is {number}</p>
{/await}
```



# Life Cycle Methods

En Svelte los componentes tienen un ciclo de vida que inicia cuando son creados y termina cuando son destruídos. Existen funciones que nos permiten ejecutar código en momentos clave de ese ciclo.

## `onMount`

El método `onMount` es el que usaremos más frecuentemente y se ejecuta luego de que el componente es renderizado en el DOM y es donde haremos peticiones a APIs externas.

```jsx
<script>
	import { onMount } from 'svelte';

	let photos = [];

	onMount(async () => {
		const res = await fetch(`https://jsonplaceholder.typicode.com/photos?_limit=20`);
		photos = await res.json();
	});
</script>
```

Las funciones de ciclo de vida **deben ser llamadas durante la inicialización del componente** de modo que los callbacks estén atados a la instancia del componente (no debemos hacerlo en un `setTimeout`)

Si el *callback* de `onMount` retorna una función esta será ejecutada cuando el componente es destruído.

## `onDestroy` 

El método `onDestroy` se ejecuta cuando el componente es eliminado del DOM y es donde nos desuscribiremos de timers o de stores por ejemplo.

Supongamos que queremos tener un timer generado con `setInterval` que se actualice cada un segundo mostrando el valor de cuenta en pantalla . Con el método `onDestroy` realizamos la limpieza de ese timer cuando no lo necesitamos mas. De esta manera evitamos **memory leaks**.

```vue
<script>
	import { onDestroy } from 'svelte';

	let counter = 0;
	const interval = setInterval(() => counter += 1, 1000);

	onDestroy(() => clearInterval(interval));
</script>

{counter}
```



**Es importante llamar a las lifecycle functions durante la inicialización del componente** pero no importa desde donde las llamemos. En esa dirección es posible abstraer la lógica a un helper llamado `utils.js` (:warning: notar la extensión  `.js` y no `.svelte`)

```jsx
import { onDestroy } from 'svelte';

export function onInterval(callback, milliseconds) {
	const interval = setInterval(callback, milliseconds);

	onDestroy(() => {
		clearInterval(interval)
	});
}
```

Esta función será invocada por el componente `Timer.svelte`

```jsx
<script>
	import { onInterval } from './utils.js';

	export let callback;
	export let interval = 1000;

	onInterval(callback, interval);
</script>

<p>This component executes a callback every {interval} ms</p>
```

Por último `Timer` es invocado en `App.svelte`

```jsx
<script>
	import Timer from './Timer.svelte';

	let open = false;
	let seconds = 0;

	const toggle = () => (open = !open);
	const handleTick = () => (seconds += 1);
</script>

<div>
	<button on:click={toggle}
		>{open ? 'Close' : 'Open'} Timer</button
	>
	<p>
		The Timer component has been open for
		{seconds}
		{seconds === 1 ? 'second' : 'seconds'}
	</p>
	{#if open}
		<Timer callback={handleTick} />
	{/if}
</div>
```

Cuando cerramos el modal estaríamos destruyendo el componente del DOM (esto se pone de manifiesto en el renderizado condicional), es por eso que para evitar pérdidas de memoria debemos desuscribirnos al timer. En caso de no eliminar los timers seguiría llamando a `handleTick` aunque el componente no esté en pantalla resultando en un aumento de `seconds` distinto al real y un aumento de la carga de CPU.



## `beforeUpdate`

La función `beforeUpdate` nos permite programar tareas para ejecutarse inmediatamente **antes** de que el DOM sea actualizado.

Notar que `beforeUpdate` será ejecutado la primera vez antes de que el componente sea montado por lo que tendremos que chequear la existencia de los elementos antes de leer sus propiedades.

## `afterUpdate`

La función `afterUpdate` en cambio nos permite programar tareas para ejecutarse **después** de que el DOM está en sync con los datos.

Tanto `beforeUpdate` como `afterUpdate` se utilizan para lograr cosas imperativas que sería dificil de lograr basándonos puramente en un modo *state-driven* como por ejemplo actualizar la posición de scroll de un elemento.

## `tick`

La función `tick` se diferencia de todas las otras en que la podemos llamar en cualquier momento (no únicamente cuando al inicializar el componente). 

Retorna una promesa que se resuelve tan pronto como los cambios de estado pendientes se hayan aplicado al DOM (o inmediatamente si no hay ningún estado pendiente de cambios).

Cuando modificamos el estado de un componente en Svelte no se actualiza el DOM de manera inmediata, sino que espera al siguiente *microtask* para determinar si hay otros cambios que deban ser aplicados, incluyendo otros componentes. De esta manera evita trabajo innecesario y le permite al navegador agrupar cosas de manera efectiva.

Por ejemplo supongamos que tenemos un `textarea` y queremos que al seleccionar determinada porción de texto, este si está en minúsculas se cambie a mayúsculas y viceversa.



```jsx
<script>
	import { tick } from 'svelte';

	let text = `Select some text and hit the tab key to toggle uppercase`;

	async function handleKeydown(event) {
		if (event.key !== 'Tab') return;
		event.preventDefault();
		const {selectionStart,selectionEnd,value} = this;
		const selection = value.slice(selectionStart,		selectionEnd);

		const replacement = /[a-z]/.test(selection)
			? selection.toUpperCase()
			: selection.toLowerCase();

		text =
			value.slice(0, selectionStart) +
			replacement +
			value.slice(selectionEnd);

		// this has no effect, because the DOM hasn't updated yet
		await tick();
		this.selectionStart = selectionStart;
		this.selectionEnd = selectionEnd;
	}
</script>

<textarea value={text} on:keydown={handleKeydown}/>
```

Si no tuvieramos el `await tick();` cuando presionamos TAB como el value del textarea cambia perderíamos la selección y el cursor se iría al final del texto.

En cuanto a `this` si lo imprimiéramos en consola veríamos algo como **<textarea class="s-mYoR_1q3v4Bg"></textarea>**

# Binding de Datos

Como regla general en Svelte los datos fluyen de arriba hacia abajo, un componente padre puede setear las props en un componente hijo y un componente puede setear atributos de un elemento, pero no al revés.

En ocasiones vamos a querer romper esta regla como veremos a continuación. 

### One way data binding

Por ejemplo tomemos el caso de un elemento `<input>` en el cual escuchamos a un evento input y ejecutamos un handler `handleInput` que actualiza el valor de la variable.

```vue
<script>
  let text = ''
     
  const handleInput = (e) => {
  	text = e.target.value
  }
</script>

<input type="text" on:input={handleInput} placeholder="Tell us something">
```

Esta técnica se conoce como *one way binding* dado que si cambiamos `text` de otro modo que no sea con el `input` el valor mostrado por ese input quedará desactualizado.

Esto lo podemos comprobar con este ejemplo que al presionar un botón le agrega un "@" al valor de `text` dejando al input desactualizado.

```vue
<script>
  let text = ''
     
  const handleInput = (e) => {
  	text = e.target.value
  }
</script>

<input 
       type="text" 
       on:input={handleInput} 
       placeholder="Tell us something"
>
<button on:click={()=> text+='@'}>Click</button>
```

> Notar que recibimos el evento `e` de manera automática.

### Two way data binding

En cambio utilizando *two way binding* como veremos a continuación, agregando `value = {text}` el valor mostrado en el input estará siempre actualizado.

```vue
<script>
  let text = ''
     
  const handleInput = (e) => {
  	text = e.target.value
  }
</script>

<input 
       type="text" 
       on:input={handleInput} 
       value = {text} 
       placeholder="Tell us something"
>
<button on:click={()=> text+='@'}>Click</button>
```

> Aunque pusimos `let text = ''; ` en realidad no hace falta darle un valor inicial podríamos haber puesto `let name;`



#### Forma reducida

La técnica implementada decimos que es un poco *boilderplatey* por lo que existe una forma reducida de actualizar el valor de `text` y de asegurarnos el two way binding. Es decir que estas dos líneas:

```jsx
<input 
       type="text" 
       on:input={handleInput} 
       value = {text} 
       placeholder="Tell us something"
>
```

y la función `handleInput` pueden ser reemplazadas por

```vue
<input 
       type="text" 
       bind:value={text}
       placeholder="Tell us something"
>
```

Como veremos a continuación:

```vue
<script>
  let text = ''
</script>

<input 
       type="text" 
       bind:value={text} 
       placeholder="Tell us something"
>
<button on:click={()=> text+='@'}>Click</button>
```



## `type="number"` y `type="range"`

En el DOM todo es un string, esto no nos ayuda mucho si estamos lidiando con inputs numéricos de tipo `type="number"` o `type="range".`

### Ejemplo 1:

Supongamos que queremos tener un componente con dos inputs asociados a los valores `a` y `b`. Esto lo hacemos colocando `bind:value={a}` de esta manera cuando cambiamos el valor de uno de los inputs será cambiado el valor de dicha variable.

```vue
<script>
	let a = 1;
	let b = 2;
</script>

<input type="number" bind:value={a}>
<input type="number" bind:value={b}>

<p>{a} + {b} = {a + b}</p>
```



:eyes: Notar que hemos podido realizar la suma sin castear como número. Históricamente en HTML un input aunque tenga especificado su `type="number"` será guardado como string y tendremos que usar `parseInt("1")` para obtener `1`. En Svelte cuando hacemos `bind:value` se encarga de hacer esto por nosotros.



Otra forma de hacer esto es mediante un bind con un único objeto.

```vue
<script>
	let numbers = { a:1 ,b:1};
</script>

<input type="number" bind:value={numbers.a}>
<input type="number" bind:value={numbers.b}>

<p>{numbers.a} + {numbers.b} = {numbers.a + numbers.b}</p>
```

Esto puede resultar particularmente útil cuando tenemos que trabajar con formularios con varios campos.



### Ejemplo 2:

Supongamos que tenemos la misma situación de generar ambos sumandos pero cada uno se puede obtener tanto por un input `type="number"` o bien por uno de `type="range"` .

```
<script>
	let a = 1;
	let b = 2;
</script>

<label>
	<input
		type="number"
		bind:value={a}
		min="0"
		max="10"
	/>

	<input
		type="range"
		bind:value={a}
		min="0"
		max="10"
	/>
</label>

<label>
	<input
		type="number"
		bind:value={b}
		min="0"
		max="10"
	/>

	<input
		type="range"
		bind:value={b}
		min="0"
		max="10"
	/>
</label>

<p>{a} + {b} = {a + b}</p>
```



## `type="checkbox"`

A la hora de trabajar con inputs de `type="checkbox` podemos hacerlo de dos formas. 

La primera es usando `bind:checked` y creando una variable para cada checkbox que inicia en `false` y  que se pondrá en `true` cuando seleccionemos ese elemento. 

La segunda es usando `bind:group={array}` y al seleccionar cada elemento será añadido a ese array.



#### Atributo`checked`

```vue
<script>
	let javascript=false;
	let react=false;
	let node=false;
	$:console.log(javascript,react,node)
</script>

<input type="checkbox" bind:checked={javascript}/>javascript<br>
<input type="checkbox" bind:checked={react}/>react<br>
<input type="checkbox" bind:checked={node}/>node<br>
```

Esta alternativa puede ser muy confusa si tenemos muchos checkboxes asociados al mismo valor ya que necesitaremos una variable para cada uno de ellos.



#### Atributo`group`

En este caso además del `bind:group={}` es necesario usar el atributo `value` y estos valores serán los añadidos al array.

```jsx
<script>
	let skils = [];
	$:console.log(skils)
</script>

<input type="checkbox" bind:group={skils} value="javascript"/>javascript<br>
<input type="checkbox" bind:group={skils} value="react"/>react<br>
<input type="checkbox" bind:group={skils} value="node"/>node<br>
```



Consideremos un ejemplo en el cual `value` provenga de un array que recorremos:

```vue
<script>
let menu = ['Cookies and cream', 'Mint choc chip', 'Raspberry ripple'];
</script>
{#each menu as flavour}
	<label>
		<input type=checkbox bind:group={flavours} name="flavours" value={flavour}>
		{flavour}
	</label>
{/each}
```



## `type="radio"`

Los elementos de tipo radio del mismo grupo son mutuamente excluyentes, es decir que solo puedo tener uno seleccionado.

```vue
<script>
	let scoops = 1;
</script>

<h2>Size</h2>

<label>
<input type=radio bind:group={scoops} name="scoops" value={1}>One scoop
</label>

<label>
  <input type=radio bind:group={scoops} name="scoops" value={2}>Two scoops
</label>

<label>
  <input type=radio bind:group={scoops} name="scoops" value={3}>Three scoops
</label>
```

Notar que en este caso el valor inicial es 1 por lo que estará seleccionado el primer elemento.

## `type="textarea"`

El input `type="textarea"` es muy similar al input de texto, tendremos que usar `bind:value`

```vue
<script>
	import { marked } from 'marked';
	let value = `Some words are *italic*, some are **bold**`;
</script>

{@html marked(value)}

<textarea bind:value={value}></textarea>
```

Como en este caso tenemos `bind:value={value}` podemos reemplazarlo por la forma abreviada: `bind:value` (este criterio aplica a todos los bindings)

## `select`

A la hora de trabajar con `<select>` también podemos usar un `bind:value`.

### Single value

```jsx
<script>
	let gender="";
  $:console.log(gender)
</script>

<select bind:value={gender}>
	<option value="male">Male</option>
	<option value="female">Female</option>
</select>
```



:bulb: Tener en cuenta que en los `<option>` es posible utilizar también `value` que sea un objeto en lugar de string.

```vue
<script>
	let questions = [
		{id: 1,text: `Where did you go to school?`},
		{id: 2,text: `What is your mother's name?`},
		{id: 3,text: `What is another personal fact that an attacker could easily find with Google?`}
	];
	let selected;
</script>

<h2>Insecurity questions</h2>
<form on:submit|preventDefault={handleSubmit}>
	<select
		bind:value={selected}
		on:change={() => (answer = '')}
	>
		{#each questions as question}
			<option value={question}>
				{question.text}
			</option>
		{/each}
	</select>
</form>

<p>
	selected question: {selected
		? selected.id
		: 'none'}
</p>
```

:bulb: Como no le dimos un valor inicial a `selected`, el binding lo seteará al valor default (que es el primer elemento de la lista) de manera automática. Pero debemos tener cuidado ya que hasta que se produzca el binding, `selected` será `undefined` por lo que no podremos referenciar a ciegas por ejemplo a `selected.id` en el template.



### Multiple value

Un `select` puede tener también el atributo `multiple` en cuyo caso completará un array en lugar de elegir un único valor.

```vue
<script>
	let flavours = ['Mint choc chip'];
  let menu = [
		'Cookies and cream',
		'Mint choc chip',
		'Raspberry ripple'
	];
  $:console.log(flavours)
</script>
<select multiple bind:value={flavours}>
	{#each menu as flavour}
		<option value={flavour}>
			{flavour}
		</option>
	{/each}
</select>
```



# Eventos

La forma de trabajar con eventos en Svelte es colocando `on:` seguido del nombre del evento, por ejemplo `on:click={}`.



## Tipos de Eventos

### `on:click`

Si queremos tener un contador que al hacer click en un botón aumente su valor, "we need to wire up an event handler" que en este caso es handle

```vue
<script>
	let count = 0;
	function increment() {
		count+=1
	}
</script>

<button on:click={increment}>
	Clicked {count}
	{count === 1 ? 'time' : 'times'}
</button>

```

Svelte a través de la asignación `count+=1` establece que debe actualizarse el DOM.

Si queremos tener un botón que al presionarlo cambie el color de un elemento (modificando el valor de una variable).

Una forma es hacerlo usando una expresión en línea:

```vue
<script>
    let color='red';
</script>

<h1 style="background: {color}">I'm the title</h1>
<button on:click={()=> color=color==='red'?'purple':'red'}>Toggle Color</button>
```

> No necesariamente tenemos que trabajar con un `<button>` podríamos haber usado un `<p>` por ejemplo.
>
> El `on:click` es una directiva.



También podemos hacerlo mediante una función aparte:

```vue
<script>
	let color='red';
    const = handleClick = () => {
        color=color==='red'?'purple':'red'
    }
</script>

<h1 style="background: {color}">I'm the title</h1>
<button on:click={handleClick}>Toggle Color</button>
```



### `on:input`
```vue
<script>
  let text = ''
</script>

<input 
       type="text" 
       bind:value = {text} 
       placeholder="Tell us something"
>
```



### `on:change`

```vue
<script>
	let selected=10
	
    const onChange = (e) => {
   		selected = e.currentTarget.value
    }
</script>

<ul class="rating">
	Opción Elegida: {selected}
  <li>
    <input type="radio" id="num1" name="rating" value="1" on:change={onChange} checked={selected===1} />
    <label for="num1">1</label>
  </li>
  <li>
    <input type="radio" id="num2" name="rating" value="2" on:change={onChange} checked={selected===2} />
    <label for="num2">2</label>
  </li>
  
	<li>
    <input type="radio" id="num10" name="rating" value="10" on:change={onChange} checked={selected===10} />
    <label for="num2">10</label>
  </li>
  
</ul>	
  
```

> Notar que en `onChange` recibimos el valor del radio input presionado en `e.currentTarget.value`



### `on:submit`

```vue
<script>
const handleSubmit = (e) => {
    e.preventDefault();
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


<form on:submit={handleSubmit}>

</form>
```



> Luego veremos cómo evitar tener que poner `e.preventDefault()` en `handleSubmit` con un event modifier.
>
> :information_source: Como queremos que el rating sea un número utilizamos el **unary operator** `+rating`
>
> :information_source: Para generar un id único utilizamos el paquete `uuid` para ello simplemente lo importamos con `  import {v4 as uuidv4} from 'uuid'` y luego lo llamamos donde queremos un id unique `uuidv4()`.



### `on:mousemove`

Queremos crear una aplicación que al pasar el mouse por encima de un div nos diga sus coordenadas:

```vue
<script>
	let m = { x: 0, y: 0 };

	function handleMousemove(e) {
		m.x = e.clientX;
		m.y = e.clientY;
	}
</script>

<div on:mousemove={handleMousemove}>
	The mouse position is {m.x} x {m.y}
</div>
```



## Inline Handlers

Es posible realizar lo mismo que vimos anteriormente pero utilizando handlers en línea.

```jsx
<div on:mousemove={e => m = {x : event.clientX,y : event.clientY}}>
	The mouse position is {m.x} x {m.y}
</div>
```

En este caso retornamos un objeto `m` con las propiedades deseadas y luego lo usamos en el marcado.

:thumbsup: A diferencia de lo que ocurre en algunos frameworks donde se recomienda evitarlos por razones de performance en particular dentro de loops, esto no aplica para Svelte.



# Event Modifiers

Los *event modifiers* como su nombre lo indica son modificadores que agregamos al final de los eventos, por ejemplo donde tenemos `on:click` usando un modificador podríamos pasar a tener `on:click|preventDefault`.



## Tipos de modificadores

Existen distintos tipos de modificadores y los mas comunes son: `once`, `preventDefault` y `self`. Además es posible encadenar modificadores.

### `once` 

El modificador de eventos `once` nos permite aseguramos que el evento sólo se dispare una vez, eliminando el handler luego de que se ejecuta por primera vez.

Supongamos que queremos contar con un botón que al presionarlo muestre una alerta y sólo lo haga en una oportunidad:

```jsx
<script>
	function handleClick() {
		alert('clicked');
	}
</script>

<button on:click={handleClick}> Click me </button>
```



### `preventDefault`

`preventDefault`  llama a `e.preventDefault()` antes de ejecutar el handler es decir que nos permite impedir la acción por default al hacer submit de un formulario que ocasionaría la recarga de la página.

Cuando trabajamos con formularios en lugar de poner  `<form on:submit={handleSubmit}>` y luego en `handleSubmit` evitar la acción default con `e.preventDefault()` podemos usar un *event modifier*. De esta forma el código nos quedaría `<form on:submit|preventDefault={handleSubmit}>` y no habría que realizar nada en `handleSubmit`.

```jsx
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



### `self`

El modificador `self` hace que el handler solo se ejecute si el elemento clickeado (`event.target`) es el propio elemento (y no uno de sus hijos).

Un ejemplo de uso sería si tenemos un modal que consiste en el `backdrop` (la parte de la pantalla que se pone en gris) y por encima de esto tenemos un div con el contenido del modal en sí.

```vue
<script>
	function handleClick() {
		alert('Cierro Modal')
	}
</script>

<div class="backdrop" on:click={handleClick}>
	<div class="modal">
		
	</div>
</div>
```

Queremos que al hacer click en la parte gris se cierre el modal. Sin embargo, como lo hemos escrito hasta ahora el modal se cerrará aun si hacemos click dentro del contenido del mismo (debido a que aunque hago click en el div interno se propagará el evento hacia afuera).

Para solucionarlo:

```vue
<script>
	function handleClick() {
		alert('Cierro Modal')
	}
</script>

<div class="backdrop" on:click|self={handleClick}>
	<div class="modal">
		
	</div>
</div>
```



### `stopPropagation`

El modificador `stopPropagation` llama a `e.stopPropagation()` impidiendo que el evento alcance al siguiente elemento.

### `passive`

El modificador `passive` mejora la performance al scrollear con eventos de touch o wheel. Svelte lo agregará automáticamente cuando sea conveniente.

### `nonpassive`

El modificador `nonpassive` setea explícitamente `passive: false`

### `capture`

El modificador `capture` dispara el handler en la *capture phase* (desde el menos anidado propagándose hacia abajo) en lugar de la *bubbling phase* (desde el mas anidado hacia arriba)

### `trusted`

El modificador `trusted` sólo dispara el handler si `event.isTrusted` es `true` (por ejemplo si fue disparado por una acción de un usuario).



## Encadenar modificadores

Es posible encadenar modificadores de eventos:

```jsx
on:click|once|capture={...}
```



# Event Forwading 

Cuando queremos propagar hacia arriba un evento, es decir desde desde el componente hijo hacia el padre debemos hacer uso de lo que se conoce como *event forwading*.

Un ejemplo típico de uso de event forwading es cuando queremos mostrar y ocultar un modal. Por ejemplo si tenemos un componente padre que llama a un componente hijo `Modal`. El padre tiene un botón para mostrar el modal y el hijo debe ocultarlo al presionar en cualquier parte de la pantalla.

Si la directiva `on:` es usada sin un valor, como por ejemplo `on:click` (y no `on:click={handle...}`) se producirá la propagación de ese evento hacia arriba en el árbol de componentes, lo que se conoce como *event forwading*. Luego será el `Modal` quien emitirá este evento por lo que el consumidor del componente podrá escucharlo `<Modal on:click={toggleModal}/>`



En `App.svelte`

```vue
<script>
	import Modal from './Modal.svelte'
	let showModal = true;

	const handleClick = () => {
		showModal = !showModal;
	}
	
</script>

<Modal {showModal} on:click={handleClick}/>
{#if !showModal}
	<button on:click={handleClick}>Show Modal</button>
{/if}

```

En `Modal.svelte`

```vue
<script>
	export let showModal = false;
</script>


{#if showModal}
	I'm a modal
	<button on:click>Hide Modal (child)</button>
{/if}


```



Esto será útil en la medida que no tengamos que pasar datos del hijo hacia el padre (recordar que tenemos `on:click`  sin pasarle datos), en ese caso podemos recurrir a lo que se conoce como *dispatch custom events*



## Event Forwading con DOM Events

En ocasiones vamos a querer ser notificados sobre eventos del DOM que ocurran en componentes hijos.

Por ejemplo si en `App.svelte` utilizamos un componente `CustomButton` que cuenta con un botón y queremos enterarnos cuando se hace click en el, podremos hacerlo dejando el `on:click` sin asociar a un valor.

En `CustomButton.svelte`

```vue
<button on:click>Click me</button>
```



En `App.svelte` recibimos este evento y lo asociamos a `handleClick`.

```vue
<script>
	import CustomButton from './CustomButton.svelte';

	function handleClick() {
		alert('Button Clicked');
	}
</script>

<CustomButton on:click={handleClick} />
```



# Components Events  / Dispatch Custom Event

Los componentes también son capaces de emitir eventos, para hacerlo deben contar con un event dispatcher.

## Ejemplo 1

Suponemos un caso en el que tenemos un componente `Inner` con un botón que al presionarlo evite un evento y será recibido por su padre `App`.

En `inner.svelte`

```vue
<script>
 import {createEventDispatcher} from 'svelte'

 const dispatch = createEventDispatcher();

 function sayHello(){
	 dispatch('message', {
		 text: 'Hello'
	 })
 }
</script>

<button on:click={sayHello}>
	Click to say hello
</button>
```

> `createEventDispatcher` debe ser llamado cuando el componente es inicializado y no puede hacerlo luego (en un callback de `setTimeout` por ejemplo) ya que vincula un dispatch a una instancia de componente.



En `App.svelte`

```vue
<script>
	import Inner from './Inner.svelte';

	function handleMessage(event) {
		alert(event.detail.text);
	}
</script>

<Inner on:message={handleMessage} />
```



## Ejemplo 2

Ahora que tenemos un componente principal `App` con un array de elementos y se los pasamos como props a un componente `List` que a su vez llama a un componente `Item` por cada uno de ellos.

A diferencia de los eventos del DOM **los eventos de los componentes no cuentan con el *bubble* por lo que si queremos escuchar un evento de un componente anidado más profundamente, los componentes intermedios deben hacer el event forwading**.

En este caso tendremos tanto el dispatch de un custom event (en `Item`) como el event forwading (en `List`). 

Además suponemos que ese elemento tiene una cruz con la cual podrá eliminar de la lista. Necesitamos de alguna forma que esto llegue hasta el componete `App` que tiene dicho array.



En `Item.svelte`

```vue
<script>
import {createEventDispatcher} from 'svelte'

const dispatch = createEventDispatcher();

const handleDelete = (itemId) => {
	dispatch('delete-feedback',itemId)
}
</script>

<button class="close" on:click={() => handleDelete(item.id)}>X</button>
```

Con `const dispatch = createEventDispatcher();` creamos una función `dispatch`. Esta función la usaremos para emitir un *custom event*.

En lugar de `itemId`  en `dispatch('delete-feedback',itemId)` podríamos utilizar un objeto con múltiples propiedades.

> :warning: Aunque aca usamos *kebab-case* en los videos de The Net Ninja utiliza *camelCase* para los nombres de los *custom events*.



En `List.svelte` veremos que como es `Item` quien emite el custom event `delete-feedback` ponemos un listener para dicho evento y hacemos *event forwading*. 

**Otra opción con mucho más código sería crear un dispatcher también aca y enviar un evento con el mismo nombre, de hecho decimos que lo que hicimos es el shorthand de esa técnica.**

```vue
<#each feedback as fb (fb.id)>
<Item item={fb} on:delete-feedback />
</each>
```

> Como en `List` tampoco tenemos acceso al array queremos que sea enviado hacia arriba a `App`.



En `App.svelte`

```vue
<script>
	const deleteFeedback= (e) => {
		//console.log(e.detail)
		const itemId = e.detail;
		feedback = feedback.filter(item => item.id!== itemId)
	}
</script>

<List on:delete-feedback={deleteFeedback} />
```

> Por defecto recibimos el evento `e` y en la propiedad `detail` recibimos el id del elemento a eliminar de la lista.
>
> Notar que estamos reasignando `feedback`



# Slots

Los componentes son piezas de código reutilizables, en ocasiones nos alcanzará con pasarle como props un string para mostrar cierto contenido personalizado. Sin embargo, cuando queremos que muestre un template HTML más complejo podemos pasarle ese contenido como un slot. Para hacer esto lo colocamos dentro de las etiquetas del componente y luego lo recibimos utilizando la etiqueta `<slot></slot>`

```vue
<Card>
	<h4>Título de la tarjeta</h4>
</Card>
```



Luego en el componente en sí podremos mostrar ese contenido de esta forma:

```vue 
<slot><slot/>
```



## Named Slots

Es posible asignarle al contenido un nombre de slot específico y luego lo mostraremos poniendo `<slot name=""/></slot>`

Cuando en el componente hijo usamos los tags `<slot></slot>` mostraremos todo el contenido que no tiene un nombre de slot específico.

Por ejemplo usamos un named slot para un párrafo

```vue
<script>
	import Card from './Card.svelte'
</script>

<Card>
	<h4>This is the title</h4>
	
	<p slot="subtitle">
		This is the subtitle
	</p>
	Some text
</Card>
```



Luego en `Card.svelte`

```vue
<script>
	
</script>


<div style="color:red">
	<slot></slot>	
</div>
<slot name="subtitle"></slot>
```

El contenido mostrado será: "This is the title" y "Some text" en rojo y abajo "This is the subtitle".

Un aspecto interesante de los named slots es que nos permiten cambiar el orden en que mostramos los elementos que recibimos.



# Transiciones

Las transiciones determinan el modo en que los elementos son agregados o eliminados del DOM. 

Sólo es posible agregar transiciones sobre elementos HTML, por lo que en ocasiones convendrá ponerle un `div` rodeando al componente que queremos colocarle dicha transición.

```
 import { fade, scale } from 'svelte/transition'
 
 {#each feedback as fb (fb.id)}
 	<div in:scale out:fade="{{ duration: 500 }}">
 		<FeedbackItem item={fb} />
 	</div>
 {/each}
```

> Otra transición disponible es `slide`.
>
> En este caso estamos configurando una transición de entrada entrada (in) que será `scale` y una de salida (out) de `fade` a la cual además le establecemos propiedades de duración. 
>
> Si quisiéramos que fuera con un fade igual para entrada y salida podríamos haber puesto directamente `transition: fade`



Si queremos que la transición de salida sólo se produzca cuando afectamos localmente a un elemento (por ejemplo cuando lo eliminamos a ese y no cuando los ocultamos a todos) debemos agregar `out:scale|local`



# Animaciones

Si queremos que al eliminar un elemento el resto se reorganicen de manera animada, podemos hacerlo utilizando animaciones.

En primer lugar importamos `import {flip} from 'svelte/animate'` y luego agregamos `animate:flip={{duration:500}}`

```jsx
 import { fade, scale } from 'svelte/transition'
 import {flip} from 'svelte/animation
 
 {#each feedback as fb (fb.id)}
 	<div in:scale out:fade="{{ duration: 500 }} animate:flip={{duration:500}}">
 		<FeedbackItem item={fb} />
 	</div>
 {/each}
```



# Tweens

Los tweens nos permiten generar frames para los movimientos para evitar tener un salto rápido entre un punto de inicio y uno de fin.

Un tween es similar a un store writable. Tenemos `const value = tweened(0)` y con `tweened(0)` le pasamos el valor inicial y lo almacenamos en `value`. 

Luego con `$value` es como si nos estuviéramos suscribiendo automáticamente a un store. Luego al hacerle click con `value.set(1)` reemplazamos el valor actual por 1, pero esto no lo hará de manera inmediata sino que nos dará una serie de valores intermedios que podremos ver en el texto del botón.

```
import {tweened} from 'svelte/motion'

const value = tweened(0)

<button on:click={()=> value.set(1)}>{$value}</value>
```



Si tenemos un porcenaje que varía y queremos ajustar el ancho de un elemento de acuerdo a ese ancho, podemos hacer un tween `const tweenedA = tweened(0);` que se vea actualizado cada vez que el porcentaje cambia con un valor reactivo `$: tweenedA.set(percentA);`.

Por último luego el ancho luego lo establecemos en función de este tween store con `style="width: {$tweenedA}%"`.

Si queremos mostrar en la consola el valor actualizado con todos los puntos intermedios `$:console.log($tweenedA)`



# Stores

En una aplicación además del estado que podemos manejar a nivel jerarquías de componentes, tenemos valores que deben ser accedidos por componentes que no tienen relación entre sí.

En Svelte hacemos esto mediante el uso de stores que son objetos con un método `suscribe` que permite a las partes involucradas ser notificadas cuando el valor cambia donde los componentes que están suscriptos a ella obtendrán los datos actualizados.

Los stores pueden ser `readable` o `writable` (aquellos casos donde podemos leer y escribir).

> Los stores nos permiten almacenar datos de manera centralizada para no tener que hacer *dispatch* de *custom events* para pasar datos hacia arriba en el árbol de componentes. Esto es particularmente útil en aplicaciones de magnitud.

## Writable stores

Los *writable stores* cuentan con métodos de `set` y `update` además del `suscribe` 

Creamos una carpeta `stores` en `src` y un archivo archivo `FeedbackStore.js`

```javascript
import { writable } from 'svelte/store'

const FeedbackStore = writable([
  {id: 1,rating: 10,text: 'Lorem'},
  {id: 2,rating: 9,text: 'Lorem ipsum.'},
  {id: 3,rating: 8,text: 'Lorem ipsum dolor'},
]);

export default FeedBackStore
```

> :warning: Notar que la extensión que estamos utilizando es `.js`
>
> El store lo creamos con `const FeedbackStore = writable();` y en nuestro caso además le pasamos datos iniciales.
>



En lugar de tener la lista hardcodeada en `App` la tenemos en un store.



Una forma de obtener los datos del store es suscribirnos y desuscribirnos en el método `onDestroy` del ciclo de vida.

```vue
<script>
import FeedbackStore from '../stores/FeedbackStore.js'
let feedback = []

const unsuscribe = FeedbackStore.suscribe(data => feedback=data)

onDestroy(()=> {
	unsuscribe();
})
</script>

{#each feedback as fb (fb.id)}
<div in:scale out:fade="{{ duration: 500 }}">
  <FeedbackItem item={fb} />
</div>
{/each}
```

Al suscribirnos nos aseguramos que los datos se mantengan actualizados y se producirá un rerender cada vez que haya cambios.

Es importante que nos desuscribamos al store al desmontar el componente pues de lo contrario nos estaremos suscribiendo cada vez que mostremos el componente y esto podría ocasionar *memory leaks*.

Otra forma más simple (la desuscripción será automática cuando el componente se elimine del DOM) y no nos hará falta una variable local.

```vue
{#each $FeedbackStore as fb (fb.id)}
<div in:scale out:fade="{{ duration: 500 }}">
  <FeedbackItem item={fb} />
</div>
{/each}
```

## Agregar Items a Store

Una forma de hacerlo es con eventos custom y pasando las props hacia arriba hasta llegar a `App` que tiene el array de elementos. Sin embargo, utilizando stores esto es mucho mas simple y podemos hacerlo desde el formulario en el cual tengamos el elemento a agregar.

```vue
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



## Eliminar Items de Store

Anteriormente vimos que para eliminar un elemento de la lista debíamos usar custom events para elevar las props.

Ahora esto no será necsario y podremos alterar el store desde el `Item`

```vue
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



# Componentes UI

A continuación mostramos un ejemplo de un componente `Button.svelte` (que ubicamos en `components/shared`) que recibe como props `type` que podrá ser "primary" o "secondary", un booleano `flat` y otro `inverse`. Además recibe como slot el contenido a mostrar en el botón.

```vue
<script>
  export let type = 'primary';
  export let flat = false;
  export let inverse = false;
</script>

<button class:flat={flat} class:inverse={inverse} class={type} on:click>
  <slot></slot>
</button>

<style>
  button{
    border: 0;
    cursor: pointer;
    border-radius: 6px;
    padding: 8px 12px;
    font-weight: bold;
    box-shadow: 1px 2px 3px rgba(0,0,0,0.2);
  }
  .flat{
    box-shadow: none;
  }
  .primary{
    background: #d91b42;
    color: white;
  }
  .secondary{
    background: #45c496;
    color: white;
  }
  .primary.inverse{
    color: #d91b42;
    background: white;
    border: 2px solid #d91b42;
  }
  .secondary.inverse{
    color: #45c496;
    background: white;
    border: 2px solid #45c496;
  }
</style>
```

> :warning: Notar que tenemos un `on:click` para hacer *event forwading* pues de lo contrario no nos funcionaría si tenemos  `<Button on:click={handleClick}/>` dado que el button no nos estaría haciendo llegar ese evento.

# Validaciones

Implementaremos una validación muy rudimentaria que se efectúa en el handler asociado al *submit event*, es decir en `submitHandler`. Los valores de los elementos del formulario los almacenamos en el objeto `fields` y hacemos el binding con cada propiedad.

Comenzamos poniendo una variable `valid` en `true` y ante cualquier validación que falle la pondremos en `false`. Cuando todos los campos son válidos `valid` permanece en `true` y mostramos un `console.log()`

Contamos además con un objeto `errors` que tiene una propiedad para cada campo del formulario y cargaremos en ella el error asociado a dicho campo en caso de tenerlo o la dejaremos vacía. Ese error es mostrado debajo de cada campo.

Tener presente que esta validación no bloquea el botón de submit aunque no se reúnan las validaciones y efectúa una nueva validación al presionarlo.

```vue
<script>
  import Button from '../shared/Button.svelte';
  let fields = { question: '', answerA: '', answerB: '' };
  let errors = { question: '', answerA: '', answerB: '' };
  let valid = false;
  const submitHandler = () => {
    valid = true;
    // question
    if (fields.question.trim().length < 5) {
      valid = false;
      errors.question = 'Question must be at least 5 chars long'
    } else {
      errors.question = ''
    }
    // answer A
    if (fields.answerA.trim().length < 1) {
      valid = false;
      errors.answerA = 'Answer A cannot be empty'
    } else {
      errors.answerA = ''
    }
    // answer B
    if (fields.answerB.trim().length < 1) {
      valid = false;
      errors.answerB = 'Answer B cannot be empty'
    } else {
      errors.answerB = ''
    }
    // add new poll
    if (valid) {
      console.log('valid', fields);
    }
  }
</script>

<form on:submit|preventDefault={submitHandler}>
  <div class="form-field">
    <label for="question">Poll Question:</label>
    <input type="text" id="question" bind:value={fields.question}>
    <div class="error">{ errors.question }</div>
  </div>
  <div class="form-field">
    <label for="answer-a">Answer A value:</label>
    <input type="text" id="answer-a" bind:value={fields.answerA}>
    <div class="error">{ errors.answerA }</div>
  </div>
  <div class="form-field">
    <label for="answer-b">Answer B value:</label>
    <input type="text" id="answer-b" bind:value={fields.answerB}>
    <div class="error">{ errors.answerB }</div>
  </div>
  <Button>Add Poll</Button>
</form>

<style>
  
</style>
```



# :warning: Concepto Referencias y `find()`

Recordar que `find()` devuelve un valor pero este valor podrá ser un primitivo o una referencia. Si tenemos un array de objetos `copiedPolls` y con `find()` obtenemos un objeto deseado y luego modificamos sus propiedades, estaremos modificando un elemento del al array original.

```javascript
let copiedPolls = [...polls];
let upvotedPoll = copiedPolls.find(poll => poll.id==id);

if(option ==='a'){
	upvotedPoll.votesA++;
}

if(option ==='a'){
	upvotedPoll.votesA++;
}

polls = copiedPolls;
```

