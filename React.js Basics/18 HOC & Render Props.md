# HOC
Los Componentes de Orden Superior (*Higher Order Components*) son una técnica para reutilizar la lógica de componentes.

## Necesidad HOC
Supongamos que un cliente nos solicita implementar un botón que muestre en su texto la cantidad de veces que se le hizo click.
Esto podríamos hacerlo creando el componente de clase `ClickCounter` y cada vez que se presiona el botón incrementamos el `count` del `state`

```jsx
import React, { Component } from 'react';

class ClickCounter extends Component {
	constructor(props) {
		super(props);

		this.state = {
			count: 0,
		};
	}
	incrementCount = () => {
		this.setState((prevState) => {
			return { count: prevState.count + 1 };
		});
	};
	render() {
		const { count } = this.state;
		return (
			<div>
				<button onClick={this.incrementCount}>Clicked {count} times</button>
			</div>
		);
	}
}

export default ClickCounter;

```
Pero supongamos que ahora el cliente nos pide también que tengamos un heading que muestre la cantidad de veces que le hacemos *mouse hover*. Para eso creamos el componente `HoverCounter.js` y repetimos prácticamente mismo proceso que hicimos anteriormente.

```jsx
import React, { Component } from 'react';

class HoverCounter extends Component {
	constructor(props) {
		super(props);

		this.state = {
			count: 0,
		};
	}

	incrementCount = () => {
		this.setState((prevState) => {
			return { count: prevState.count + 1 };
		});
	};
	render() {
		const { count } = this.state;
		return (
			<div>
				<h2 onMouseOver={this.incrementCount}>Hover {count} times</h2>
			</div>
		);
	}
}

export default HoverCounter;

```

Supongamos que ahora nos pide lo mismo pero en un `input` mostrar la cantidad de teclas presionadas. Si bien podríamos implementarla del mismo modo que `ClickCounter` y `HoverCounter` ya comenzamos a notar que estamos duplicando código (la funcionalidad del contador) y no estamos reutilizando funcionalidades.
En este caso como son todos hijos del mismo componente padre, podríamos solucionar esto mediante levantar el `state` al componente padre y este nos pasa el handler como `prop`. Sin embargo en una situación donde estos componentes estén dispersos en el arbol de componentes esto no sería posible. Los HOC vienen a satisfacer esta necesidad de compartir funcionalidades entre componentes sin repetir código.

## Qué son los HOC
Los HOC son un patrón donde una función toma un componente como argumento y retorna un nuevo componente.
`const EnhancedComponent= HigherOrderComponent(OriginalComponent)`
Un HOC normalmente añade nuevos datos o funcionalidades al componente original, es por eso que se lo suele llamar *enhanced component*

Creamos el archivo `withCounter.js`. En el importamos `React` y creamos una *arrow function* `UpdatedComponent` que recibe un componente `OriginalComponent` y retorna un `NewComponent`. A su vez `NewComponent` es un componente de clase que renderiza al componente original con algún agregado. ==modificar nombres funciones==
```jsx
import React from 'react';

const withCounter= (WrappedComponent) => {
	class WithCounter extends React.Component {
		render() {
			return <WrappedComponent name="Vishwas"/>;
		}
	}
	return WithCounter;
};

export default withCounter;

```
A continuación en `ClickCounter` y `HoverCounter` en lugar de exportar el componente exportamos el HOC `export default UpdatedComponent(ClickCounter);` y `export default UpdatedComponent(HoverCounter);` respectivamente. Como consecuencia de esto en ambos componentes podremos renderizar la `prop` que le pasamos al HOC. Esto signfica que el HOC inyecta la propiedad `name` a cualquier componente que sea requerido.

Mostramos `ClickCounter` a modo de ejemplo ya que `HoverCounter` es prácticamente igual.
```jsx
import React, { Component } from 'react';
import withCounter from './withCounter';
class ClickCounter extends Component {
	constructor(props) {
		super(props);

		this.state = {
			count: 0,
		};
	}
	incrementCount = () => {
		this.setState((prevState) => {
			return { count: prevState.count + 1 };
		});
	};
	render() {
		const { count } = this.state;
		return (
			<div>
				<button onClick={this.incrementCount}>
					{this.props.name} Clicked {count} times
				</button>
			</div>
		);
	}
}

export default withCounter(ClickCounter);

```
Para solucionar nuestro problema queremos compartir la funcionalidad del contador entre los dos componentes. Para ello el HOC debe encargarse de mantener el `state` y la funcionalidad de incrementarlo y luego le pasa como props al componente mejorado el valor de `count` y el método `incrementCount`.

