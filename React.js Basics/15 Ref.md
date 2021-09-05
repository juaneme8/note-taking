# Ref
En *VainillaJS* para obtener el valor de un campo`input` de un formulario haríamos `document.getElementById('username').value`. En React buscamos en cambio una abstracción de trabajar directamente sobre el DOM logrando así un mantenimiento más simple y *unit tests* más fáciles. 

Si es realmente necesario acceder a un elemento del DOM debemos darle una referencia.

Los referencias nos permiten acceder a nodos del DOM de manera directa desde React.  
Podemos hacerlo de dos formas una de ellas es con `React.createRef()` y la otra es con *callback refs*. 

> La API `React.createRef()` introducido en React 16.3 por lo que en caso de usar una versión anterior es recomendable usar callback refs.

## `Create.ref()`

### Generar focus en Input
Uno de los casos típicos de uso es al trabajar con formularios. Supongamos que tenemos un formulario de login y queremos que cuando la página cargue llamar a `focus()` en el `input` de `username`.

1. Es habitual **crear las refs el constructor** del componente de modo que podamos accederlos en todo el componente. Creamos una nueva propiedad `this.inputRef = React.createRef()`

2. **Vincular** el ref al elemento input en el método `render()` lo cual hacemos con `<input type="text" ref={this.inputRef} />`

3. Llamar al método `focus()` asociado al elemento input. Esto lo hacemos en `componentDidMount()` con `this.inputRef.current.focus();` 

```js
import React, { Component } from 'react';

class RefsDemo extends Component {
	constructor(props) {
		super(props);

		this.inputRef = React.createRef();
	}
	componentDidMount() {
		console.log(this.inputRef);
		this.inputRef.current.focus();
	}
	render() {
		return (
			<div>
				<input type="text" ref={this.inputRef} />
			</div>
		);
	}
}

export default RefsDemo;

```

### Obtener valor Input
Otro de los casos típicos de uso es para hacer un fetch del valor del `input`. Por ejemplo colocamos un botón que al clickearlo muestra un alert con el valor de dicho `input` lo cual logramos con `this.inputRef.current.value`

Debemos tratar de minimizar el uso de refs, pudiendo usarlo en situaciones como:
* manejar el focus de un input
* animaciones
* third party DOM libraries

## Callback refs
Vamos a repetir el ejemplo anterior en el cual teníamos que hacer foco sobre un input element utilizando `refs`.

Para utilizar **referencias de callback** debemos seguir 4 simples pasos:
1. Crear la referencia: lo hacemos también en el constructor creando una propiedad y asignándole el valor null `this.cbRef = null`
2. Crear un método como propiedad de la clase (para tener acceso a `this` ) que le asignará a la `ref` el elemento del dom. 
```js
this.setCbRef = (element) =>{
	this.cbRef = element;
}
```
3. Vincular el ref al elemento input `<input type="text" ref={this.setCbRef} />` Notar que el elemento es pasado de manera implícita como un parámetro.
4.  Tenemos que tener presente que React va a llamar al callback ref con el `elemento` del DOM cuando el componente se monta y con `null` cuando el componente se desmonta. Es por eso que chequeamos que `this.cbRef` no sea `null`
En `componentDidMount()` ejecutamos `this.cbRef.focus();` (notar que aca no aparece `current`)

```js
import React, { Component } from 'react';

class RefsDemo extends Component {
	constructor(props) {
		super(props);

		this.inputRef = React.createRef();

		this.cbRef = null;
		this.setCbRef = (element) => {
			this.cbRef = element;
		};
	}
	clickHandler = () => {
		alert(this.inputRef.current.value);
	};
	componentDidMount() {
		// console.log(this.inputRef);
		// this.inputRef.current.focus();
		if (this.cbRef) {
			this.cbRef.focus();
		}
	}
	render() {
		return (
			<div>
				<input type="text" ref={this.inputRef} />
				<input type="text" ref={this.setCbRef} />
				<button onClick={this.clickHandler}>Click me</button>
			</div>
		);
	}
}

export default RefsDemo;

```

