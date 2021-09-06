# Hook de Efecto
En los componentes de clase causamos efectos secundarios (como ser actualizar el DOM, obtener datos de un endpoint, setear timers, etc). Como hacerlo en el método `render()` es muy pronto es que debemos usar los métodos del ciclo de vida. 

## Introducción
Para explicar lo que motivacó la creación del hook de efecto supongamos primero que contamos con un contador que al presionarlo aumenta un el valor de cuenta. 
Si quisiéramos modificar el título del documento de acuerdo al valor actual, lo haríamos en primero en `componentDidMount()` para mostrar el valor inicial ya que se ejecuta sólo una vez y luego en `componentDidUpdate()` para mostrar el nuevo valor terminaríamos poniendo el mismo código.

```jsx
componentDidMount(){
	document.title = `You clicked ${this.state.count} times`;
}
componentDidUpdate(){
	document.title = `You clicked ${this.state.count} times`;
}
```
Podemos ver que **estamos escribiendo el mismo código dos veces**.

Ahora supongamos que queremos tener un timer que muestre cada cierto tiempo una palabra, en `componentDidMount()` lo seteamos y lo borramos cuando el componente está siendo eliminado lo en `componentWillUnmount()` 

```jsx
componentDidMount(){
	this.interval = setInterval(this.tick, 1000);
}
componentWillUnmount(){
	clearInterval(this.interval);
}
```
Si combinamos ambos efectos (ejemplo del DOM y el del timer) veremos que código relacionado al mismo timer está separado en distintos métodos del ciclo de vida y a su vez en `componentDidMount()` tenemos código no relacionado (actualizar el DOM y setear el timer) ubicado en el mismo bloque. El hook de efecto viene a solucionar estos dos problemas. Nos permite causar *side effects* en componentes funcionales, por lo que podemos pensarlo como el reemplazo de `componentDidMount`, `componentDidUpdate` y `componentWillUnmount`

## Implementación del hook de efecto
Queremos crear un contador con un botón que en el título de la página muestre la cantidad de veces que fue presionado. 

En primer lugar creamos un componente de clase `ClassCounterOne` para implementar esta funcionalidad:
```jsx
import React, { Component } from 'react';

class ClassCounterOne extends Component {
	constructor(props) {
		super(props);

		this.state = {
			count: 0,
		};
	}

	componentDidMount() {
		document.title = `Clickeado ${this.state.count} veces`;
	}
	componentDidUpdate() {
		document.title = `Clickeado ${this.state.count} veces`;
	}

	render() {
		return (
			<div>
				<button onClick={() => this.setState({ count: this.state.count + 1 })}>
					Incrementar Cuenta {this.state.count}
				</button>
			</div>
		);
	}
}

export default ClassCounterOne;

```

Ahora vamos a implementar esto mismo usando el hook de efecto:
```jsx
import React, { useState, useEffect } from 'react';

function HookCounterOne() {
	const [count, setCount] = useState(0);

	useEffect(() => {
		document.title = `Clickeaste ${count} veces`;
	});

	return (
		<div>
			<button onClick={() => setCount(count + 1)}>
				Incrementar Cuenta {count}
			</button>
		</div>
	);
}

export default HookCounterOne;

```
Notar en primer lugar que `useEffect` está ubicada dentro del componente (así podremos acceder al estado y props sin tener que escribir código adicional).
`useEffect()` se trata de una función por lo que debemos llamarla y lo hacemos pasánadole como argumento una función flecha que será ejecutada después de cada renderización.

## Ejecutar `useEffect` condicionalmente
El hecho de que `useEffect` se ejecute después de cada renderizado puede traer problemas de **performance** en ciertos casos, por lo que necesitamos una forma de ejecutar condicionalmente el hook de efecto.

Al igual que antes trabajaremos primero con componentes de clase. Tomando la base del contador `ClassCounterOne` le agregamos un `input` usando `value` y `onChange` de manera habitual. Veremos que si ingresamos un texto en este campo ingresa por cada tecla presionada a `componentDidUpdate` y ejecuta `document.title={this.state.count}` sin que haya cambiado `count`. Para evitar esto vamos a comparar el valor de `prevState.count` con el de `this.state.count` y sólo ejecutarlo cuando son distintos.

```jsx
import React, { Component } from 'react';

class ClassCounterOne extends Component {
	constructor(props) {
		super(props);

		this.state = {
			count: 0,
			name: '',
		};
	}

	componentDidMount() {
		document.title = `Clickeado ${this.state.count} veces`;
	}
	componentDidUpdate(prevProps, prevState) {
		if (prevState.count !== this.state.count) {
			console.log('componentDidUpdate');
			document.title = `Clickeado ${this.state.count} veces`;
		}
	}

	render() {
		return (
			<div>
				<input
					type="text"
					value={this.state.name}
					onChange={(e) => this.setState({ name: e.target.value })}
				></input>
				<button onClick={() => this.setState({ count: this.state.count + 1 })}>
					Incrementar Cuenta {this.state.count}
				</button>
			</div>
		);
	}
}

export default ClassCounterOne;

```
Ahora queremos implementar lo mismo usando componentes funcionales y el hook de efecto por lo que tomamos como base a `HookCounterOne` y agregamos el campo de texto.
Para ejecutar condicionalmente un efecto debemos pasarle a `useEffect()` un segundo parámetro que es un array (llamado *dependency array*) en el cual le pasamos props o state que deben cambiar para que se ejecute el efecto.
En este caso le ponemos  `useEffect(() => {...}, [count]);` ya que sólo queremos que se ejecute luego de renderizar si cambia el valor de `count` con lo cual lograremos una mejora en términos de **optimización**.

