# Renderizado Condicional
En ocasiones vamos a querer mostrar u ocultar contenido de acuerdo a alguna condición, tenemos cuatro tipo de implementaciones de renderizado condicional:
* if/else
* variables
* ternary operator
* short circuit operator

Supongamos que queremos mostrar un mensaje de bienvenida que cambiará dependiendo de si el usuario está logueado o no. 
Creamos un componente de clase (ya que vamos a utilizar `state`) llamado `UserGreeting`.

## Utilizando `if/else` y múltiples `return`
```jsx
import React, { Component } from 'react';

class UserGreeting extends Component {
	constructor(props) {
		super(props);

		this.state = {
			isLogged: false,
		};
	}

	render() {
		if (this.state.isLoggedIn) {
			return <div>Bienvenido Juan</div>;
		} else {
			return <div>Bienvenido Visitante</div>;
		}
	}
}

export default UserGreeting;
```
En este caso podríamos no haber puesto el `else`.
Tener en cuenta que no podemos poner `if/else statements` dentro de `jxs` ya que como dijimos anteriormente `jsx` es *syntactic sugar* para llamados a función y construcción de objetos.

Esta técnica se podría complementar utilizando helper methods. Por ejemplo:
```jsx
renderWelcome(){
if (this.state.isLoggedIn) {
	return <div>Bienvenido Juan</div>;
} 
return <div>Bienvenido Visitante</div>;
}
render() {
	return(
		<div>
			{this.renderWelcome()}
		</div>
	);
}
```

## Utilizando `if/else` y  variables
Esta solución es mejor que la anterior y consiste en almacenar en elementos en variables y luego retornar la variable.

```jsx
import React, { Component } from 'react';

class UserGreeting extends Component {
	constructor(props) {
		super(props);

		this.state = {
			isLoggedIn: true,
		};
	}

	render() {
		let message;

		if (this.state.isLoggedIn) {
			message = <div>Bienvenido Juan</div>;
		} else {
			message = <div>Bienvenido Visitante</div>;
		}
		return message;
	}
}

export default UserGreeting;

```
### `if/else` y  variables con`map()`
```jsx
const Ninjas = ({ninjas}) => {
	const ninjaList = ninjas.map(ninja => {
		if(ninja.age > 20){
			return (
				<div className="ninja" key={ninja.id}>
					<div>(...)</div>
				</div>
			)
		}
		else{
			return null;
		}
	});
	return (
		<div className="ninja-list">{ninjaList}</div>
	)
}
```

## Utilizando el Ternary Operator
La mayoría de las veces utilizaremos este método que tiene como ventaja el hecho de que se puede utilizar dentro del return de `jsx`.
```jsx
import React, { Component } from 'react';

class UserGreeting extends Component {
	constructor(props) {
		super(props);

		this.state = {
			isLoggedIn: false,
		};
	}

	render() {
		return this.state.isLoggedIn ? <div>Bienvenido Juan</div> : <div>Bienvenido Visitante</div>;
	}
}

export default UserGreeting;

```

## Alternativa al ternary operator con dos `return` 
Normalmente tendremos un sólo `return` pero en ocasiones puede que nos convenga tener algo así:

```jsx
function App() {
	if(cond){
		return (
			//...
		)
	}
	return (
		//...
	)
}
```

### ternary operator y variables con `map()`
Es aconsejable tener la lógica separada y almacenada en una variable y luego la agregamos al template.
```jsx
const Ninjas = ({ninjas}) => {
	const ninjaList = ninjas.map(ninja => {
		return ninja.age > 20 ? (
		<div className="ninja" key={ninja.id}>
			<div>(...)</div>
		</div>
		):
		null;
	});
	return (
		<div className="ninja-list">{ninjaList}</div>
	)
}
```

## Utilizando el Short Circuit Operator
Se trata en definitiva de un caso particular del ternary operator es útil en aquellos casos que dependiendo de la condición queremos renderizar algo o no renderizar nada.
Supongamos entonces que quiero mostrar en pantalla `Bienvenido Juan` si `this.state.isLoggedIn` es `true` y no renderizar nada si es `false`.
Al colocar `return this.state.isLoggedIn && <div>Bienvenido Juan</div>;` evaluamos primero la expresión de la izquierda y si es verdadera, luego evaluamos la de la derecha que es `jsx` por lo que es verdadera.
```jsx
import React, { Component } from 'react';

class UserGreeting extends Component {
	constructor(props) {
		super(props);

		this.state = {
			isLoggedIn: true,
		};
	}

	render() {
		return this.state.isLoggedIn && <div>Bienvenido Juan</div>;
	}
}

export default UserGreeting;

```

Si tuviéramos `true && "Hello" && 1` el resultado será 1 

# Estilos Condicional
De la misma manera que tenemos el Renderizado Condicional utilizando el operador ternario por ejemplo, existe otro patrón gracias al cual es posible mostrar u ocultar elementos usando estilos en línea condicionados por el valor de una variable.
En html cuando utilizamos estilos en línea utilizamos un string`style="..."` mientras que en React debemos pasarle un objeto por lo que ponemos `style={{color: "red"}}` o si este valor lo recibimos mediante `props.color` podríamos poner:
`<h1 style={{color: props.color}}>{props.message}</h1>`

Por simplicidad el ejemplo lo mostramos usando hooks, pero se podría aplicar el mismo criterio sin ellos: 
```jsx

import { useState } from "react";
import "./styles.css";

export default function App() {
  const [loggedIn, setLoggedIn] = useState(false);

  const hideWhenLoggedIn = { display: loggedIn ? "none" : "" };
  const showWhenLoggedIn = { display: loggedIn ? "" : "none" };
  return (
    <>
      <div style={hideWhenLoggedIn}>
        <button onClick={() => setLoggedIn(true)}>Iniciar Sesión</button>
      </div>

      <div style={showWhenLoggedIn}>
        <button onClick={() => setLoggedIn(false)}>Cerrar Sesión</button>
      </div>
    </>
  );
}

```
En el renderizado condicional los elementos no forman parte del html mientras que en este caso de visibilidad condicional los elementos estarán en el HTML pero los ocultamos mediante estilos.

Esto que implementamos usando estilos en línea podría utilizarse también a la hora de definir las clases que afectan a ese elemento y sería una mejor práctica.