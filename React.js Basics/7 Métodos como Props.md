# Métodos como Props
Hasta el momento vimos cómo pasar `props` del componente padre a los hijos. Sin embargo en ocasiones vamos a querer pasar datos de los hijos al padre y esto también lo haremos mediante `props`. El modo de hacerlo consistirá en pasarle una referencia a un método.

Planteamos una situación en la cual tenemos un componente  padre `<ParentComponent>` y un componente hijo `<ChildComponent>`. El hijo tiene un botón y queremos que cuando lo presione, el padre ejecute una función. 

El componente `ParentComponent.js` será de un componente de clase. En el constructor definimos en `state` con un mensaje y un método `greetParent()`

En `greetParent()` como estamos usando `this` debemos hacer el binding. Utilizamos la implementación **binding en el constructor**  por lo que ponemos `this.greetParent = this.greetParent.bind(this)`

```jsx
import React, { Component } from 'react';
import ChildComponent from './ChildComponent';

class ParentComponent extends Component {
	constructor(props) {
		super(props);

		this.state = {
			message: 'Hola',
		};

		this.greetParent = this.greetParent.bind(this);
	}

	greetParent() {
		alert(`El mensaje es:${this.state.message}`);
	}

	render() {
		return (
			<div>
				<ChildComponent greetHandler={this.greetParent} />
			</div>
		);
	}
}

export default ParentComponent;

```

Creamos un componente llamado `ChildComponent.js` y como este no tendrá estado, utilizo un componente funcional.

```jsx
import React from 'react';

function ChildComponent(props) {
	return (
		<div>
			<button onClick={props.greetHandler}>Greet Parent!</button>
		</div>
	);
}

export default ChildComponent;

```
## Pasar un parámetro al llamar el método del padre desde el hijo
El uso de *arrow functions* es la forma más fácil de pasar parámetros de un child component a un parent component, esto se conoce como **event arguments**
Entonces en `ChildComponent.js`:

```jsx
import React from 'react';

function ChildComponent(props) {
	return (
		<div>
			<button onClick={() => props.greetHandler('child')}>Greet Parent!</button>
		</div>
	);
}

export default ChildComponent;
```

En `ParentComponent.js` recibimos este parámetro `childName`
```jsx
import React, { Component } from 'react';
import ChildComponent from './ChildComponent';

class ParentComponent extends Component {
	constructor(props) {
		super(props);

		this.state = {
			parentName: 'parent',
		};

		this.greetParent = this.greetParent.bind(this);
	}

	greetParent(childName) {
		alert(`Hello ${this.state.parentName} from ${childName}`);
	}

	render() {
		return (
			<div>
				<ChildComponent greetHandler={this.greetParent} />
			</div>
		);
	}
}

export default ParentComponent;

```

Este esquema será particularmente útil cuando un el `ChildComponent` tengamos un formulario que al presionar submit agregue un nuevo elemento a un array en el padre. En este caso tendremos en el estado del hijo almacenado el valor y al presionar submit invocaremos directamente `props.addNinja(ninja)` siendo ninja una porción de estado de ese componente funcional.