`withCounter.js`
```jsx
import React from 'react';

const withCounter = (WrappedComponent) => {
	class WithCounter extends React.Component {
		constructor(props) {
			super(props);

			this.state = {
				count: 0,
			};
		}
		incrementCount = () => {
			this.setState((prevState) => {
				return { count: prevState.count + 1 };
			});
		};
		render() {
			return <WrappedComponent count={this.state.count} incrementCount={this.incrementCount} />;
		}
	}
	return WithCounter;
};

export default withCounter;


```
`HoverCounter`
```jsx
import React, { Component } from 'react';
import withCounter from './withCounter';
class HoverCounter extends Component {
	render() {
		return (
			<div>
				<h2 onMouseOver={this.props.incrementCount}>Hover {this.props.count} times</h2>
			</div>
		);
	}
}

export default withCounter(HoverCounter);


```
`ClickCounter `
```jsx
import React, { Component } from 'react';
import withCounter from './withCounter';
class ClickCounter extends Component {
	render() {
		return (
			<div>
				<button onClick={this.props.incrementCount}>Clicked {this.props.count} times</button>
			</div>
		);
	}
}

export default withCounter(ClickCounter);

```

`App`
```jsx
import React, { Component } from 'react';
import ClickCounter from './components/ClickCounter';
import HoverCounter from './components/HoverCounter';

class App extends Component {
	render() {
		return (
			<div className="app">
				<ClickCounter />
				<HoverCounter />
			</div>
		);
	}
}

export default App;


```

### Pasaje de Props
Si en `App` le queremos pasar una prop a ClickCounter y lo hacemos con `<ClickCounter name="Vishwas"/>` veremos que esta no llega al componente deseado sino que le llegan al `HOC`.
Para solucionar esto debemos utilizar el *spread operator* desde el HOC para pasarle las `props` al *wrapped component* `{...this.props}`

`App.js`
```jsx
import React, { Component } from 'react';
import ClickCounter from './components/ClickCounter';
import HoverCounter from './components/HoverCounter';

class App extends Component {
	render() {
		return (
			<div className="app">
				<ClickCounter name="Vishwas" />
				<HoverCounter />
			</div>
		);
	}
}

export default App;

```
`withCounter.js`
```jsx
import React from 'react';

const withCounter = (WrappedComponent) => {
	class WithCounter extends React.Component {
		constructor(props) {
			super(props);

			this.state = {
				count: 0,
			};
		}
		incrementCount = () => {
			this.setState((prevState) => {
				return { count: prevState.count + 1 };
			});
		};
		render() {
			return <WrappedComponent count={this.state.count} incrementCount={this.incrementCount} {...this.props} />;
		}
	}
	return WithCounter;
};

export default withCounter;

```
`ClickCounter.js`
```jsx
import React, { Component } from 'react';
import withCounter from './withCounter';
class ClickCounter extends Component {
	render() {
		return (
			<div>
				<button onClick={this.props.incrementCount}>
					{this.props.name} Clicked {this.props.count} times
				</button>
			</div>
		);
	}
}

export default withCounter(ClickCounter);

```

### Pasaje de Parámetros al HOC
Supongamos que en luegar de incrementar la cuenta en una unidad quiero incrementar la cuenta en el valor determinado mediante un parámetro que recibe la función del HOC. Esto significa que la *arrow function* ahora tendrá dos parámetros `const withCounter = (WrappedComponent, incrementNumber) => {...}`

### HOC en Bibliotecas de Terceros
Los _HOCs_ son comunes en bibliotecas de terceros usadas en React, tales como `connect` en Redux, `createFragmentContainer` en Relay,  `withRouter` en React Router, `withStyles` en material UI.


# Render Props
El término `render prop` se refiere a una técnica utilizada para compartir código entre componentes usando una `prop` cuyo valor es una función.
Así como los **HOC** son una técnica para reutilizar la lógica de los componentes, **render props** también lo es.

## Necesidad de Render Props
 Supongamos una situación exactamente igual a la descripta al estudiar HOC donde un cliente nos pide primero un contador de clicks en un botón `ClickCounterTwo` y luego un contador de hover sobre un heading `HoverCountTwo`. En ambos casos tendríamos gran parte de la lógica duplicada.
> Si bien es muy parecido a lo hecho anteriormente copiamos el código sin optimizar a continuación porque será de utilidad más adelante para seguir el hilo.

`App.js`
```jsx
import React, { Component } from 'react';
import ClickCounterTwo from './components/ClickCounterTwo';
import HoverCounterTwo from './components/HoverCounterTwo';

class App extends Component {
	render() {
		return (
			<div className="app">
				<ClickCounterTwo />
				<HoverCounterTwo />
			</div>
		);
	}
}

export default App;

```

`ClickCounterTwo`
```jsx
import React, { Component } from 'react';

class ClickCounterTwo extends Component {
	constructor(props) {
		super(props);

		this.state = {
			count: 0,
		};
	}
	incrementCount = () => {
		this.setState((prevState) => {
			return { count: prevState.count + 1 };
		});
	};

	render() {
		return <button onClick={this.incrementCount}>Clicked {this.state.count} Times</button>;
	}
}

export default ClickCounterTwo;

```

