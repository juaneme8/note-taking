# useReducer
`useReducer` es un hook que se utiliza para el manejo de estado como alternativa a `useState` . De hecho, podemos decir que `useReducer` es un hook más primitivo, de hecho `useState` está hecho con `useReducer`.
Al final del capítulo podremos indicar cuando conviene usar uno por sobre el otro. 
Así como `useState` está relacionado con estado, `useEffect` está relacionado con efectos secundarios, `useContext` está relacionado con Context API, el hook `useReducer` está relacionado con **reducers**.

En JavaScript el método `reduce()` ejecuta una función **reductora** sobre cada elemento de un array, devolviendo como resultado un único valor.
```js
const array1 = [1, 2, 3, 4];
const reducer = (accumulator, currentValue) => accumulator + currentValue;

// 1 + 2 + 3 + 4
console.log(array1.reduce(reducer));
// expected output: 10

// 5 + 1 + 2 + 3 + 4
console.log(array1.reduce(reducer, 5));
// expected output: 15

```
Podemos ver que la segunda forma de `reduce()` acepta dos parámetros, siendo este último un valor inicial para el acumulador.

Existe una gran similitud entre este método y el hook `useReducer`:

1. `useReducer(reducer, initialState)`

2. `newState = reducer(currentState, action)` Siendo `action` lo que dicta la transición entre `currentState` y `newState`.

3. `useReducer` retorna un par de valores `[newState, dispatch]` Siendo `dispatch` un método que se usa para especificar la `action`

Resumiendo:
```js
useReducer(reducer, initialState);
reducer(currentState, action)
```

## Simple State & Action
Creamos un contador `CounterOne` (que sea capaz de incrementar, decrementar y resetear) para luego compararlo con el implementado anteriormente usando `useState`

En primer lugar en el `jsx` creamos los tres botones y como queremos mostrar el valor de `count` es que requerimos el uso del hook `useReducer`

1. Importamos `{useReducer}` de `'react`
`import React, { useReducer } from 'react';`

2. Al igual que los otros hooks `useReducer` es una función y debemos llamarlo **dentro del componente** funcional.
`useReducer(reducer,initialState);`

3. Como no tenemos definidos a `reducer` y `initialState` los definimos y esto lo hacemos **fuera del componente**.
En cuanto a `initialState` como el valor inicial será 0 ponemos
```jsx 
const initialState = 0;
```
En cuanto a `reducer` decimos que es una *arrow function* que acepta dos valores (el estado actual que llamamos `state` y `action`) y retorna uno `newState`
Por lo que podríamos expresarlo como:
```jsx
const reducer = (state, action) => {
	return newState;
}
```
De acuerdo al valor de `action` el reducer realiza la **transición de estado** necesaria. La convención para ejecutar código basados en `action` es mediante el uso de `switch` y se suele incluir un caso `default` que retorna el mismo `state`.

```js
const initialState = 0;
const reducer = (state, action) => {
	switch (action) {
		case 'increment':
			return state + 1;
		case 'decrement':
			return state - 1;
		case 'reset':
			return initialState;
		default:
			return state;
	}
};
```

4. Necesitamos ahora un valor de `count` para mostrar en el `jsx` y un modo de llamar a `reducer()` con la `action` apropiada y justamente es esto lo que retorna `useReducer()`.
Utilizando *array destructuring*:
```js
const [count, dispatch] = useReducer(reducer, initialState);
```
Con `dispatch` vamos a poder ejecutar el código para una `action` en particular.

5. Luego en los botones agregamos el `onClick` handler con `onClick={() => dispatch('increment')}`. 

Cuando presionamos el botón el state cambia y se produce el re-renderizado

En este caso decimos que esstamos usando `useReducer` con `state` y `action` simples, ya que el `state` es un valor numérico (y no un objeto) y el `action` un string (y no un objeto con la propiedad `type`)

```js
import React, { useReducer } from 'react';

const initialState = 0;
const reducer = (state, action) => {
	switch (action) {
		case 'increment':
			return state + 1;
		case 'decrement':
			return state - 1;
		case 'reset':
			return initialState;
		default:
			return state;
	}
};

function CounterOne() {
	const [count, dispatch] = useReducer(reducer, initialState);
	return (
		<div>
			<div>ConterOne: {count}</div>
			<button onClick={() => dispatch('increment')}>Increment</button>
			<button onClick={() => dispatch('decrement')}>Decrement</button>
			<button onClick={() => dispatch('reset')}>Reset</button>
		</div>
	);
}

export default CounterOne;
```

