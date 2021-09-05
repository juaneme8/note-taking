# Event Handling
Las aplicaciones que desarrollemos tendrán interacción por parte de los usuarios y esto ocasionará determinados eventos: mouse click, mouse hover, keypress, etc. La aplicación deberá manejar estos eventos y ejecutar el código necesario.

## Event Handlers en Componentes Funcionales
Creamos un componente llamado `FunctionClick.js` y colocamos en el un botón. Al hacer click se dispara (*fire*) un click event y nuestro objetivo es atraparlo y ejecutar cierto código.

Veremos algunas diferencias respecto de lo que ocurre en Vanilla JS:
* Vamos a especificar los eventos en *camelCase*. `onClick=`
*  En lugar de un string le pasamos una función `onClick = {clickHandler}`.

Notar que ponemos una función `clickHandler` y no una llamada a función `clickHandler()` pues sino la llamaríamos automáticamente y luego no funcionaría al hacer click de verdad. Este mismo error en el caso de componentes de clase que modifican el `state` es todavía más grave ya que el componente constantemente se re-renderizará (ya que un cambio en el `state` hará que vuelva a renderizarse y nuevamente volverá a ejecutarse el handler)

```jsx
import React from 'react';

function FunctionClick() {
	function clickHandler() {
		console.log('Button Clicked');
	}
	return (
		<div>
			{/* */}
			<button onClick={clickHandler}>Click</button>
		</div>
	);
}

export default FunctionClick;

```

Podríamos haber usado una arrow function también

```jsx
import React from "react";

function FunctionClick() {
  const clickHandler = () => {
    console.log("Button Clicked");
  };
  return (
    <div>
      {/* */}
      <button onClick={clickHandler}>Click</button>
    </div>
  );
}

export default FunctionClick;

```

## Event Handlers en Componentes de Clase
Cuando trabajamos con componentes de clase manejamos los eventos de manera similar a cuando lo hacemos en componentes funcionales. Creamos el componente `ClassClick`
```jsx
import React, { Component } from 'react';

class ClassClick extends Component {
	clickHandler() {
		console.log('click');
	}
	render() {
		return (
			<div>
				<button onClick={this.clickHandler}>Click</button>
			</div>
		);
	}
}

export default ClassClick;
```
Si bien la implementación mostrada es muy sencilla, cuando en los event handlers intentamos modificar el estado usando `this.setState()` existe cierta confusión en torno al uso del `this` keyword.

## Binding Event Handlers
La razón por la que debemos hacer el `bind` de los *event handlers* es puramente por el modo en que funciona el `this` *keyword* en JavaScript.

Supongamos ahora que tenemos un botón y al hacer click sobre el, queremos cambiar un mensaje que tenemos almacenado en el `state`.

Si esto lo hacemos del mismo modo que venimos trabajando hasta ahora es decir indicando `onClick={this.handleClick}` e intentando en esa función utilizar `this.setState()` obtendremos `TypeError: Cannot read property 'setState' of undefined` De hecho si dentro de esa función hacemos `console.log(this)` veremos que vale `undefined`. Este es el comportamiento esperado en JavaScript (debería ser window pero en strict mode es undefined) por lo que será necesario hacer *event binding*. Esto podemos resolverlo de cuatro formas distintas:

En `render()` podemos utilizar el this keyword ya que por tratarse de un *built in method* react hace automáticamente el binding.

### 1) Binding en Método Render
La primera forma de hacerlo es poniendo `onClick={this.handleClick.bind(this)}`

