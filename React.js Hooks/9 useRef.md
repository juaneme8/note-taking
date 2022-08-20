# `useRef`
Este hook nos permite guardar en un objeto una referencia que no va a cambiar en los distintos renderizados que sufra ese componente. 

El hook `useRef` nos permite acceder a nodos del DOM de manera directa desde componentes funcionales. 

## Casos de Uso

Un ejemplo donde lo podríamos utilizar es si queremos trabajar con el método`focus()`en un `input` de texto. También podríamos querer utilizar esta referencia para obtener las características del navegador como su tamaño con `clientWidth` o escuchar un evento (no es la forma correcta de hacerlo pero puede que nos veamos forzados a hacerlo por algún motivo).

# Ejemplo 1
Supongamos que tenemos un formulario de login en el cual queremos que cuando carga la página obtener el foco puesto en el campo `username`.

Creamos un componente `FocusInput`
1. Importamos `{useRef}` con `import React, { useEffect, useRef } from 'react';`
2. Creamos una variable `ref` con valor inicial `null`: `const inputRef = useRef(null);`
3. Vinculamos la referencia al `input` utilizando el atributo `ref`: `<input type="text" ref={inputRef} />`
4. Luego con `useEffect` hacemos foco propiamente en el elemento

```jsx
import React, { useEffect, useRef } from 'react';

function FocusInput() {
	const inputRef = useRef(null);

	useEffect(() => {
		// focus
		inputRef.current.focus();
	}, []);
	return (
		<div>
			<input type="text" ref={inputRef} />
		</div>
	);
}

export default FocusInput;

```

# Ejemplo 2
Trabajaremos con un componente de clase `ClassTimer` el mismo muestra un contador que aumenta de valor cada 1 segundo, además cuenta con un botón que al presionarlo detiene la cuenta.
Notar que `interval` es una propiedad de clase

```jsx
import React, { Component } from 'react';

class ClassTimer extends Component {
	interval;
	constructor(props) {
		super(props);

		this.state = {
			timer: 0,
		};
	}

	componentDidMount() {
		this.interval = setInterval(() => {
			this.setState((prevState) => ({
				timer: prevState.timer + 1,
			}));
		}, 1000);
	}
	componentWillUnmount() {
		clearInterval(this.interval);
	}

	render() {
		return (
			<div>
				Class Timer - {this.state.timer}
				<button onClick={() => clearInterval(this.interval)}>Clear Timer</button>
			</div>
		);
	}
}

export default ClassTimer;

```

Supongamos ahora que queremos replicar este mismo funcionamiento pero utilizando un componente funcional `HookTimer`

Si hiciéramos lo siguiente:
```jsx
import React, { useState, useEffect, useRef } from 'react';

function HookTimer() {
	const [timer, setTimer] = useState(0);
	useEffect(() => {
		const interval = setInterval(() => {
			setTimer((prevTimer) => prevTimer + 1);
		}, 1000);
		return () => {
			clearInterval(interval);
		};
	}, []);

	return (
		<div>
			Hook Timer - {timer}
			<button onClick={() => clearInterval(interval)}>Clear Interval</button>
		</div>
	);
}

export default HookTimer;

```
Obtendríamos un error diciéndonos `'interval' is not defined  no-undef` que proviene de la línea `<button onClick={() => clearInterval(interval)}>Clear Interval</button>` esto se debe a que `interval` es accesible sólo dentro de `useEffect` por lo que podremos limpiar el intervalo dentro del hook pero no desde el event handler.
Para solucionar esto contamos con `useRef` ya que de la misma manera que puede almacenar una referencia a un nodo del DOM también puede almacenar cualquier valor (de manera similar a propiedades de instancia en un componente de clase). **Este valor será preservado durante los distintos re-reenderizados y no causará nuevos renderizados cuando el valor cambia.**

```jsx
import React, { useState, useEffect, useRef } from 'react';

function HookTimer() {
	const [timer, setTimer] = useState(0);
	const intervalRef = useRef();
	useEffect(() => {
		intervalRef.current = setInterval(() => {
			setTimer((prevTimer) => prevTimer + 1);
		}, 1000);
		return () => {
			clearInterval(intervalRef.current);
		};
	}, []);

	return (
		<div>
			Hook Timer - {timer}
			<button onClick={() => clearInterval(intervalRef.current)}>Clear Interval</button>
		</div>
	);
}

export default HookTimer;
```