## Complex State & Action
Este patrón resultará familiar a los usuarios de Redux, ya que `state` será un objeto en lugar de un valor numérico y `action` será un objeto con una propiedad `type` en lugar de ser un string.
Para este ejemplo trabajamos con el componente `CounterTwo` y usamos como base el mismo código de `CounterOne`

1. Convertirmos el `state` numérico en un objeto por lo que en vez de tener `const initialState = 0` tenemos:
```js
const initialState = {
	firstCounter: 0,
};
```

2. En el `jsx` en lugar de renderizar `{count}` mostramos `{count.firstCounter}`

3. Convertimos el string `action` en un objeto con la propiedad `type` que es un string.
```js
<button onClick={() => dispatch({ type: 'increment' })}>Increment</button>
```
4. En el `reducer()` cambiar `action` por `action.type` en el switch
 y retornar el nuevo objeto `state` en lugar de un valor

El código completo nos queda:
```js
import React, { useReducer } from 'react';

const initialState = {
	firstCounter: 0,
};
const reducer = (state, action) => {
	switch (action.type) {
		case 'increment':
			return { firstCounter: state.firstCounter + 1 };
		case 'decrement':
			return { firstCounter: state.firstCounter - 1 };
		case 'reset':
			return initialState;
		default:
			return state;
	}
};

function CounterTwo() {
	const [count, dispatch] = useReducer(reducer, initialState);
	return (
		<div>
			<div>ConterTwo: {count.firstCounter}</div>
			<button onClick={() => dispatch({ type: 'increment' })}>Increment</button>
			<button onClick={() => dispatch({ type: 'decrement' })}>Decrement</button>
			<button onClick={() => dispatch({ type: 'reset' })}>Reset</button>
		</div>
	);
}

export default CounterTwo;

```

## Ventajas de que `action` sea un objeto
Un posible caso donde se aprovecharían las ventajas del patrón, en particular el hecho de que `action` sea un objeto sería si el contador no sólo vamos a querer incrementarlo por una unidad sino por un valor distinto. Esto es simple como `action` es un objeto podremos agregarle una propiedad `value` a dicho objeto.
Trabajamos con el componente `CounterTwoi`

En primer lugar a los botones iniciales les colocamos aparte de `type`, `value: 1`
```js
<button onClick={() => dispatch({ type: 'increment', value: 1 })}>
	Increment
</button>
```
Luego agregamos dos botones nuevos para incremenentar en 5 unidades y decrementar en 5 unidades:
```jsx
<button onClick={() => dispatch({ type: 'increment', value: 5 })}>
	Increment5
</button>
<button onClick={() => dispatch({ type: 'decrement', value: 5 })}>
	Decrement5
</button>
```
Por último en el switch en vez de trabajar con el valor hardcodeado de 1, usamos `action.value`
```js
import React, { useReducer } from 'react';

const initialState = {
	firstCounter: 0,
};
const reducer = (state, action) => {
	switch (action.type) {
		case 'increment':
			return { firstCounter: state.firstCounter + action.value };
		case 'decrement':
			return { firstCounter: state.firstCounter - action.value };
		case 'reset':
			return initialState;
		default:
			return state;
	}
};

function CounterTwoi() {
	const [count, dispatch] = useReducer(reducer, initialState);
	return (
		<div>
			<div>ConterTwoi: {count.firstCounter}</div>
			<button onClick={() => dispatch({ type: 'increment', value: 1 })}>
				Increment
			</button>
			<button onClick={() => dispatch({ type: 'decrement', value: 1 })}>
				Decrement
			</button>
			<button onClick={() => dispatch({ type: 'reset', value: 1 })}>
				Reset
			</button>

			<button onClick={() => dispatch({ type: 'increment', value: 5 })}>
				Increment5
			</button>
			<button onClick={() => dispatch({ type: 'decrement', value: 5 })}>
				Decrement5
			</button>
		</div>
	);
}

export default CounterTwoi;

```

