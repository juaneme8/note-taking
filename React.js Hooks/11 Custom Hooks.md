# Custom Hooks

> **BootCamp midudev:** [video 1](https://youtu.be/K4vCTeKKCkU) - [video 2](https://youtu.be/1zYf4Yw1jqs) - [video 3](https://youtu.be/aCVlqh1pQF4?list=PLV8x_i1fqBw0Kn_fBIZTa3wS_VZAqddX7)

Un *custom hook* es una función de JavaScript cuyo nombre comienza con `use`, React utiliza esta notación para determinar que es un custom hook y de esta manera realizar algunas cosas detrás de escenas (guardar cosas en memoria, optimizar, etc)
Hasta ahora estudiamos los hooks `useState`, `useEffect`, `useContext`, `useReducer`, `useCallback`, `useMemo`, `useRef` y sabemos que en definitiva los hooks son funciones que son invocadas dentro de los componentes. Es posible e incluso recomendable crear nuestros propios hooks, extrayendo lógica y colocándola en funciones reutilizables.

Un *custom hook* puede invocar a otros hooks en caso de necesitarlo, por lo que podremos utilizar en el *custom hook* los built-in hooks.

La motivación para utilizar *custom hooks* está dada por el hecho de poder compartir lógica entre componentes como alternativa más simple frente a **HOCs** o **render props**

* Los custom hooks permiten que las aplicaciones sean más **mantenibles y escalables**, ya que vamos a **reutilizar la lógica**.
* Los custom hooks pueden someterse a tests.

* Debemos tratar de utilizar los `useEffect` lo menos posible dentro de los componentes, por eso cada vez que veamos uno debemos preguntarnos si no podría implementarse la misma solución en un custom hook. Recordemos que dentro de un custom hook podemos usar otros hooks (en este caso `useEffect`).
* No debemos darles nombres que denoten la implementación por ejemplo en lugar de usar `useApiFetchNotes` debemos usar `useNotes`



> :link: En la página https://usehooks.com/ podremos encontrar varios ejemplos de custom hooks.



## Ejemplo Contador

Suponemos un caso en el que tenemos un contador simple:

```javascript
import { useState } from "react";

const Counter = () => {
  const [counter, setCounter] = useState(0);

  return (
    <div>
      {counter}
      <button onClick={() => setCounter(counter + 1)}>+</button>
      <button onClick={() => setCounter(counter - 1)}>-</button>
      <button onClick={() => setCounter(0)}>zero</button>
    </div>
  );
};

export default Counter;
```

Podemos ver que **la lógica y la visualización están juntas**, por lo que si quisiéramos implementar otro contador tendríamos que duplicar gran parte del código.

Lo que hacemos en cambio será crear el custom hook `useCounter` con la finalidad de desacoplar la parte visual de la gestión del estado.

```javascript
import { useState } from "react";

const useCounter = () => {
  const [counter, setCounter] = useState(0);

  const increment = () => setCounter(counter + 1);
  const decrement = () => setCounter(counter - 1);
  const reset = () => setCounter(0);

  return { counter, increment, decrement, reset };
};

const Counter = () => {
  const { counter, increment, decrement, reset } = useCounter();

  return (
    <div>
      {counter}
      <button onClick={increment}>+</button>
      <button onClick={decrement}>-</button>
      <button onClick={reset}>zero</button>
    </div>
  );
};

export default Counter;
```

Tenemos tres operaciones de gestión del estado. 

Notar que no devolvemos el `setCounter` para **ocultar la implementación** (el modo en que actualizamos nuestro estado). No sería posible que el usuario lo cambiar al valor que le diera la gana.



Suponemos un escenario en el que tenemos dos contadores:

```javascript
import { useState } from "react";

const useCounter = () => {
  const [counter, setCounter] = useState(0);

  const increment = () => setCounter(counter + 1);
  const decrement = () => setCounter(counter - 1);
  const reset = () => setCounter(0);

  return { counter, increment, decrement, reset };
};

const Counter = () => {
  const counterA = useCounter();
  const counterB = useCounter();

  return (
    <div>
      <div>
        {counterA.counter}
        <button onClick={counterA.increment}>+</button>
        <button onClick={counterA.decrement}>-</button>
        <button onClick={counterA.reset}>zero</button>
      </div>
      <div>
        {counterB.counter}
        <button onClick={counterB.increment}>+</button>
        <button onClick={counterB.decrement}>-</button>
        <button onClick={counterB.reset}>zero</button>
      </div>
    </div>
  );
};

export default Counter;
```



También podríamos pasarle un valor inicial al custom hook (con un valor por defecto que será usado en caso de que no le pasemos ninguno)

```js
import { useState } from "react";

const useCounter = (initialValue = 0) => {
  const [counter, setCounter] = useState(initialValue);

  const increment = () => setCounter(counter + 1);
  const decrement = () => setCounter(counter - 1);
  const reset = () => setCounter(0);

  return { counter, increment, decrement, reset };
};

const Counter = () => {
  const { counter, increment, decrement, reset } = useCounter(99);

  return (
    <div>
      {counter}
      <button onClick={increment}>+</button>
      <button onClick={decrement}>-</button>
      <button onClick={reset}>zero</button>
    </div>
  );
};

export default Counter;
```

> En este caso el valor inicial es 99 y el valor por defecto es 0.



## Estructura Hooks

Los custom hooks debemos colocarlos en una carpeta `hooks` por ejemplo para lo visto anteriormente creamos `useCounter.js`

```js
import { useState } from "react";

const useCounter = (initialValue = 0) => {
  const [counter, setCounter] = useState(initialValue);

  const increment = () => setCounter(counter + 1);
  const decrement = () => setCounter(counter - 1);
  const reset = () => setCounter(0);

  return { counter, increment, decrement, reset };
};

export { useCounter };

```



Luego lo importamos como `import { useCounter } from "./hooks/useCounter";`



## Custom Hooks para Formularios

Podríamos crear un custom hook `useField` que le pasemos como valor inicial el `type` (text, password, number, etc) y se encargue de devolvernos un objeto con las propiedades `value`, `type`, ``onChange`, etc para manejar dicho campo.

> :link: Si queremos opciones mucho más potentes con validaciones y otros beneficios podemos usar [react-hook-form.com](react-hook-form.com) o [formik](https://formik.org/).



Por ejemplo en un caso donde queremos manejar dos inputs username y password, podríamos pasar de esto:

```js
import { useState } from "react";

const Form = () => {
  const [username, setUsername] = useState("");
  const [password, setPassword] = useState("");

  const handleChangeUsername = (e) => {
    setUsername(e.target.value);
  };
  const handleChangePassword = (e) => {
    setPassword(e.target.value);
  };

  return (
    <div>
    <div>
      <label htmlFor="username">Username: </label>
      <input
        id="username"
        type="text"
        name="username"
        value={username}
        onChange={handleChangeUsername}
      />
    </div>
    <div>
      <label htmlFor="password">Password: </label>
      <input
        id="password"
        type="password"
        name="password"
        value={password}
        onChange={handleChangePassword}
      />
    </div>
    </div>
  );
};

export default Form;
```



Lo mismo podría ser refactorizado utilizando custom hooks de la siguiente manera:

```js
import { useState } from "react";

const useField = ({ type }) => {
  const [value, setValue] = useState("");

  const onChange = (e) => {
    setValue(e.target.value);
  };

  return {
    type,
    value,
    onChange
  };
};

const Form = () => {
  const username = useField({ type: "text" });
  const password = useField({ type: "password" });

  return (
    <div>
      <div>
        <label htmlFor="username">Username: </label>
        <input id="username" name="username" {...username} />
      </div>
      <div>
        <label htmlFor="password">Password: </label>
        <input id="password" name="password" {...password} />
      </div>
    </div>
  );
};

export default Form;
```

> Notar que usamos el *spread operator* `{...username} ` porque hubiera sido lo mismo que poner `type: {username.type} value: {username.value} onChange:{username.onChange}` 



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
Notar el uso del *spread operator* por estar trabajando con los atributos. 