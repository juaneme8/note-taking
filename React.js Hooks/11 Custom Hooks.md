# Custom Hooks
Un *custom hook* es una función de JavaScript cuyo nombre comienza con `use`.
Hasta ahora estudiamos los hooks `useState`, `useEffect`, `useContext`, `useReducer`, `useCallback`, `useMemo`, `useRef` y sabemos que en definitiva los hooks son funciones que son invocadas dentro de los componentes. Es posible e incluso recomendable crear nuestros propios hooks, extrayendo lógica y colocándola en funciones reutilizables.
Un *custom hook* puede invocar a otros hooks en caso de necesitarlo, por lo que podremos utilizar en el *custom hook* los built-in hooks.
La motivación para utilizar *custom hooks* está dada por el hecho de poder compartir lógica entre componentes como alternativa más simple frente a **HOCs** o **render props**.

## Ejemplo `useDocumentTitle`
 El propósito es crear un *custom hook* que actualice el título del documento con el valor de un contador de clics de un botón. Primero lo haremos sin utilizar *custom hooks* y luego extraeremos la lógica usando uno. Comenzamos con `DocTitleOne`
```jsx
import React, { useState, useEffect } from 'react';

function DocTitleOne() {
	const [count, setCount] = useState(0);
	useEffect(() => {
		document.title = `Cuenta ${count}`;
	}, [count]);

	return (
		<div>
			<button onClick={() => setCount(count + 1)}>Cuenta - {count}</button>
		</div>
	);
}

export default DocTitleOne;

```

Supongamos ahora que debemos incorporar otro botón con funciones similares en otra parte de la aplicación, supongamos que creamos  `DocTitleTwo` y este tendrá exactamente el mismo código. Como queremos evitar la repetición este sería un buen caso para implementar un *custom hook*.
En `src` creamos una carpeta `hooks` y dentro de ella un archivo `useDocumentTitle` (por convención el nombre debe empezar con `use` y también ayuda a los linters a determinar si deben aplicar reglas de hooks).
En el componente donde queremos usar el hook simplmente lo llamamos con `useDocumentTitle(count)`

`App`
```jsx
import React from 'react';
import DocTitleOne from './components/DocTitleOne';
import DocTitleTwo from './components/DocTitleTwo';

function App() {
	return (
		<div>
			<DocTitleOne />
			<DocTitleTwo />
		</div>
	);
}

export default App;

```

`DocTitleOne` (`DocTitleTwo` es igual)
```jsx
import React, { useState } from 'react';
import useDocumentTitle from '../hooks/useDocumentTitle';
function DocTitleOne() {
	const [count, setCount] = useState(0);
	useDocumentTitle(count);

	return (
		<div>
			<button onClick={() => setCount(count + 1)}>Cuenta - {count}</button>
		</div>
	);
}

export default DocTitleOne;

```

`useDocumentTitle`
```jsx
import { useEffect } from 'react';

function useDocumentTitle(count) {
	useEffect(() => {
		document.title = `Cuenta ${count}`;
	}, [count]);
}

export default useDocumentTitle;

```

## Ejemplo `useCounter`
A continuación crearemos un *custom hook* llamado `useCounter` que nos permitirá reutilizar la lógica de un contador de los de 3 botones. 
Primero implementaremos la lógica como en el ejemplo anterior y luego extraeremos la lógica reutilizable en el *custom hook*.
Creamos el componente `Counter1` que cuenta con tres botones uno para incrementar, decrementar y resetear.

```jsx
import React, { useState } from 'react';

function Counter1() {
	const [count, setCount] = useState(0);
	const increment = () => {
		setCount(prevCount => prevCount + 1);
	};
	const decrement = () => {
		setCount(prevCount => prevCount - 1);
	};
	const reset = () => {
		setCount(0);
	};
	return (
		<div>
			<h2>Cuenta: {count}</h2>
			<button onClick={increment}>Incrementar</button>
			<button onClick={decrement}>Decrementar</button>
			<button onClick={reset}>Resetear</button>
		</div>
	);
}

export default Counter1;

```
Si tuviéramos que implementar esto mismo en otro componente, supongamos `Counter2` habría que duplicar la lógica, queremos evitar esto usando un *custom hook*.

Creamos el archivo `useCounter` en la carpeta `hooks` en el cual colocamos la lógica incluyendo en este caso las variables de estado y los métodos. Como necesitamos accederlos desde los componentes, en el custom hook retornamos un array con `count` y los tres métodos.