## Ventajas de que state sea un objeto
Otro posible caso donde aprovecharíamos las ventajas del patrón, sería aprovechando que `state` es un objeto por ejemplo para mantener dos contadores. Trabajamos con el componente `CounterTwoii`

Primero modificamos `initialState` agregando la propiedad `secondCounter: 10`. Luego agregamos dos casos mas al `switch` uno para incrementar el nuevo contador y otro para decrementarlo.
**IMPORTANTE** Al retornar como ahora el estado tiene dos propiedades y solo estamos cambiando una, debemos hacer el merge del objeto, es decir por ejemplo `return { ...state, firstCounter: state.firstCounter + action.value };`


```jsx
import React, { useReducer } from 'react';

const initialState = {
	firstCounter: 0,
	secondCounter: 10,
};
const reducer = (state, action) => {
	switch (action.type) {
		case 'increment':
			return { ...state, firstCounter: state.firstCounter + action.value };
		case 'decrement':
			return { ...state, firstCounter: state.firstCounter - action.value };
		case 'increment2':
			return { ...state, secondCounter: state.secondCounter + action.value };
		case 'decrement2':
			return { ...state, secondCounter: state.secondCounter - action.value };
		case 'reset':
			return initialState;
		default:
			return state;
	}
};

function CounterTwoii() {
	const [count, dispatch] = useReducer(reducer, initialState);
	return (
		<div>
			<div>
				ConterTwoii firstCounter: {count.firstCounter}
				<button onClick={() => dispatch({ type: 'increment', value: 1 })}>
					Increment
				</button>
				<button onClick={() => dispatch({ type: 'decrement', value: 1 })}>
					Decrement
				</button>
				<button onClick={() => dispatch({ type: 'reset', value: 1 })}>
					Reset
				</button>
				<button onClick={() => dispatch({ type: 'increment', value: 5 })}>
					Increment5
				</button>
				<button onClick={() => dispatch({ type: 'decrement', value: 5 })}>
					Decrement5
				</button>
			</div>
			<div>
				ConterTwoii secondCounter: {count.secondCounter}
				<button onClick={() => dispatch({ type: 'increment2', value: 1 })}>
					Increment
				</button>
				<button onClick={() => dispatch({ type: 'decrement2', value: 1 })}>
					Decrement
				</button>
			</div>
		</div>
	);
}

export default CounterTwoii;

```

## Múltiples `useReducer`
Si bien en la sección anterior trabajamos con un objeto de `state` para trabajar con dos contadores colocando distintos `cases` en el `switch`. Una alternativa más simple sería usar múltiples hooks `useReducer` como veremos a continuación.
Trabajamos con el componente `CounterThree` y tomamos como punto de partida del código de `CounterOne` que es el contador base con **simple state & action**
Como el segundo contador tendrá las mismas transiciones de estado (increment, decrement, reset) podemos crear múltiples `useReducer` con el mismo `reducer`.
```js
const [count, dispatch] = useReducer(reducer, initialState);
const [countTwo, dispatchTwo] = useReducer(reducer, initialState);
```

El código completo nos queda:
```js
import React, { useReducer } from 'react';

const initialState = 0;
const reducer = (state, action) => {
	switch (action) {
		case 'increment':
			return state + 1;
		case 'decrement':
			return state - 1;
		case 'reset':
			return initialState;
		default:
			return state;
	}
};

function CounterThree() {
	const [count, dispatch] = useReducer(reducer, initialState);
	const [countTwo, dispatchTwo] = useReducer(reducer, initialState);
	return (
		<div>
			<div>
				<div>count: {count}</div>
				<button onClick={() => dispatch('increment')}>Increment</button>
				<button onClick={() => dispatch('decrement')}>Decrement</button>
				<button onClick={() => dispatch('reset')}>Reset</button>
			</div>
			<div>
				<div>countTwo: {countTwo}</div>
				<button onClick={() => dispatchTwo('increment')}>Increment</button>
				<button onClick={() => dispatchTwo('decrement')}>Decrement</button>
				<button onClick={() => dispatchTwo('reset')}>Reset</button>
			</div>
		</div>
	);
}

export default CounterThree;

```
De esta manera evitamos tener que hacer el merge con el *spread operator* (en caso de que el state sea un objeto) y también evitamos tener código duplicado dentro del `reducer`


