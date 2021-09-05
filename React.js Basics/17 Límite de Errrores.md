# Error Boundary
En la fase de manejo de errores (o *error handling phase*) contamos con dos métodos del ciclo de vida (*life cycle methods*) que son `static getDerivedStateFromError(error)` y `componentDidCatch(error, info)`.

Hasta el momento vimos que los *runtime errors* rompen la aplicación y React básicamente desmonta todo el árbol de componentes. 

Un error de JavaScript en una parte de la interfaz no debería romper toda la aplicación. Para resolver este problema, React 16 introduce el nuevo concepto de **límite de errores**.

Los límites de errores son componentes de React que  capturan errores de JavaScript **en cualquier parte de su árbol de componentes hijo**, registran esos errores, y muestran una interfaz de repuesto (*fallback UI*)  en lugar del árbol de componentes que ha fallado. Los límites de errores capturan errores durante el renderizado, en métodos del ciclo de vida, y en constructores de todo el árbol bajo ellos. Debemos tener presente que no lo hacen dentro de **event handlers** por lo que si queremos capturar errores dentro de un *onClick handler* tendremos que hacerlo utilizando `try/catch`

Un componente de clase se convierte en límite de errores si implementa uno u ambos métodos del ciclo de vida: `static getDerivedStateFromError(error)` y `componentDidCatch(error, info)`.

`static getDerivedStateFromError(error)` se usa para mostrar una fallback UI luego de que ocurre un error.
`componentDidCatch(error, info)` se usa para loguear la información del error.

Creamos un componente funcional `Hero.js` que recibe una `prop` `heroName`. Queremos que cuando este valor recibido sea `Joker` tirar un error: `throw new Error('Not a hero');` Queremos que cuando este componente tire un error sólo el tenga una UI de repuesto y que no afecte a los componentes restantes.

Creamos un componente de clase `ErrorBoundary.js`y en el implementamos `static getDerivedStateFromError(error)` y este debe retornar el nuevo objeto `state` con `hasError` en `true`. En el método `render()` si hubo error retorno un mensaje especial y si no lo hubo retorno `this.props.children` es decir los componentes `Hero`

Por último en `App.js` utilizamos tanto al componente `ErorBoundary` como a los componentes `Hero` de la siguiente forma:
```jsx
<ErrorBoundary>
	<Hero heroName="Batman" />
	<Hero heroName="Joker" />
</ErrorBoundary>
```

Luego de hacer esto aún seguiremos viendo el error y esto se debe a que los límites de errores tienen su motivación para producción y en la fase de desarrollo el equipo de React intenta que los errores sean lo más visibles posibles. Sin embargo si cerramos haciendo click en la crucecita de la parte superior veremos que el comportamiento fue el esperado.

`Hero.js`
```jsx
import React from 'react';

function Hero({ heroName }) {
	if (heroName === 'Joker') throw new Error('Not a hero');
	return <div>{heroName}</div>;
}

export default Hero;
```

`ErrorBoundary.js`
```jsx
import React, { Component } from 'react';

class ErrorBoundary extends Component {
	constructor(props) {
		super(props);

		this.state = {
			hasError: false,
		};
	}

	static getDerivedStateFromError(error) {
		return { hasError: true };
	}

	render() {
		if (this.state.hasError) {
			return <h1>Something went wrong!</h1>;
		}
		return this.props.children;
	}
}

export default ErrorBoundary;

```

`App.js`
```jsx
import React, { Component } from 'react';
import ErrorBoundary from './components/ErrorBoundary';
import Hero from './components/Hero';

class App extends Component {
	render() {
		return (
			<div className="app">
				<ErrorBoundary>
					<Hero heroName="Batman" />
					<Hero heroName="Superman" />
					<Hero heroName="Joker" />
				</ErrorBoundary>
			</div>
		);
	}
}

export default App;

```
Un punto importante relacionado con los límites de errores es donde ubicarlos. En nuestro ejemplo `ErrorBoundary` envuelve a todos los `Hero`, por lo que ante un error ninguno de los `Hero` se mostrará y veremos la UI de repuesto. Imaginemos que tenemos un sitio de ecommerce con 1000 producto y por un error en uno no mostramos los otros 999 sin error. Es por eso que vamos a envolver cada `Hero` con `ErrorBoundary`

Pasamos a tener:
```jsx
<ErrorBoundary>
	<Hero heroName="Batman" />
</ErrorBoundary>
<ErrorBoundary>
	<Hero heroName="Superman" />
</ErrorBoundary>
<ErrorBoundary>
	<Hero heroName="Joker" />
</ErrorBoundary>
```
Por lo que ante un error podemos ver aquellos componentes que no fallaron.

En cuanto a `componentDidCatch(error, info)` como dijimos se utiliza para loguear los errores pero en development React se encarga de loguear los errores en la consola de manera automática por lo que en caso de implementarlo del siguiente modo:
```jsx
componentDidCatch(error, info) {
		console.log(error);
		console.log(info);
	}
```
Veremos la misma información que si no lo hubieramos hecho.