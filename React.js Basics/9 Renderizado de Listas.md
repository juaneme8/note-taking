# Renderizado de Listas
Muy frecuentemente tendremos que renderizar listas de items y queremos repetir cierto `jsx` para cada elemento de la lista.
Utilizaremos el método `map()`.
Supongamos que tenemos una array de strings y cada uno de ellos contiene el nombre de una persona y lo queremos mostrar en pantalla.

Notar que a pesar de estar dentro de un par de `{}` coloco otros cuando quiero evaluar la expresión.
```jsx
import React from 'react';

function NameList() {
	const names = ['Bruce', 'Clark', 'Diana'];
	return (
		<div>
			{names.map((name) => (
				<h2>{name}</h2>
			))}
		</div>
	);
}

export default NameList;
```

Si queremos dejar el código más conciso y dejar `return` lo más simple posible podemos mover la lógica hacia otra parte.
Notar que al estar fuera del `return` utilizamos el código JavaScript de manera habitual y no envuelto por llaves.
```jsx
import React from 'react';

function NameList() {
	const names = ['Bruce', 'Clark', 'Diana'];
	const nameList = names.map((name) => <h2>{name}</h2>);
	return <div>{nameList}</div>;
}

export default NameList;

```

En ocasiones tendremos que renderizar listas de objetos con varias propiedades como por ejemplo:
```jsx
const persons = [
		{
			id: 1,
			name: 'Bruce',
			age: 30,
			skill: 'React',
		},
		{
			id: 2,
			name: 'Clark',
			age: 25,
			skill: 'Angular',
		},
		{
			id: 3,
			name: 'Diana',
			age: 20,
			skill: 'Vue',
		},
	];
```

Podríamos implementar una solución similar a la mostrada anteriormente:

```jsx
const personList = persons.map((person) => (
		<h2>
			Mi nombre es {person.name}, tengo {person.age} años y mi talento es {person.skill}
		</h2>
	));
	return <div>{personList}</div>;
```

Sin embargo es una buena práctica refactorizar el `jsx` creando un nuevo componente `Person.js` y luego utilizar el componente dentro del método `map()`. De esta manera el componente `NameList` sólo será responsable de renderizar una lista y `Person` sólo será responsable de renderizar una persona.
```jsx
import React from 'react';
import Person from './NameList';

function NameList() {
	const persons = [
		{
			id: 1,
			name: 'Bruce',
			age: 30,
			skill: 'React',
		},
		{
			id: 1,
			name: 'Clark',
			age: 25,
			skill: 'Angular',
		},
		{
			id: 3,
			name: 'Diana',
			age: 20,
			skill: 'Vue',
		},
	];
	const personList = persons.map((person) => <Person person={person} />);
	return <div>{personList}</div>;
}

export default NameList;

```

> Notar que almaceno en una variable el listado usando el componente `Person` fuera del `return`

Luego en `Person.js` recibo a `props.person` y con destructuring logro trabajar directamente con `person`.

Si recibiéramos muchas props y sólo queremos hacer destructuring de `person` podemos poner `function Person({person, ...props})`

```jsx
import React from 'react';

function Person({ person }) {
	return (
		<div>
			<h2>
				Mi nombre es {person.name}, tengo {person.age} años y mi talento es {person.skill}
			</h2>
		</div>
	);
}

export default Person;
```

# Listas y keys
Cuando mostramos una lista de elementos obtendremos el siguiente warning: `Each child in an array or iterator should have a unique "key" prop`
Esto significa que al componente `Person` debemos pasarle una `prop` llamada `key` que debe ser *unique* por lo que hacemos uso de la propiedad `id`.

```jsx
import React from 'react';
import Person from './Person';

function NameList() {
	const persons = [
		{
			id: 1,
			name: 'Bruce',
			age: 30,
			skill: 'React',
		},
		{
			id: 2,
			name: 'Clark',
			age: 25,
			skill: 'Angular',
		},
		{
			id: 3,
			name: 'Diana',
			age: 20,
			skill: 'Vue',
		},
	];
	const personList = persons.map((person) => <Person key={person.id} person={person} />);
	return <div>{personList}</div>;
}

export default NameList;
```