# `useReducer` con `useContext`
Hasta ahora hemos visto ejemplos de `useReducer` para el **manejo local de estado** es decir a nivel componente, pero puede que queramos compartir estado entre componentes es decir realizar un **manejo global de estado**. Esto podemos lograrlo combinando `useReducer` con `useContext`

Consideramos una aplicación con `App` como *root component* que tiene como hijos `CompA`, `CompB` y `CompC`. Tenemos un único contador que debe ser accedido por los tres componentes. Usando conceptos de **React Basics** podríamos tener un contador global en `App`, pasar los métodos a los hijos como `props` y los hijos luego contar con *handlers* que llamen a las `props`. Sin embargo, en aplicaciones reales los componentes estarán muy anidados en el árbol de componentes por lo que pasar `props` por todos los componentes no es una buena idea.

Suponiendo entonces que tenemos el siguiente arbol de componentes: `App` es padre de `CompA`, `CompB` y `CompC`
A su vez `CompB` es padre de `CompD`
A su vez `CompC` es padre de `CompE` y `CompE` es padre de `CompF`

El requerimiento entonces es mantener el estado del contador en `App` y modificarlo en  `CompA`, `CompD` y `CompF` 

`App`
```jsx
import React, { useReducer } from 'react';
import CompA from './components/CompA';
import CompB from './components/CompB';
import CompC from './components/CompC';

export const CountContext = React.createContext();

const initialState = 0;
const reducer = (state, action) => {
	switch (action) {
		case 'increment':
			return state + 1;
		case 'decrement':
			return state - 1;
		case 'reset':
			return initialState;
		default:
			return state;
	}
};

function App() {
	const [count, dispatch] = useReducer(reducer, initialState);
	return (
		<CountContext.Provider value={{ countState: count, countDispatch: dispatch }}>
			<div className="App">
				Valor Contador: {count}
				<CompA />
				<CompB />
				<CompC />
			</div>
		</CountContext.Provider>
	);
}

export default App;

```

`CompA`
```jsx
import React, { useContext } from 'react';
import { CountContext } from '../App';

function CompA() {
	const countContext = useContext(CountContext);
	return (
		<div>
			CompA
			<button onClick={() => countContext.countDispatch('increment')}>Increment</button>
			<button onClick={() => countContext.countDispatch('decrement')}>Decrement</button>
			<button onClick={() => countContext.countDispatch('reset')}>Reset</button>
		</div>
	);
}

export default CompA;

```

`CompB`
```jsx
import React from 'react';
import CompD from './CompD';

function CompB() {
	return (
		<div>
			<CompD />
		</div>
	);
}

export default CompB;

```


`CompC`
```jsx
import React from 'react';
import CompE from './CompE';
function CompC() {
	return (
		<div>
			<CompE />
		</div>
	);
}

export default CompC;


```

`CompD`
```jsx
import React, { useContext } from 'react';
import { CountContext } from '../App';

function CompD() {
	const countContext = useContext(CountContext);
	return (
		<div>
			CompD
			<button onClick={() => countContext.countDispatch('increment')}>Increment</button>
			<button onClick={() => countContext.countDispatch('decrement')}>Decrement</button>
			<button onClick={() => countContext.countDispatch('reset')}>Reset</button>
		</div>
	);
}

export default CompD;

```

`CompE`
```jsx
import React from 'react';
import CompF from './CompF';
function CompE() {
	return (
		<div>
			<CompF />
		</div>
	);
}

export default CompE;

```

`CompF`
```jsx
import React, { useContext } from 'react';
import { CountContext } from '../App';

function CompF() {
	const countContext = useContext(CountContext);
	return (
		<div>
			CompF
			<button onClick={() => countContext.countDispatch('increment')}>Increment</button>
			<button onClick={() => countContext.countDispatch('decrement')}>Decrement</button>
			<button onClick={() => countContext.countDispatch('reset')}>Reset</button>
		</div>
	);
}

export default CompF;

```


## Fetching Data con `useReducer`
En primer lugar crearemos una aplicación usando `useEffect` y `useState` para luego compararla con una aplicación similar pero que usa `useEffect` y `useReducer`.
El requierimiento es el siguiente: Luego de montar el componente, debemos realizar un llamado a la API y mostrar mientras tanto un cartel "LOADING...", cuando los datos están disponibles ocultamos este cartel y mostramos el contenido del post obtenido. De igual forma en caso de un error ocultamos el "LOADING..." y mostramos un mensaje de error.