# `Ref` a Componentes de Clase
Así como anteriormente agregamos `refs` a un elemento html (input) también es posible agregarlos a componentes de clase (no es posible hacer esto mismo en componentes funcionales). Es decir que por ejemplo en el componente padre tendremos un botón que al presionarlo, ocasione el focus en un input presente en el componente hijo.

Creamos un componente llamado `Input.js` que será el componente hijo y le asignamos una **referencia al input**, además el implementamos algo similar a lo que hicimos anteriormente pero en vez de trabajar con `componentDidMount()` lo hacemos mediante un método que será llamado por el componente padre.

A su vez en el padre `FocusInput` le asignamos una **referencia al componente hijo** y colocamos un botón que al presionarlo gracias a la referencia llamará al método del componente hijo.

Resumiendo tenemos referencias tanto en el padre como en el hijo. 

==Queda pendiente analizar porque no es una propiedad de la clase el método focusInput() ==

Código del componente padre `FocusInput`
```js
import React, { Component } from 'react';
import Input from './Input';

class FocusInput extends Component {
	constructor(props) {
		super(props);
		this.componentRef = React.createRef();
	}

	clickHandler = () => {
		this.componentRef.current.focusInput();
	};

	render() {
		return (
			<div>
				<Input ref={this.componentRef} />
				<button onClick={this.clickHandler}>Focus Input</button>
			</div>
		);
	}
}

export default FocusInput;

```

Código del componente hijo `Input`
```js
import React, { Component } from 'react';

class Input extends Component {
	constructor(props) {
		super(props);
		this.inputRef = React.createRef();
	}

	focusInput() {
		this.inputRef.current.focus();
	}

	render() {
		return (
			<div>
				<input ref={this.inputRef} />
			</div>
		);
	}
}

export default Input;

```

# Reenvío de refs
Se conoce como **ref forwading** a la técnica de pasar automáticamente una `ref` de un componente a un elemento de sus hijos.

Esto es algo que rara vez lo vamos a utilizar en nuestras aplicaciones. Sin embargo puede ser útil al trabajar con *libraries* o con HOC (*higher-order components*)
Creamos un componente funcional `FRInput` que será el hijo y tendrá un `input`.

Creamos un componente de clase `FRParentInput` que será el padre y tendrá un botón que cuando lo presionemos generará que el foco se establezca sobre el input del componente hijo.

A diferencia de la sección anterior cuando con el padre referenciábamos al componente de clase hijo, aca gracias a la técnica de ref forwading queremos desde el padre referenciar de manera directa al elemento nativo input del hijo.

En el componente hijo debemos usar el método `React.ForwardRef()` pero antes debemos convertir la función tradicional en una *arrow function*.
```jsx
function FRInput() {
	return (
		<div>
			<input type="text" />
		</div>
	);
}
```
Luego de convertirla a *arrow function*
```jsx
const FRInput = () => {
	return (
		<div>
			<input type="text" />
		</div>
	);
};
```
Luego le pasamos esta función a `React.forwardRef()` que toma un componente como parámetro por lo que le pasamos esa arrow function. Como primer parámetro recibimos como es habitual `props` y como segundo `ref` y usamos este valor y se lo pasamos como atributo ref al elemento nativo.

En el padre ejecutamos `this.inputRef.current.focus();` ya que como estamos implementando la técnica de reenvío de ref, `this.inputRef.current` apunta al elemento input nativo y no a la instancia `FrInput`

Resumiendo el padre crea una ref y la vincula al componente hijo. El hijo recibe esta ref y la vincula al elemento input nativo.

`FRParentInput` 
```jsx
import React, { Component } from 'react';
import FRInput from './FRInput';

class FRParentInput extends Component {
	constructor(props) {
		super(props);
		this.inputRef = React.createRef();
	}

	clickHandler = () => {
		this.inputRef.current.focus();
	};

	render() {
		return (
			<div>
				<FRInput ref={this.inputRef} />
				<button onClick={this.clickHandler}>Focus Input</button>
			</div>
		);
	}
}

export default FRParentInput;
```

`FRInput`
```jsx
import React from 'react';

const FRInput = React.forwardRef((props, ref) => {
	return (
		<div>
			<input type="text" ref={ref} />
		</div>
	);
});

export default FRInput;
```