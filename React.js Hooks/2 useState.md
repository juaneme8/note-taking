# Hook de Estado
## Introducción
Para explicar lo que motivó la creación del hook de estado recordemos que hasta el momento sólo podíamos tener `state` en componentes de clase, los Hooks nos permitirán contar con esta característica en componentes funcionales.

Queremos implementar un contador y lo haremos primero con un componente de clase `ClassCounter` donde veremos que hay tres etapas: la primera es la creación del componente de clase, la segunda la definición de la variable de estado  `count: 0`, la tercera es crear un método `this.incrementCount` capaz de incrementar la cuenta `count`.

```jsx
import React, { Component } from 'react';

class ClassCounter extends Component {
	constructor(props) {
		super(props);

		this.state = {
			count: 0,
		};
	}

	incrementCount = () => {
		this.setState({ count: this.state.count + 1 });
	};

	render() {
		return (
			<div>
				<button onClick={this.incrementCount}>Clicks {this.state.count}</button>
			</div>
		);
	}
}

export default ClassCounter;

```
Notar que a pesar de estar trabajando con el valor anterior del estado, lo modificamos de manera directa ya que más adelante demostraremos los inconvenientes de esta práctica (ya estudiado anteriormente para los componentes de clase)

Creamos ahora el componente `HookCounter.js` e intentaremos replicar estos mismos pasos. Un Hook es una función especial que nos permite engancharnos de características de React y por tratarse de funciones simplemente debemos llamar a  `useState()` habiéndola importado previamente `import React, {useState} from 'react';` 
Esta función acepta un parámetro que es el valor inicial y retorna un array de valores. En el primer elemento tendremos el valor actual de la variable y el otro un método para modificar el valor de dicha variable, por lo que usando destructuring los almacenaremos en dos variables.

```jsx
import React, { useState } from 'react';

function HookCounter() {
	const [count, setCount] = useState(0); //array destructuring
	return (
		<div>
			<button onClick={() => setCount(count + 1)}>Count {count}</button>
		</div>
	);
}

export default HookCounter;
```

El llamado al método `setCount()` ocasiona que el componente se re-renderice por lo que veremos el valor actualizado.
Notar que en `onClick={() => setCount(count + 1)` usamos *arrow function* ya que tenemos que pasarle un parámetro y no queremos que sea un llamado a función.

## Reglas de Hooks
* Sólo llamar a Hooks en el **nivel superior** y no debemos hacerlo dentro de loops, condicionales o funciones anidadas.
* Sólo debemos llamar a Hooks desde dentro de funciones React (componentes funcionales y desde *custom hooks*).

## `useState()`  con valor previo
Si bien lo implementado en el contador anterior funciona de manera correcta, veremos que cuando trabajamos con el estado basándonos en el valor anterior tenemos que implementar una técnica para evitar un comportamiento indeseado.

Pará empezar a entender esto supongamos que queremos crear un nuevo contador `HookCounterTwo` pero queremos que este tenga un botón para incrementar, un botón para decrementar y uno para resetear la cuenta. Lo implementamos como vimos hasta ahora. 

```jsx
import React, { useState } from 'react';

function HookCounterTwo() {
	const initialCount = 0;
	const [count, setCount] = useState(initialCount);
	return (
		<div>
			Count: {count}
			<button onClick={() => setCount(initialCount)}>Reset</button>
			<button onClick={() => setCount(count + 1)}>+</button>
			<button onClick={() => setCount(count - 1)}>-</button>
		</div>
	);
}

export default HookCounterTwo;
```

Si bien esto funciona, al igual que funcionaba lo que hicimos en `HookCounter` no es un modo seguro de modificar a `count`. Si tuviéramos un botón que incrementa en cinco unidades y lo hace llamando cinco veces a `setCount(count+1)` veríamos que el UI no se actualiza con el valor esperado. 



```jsx
import React, { useState } from 'react';

function HookCounterTwo() {
	const initialCount = 0;
	const [count, setCount] = useState(initialCount);

	const incrementFive = () => {
		for (let i = 0; i < 5; i++) {
			setCount(count + 1);
		}
	};

	return (
		<div>
			Count: {count}
			<button onClick={() => setCount(initialCount)}>
			Reset
			</button>
			<button onClick={() => setCount(count + 1)}>+</button>
			<button onClick={() => setCount(count - 1)}>-</button>
			<button onClick={incrementFive}>Increment 5</button>
		</div>
	);
}

export default HookCounterTwo;
```

Para solucionar esto en lugar de pasarle un valor a `setCount()` debemos usar una *arrow function* que recibe como primer parámetro el valor de `prevCount`

```jsx
import React, { useState } from 'react';

function HookCounterTwo() {
	const initialCount = 0;
	const [count, setCount] = useState(initialCount);

	const incrementFive = () => {
		for (let i = 0; i < 5; i++) {
			setCount((prevCount) => prevCount + 1);
		}
	};

	return (
		<div>
			Count: {count}
			<button onClick={() => setCount(initialCount)}>Reset</button>
			<button onClick={() => setCount((prevCount) => prevCount + 1)}>+</button>
			<button onClick={() => setCount((prevCount) => prevCount - 1)}>-</button>
			<button onClick={incrementFive}>Increment 5</button>
		</div>
	);
}

export default HookCounterTwo;


```