Instalamos `axios` con `npm i axios`

### `useEffect` y `useState`
En primer lugar trabajaremos con `DataFetchingOne` con `useEffect` y `useState` necesitaremos tres variables de estado (loading, error, post):

```jsx
const [loading, setLoading] = useState(true);
const [error, setError] = useState('');
const [post, setPost] = useState({});
```

```jsx
import React, { useState, useEffect } from 'react';
import axios from 'axios';

function DataFetchingOne() {
	const [loading, setLoading] = useState(true);
	const [error, setError] = useState('');
	const [post, setPost] = useState({});

	useEffect(() => {
		axios
			.get('https://jsonplaceholder.typicode.com/posts/1')
			.then(response => {
				setLoading(false);
				setError('');
				setPost(response.data);
			})
			.catch(error => {
				setLoading(false);
				setError('Ocurrio un error');
				setPost({});
			});
	}, []);

	if (error) {
		return <div>{error}</div>;
	} else {
		return <div>{loading ? 'LOADING' : post.title}</div>;
	}
}

export default DataFetchingOne;

```

### `useEffect` y `useReducer`
Utilizamos el componente `DataFetchinTwo`.

Trabajamos inicialmente con las mismas 3 variables: `const initialState = { loading: true, error: '', post: {} };` En cuanto a las transiciones podemos ver del ejemplo anterior que son dos: cuando los datos obtenidos son correctos y cuando se produce un error. 

```jsx
import React, { useEffect, useReducer } from 'react';
import axios from 'axios';

const initialState = { loading: true, error: '', post: {} };
const reducer = (state, action) => {
	switch (action.type) {
		case 'FETCH_SUCCESS':
			return { loading: false, error: '', post: action.payload };
		case 'FETCH_ERROR':
			return { loading: false, error: 'Ocurrio un Error', post: {} };
		default:
			return state;
	}
};

function DataFetchingTwo() {
	const [state, dispatch] = useReducer(reducer, initialState);
	useEffect(() => {
		axios
			.get('https://jsonplaceholder.typicode.com/posts/1')
			.then(response => {
				dispatch({ type: 'FETCH_SUCCESS', payload: response.data });
			})
			.catch(error => {
				dispatch({ type: 'FETCH_ERROR' });
			});
	}, []);
	if (state.error) {
		return <div>{state.error}</div>;
	} else {
		return <div>{state.loading ? 'LOADING' : state.post.title}</div>;
	}
}

export default DataFetchingTwo;

```

# `useReducer` vs `useState`
Como dijimos tanto `useReducer` como `useState` nos permiten realizar un manejo de estado, pero todavía no definimos cuando es conveniente utilizar una opción frente a la otra.

De acuerdo al **tipo de variable de estado**: si tenemos que manejar primitive types (Number, String o Boolean) `useState` es la mejor opción mientras que si tenemos que trabajar con Object o Array `useReducer`. Si usamos useState con objetos tendremos que usar el *spread operator*

De acuerdo al **número de transiciones de estado**: si tenemos que hacer una o dos conviene `useState` mientras que si tenemos que hacer muchas `useReducer`. Esto hace que nuestras transiciones sean predecibles ya que todas actualizaremos muchas cosas en un único lugar.

De acuerdo a **la relación entre las transiciones de estado**: si no están relacionadas usar `useState` y si lo están `useReducer`

De acuerdo a **la lógica de negocios de las transiciones de estado**: si no requiere lógica `useState` si requiere lógica compleja `useReducer`. De esta manera la lógica estará en el `reducer` y tendremos una buena separación de intereses, logrando así un código de más fácil lectura y mantenimiento.

De acuerdo a si necesitamos un **estado global o local** si necesitamos un estado local a nivel componente `useState` es una buena opción mientras que si queremos un estado global que pueda ser alterado por componentes anidados en el arbol de componentes `useReducer` es una mejor opción. Usa esto debido a que con `useReducer` tiene la ventaja de que sólo tendremos que pasar al arbol de componentes el `dispatch` mientras que con `useState` tendríamos que pasar múltiples update functions.