```jsx
import React, { useState, useEffect } from 'react';

function HookCounterOne() {
	const [count, setCount] = useState(0);
	const [name, setName] = useState('');

	useEffect(() => {
		console.log('useEffect');
		document.title = `Clickeaste ${count} veces`;
	}, [count]);

	return (
		<div>
			<input
				type="text"
				value={name}
				onChange={(e) => setName(e.target.value)}
			/>
			<button onClick={() => setCount(count + 1)}>
				Incrementar Cuenta {count}
			</button>
		</div>
	);
}

export default HookCounterOne;

```

## Ejecutar `useEffect` sólo una vez
Queremos una forma de ejecutar un efecto sólo una vez o en otras palabras emular el comportamiento de `componentDidMount` en componentes funcionales.

Trabajaremos primero con un componente de clase `ClassMouse` que queremos que muestre en pantalla las coordenadas `x` e `y` de la ubicación del mouse en pantalla. Para ello en `componentDidMount` establecemos un *event listener* 
`window.addEventListener('mousemove', this.logMousePosition);`


```jsx
import React, { Component } from 'react';

class ClassMouse extends Component {
	constructor(props) {
		super(props);

		this.state = {
			x: 0,
			y: 0,
		};
	}

	logMousePosition = (e) => {
		this.setState({ x: e.clientX, y: e.clientY });
	};
	componentDidMount() {
		window.addEventListener('mousemove', this.logMousePosition);
	}
	render() {
		return (
			<div>
				x: {this.state.x} - y: {this.state.y}
			</div>
		);
	}
}

export default ClassMouse;

```

Ahora queremos implementar esto en un componente funcional `HookMouse` y como queremos evitar que el efecto sea llamado después de cada renderizado, colocamos como segundo parámetro una array vacío: `useEffect(()=>{...},[])`

```jsx
import React, { useState, useEffect } from 'react';

function HookMouse() {
	const [x, setX] = useState(0);
	const [y, setY] = useState(0);

	useEffect(() => {
		console.log('useEffect called');
		window.addEventListener('mousemove', logMousePosition);
	}, []);

	const logMousePosition = (e) => {
	    console.log('Mouse Event');
		setX(e.clientX);
		setY(e.clientY);
	};

	return (
		<div>
			Hooks x: {x} - y: {y}
		</div>
	);
}

export default HookMouse;

```

## `useEffect` para cleanup
De la misma manera que anteriormente emulamos `componentDidMount` con `useEffect` ahora queremos emular `componentWillUnmount`.
Para entender la necesidad de esto supongamos que tenemos un componente contenedor que tiene un botón que al presionarlo muestra u oculta a `HookMouse` (con lo cual monta/desmonta del DOM al componente)

En este caso en `App` insertamos a `MouseContainer` y en `MouseContainer` tendremos lo siguiente:

```jsx
import React, { useState } from 'react';
import HookMouse from './HookMouse';

function MouseContainer() {
	const [display, setDisplay] = useState(true);
	return (
		<div>
			<button onClick={() => setDisplay(!display)}>Toggle Display</button>
			{display && <HookMouse />}
		</div>
	);
}

export default MouseContainer;
```

Veremos que al mover el mouse todo funciona del modo esperado (ya que inicialmente `display` es `true`) pero al presionar el botón y mover el mouse obtenemos un error: `Warning: Can't perform a React state update on an unmounted component. This is a no-op, but it indicates a memory leak in your application. To fix, cancel all subscriptions and asynchronous tasks in a useEffect cleanup function.`
A pesar del error seguiremos viendo `Mouse Event` en la consola, lo cual refuerza la idea de lo que está sucediendo. A pesar de que el componente fue eliminado, el event listener que pertenece a dicho componente sigue escuchando y ejecutando el código que a actualiza a `x` e `y`. Al desmontar un componte debemos asegurarnos de cancelar todas sus suscripciones, timers y event listeners (lo que se conoce como *clean up*).

En los componentes de clase esto lo hacemos de la siguiente forma:

```jsx
componentWillUnmount(){
	window.removeEventListener('mousemove', this.logMousePosition)
}
```

Para lograr esta funcionalidad con `useEffect` en la función que le pasamos como primer parámetro  debemos retornar una función y esta será ejecutada cuando el componente sea desmontado.

