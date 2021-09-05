# Pure Components
Hasta ahora creamos componentes de clase extendiendo la clase `Component` de `react` (con el snippet `rce`)
```js
import React, { Component } from 'react';

class ClassClick extends Component {...}
```

Sin embargo hay otra forma de hacerlo extendiendo la clase `PureComponent` de `react`

Creamos el archivo `PureComp` donde creamos el **Pure Component** (con el snippet `rpc`)

```js
import React, { PureComponent } from 'react'

class PureComponent extends PureComponent {...}
```

Para entender cual es la diferencia entre un **componente de clase**  y un **componente puro de clase** trabajaremos con un ejemplo. Tenemos un componente padre `ParentComp` que "cambia" su estado con un intervalo de 2 segundos en `componentDidMount()` y tiene dos componentes de clase hijos uno de cada tipo: `RegComp` y `PureComp`  que reciben dicho estado como `prop`.

Luego colocamos un `console.log()` en el método `render()` de los 3 componentes y veremos que el componente puro si no hay un cambio respecto del valor que tenía anteriormente el estado, no renderiza el componente. Esto significa que retorna `false` en `shouldComponentUpdate()`

`ParentCom.js`
```jsx
import React, { Component } from 'react';
import RegComp from './RegComp';
import PureComp from './PureComp';

class ParentComp extends Component {
	constructor(props) {
		super(props);

		this.state = {
			name: 'Juan',
		};
	}

	componentDidMount() {
		setInterval(() => {
			this.setState({ name: 'Juan' });
		}, 2000);
	}

	render() {
		console.log('ParentComp Render');
		return (
			<div>
				Parent Component
				<RegComp name={this.state.name} />
				<PureComp name={this.state.name} />
			</div>
		);
	}
}

export default ParentComp;

```

`PureComp`
```jsx
import React, { PureComponent } from 'react';

class PureComp extends PureComponent {
	render() {
		console.log('PureComp Render');
		return <div>Pure Component {this.props.name}</div>;
	}
}

export default PureComp;

```

`RegComp`
```jsx
import React, { Component } from 'react';

class RegComp extends Component {
	render() {
		console.log('RegComp Render');
		return <div>Regular Component {this.props.name}</div>;
	}
}

export default RegComp;

```

Al iniciar obtenemos:
1. ParentComp Render
2. RegComp Render
3. PureComp Render

Dos segundos después:
1. ParentComp Render
2. RegComp Render

Resumiendo los **componentes regulares** no implementan `shouldComponentUpdate` siempre retornan `true` mientras que los **componentes puros** implementan `shouldComponentUpdate(nextProps, nextState)` con una comparación (llamada *shallow comparison* o **SC**) de `props` y `state`

SC de `nextProps` con `props` 
SC de `nextState` con `state`
En la medida que haya diferencias realiza el re-renderizado.

En nuestro ejemplo estamos trabajando con *primitive types* (string) y efectuando una comparación de `props`.

## Shalow Comparison
Se trata de una comparación utilizando `===` o *strict equality*

**Primitive Types**
Para `Number`, `String`, `Boolean` una `a SC b` retorna `true` si tanto `a` como `b` tienen el mismo **valor** y son del mismo **tipo**.

**Complex Types:**
Para `Object` y `Array` una `a SC b` retorna `true`, si tanto `a` como `b` referencian al mismo objeto (en la misma posición de memoria).


# Padre Pure Component
Si al componente padre `ParentComp` lo convertimos en **Pure Component** veremos que nunca hace el re-render dado que efectúa la comparación del `state` y detecta que no hay diferencias. Si el padre no re-renderiza tampoco lo harán los hijos. 

Como conclusión podemos decir que los componentes puros impiden re-renders innecesarios pudiendo **aumentar la performance** en ciertos escenarios.

Debemos tener presente que no debemos mutar arrays u objetos en `props` o `state`. Si tenemos que agregar un item a la lista no debemos mutarla usando `.push()`, `.unshift()`, `.pop()`, `.shift()` ya que la referencia al array no cambiará por lo que no habrá renderizado. Al trabajar con componentes puros siempre retornar un nuevo objeto o array (usando `.concat()`, spread operator, `.filter()`.

Al crear un componente puro es una buena idea que todos los componentes hijos también lo sean para evitar un comportamiento inesperado.

Podemos decir que lo más aconsejable será usar componentes regulares a menos que queramos solucionar un tema de performance en algún componente.

# Memo
De la misma manera que contamos con los **pure components para los componentes de clase,** existen el concepto de **memo para los componentes funcionales**.
Trabajando con `ParentComp` que es un componente de clase regular (no es Pure Component) importamos a `MemoComp` inicialmente un componente funcional común y corriente.
```jsx
import React, { Component } from 'react';
import MemoComp from './MemoComp';

class ParentComp extends Component {
	constructor(props) {
		super(props);

		this.state = {
			name: 'Juan',
		};
	}

	componentDidMount() {
		setInterval(() => {
			this.setState({ name: 'Juan' });
		}, 2000);
	}

	render() {
		console.log('ParentComp Render');
		return (
			<div>
				Parent Component
				<MemoComp name={this.state.name} />
			</div>
		);
	}
}

export default ParentComp;

```
Veremos que cada dos segundos se produce el re-rendering del padre y del hijo. Como nuevamente como la `prop` recibida por `MemoComp` no está realmente cambiando cada 2 segundos podemos obtener un aumento de performance evitando el re-rendering. 

Debemos hacer `export default React.memo(MemoComp);` Luego de hacer esto, cada dos segundos sólo el padre hará el re-rendering. 

`React.memo(MemoComp);` se trata de HOC (*higher order component*) `React.memo()` acepta un componente al cual le agrega algunas característicos y entrega un nuevo componente con dichas mejoras.

`React.Memo()` fue incorporado en React v16.6, revisar en `package.json` que `react` y `react-dom` tengan una versión superior a esa.

En `MemoComp.js`
```js
import React from 'react';

function MemoComp({ name }) {
	console.log('Rendering Memo');
	return <div>{name}</div>;
}

export default React.memo(MemoComp);

```