`HoverCounterTwo`
```jsx
import React, { Component } from 'react';

class HoverCounterTwo extends Component {
	constructor(props) {
		super(props);

		this.state = {
			count: 0,
		};
	}
	incrementCount = () => {
		this.setState((prevState) => {
			return { count: prevState.count + 1 };
		});
	};
	render() {
		const { count } = this.state;
		return <h2 onMouseOver={this.incrementCount}>{count}</h2>;
	}
}

export default HoverCounterTwo;

```

Creamos un componente `User`
 1. En `App` simplemente ponemos `<User/>` y en el `render()` de `User` ponemos `return <div>Juan</div>;`
 2. En `App` le pasamos una `prop` con el nombre `<User name="Juan"/>` y en el `render()` de `User` ponemos `return <div>{this.props.name}</div>;`
3. En `App` le pasamos como prop una función que retorna un string `<User name={() => 'Juan'} />` y en el `render()` de `User` ponemos `return <div>{this.props.name()}</div>;` notar que `this.props.name` es una referencia a una función por lo que debemos llamarla.
4. Queremos ahora que la función que le pasamos como prop, reciba un parámetro y en función de su valor imprima una cosa u otra. En `App` `<User name={(isLoggedIn) => (isLoggedIn ? 'Juan' : 'Guest')} />` y en en `User` ponemos `return <div>{this.props.name(true)}</div>;`
5. Renombramos la prop `name` a `render` de modo tal que en `App` nos queda `<User render={(isLoggedIn) => (isLoggedIn ? 'Juan' : 'Guest')} />` y en en `User` nos queda`return <div>{this.props.render(true)}</div>;`

Dijimos que el término "render prop" es una técnica que nos permite compartir código entre componentes usando una prop cuyo valor es una función. Por el momento implementamos esta última parte con el componente `User`

Creamos un nuevo componente de clase `CounterTwo.js` que será el container donde implementaremos la funcionalidad en común.

`App`
```jsx
import React, { Component } from 'react';
import ClickCounterTwo from './components/ClickCounterTwo';
import HoverCounterTwo from './components/HoverCounterTwo';
import CounterTwo from './components/CounterTwo';

class App extends Component {
	render() {
		return (
			<div className="app">
				<CounterTwo render={(count, incrementCount) => <ClickCounterTwo count={count} incrementCount={incrementCount} />} />
				<CounterTwo render={(count, incrementCount) => <HoverCounterTwo count={count} incrementCount={incrementCount} />} />
			</div>
		);
	}
}

export default App;

```

La `prop` render como dijimos se trata de una función, por ese motivo la llamamos de modo de que renderize al componente que retorna. Le pasamos `count` e `incrementCount` de modo que el componente los reciba como props.
`CounterTwo`
```jsx
import React, { Component } from 'react';

class CounterTwo extends Component {
	constructor(props) {
		super(props);

		this.state = {
			count: 0,
		};
	}
	incrementCount = () => {
		this.setState((prevState) => {
			return { count: prevState.count + 1 };
		});
	};
	render() {
		return <div>{this.props.render(this.state.count, this.incrementCount)}</div>;
	}
}

export default CounterTwo;

```
Tanto en `ClickCounterTwo` como `HoverCounterTwo` recibimos via props el valor de `count` y la función para incrementar.

`ClickCounterTwo`
```jsx
import React, { Component } from 'react';

class ClickCounterTwo extends Component {
	render() {
		const { count, incrementCount } = this.props;
		return <button onClick={incrementCount}>Clicked {count} Times</button>;
	}
}

export default ClickCounterTwo;

```

`HoverCounterTwo`
```jsx
import React, { Component } from 'react';

class HoverCounterTwo extends Component {
	render() {
		const { count, incrementCount } = this.props;
		return <h2 onMouseOver={incrementCount}>{count}</h2>;
	}
}

export default HoverCounterTwo;

```

## Variación de la técnica
Debemos tener presente que no es necesario llamar a la `prop` `render` pero es la convención que se suele adoptar. Incluso existe una variación del patrón `render props` que no hace uso de dicha `prop` sino que usa `children` `prop`

En lugar de tener:
```jsx
<CounterTwo render={(count, incrementCount) => <HoverCounterTwo count={count} incrementCount={incrementCount} />} />
```

Tenemos
```jsx
<CounterTwo>
	{(count, incrementCount) => <HoverCounterTwo count={count} 	incrementCount={incrementCount} />}
</CounterTwo>
```

Finalmente en `CounterTwo` en lugar de llamar a `this.props.render()` llamamos a `this.props.children()`