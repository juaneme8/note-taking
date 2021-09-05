# `React.Fragment`
Los fragmentos nos permiten agrupar una lista de elementos hijos sin añadir nodos extras al DOM.
Creamos un componente `FragmentDemo.js` en el cual queremos retornar un `<h1>` y un `<p>`. Como sabemos las expresiones JSX deben retornar un elemento padre que las  wrapee. 
```jsx
import React from 'react';

function FragmentDemo() {
	return (
		<div>
			<h1>Fragment Demo</h1>
			<p>Description</p>
		</div>
	);
}

export default FragmentDemo;
```
Una posible solución es agregar un `<div>` que envuelva a todos los elementos.Esto ocasionará un agregado de dicho `div` al DOM, lo cual en ocasiones puede no ser deseado.

Otra forma de solucionarlo es wrappear los elementos con `<React.Fragment>` que no produce el agregado de dicho nodo. 

```jsx
import React from 'react';

function FragmentDemo() {
	return (
		<React.Fragment>
			<h1>Fragment Demo</h1>
			<p>Description</p>
		</React.Fragment>
	);
}

export default FragmentDemo;
```

Si bien en este caso no nos afecta mucho una u otra solución, habrá casos que no podríamos usar un `<div>` por ejemplo una situación en la que tenemos un `Table` y `tr` y dentro un componente `Columns` donde queremos devolver múltiples `td`

En `Table.js`
```jsx
import React from 'react';
import Columns from './Columns';

function Table() {
	return (
		<table>
			<tbody>
				<tr>
					<Columns />
				</tr>
			</tbody>
		</table>
	);
}

export default Table;
```

En `Columns.js` queremos devolver múltiples elementos `<td>`, si los colocamos envueltos en un `<div>` nos aparecerá un Warning que nos dice: `<div> cannot appear as a child of <tr>.` y `<td> cannot appear as a child of <div>.`

```jsx
import React from 'react';

function Columns() {
	return (
		<div>
			<td>Nombre</td>
			<td>Juan</td>
		</div>
	);
}

export default Columns;
```
Si reemplazamos el `div` por `React.Fragment` esto se soluciona:

```jsx
function Columns() {
	return (
		<React.Fragment>
			<td>Nombre</td>
			<td>Juan</td>
		</React.Fragment>
	);
}
```

## `React.Fragment` & `key` 
Otra particularidad importante de los fragmentos es que pueden aceptar el atributo `key` cuando estamos renderizando listas de hecho `key`  es el único atributo que se puede pasar a  `React.Fragment`. 

```jsx
import React from 'react';

function FragmentKey() {
	const authors = [
		{ id: 1, name: 'Juan', age: 33 },
		{ id: 2, name: 'Ocho', age: 32 },
	];
	return (
		<React.Fragment>
			{authors.map((author) => (
				<React.Fragment key={author.id}>
					<h1>{author.name}</h1>
					<p>{author.age}</p>
				</React.Fragment>
			))}
		</React.Fragment>
	);
}

export default FragmentKey;

```


## Sintaxis corta
Podemos utilizar `<>` y `</>` en lugar de `<React.Fragment>` y `</React.Fragment>` utilizando un símbolo que representa la idea de que no agregaremos dicho elemento al DOM.
En caso de utilizar este shorthand no podemos pasarle el atributo `key`