```jsx
import React, { useState, useEffect } from 'react';

function HookMouse() {
	const [x, setX] = useState(0);
	const [y, setY] = useState(0);

	const logMousePosition = (e) => {
		console.log('Mouse Event');
		setX(e.clientX);
		setY(e.clientY);
	};

	useEffect(() => {
		console.log('useEffect called');
		window.addEventListener('mousemove', logMousePosition);

		return () => {
			console.log('Component unmounting code');
			window.removeEventListener('mousemove', logMousePosition);
		};
	}, []);

	return (
		<div>
			Hooks x: {x} - y: {y}
		</div>
	);
}

export default HookMouse;

```

## Error común en Array de Dependencias
Supongamos que queremos desarrollar un contador que automáticamente se incrementa cada 1 segundo.
Primero trabajamos con un componente de clase `IntervalClassCounter`
Configuramos un timer de intervalo en `componentDidMount` que llama al método `this.tick()` e incrementa el contador en una unidad. Además los timers necesitan ser limpiados para evitar *memory leaks* y esto lo hacemos en `componentWillUnmount`. Finalmente en el método `render` mostramos el valor de `count`
```jsx
import React, { Component } from 'react';

class IntervalClassCounter extends Component {
	constructor(props) {
		super(props);

		this.state = {
			count: 0,
		};
	}

	componentDidMount() {
		this.interval = setInterval(this.tick, 1000);
	}

	componentWillUnmount() {
		clearInterval(this.interval);
	}

	tick = () => {
		this.setState({ count: this.state.count + 1 });
	};

	render() {
		return <div>{this.state.count}</div>;
	}
}

export default IntervalClassCounter;
```

Ahora vamos a implementar lo mismo con componentes funcionales y hooks en `IntervalHookCounter`
Como queremos que el intervalo se configure solo una vez podríamos estar tentados a poner `useEffect(() => {...}, []);` es decir con el array de dependencias vacío. Sin embargo si hiciéramos esto el contador permanecería en 1.
Esto se debe a que estamos pensando de la misma manera que lo hicimos para el componente de clase, es decir creando el intervalo una vez y destruyendolo una vez. No debemos pensar en el array de dependencias como que queremos que el efecto vuelva a ejecutarse sino debemos pensarlo como un modo de hacerle saber a React acerca de todos los cambios que debe estar atento. Al ponerle un array vacío le dijimos a React que ignore mirar cambios en `count`. Por lo que debemos poner ``useEffect(() => {...}, [count]);``
```jsx
import React, { useState, useEffect } from 'react';

function IntervalHookCounter() {
	const [count, setCount] = useState(0);

	const tick = () => {
		setCount(count + 1);
	};

	useEffect(() => {
		const interval = setInterval(tick, 1000);

		return () => {
			clearInterval(interval);
		};
	}, [count]);

	return <div>{count}</div>;
}

export default IntervalHookCounter;


```

Como alternativa existe otra forma de lograr que el contador funcione pasándole como lista de dependencias el array vacío y consiste en el modo en el que usamos el setter `setCount` en lugar de usarlo `setCount(count+1)` ponemos `setCount(prevCount =>prevCount+1)` 

```jsx
import React, { useState, useEffect } from 'react';

function IntervalHookCounter() {
	const [count, setCount] = useState(0);

	const tick = () => {
		setCount((prevCount) => prevCount + 1);
	};

	useEffect(() => {
		const interval = setInterval(tick, 1000);

		return () => {
			clearInterval(interval);
		};
	}, []);

	return <div>{count}</div>;
}

export default IntervalHookCounter;
```

## Llamado a funciones
En ocasiones vamos a querer llamar a funciones desde `useEffect` es conveniente en esos casos que las mismas esten definidas dentro de `useEffect`.

De lo caso contrario seria muy fácil olvidarnos que `someProp` es una dependencia y debe estar dentro del array de dependencias.

Por ejemplo en este caso es dificil de notar que `someProp` debe formar parte del array de dependencias:
```jsx
function doSomething(){
	console.log(someProp);
}

useEffect(() => {
		doSomething();
		const interval = setInterval(tick, 1000);

		return () => {
			clearInterval(interval);
		};
	}, [someProp]);
```

Mientras que en este caso es mucho mas fácil darnos cuenta:
```jsx
useEffect(() => {
		function doSomething(){
			console.log(someProp);
		}
		doSomething();
		const interval = setInterval(tick, 1000);

		return () => {
			clearInterval(interval);
		};
	}, [someProp]);
```

## Multiples Efectos
Recordemos que una de las motivaciones para la creacion de hooks fue el hecho de que en los componentes de clase el codigo relacionado esta en distintos métodos del ciclo de vida y código sin relación está en el mismo método. Los hooks solucionan esto ya que es posible incluir múltiples llamados a `useEffect` dentro de un mismo componente.

Como podemos ver en la documentacion de React: https://es.reactjs.org/docs/hooks-rules.html

```jsx
function Form() {
  const [name, setName] = useState('Mary');

  useEffect(function persistForm() {
    localStorage.setItem('formData', name);
  });

  const [surname, setSurname] = useState('Poppins');

  useEffect(function updateTitle() {
    document.title = name + ' ' + surname;
  });

  // ...
}
```

Notar que hemos organizado el código relacionado en ubicaciones cercanas de manera tal que todo luce mas organizado.