`App`
```jsx
import React from 'react';
import Counter1 from './components/Counter1';
import Counter2 from './components/Counter2';

function App() {
	return (
		<div>
			<Counter1 />
			<Counter2 />
		</div>
	);
}

export default App;

```

`Counter1` (`Counter2` es igual). 

Notar que usamos *array destructuring*
```jsx
import React, { useState } from 'react';
import useCounter from '../hooks/useCounter';

function Counter1() {
	const [count, increment, decrement, reset] = useCounter();
	return (
		<div>
			<h2>Cuenta: {count}</h2>
			<button onClick={increment}>Incrementar</button>
			<button onClick={decrement}>Decrementar</button>
			<button onClick={reset}>Resetear</button>
		</div>
	);
}

export default Counter1;

```

`useCounter`
```jsx
import { useState } from 'react';

function useCounter() {
	const [count, setCount] = useState(0);
	const increment = () => {
		setCount(prevCount => prevCount + 1);
	};
	const decrement = () => {
		setCount(prevCount => prevCount - 1);
	};
	const reset = () => {
		setCount(0);
	};
	return [count, increment, decrement, reset];
}

export default useCounter;

```

El uso de *custom hooks* también nos aporta bastante flexibilidad por ejemplo en este caso podríamos agregar un parámetro que sea el valor inicial del contador.

Con este parámetro `useCounter` nos queda:
```jsx
import { useState } from 'react';

function useCounter(initialCount = 0) {
	const [count, setCount] = useState(initialCount);
	const increment = () => {
		setCount(prevCount => prevCount + 1);
	};
	const decrement = () => {
		setCount(prevCount => prevCount - 1);
	};
	const reset = () => {
		setCount(0);
	};
	return [count, increment, decrement, reset];
}

export default useCounter;

```

Si no especificamos el valor inicial, usará 0, mientras que si ponemos por ejemplo: `const [count, increment, decrement, reset] = useCounter(72);` empezará en ese valor.

También podríamos colocar un parámetro que represente la cantidad en la que queremos aumentar o disminuir al presionar el botón. Es decir `function useCounter(initialCount = 0, value) {...}` y luego pondremos por ejemplo `const [count, increment, decrement, reset] = useCounter(72,10);` para tener incrementos de a 10 unidades.

## Ejemplo con `Input`
El propósito de este ejemplo es crear un formulario con los campos de texto `firstname` y `lastname` que al presionar Submit muestre un alerta con dichos elementos. 
Primero lo haremos sin utilizar hooks y luego utilizándolos.
Creamos el archivo `UserForm`

```jsx
import React, { useState } from 'react';

function UserForm() {
	const [firstName, setFirstName] = useState('');
	const [lastName, setLastName] = useState('');
	const handleSubmit = (e) => {
		e.preventDefault();
		alert(`Mi nombre es ${firstName} ${lastName}`);
	};
	return (
		<div>
			<form onSubmit={handleSubmit}>
				<div>
					<label>First Name</label>
					<input type="text" value={firstName} onChange={(e) => setFirstName(e.target.value)} />
				</div>
				<div>
					<label>Last Name</label>
					<input type="text" value={lastName} onChange={(e) => setLastName(e.target.value)} />
				</div>
				<button type="submit">Submit</button>
			</form>
		</div>
	);
}

export default UserForm;

```

Luego tenemos que crear un *custom hook* que encapsule al comportamiento de  *controlled component* del `input`
Esto lo hacemos creando primero `useInput`
```jsx
import { useState } from 'react';

function useInput(initialValue) {
	const [value, setValue] = useState(initialValue);

	const reset = () => {
		setValue(initialValue);
	};
	const bind = {
		value: value,
		onChange: (e) => {
			setValue(e.target.value);
		},
	};
	return [value, bind, reset];
}

export default useInput;

```

En tanto `userForm` nos queda:
```jsx
import React, { useState } from 'react';
import useInput from '../hooks/useInput';

function UserForm() {
	const [firstName, bindFirstName, resetFirstName] = useInput('');
	const [lastName, bindLastName, resetLastName] = useInput('');

	const handleSubmit = (e) => {
		e.preventDefault();
		alert(`Mi nombre es ${firstName} ${lastName}`);
		resetFirstName();
		resetLastName();
	};
	return (
		<div>
			<form onSubmit={handleSubmit}>
				<div>
					<label>First Name</label>
					<input type="text" {...bindFirstName} />
				</div>
				<div>
					<label>Last Name</label>
					<input type="text" {...bindLastName} />
				</div>
				<button type="submit">Submit</button>
			</form>
		</div>
	);
}

export default UserForm;

```
Notar el uso del *spread operator*por estar trabajando con los atributos. 