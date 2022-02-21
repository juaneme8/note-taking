# Svelte

> Basado en el [Crash Course](https://youtu.be/3TVy6GdtNuQ) de Traversy Media (COMPLETO).
>
> Basado en el curso [Introducción a Svelte](https://www.youtube.com/playlist?list=PLV8x_i1fqBw2QScggh0pw2ATSJg_WHqUN) de midudev de 7 videos (COMPLETO).
>
> Basado en el curso [Svelte Tutorial for Beginners](https://www.youtube.com/playlist?list=PL4cUxeGkcC9hlbrVO_2QFVqVPhlZmz7tO) de The Net Ninja de 35 videos.

## Introducción

Svelte fue creado por Rich Harris y nos permite construir aplicaciones web e interfaces de usuario **reactivas** de la misma manera que React, Vue o Angular. Svelte es un **compilador** que genera un bundle de vanilla JavaScript optimizado. Es frecuente que se lo llame framework pero en realidad no lo es.

Nos permite crear una SPA (*single page application*) o bien controlar una porción pequeña de un sitio con componentes reutilizables.

El funcionamiento de Svelte es bastante simple de entender (por ejemplo si lo comparamos con React con el Virtual DOM y esas conceptos). Simplemente escribiremos el código con la sintaxis de Svelte y este luego será compilado en un bundle de vanilla JavaScript puro optimizado que será leído por el navegador. Luego deployaremos este único archivo y no hará falta ninguna librería extra como sí sucede con React o Vue que si bien también compilan el código *at build-time* aparte de este bundle requieren de la librería de React o de Vue. 

Hablamos de **aplicaciones reactivas** ya que queremos que si tenemos un cierto dato o estado en pantalla y este cambia de valor reflejarlo de manera inmediata.



## Características

* Nos permite [escribir menos código](https://svelte.dev/blog/write-less-code) que el que necesitaríamos en una aplicación de React por ejemplo.
* Como el código es compilado, nos permite detectar errores que de otra forma veríamos en run-time (aunque antes el linter nos ayudaría con alguno de ellos).
* Nos permite crear UIs dinámicos
* Produce JavaScript optimizado
* 30% más rápido que otros frameworks.
* Animaciones y transiciones interesantes disponibles por defecto.
* Gran ecosistema.
  * **SvelteKit**: similar a lo que es Next.js para React aportando SSR, routing, etc.
  * **Svelte Native**
* Simplicidad de uso.



## Creación Proyecto

Si queremos probar código de Svelte online podemos usar el [Playground](https://svelte.dev/repl).

Existen distintas formas de crear un proyecto con Svelte (podríamos utilizar por ejemplo SvelteKit) pero utilizaremos la indicada en al documentación usando la herramienta de scafolding llamada **degit** (también creada por Rich Harris) que realizará una copia del último commit de un repositorio (en este caso del de svelte) sin descargar el historial completo de git.

Desde el directorio padre de la futura carpeta del proyecto:

```bash
npx degit sveltejs/template md-svelte
cd md-svelte
npm install
npm run dev
```

> Recordemos que `npx` es un comando de`npm` que nos permite ejecuctar un paquete, en este caso `degit` que de otra forma tendríamos que instalar como global. 
>
> ```bash
> npm install -g degit
> degit sveltejs/template md-svelte
> ```
>
> Mientras que en este caso lo instalará en una carpeta temporal y lo ejecutará al vuelo.

Obtendremos un servidor de desarrollo y utilizaremos rollup como module bundler.



### Extensión VS Code

Se aconseja instalar la extensión Svelte for VS Code la cual nos aportará de formateado, autocompletado, nos permitirá el uso de emmet.



### Scripts

En `package.json` podremos ver que tenemos los siguientes scripts:

```json
"scripts": {
    "build": "rollup -c",
    "dev": "rollup -c -w",
    "start": "sirv public --no-clear"
},
```



* Con `npm run dev` iniciamos un servidor de desarrollo con live reload.
* Con `npm run build`para compilar el código Svelte en código JavaScript optimizado para producción.
* Con `npm start` con `sirv`serviremos la carpeta `public` para producción.



### Estructura de Carpetas

* `scripts` contiene el archivo `setupTypescript.js` y si ejecutamos `node scripts/setupTypescript.js` podremos utilizar TypeScript como vemos en la [documentación](https://svelte.dev/blog/svelte-and-typescript). 

* `public` en ella el archivo `index.html` que será la página que será cargada por el navegador. Veremos que en el `<body>` no tenemos nada y que importamos un archivo `/build/bundle.js` (archivo y carpeta que serán creados cuando ejecutamos `npm run build`). 

  En `global.css` podremos colocar los estilos globales y en `/build/bundle.css` los estilos de los componentes.

* `src` tenemos dos archivos `main.js` que es el entrypoint o punto de entrada en el cual hacemos el binding del componente principal (root o top level component) `App.svelte` con `document.body` . Si en cambio en `index.html` tenemos por ejemplo un div con `id=root` donde queremos renderizar los componentes podemos poner `target: document.getElementById("root")`. El resto de los componentes los iremos anidando dentro de `App`.

* `rollup.config.js` es un archivo de configuración similar al que tenemos en webpack, con el propósito de configurar la compilación y de hacer el bundle de salida.

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

> Notar además que le podemos pasar props (en este caso le pasamos una llamada `name`) y luego en `App.svelte` la recibimos haciendo `export let name;` dentro de `<script></script>` y luego podremos mostrarla en pantalla como `{name}`. Obviamente si no lo necesitamos, podremos quitar esta propiedad.

# Componentes

Los componentes son piezas reutilizables de UI que tienen la extensión `.svelte` y encapsulan la salida (HTML), la lógica (JavaScript) y los estilos (CSS). Además nos permiten tener un código más legible que si tuviésemos todo en `App.svelte`.

Tenemos distintas formas de organizar los componentes pero la mas frecuente es colocarlos dentro de una carpeta `components` en `src`. Sin embargo, en proyectos de magnitud es habitual una carpeta dentro de `components` para cada feature o sección.



El modo de importar un componente en otro (por ejemplo en `App.svelte`) es idéntico al de React dentro de los tags `<script>` colocamos `import FeedbackList from './components/FeedbackList.svelte'` y luego lo usamos con `<FeedbackList/>`.

> Cuando en un componente utilizamos otro componente se dice que lo consumimos.



> :warning: Es importante planear el **árbol de componentes** con antelación para tener certeza de cómo van a fluir los datos para evitar dolores de cabeza.
>
> :warning: Las imágenes debemos colocarlas en una carpeta `img` en `public` y las referenciamos como `"/img/imageName.svg"`.
>
> :warning: Los componentes que serán reutilizables en distintas partes de la aplicación los colocamos en`/components/shared` por ejemplo `Button.svelte`.



## Estructura de un componente

* Lógica escrita en JavaScript dentro de tags `<script>` allí podremos setear variables, crear funciones o valores reactivos como veremos más adelante.
* Luego podemos colocar las etiquetas de marcado de html en el medio donde podremos emplear llaves `{}` cuando queramos mostrar una variable o evaluar una expresión. También podremos utilizar condicionales o iteraciones.
* Estilos de CSS dentro de tags `<style>`
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



# Props

El pasaje de props nos permite que los componentes sean reutilizables y que reciban datos dinámicos.

Si queremos pasarle ciertos datos a un componente podemos hacerlo via props por ejemplo `<FeedbackList feedback={feedback}/>`. 

Luego en el componente `FeedbackList` accederemos a esa prop de la siguiente forma:

```
<script>
	export let feedback = [];
</script>
```

> Si bien lo recibiremos desde el componente padre le damos un **valor default** de un array vacío por si invocamos el componente sin esa prop.
>
> Con `export` indicamos que se trata de un valor que recibiremos como props es decir que proviene de afuera del componente.
>
> Si recibe más de una props podríamos poner `export let prop1, prop2, prop3`



* Cuando tenemos el mismo nombre de prop que el de la variable que almacena el dato como en `<FeedbackList feedback={feedback}/>` podemos usar el *shorthand* `<FeedbackList {feedback}/>`

  

* En ocasiones nos convendrá almacenar las props en un objeto y pasárselas todas al componente utilizando el spead operator.

  ```
  <script>
  	const commonProps = { maxCounter: 5}
  </script>
  
  <Counter {...commonProps}/>
  ```

  



# Manejo de Estado

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



## Modificar Estado

Como la reactividad de Svelte es disparada por asignaciones, si usamos métodos como `push` o `splice` no lograremos actualizaciones automáticas.

Si tenemos que modificar el estado agregando por ejemplo un elemento a un array de `users` no podemos hacerlo con `users.push()` sino que debemos reasignarlo a otro array, lo cual hacemos con `users = [..users, {id:'4',name:'Jen'}]` o bien podríamos utilizar `users = users.concat({id:'4',name:'Jen'})` que también retorna un nuevo array.



Si queremos que el nuevo elemento sea agregado al comienzo podemos hacer `users = [{id:'4',name:'Jen'},..users]`



# Life Cycle Methods

De manera similar a los componentes de clases en React tenemos métodos del ciclo de vida de los componentes en Svelte.

```vue
import {onMount, onDestroy} from 'svelte'

onMount(()=> {
	console.log('mounted')
	//http requests para obtener data de backend
})

onDestroy(()=> {
	console.log('destroy')
})
```



# Estilos

## Estilos Globales

Es posible aplicar estilos globales en `public/global.css` que será importando en `index.html`.



## Estilos Locales

Los estilos locales son aquellos específicos de los elementos de un componente. Debemos hacerlo dentro de los tags `<style>` y veremos que Svelte le asigna un nombre de clase especial con un hash único para evitar colisiones. Esto podremos verificarlo en las DevTools o analizando el contenido de `build/bundle.css`.

```vue
<style>
	h1{
		color: #09f;
	}
</style>
```

> Los `h1` que tengamos en otros componentes de la aplicación no se verán afectados por estos estilos.



## Estilos Condicionales

Si queremos que un estilo se aplique o no dependiendo de una expresión podemos hacerlo con con esta sintaxis `class:promo={isPromo}` esto significa que la clase `promo` se aplicará si `isPromo` (que también podría ser una expresión, no sólo un booleano) es `true`. En cambio, la clase `backdrop` se aplicará siempre.

```
<div class="backdrop" class:promo={isPromo}>
	<div class="modal">
		<p>Sign up for offers!</p>
	</div>
</div>
```



## Estilos Variables

Si tenemos una variable `let color='blue';` y queremos asignarlo a un elemento podemos hacerlo con `<h1 style="color: {color}">Título</h1>`

# Binding de Datos

### One way data binding

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

> Notar que recibimos el evento `e` de manera automática.
>
> Aunque pusimos `let text = ''; ` en realidad no hace falta darle un valor inicial podríamos haber puesto `let name;`



#### Forma reducida

Existe una forma reducida de actualizar el valor de `text` y de asegurarnos el two way binding. Es decir que estas dos líneas:

```vue
on:input={handleInput} 
value = {text} 
```

y la función `handleInput` pueden ser reemplazadas por

```vue
bind:value={text}
```

Como veremos a continuación:

```vue
<script>
  let text = ''
</script>

<input 
       type="text" 
       bind:value = {text} 
       placeholder="Tell us something"
>
<button on:click={()=> text+='@'}>Click</button>
```



## `type="number"` o `type="text"`



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



## `type="checkbox"`

A la hora de trabajar con inputs de tipo checkbox podemos hacerlo de dos formas. 

La primera es usando `bind:checked` y creando una variable para cada checkbox que inicia en `false` y  que se pondrá en `true` cuando seleccionemos ese elemento. 

La segunda es usando `bind:group={array}` y al seleccionar cada elemento será añadido a ese array.



### Atributo`checked`

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

Esta alternativa puede ser muy confusa si tenemos muchos checkboxes ya que necesitaremos una variable para cada uno de ellos.



#### Atributo`group`

En este caso además del `bind:group={}` es necesario usar el atributo `value` y estos valores serán los añadidos al array.

```vue
<script>
	let skils = [];
	$:console.log(skils)
</script>

<input type="checkbox" bind:group={skils} value="javascript"/>javascript<br>
<input type="checkbox" bind:group={skils} value="react"/>react<br>
<input type="checkbox" bind:group={skils} value="node"/>node<br>
```



## `Select`

A la hora de trabajar con `<select>` hacemos un `bind:value`

```vue
<script>
	let gender="";
</script>


<select bind:value={gender}>
	<option value="male">Male</option>
	<option value="female">Female</option>
</select>
```



# Eventos

La forma de trabajar con eventos en Svelte es colocando `on:` seguido del nombre del evento, por ejemplo `on:click={}`.



## `on:click`

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



## `on:input`
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



## `on:change`

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



## `on:submit`

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



# Event Forwading 

Cuando queremos propagar hacia arriba un evento, es decir desde desde el componente hijo hacia el padre debemos hacer uso de lo que se conoce como *event forwading*.

Un ejemplo típico de uso de event forwading es cuando queremos mostrar y ocultar un modal. Por ejemplo si tenemos un componente padre que llama a un componente hijo `Modal`. El padre tiene un botón para mostrar el modal y el hijo debe ocultarlo al presionar en cualquier parte de la pantalla.

Si la directiva `on:` es usada sin un valor, como por ejemplo `on:click` se producirá la propagación de ese evento hacia arriba en el árbol de componentes, lo que se conoce como *event forwading*. Luego será el `Modal` quien emitirá este evento por lo que el consumidor del componente podrá escucharlo `<Modal on:click={toggleModal}/>`



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



# Event Modifiers

Los *event modifiers* como su nombre lo indica son modificadores que agregamos al final de los eventos, por ejemplo donde tenemos `on:click` usando un modificador podríamos pasar a tener `on:click|preventDefault`.

Existen distintos tipos de modificadores y los mas comunes son: `once`, `preventDefault` y `self`. Además es posible encadenar modificadores.

## `once` 

El modificador de eventos `once` nos permite aseguramos que el evento sólo se dispare una vez.



## `preventDefault`

`preventDefault` nos permite impedir la acción por default de la misma manera que lo hacemos con `e.preventDefault()`.

Cuando trabajamos con formularios en lugar de poner  `<form on:submit={handleSubmit}>` y luego en `handleSubmit` evitar la acción default con `e.preventDefault()` podemos usar un *event modifier*. De esta forma el código nos quedaría `<form on:submit|preventDefault={handleSubmit}>` y no habría que realizar nada en `handleSubmit`.

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



## `self`

El modificador `self` hace que el evento sólo se dispare si el elemento clickeado es el target propiamente (y no uno de sus hijos).

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



# Declaraciones Reactivas

Los valores reactivos son aquellos que se actualizan automáticamente cuando cambian los datos de los cuales dependen.



Svelte actualiza automáticamente el DOM cuando el estado del componente cambia.

Supongamos que tenemos un componente `Counter` que consiste en un contador que se incrementa al presionar un botón. En este caso el valor de `{count}` se verá actualizado en pantalla cada vez que lo presionemos.

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



> Podemos ver que tiene un comportamiento similar al de un efecto en React y en ese caso `count` sería la dependencia (lo cual es detectado automáticamente por Svelte). En tanto que se ejecutará el código siempre que cambie el valor de `count` (y también la primera vez cuando se monta el componente).



Parecido a los **reactive values** contamos con los **reactive statements** que son ejecutados cuando los datos dentro del statement cambian.

```
$: console.log(count)
```

También podemos tener reactive statements con condicionales:

```vue
$: if (count > maxCounter ) {
	console.log('limit contador')
	count = maxCounter
}
```

O directamente poner un bloque de código:

```vue
$:{
	console.log(value);
	console.log(isEvenOrOdd);
}
```

Este bloque de código se ejecutará cuando cambie `value` o `isEvenOrOdd` (que también es un valor reactivo).



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



# Condicionales

En Svelte es posible realizar un renderizado condicional, es decir que mostrar o no el contenido de acuerdo a una condición. Esto podría ser útil por ejemplo si queremos mostrar un contenido si el usuario está logueado y otro distinto si no lo está. 

Supongamos que queremos mostrar la cantidad de resultados obtenidos luego de una llamada a una API o en su defecto un cartel que indique que no encontramos ningún resultado. La forma mas sencilla de hacerlo es con el operador ternario:

```vue
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



# Loops

Utilizamos loops cuando queremos recorrer un array y mostrar HTML para cada item que tiene.

## Bloque #each

```vue
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



> Es posible trabajar también con un segundo parámetro `index`

```vue
{#each users as user,index (user.id)}
	<h3>{user.id}: {user.name}</h3>
{/each}
```



> Si así lo quisiéramos podríamos utilizar **object destructuring**:

```vue
{#each users as {id,name} (id)}
	<h3>{id}: {name}</h3>
{/each}
```



> Si queremos agregar a esta lista la posibilidad de borrar elementos, es decir queremos pasar argumentos a un handler lo hacemos mediante **inline functions**.

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



# Dispatch Custom Event

Supongamos que tenemos un componente principal `App` con un array de elementos y se los pasamos como props a un componente `List` que a su vez llama a un componente `Item` por cada uno de ellos.

Además suponemos que ese elemento tiene una cruz con la cual podrá eliminar de la lista. Necesitamos de alguna forma que esto llegue hasta el componete `App` que tiene dicho array.



En `Item.svelte`

```vue
</script>
import {createEventDispatcher} from 'svelte'

const dispatch = createEventDispatcher();

const handleDelete = (itemId) => {
	dispatch('delete-feedback',itemId)
}
</script>

<button class="close" on:click={() => handleDelete(item.id)}>X</button>
```

> Con `const dispatch = createEventDispatcher();` creamos una función `dispatch`. Esta función la usaremos para emitir un *custom event*.
>
> En lugar de `itemId`  en `dispatch('delete-feedback',itemId)` podríamos utilizar un objeto con múltiples propiedades.
>
> :warning: Aunque aca usamos *kebab-case* en los videos de The Net Ninja utiliza *camelCase* para los nombres de los *custom events*.

En `List.svelte` veremos que como es `Item` quien emite el custom event `delete-feedback` ponemos un listener para ducho evento y hacemos *event forwading*. 

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

> Por defecto recibimos el evento `e` y en la propiedad `detail` recibimos el id del elemento a eliminar de la lista.
>
> Notar que estamos reasignando `feedback`



# Transiciones

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
> Si quisiéramos que fuera con un fade igual para entrada y salida podríamos haber puesto directamente `transition: fade`



# Stores

El uso de stores nos permite no tener que crear eventos custom para pasar props hacia arriba.

Creamos un archivo `stores.js`

```vue
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

```vue
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

```vue
{#each $FeedbackStore as fb (fb.id)}
<div in:scale out:fade="{{ duration: 500 }}">
  <FeedbackItem item={fb} />
</div>
{/each}
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



# Fetching de Datos

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



## `{#await}`

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