De la misma manera si no hubiera creado este componente y siguiéramos trabajando sólo con `NameList`, debería agregar el atributo `key` al elemento h2.

```jsx
const personList = persons.map((person) => (
		<h2 key={person.id}>
			Mi nombre es {person.name}, tengo {person.age} años y mi talento es {person.skill}
		</h2>
	));
	return <div>{personList}</div>;
```

Debemos tener presente que la `key prop` no es accesible en el componente hijo, si tuviéramos que utilizar ese valor deberíamos pasarlo como una nueva `prop`.
El uso de la `key prop` le permite a React identificar qué items de la lista fueron agregados, actualizados o eliminados y así poder **actualizar la UI de manera eficiente**. 
Si tenemos por ejemplo una lista como la siguiente:
```html
<ul>
    <li>Bruce</li>
    <li>Clark</li>
</ul>
```
Y actualizamos agregando un elemento en la parte inferior de la lista:
```html
<ul>
    <li>Bruce</li>
    <li>Clark</li>
    <li>Diana</li>
</ul>
```
React iterará comparando los elementos de la lista, compará los primeros elementos de cada lista y verá que son iguales, los segundos también verá que son iguales y al llegar al tercero verá que hay diferencias por lo que **mutará** la primera lista agregando dicho elemento al DOM. 

Si en cambio tuviéramos la misma lista inicial
```html
<ul>
    <li>Bruce</li>
    <li>Clark</li>
</ul>
```
Pero el nuevo elemento lo queremos agregar como primer elemento:
```html
<ul>
    <li>Diana</li>
    <li>Bruce</li>
    <li>Clark</li>
</ul>
```
React al comparar el primer elemento de ambas listas ya encontraría diferencias y terminará **mutando** todos los elementos hijos en vez de darse cuenta que los primeros dos debe dejarlos intactos y agregar el primero arriba. Utilizando el atributo `key` pasamos de tener:
```html
<ul>
    <li key="1">Bruce</li>
    <li key="2">Clark</li>
</ul>
```
A tener
```html
<ul>
    <li key="3">Diana</li>
    <li key="1">Bruce</li>
    <li key="2">Clark</li>
</ul>
```
Con lo cual React se da cuenta que los elementos originales se desplazaron por lo que debe insertar el elemento al comienzo de la lista lo cual es mucho más eficiente. 

## Index como Key
En aquellos casos donde no contamos con un *id* que identifique de manera *unique* al elemento, podemos recurrir al uso del `index` del elemento dentro del array como `key`. La arrow function que le pasamos al método `map()` recibe un segundo parámetro con el índice del elemento en la iteración actual.

```jsx
import React from 'react';

function NameList() {
	const names = ['Bruce', 'Clark', 'Diana'];
	return (
		<div>
			{names.map((name, key) => (
				<h2 key={key}>{name}</h2>
			))}
		</div>
	);
}

export default NameList;

```
En caso de no especificar un `key` React por default usa el `index` como `key`.

Si bien esta es una solución puede parecer simple y elegante, en determinados escenarios puede traer inconvenientes importantes por lo que lo aconsejable es evitarla. Por ejemplo si queremos agregar un elemento al comienzo de la lista, como ya hay anteriormente un `<li key="0"></li>` React pensará que tiene que reutilizarlo, por lo que habrá problemas en el contenido. De manera similar al reordenar dichos elementos.

Sólo debemos usarlo sólo si no tenemos un id *unique*, si la lista es estática y no va a cambiar y si nunca la vamos a ordenar o filtrar. En caso de no reunir estas tres condiciones podemos recurrir a un **npm package** que genere un id unique o hashear un valor unique a partir de una de las propiedades existentes.

> Investigando encontré este paquete que parece cumplir esa tarea: https://www.npmjs.com/package/uniqid