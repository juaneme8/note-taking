# BOM
El BOM (browser object model) es la forma en la que JavaScript representa al navegador en formato JSON.
Se trata de un objeto global que puede ser accedido con el nombre de `window`.

Cuando mostramos algo en la consola usando `console.log('Mensaje');` es como si estuviéramos haciendo `window.console.log('Mensaje');` y cuando mostramos una variable `a` con `console.log(a);` estamos haciendo `window.console.log(window.a);`

Este objeto tiene muchas propiedades, mencionamos algunas de ellas a continuación:
* `innerHeight` (con respecto al documento abierto)
* `innerWidth`
* `outerHeight` (con respecto al navegador)
* `outerWidth`

# DOM
> Basado en la lista de reproducción [JavaScript & the DOM](https://www.youtube.com/watch?v=FIORjGvT0kk&list=PL4cUxeGkcC9gfoKa5la9dsdCNpuey2s-V)

El DOM (*document object model*) es la forma en la que JavaScript representa al HTML en formato JSON. **Pertenece al objeto global** `window`

La Web API del DOM nos permite manipular el HTML y esto podemos verificarlo haciendo simplemente `window.document` y veremos que nos sugiere gran cantidad de métodos.

## Características del DOM
* Es creado cuando se carga la página.
* Podemos pensarlo gráficamente como un árbol de nodos (tendremos nodos padres y nodos hijos).
* Interactuamos con él usando JavaScript: podemos alterar elementos HTML, modificar estilos CSS, agregar event listeners (click, keypress, submit, etc). Esto mismo podría ser realizado usando por ejemplo jquery pero tendríamos que invocar un archivo con la consecuente ralentización del sistema, por lo que es más performante realizarlo en Vanilla JS.

## Creación y Edición de Nodos
Si queremos crear un elemento `div` podemos hacerlo con `const newDiv = document.createElement("div")`

Si queremos darle un `id` a ese elemento hacemos `newDiv.id= 'titulo'`

Si quermeos agregar, quitar o togglear (agregar si no la tiene y quitar si la tiene) una clase al id `newDiv.classList.add('clase1');'`, `newDiv.classList.remove('clase2');` ,  `newDiv.classList.toggle('clase3');`

Para crear un nodo de texto hacemos: `const newContent = document.createTextNode("Hola Mundo!");`

Luego para agregar el texto al `div` hacemos `newDiv.appendChild(newContent)`

Finalmente para agregar el `div` al `body` hacemos `document.body.appendChild(newDiv)`

## Eliminar Nodo
De la misma manera que para agregar un elemento tuvimos que trabajar con su padre y luego ejecutar `appendChild` para eliminarlo tendremos que recurrir a `removeChild`.

Si quisiéramos borrar al elemento recientemente agregado bastaría con hacer: `document.body.removeChild(newDiv)`

Supongamos ahora que en lugar de tener como padre al `body` queremos borrar un elemento `p` cuyo padre desconocemos:
```jsx
const p = document.querySelector('p');
const parent = p.parentNode;
parent.removeChild(p);
```

Hubiera sido lo mismo usar `p.parentElement`

## Querying the DOM
### `getElementById`
Aprovechando que los ids deben ser únicos y referenciar sólo a un elemento HTML con `getElementById()` obtenemos directamente la porción de HTML de dicho id. Es decir que si tenemos:  `<div id="app"></div>` y hacemos:
```js
const app = document.getElementById("app");
console.log(app); //Obtenemos <div id="app"></div>
```

### `getElementByClassName`
Con `getElementByClassName` obtenemos una **HTML Collection** y para acceder a los distintos elementos podremos usar la notación con corchetes como si se tratara de un array  `[0]`,`[1]`, etc.
```js
const containers = document.getElementsByClassName("container");

console.log(containers); 
//HTMLCollection {0: HTMLDivElement, 1: HTMLDivElement, 2: HTMLDivElement, length: 3, item: ƒ item()…}

console.log(containers[0]); 
//<div class="container">1</div>

```
Con una **HTML Collection** no es posible usar `forEach`, sino que antes debemos convertirlo a un array cosa que podemos hacer con `Array.from(containers)` por lo que si ahora queremos iterar todo ese array: 
```js
Array.from(containers).forEach((container) => console.log(container));
```

Si queremos iterarlo sin convertirlo a un array podemos hacerlo con `for` 
```js
for (let i = 0; i < containers.length; i++) {
  console.log(containers[i]);
}
```

### `getElementByTagName`
Con `getElementByTagName`también obtendremos una **HTML Collection**.

Si tenemos el siguiente html:
```html
<ul>
	<li class="item">1</li>
	<li class="item">2</li>
	<li class="item">3</li>
</ul>
```
Queremos obtener los elementos con el tag `li`, entonces:
```js
const item = document.getElementsByTagName("li");
console.log(item); 
//HTMLCollection {0: HTMLLIElement, 1: HTMLLIElement, 2: HTMLLIElement, length: 3, item: ƒ item()…}
```

### `querySelector`
Con `querySelector` obtendremos el primer elemento que reúne el criterio especificado. Si tenemos el siguiente html:
```html
<ul>
	<li class="item">1</li>
	<li class="item">2</li>
	<li class="item">3</li>
</ul>
```
Luego en el script:
```js
const item = document.querySelector("li");
console.log(item); //Obtenemos <li class="item">1</li>
```

### `querySelectorAll`
Con `querySelectorAll` obtenemos una lista de resultados de tipo **Node List** (aunque sólo un nodo matchee con el criterio).
A diferencia del **HTML Collection** que obtuvimos con `getElementByClassName` y con `getElementByTagName`, con `querySelectorAll` obtenemos un **Node List** y por ende podemos usar `forEach` de manera directa.
```html
<ul>
	<li class="item">1</li>
	<li class="item">2</li>
	<li class="item">3</li>
</ul>
```

```js
const items = document.querySelectorAll("li");
items.forEach((item) => console.log(item));
//<li class="item">1</li>
//<li class="item">2</li>
//<li class="item">3</li>
```

Es posible encadenar búsquedas para buscar en el scope de la búsqueda anterior, es decir podríamos hacer a continuación `items.querySelector('...')`

## Visualizar Contenido
Una vez que seleccionamos un elemento, es posible ver su contenido de distintas formas. Una de ellas es con `innerHTML` que entrega todo el contenido como html y como consecuencia de esto es más lento, otra forma es con  `textContent` que entrega todo el texto contenido por un elemento y sus hijos y la ultima es con `innerText` que nos entrega sólo el texto que es visible (si tiene `display: none` no lo veremos)

Supongamos que tenemos el siguiente html:
```html
<ul>
  <li class="item">1</li>
  <li class="item" style="display: none;">2</li>
  <li class="item">3</li>
</ul>
```

```js
const lista = document.querySelector("ul");
console.log(list.innerHTML);
//<li class="item">1</li>
//<li class="item" style="display: none;">2</li>
//<li class="item">3</li>

console.log(list.innerText);
//1
//3

console.log(list.textContent);
//1
//2
//3
```

> **Regla mnemotécnica** podemos asociar la "i" de innerText con la "i" de visible.

### Inner
```js

```

```js

```

```js

```

```js

```

```js

```