Esta actualización se conoce como **UseState Functional Update** y también será de utilidad cuando el estado sea actualizado luego de una operación asincrónica. De esta manera nos aseguraremos que al momento de actualizar tengamos el valor real y no uno desactualizado.

Por ejemplo supongamos que tenemos dos botones que incrementan y uno lo hace de manera instantánea y otro que lo hace asincrónicamente después de dos segundos. En ese caso si presionamos el botón asincrónico y luego una o varias veces el instantáneo, después de 2 segundos a la hora de actualizar el valor mostraría algo incorrecto si no tuvieramos una arrow function para ctualizar.

```jsx
function anotherCounter() {
    
const increaseAsync = () => {
	setNumber(number+1)
}

const increaseAsync = () => {
	setTimeout(()=> {
		setNumber(current => current+1)
	}, 2000)
}

return (
		<div>
			Count: {count}
		</div>
	);
}

export default anotherCounter;

```



Esto tiene un equivalente en los componentes de clase (que fue estudiado oportunamente) y que mostramos a continuación:

```jsx
import React, { Component } from 'react';

class ClassCounterTwo extends Component {
	constructor(props) {
		super(props);

		this.state = {
			count: 0,
		};
	}

	incrementCount = () => {
		this.setState((prevState) => {
			return { count: this.state.count + 1 };
		});
	};

	render() {
		return (
			<div>
				<button onClick={this.incrementCount}>Clicks {this.state.count}</button>
			</div>
		);
	}
}

export default ClassCounterTwo;

```

## `useState()` con Objeto
En las clases el `state` es un objeto, en los al trabajar con el hook `useState` veremos que el `state` no necesariamente debe ser un objeto sino que puede ser Number, String, Boolean, Array u Object.

Creamos un componente `HookCounterThree` que tiene un dos campos input uno para `firstName` y otro para `lastName`

```jsx
import React, { useState } from 'react';

function HookCounterThree() {
	const [name, setName] = useState({ firstName: '', lastName: '' });
	return (
		<div>
			<input
				type="text"
				value={name.firstName}
				onChange={(e) => setName({ firstName: e.target.value })}
			/>
			<input
				type="text"
				value={name.lastName}
				onChange={(e) => setName({ lastName: e.target.value })}
			/>
			<h2>Mi nombre es {name.firstName}</h2>
			<h2>Mi apellido es {name.lastName}</h2>
			<h2>{JSON.stringify(name)}</h2>
		</div>
	);
}

export default HookCounterThree;


```
Cuando ejecutamos este código veremos que mientras escribimos `firstName` el `h2` se actualiza de manera correcta, sin embargo al empezar a escribir el `lastName` veremos que el nombre desaparece. Esto obedece al hecho de haber usado `setName({firstName: e.target.value})` ya que estamos  en este caso pasamos a tener un objeto con una única propiedad.
Esto significa que a diferencia de lo visto al usar `setState`, el setter de `useState` no realiza el merge y la actualización automáticamente del estado, por lo que tendremos que hacerlo manualmente con el *spread operator*.
Con `{... name, firstName: e.target.value}` lo que hacemos es copiar todas las propiedades del objeto y luego sobre escribimos la que deseamos.
```jsx
import React, { useState } from 'react';

function HookCounterThree() {
	const [name, setName] = useState({ firstName: '', lastName: '' });
	return (
		<div>
			<input
				type="text"
				value={name.firstName}
				onChange={(e) => setName({ ...name, firstName: e.target.value })}
			/>
			<input
				type="text"
				value={name.lastName}
				onChange={(e) => setName({ ...name, lastName: e.target.value })}
			/>
			<h2>Mi nombre es {name.firstName}</h2>
			<h2>Mi apellido es {name.lastName}</h2>
			<h2>{JSON.stringify(name)}</h2>
		</div>
	);
}

export default HookCounterThree;

```

## `useState()`  con Array
Creamos el componente `HookCounterFour`y vamos a trabajar con un Array de objetos que inicialmente está vacío y contamos con un botón que al presionarlo pushea elementos en dicho array.
Basándonos en lo aprendido en el punto anterior donde vimos que la función setter no cuenta con hace el merge y actualización al trabajar con objetos, al trabajar con arrays verrmos que la función setter no agrega automáticamente el elemento al final del array sino que debemos hacerlo manualmente. Debemos agregarlo manualmente usando nuevamente el spread operator.

El `id` lo establecemos en base al `length` del array.
El `value` lo establecemos con `Math.floor(Math.random() * 10) + 1` obtenemos un número random entre 1 y 10, ya que `Math.floor(Math.random() * 10)` entrega un número entre 0 y 9.

```jsx
import React, { useState } from 'react';

function HookCounterFour() {
	const [items, setItems] = useState([]);

	const addItem = () => {
		setItems([
			...items,
			{
				id: items.length,
				value: Math.floor(Math.random() * 10) + 1,
			},
		]);
	};

	return (
		<div>
			<button onClick={addItem}>Add a Number</button>
			<ul>
				{items.map((item) => (
					<li key={item.id}>{item.value}</li>
				))}
			</ul>
		</div>
	);
}

export default HookCounterFour;

```