```jsx
import React, { Component } from 'react';

class EventBind extends Component {
	constructor(props) {
		super(props);

		this.state = {
			message: 'Mensaje Inicial',
		};
	}

	handleClick() {
		console.log('click');
		this.setState({ message: 'Mensaje Final' });
	}

	render() {
		return (
			<div>
				<div>{this.state.message}</div>
				<button onClick={this.handleClick.bind(this)}>Click Bind</button>
			</div>
		);
	}
}

export default EventBind;
```
Si bien esta opción funciona bien, tiene el inconveniente de que cada actualización del `state` hace que el componente vuelva a renderizarse con lo cual generará un nuevo event handler en cada oportunidad. Si bien el impacto en cuanto a performance no es significativo en aplicaciones chicas, si puede serlo en aplicaciones de magnitud (con el rerenderizado de los componentes hijos anidados)

### 2) Arrow Functions en el método render
Una forma de hacerlo es utilizando arrow functions. Veremos que tenemos `onClick={() => this.clickHandler()}` en lugar de `onClick={this.clickHandler}` como teníamos hasta ahora. En este caso sí colocamos los paréntesis pues estamos llamando al event handler y retornando su valor.

```jsx
import React, { Component } from 'react';

class EventBind extends Component {
	constructor(props) {
		super(props);

		this.state = {
			message: 'Mensaje Inicial',
		};
	}

	handleClick() {
		console.log('click');
		console.log(this.state.message);
	}
	render() {
		return (
			<div>
				<button onClick={() => this.handleClick()}>Click Clase</button>
			</div>
		);
	}
}

export default EventBind;
```

Esta implementación también tiene inconvenientes en cuanto a performance en determinados escenarios. Si nuestro código no trabaja *rerendering de nested children components* sigue siendo una buena opción.
Sin embargo es el método más fácil de pasar parámetros. 

### 3) Binding en Constructor
Se trata de la implementación utilizada en la documentación de React. En ella volvemos a colocar `onClick={this.handleClick}` y en el constructor hacemos el binding `this.handleClick= this.handleClick.bind(this);`

```jsx
import React, { Component } from 'react';

class EventBind extends Component {
	constructor(props) {
		super(props);

		this.state = {
			message: 'Mensaje Inicial',
		};
		this.handleClick = this.handleClick.bind(this);
	}

	handleClick() {
		console.log('click');
		this.setState({ message: 'Mensaje Final' });
	}

	render() {
		return (
			<div>
				<div>{this.state.message}</div>
				<button onClick={this.handleClick}>Click Bind</button>
			</div>
		);
	}
}

export default EventBind;

```
En este caso como el binding sucede una única vez en el constructor por lo que es mejor que el binding en el método render.

### 4) Propiedad de la Clase como Arrow Function
En lugar de tener `handleClick() {}` tenemos `handleClick = () =>{}` se trata de una propiedad de la clase.

```jsx
handleClick = () => {
	console.log('click');
	this.setState({ message: 'Mensaje Final' });
};
```

El componente completo se verá así:
```jsx
import React, { Component } from 'react';

class EventBind extends Component {
	constructor(props) {
		super(props);

		this.state = {
			message: 'Mensaje Inicial',
		};
	}

	handleClick = () => {
		console.log('click');
		this.setState({ message: 'Mensaje Final' });
	};

	render() {
		return (
			<div>
				<div>{this.state.message}</div>
				<button onClick={this.handleClick}>Click Bind</button>
			</div>
		);
	}
}

export default EventBind;

```
Las arrow functions hacen el bind del `this` con lo que sea que haya fuera de esa función, en este caso con la instancia del componente.

# Raising and Handling Events
Debemos tener presenta la siguiente regla **el componente dueño de una porción de estado debe ser el encargado de modificarla**. Esto significa que si tenemos un componente padre `Counters` con un array de contadores y luego componentes hijos `Counter` con un botón para eliminarlos, debemos desde los hijos elevar el evento *raise an event* y en el padre serán eliminados *handle the event*. La *naming convention* dice que en el hijo deben ser del tipo `onDelete` y en el padre `handleDelete`
En `Counters` tendremos: `<Counter key={counter.id} onDelete={this.handleDelete} id={counter.id} (...)/>`
Luego en `Counter` tendremos un botón `<button onClick={() => this.props.onDelete(this.props.id}`