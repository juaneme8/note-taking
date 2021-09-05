# Formularios
En HTML los `input`, `textarea`, `select`, etc **son responsables** de manejar la interacción provista por el usuario y actualizar sus valores. 
React en cambio se encargará de manejar a los elementos de los formularios y aquellos elementos cuyo valor es controlado por React reciben el nombre de **componentes controlados**.
Por ejemplo si tenemos un `<input type="text"/>` este tendrá un cierto `value` y un evento `onChange`. El valor del componente es determinado de acuerdo a la propiedad del `state` es decir `value= {this.state.email}` y `onChange={this.handleEmailChange}` ya que en `handleEmailChange` mediante `this.setState()` cambiaremos el `state` lo cual hará que el método `render()` sea llamado.

Creamos un componente de clase llamado `Form`

Notar que por lo previsto en **Event Handling** definimos el método como una propiedad de la clase, de manera que podamos acceder a `this.state`

Estamos trabajando con un **componente controlado** y el `state` es la única fuent ede verdad (*single source of truth*) del elemento.

Al hacer click en el botón de submit podemos acceder a todos los valores ingresados por el usuario los cuales mostramos en un `alert()`. Además el comportamiento default al hacer submit es refrescar la pantalla lo cual lo podemos evitar con `event.preventDefault()` en  `handleSubmit()`

```jsx
import React, { Component } from 'react';

class Form extends Component {
	constructor(props) {
		super(props);

		this.state = {
			username: '',
			comments: '',
			topic: 'react',
		};
	}

	handleUsernameChange = (event) => {
		this.setState({ username: event.target.value });
	};
	handleCommentsChange = (event) => {
		this.setState({ comments: event.target.value });
	};
	handleCTopicChange = (event) => {
		this.setState({ topic: event.target.value });
	};

	handleSubmit = (event) => {
		event.preventDefault();
		alert(`${this.state.username} ${this.state.comments} ${this.state.topic} `);
	};

	render() {
		return (
			<form onSubmit={this.handleSubmit}>
				<div>
					<label>Username</label>
					<input type="text" value={this.state.username} onChange={this.handleUsernameChange} />
				</div>
				<div>
					<label>Comments</label>
					<textarea type="text" value={this.state.comments} onChange={this.handleCommentsChange} />
				</div>
				<div>
					<label>Topic</label>
					<select value={this.state.topic} onChange={this.handleCTopicChange}>
						<option value="react">React</option>
						<option value="angular">angular</option>
						<option value="vue">vue</option>
					</select>
				</div>
				<button type="submit">Submit</button>
			</form>
		);
	}
}

export default Form;

```
En caso de no contar con el tag `<form>` sobre el cual escuchar el evento `onSubmit` podríamos colocar en el botón el evento `onClick` y el resto de la misma forma.
Al tener el tipo `type="submit"` permite enviar el formulario presionando la tecla ENTER lo cual es bueno en términos de UX.

Haciendo destructuring en `render()` nos daría una limpieza del código importante `const {username, comments, topic} = this.state`

> NOTA: En curso de The Net Ninja utiliza como valor inicial `null` y no `''`

> NOTA: En vez de utilizar tres handlers distintos para `username`, `comments` y `topic` podríamos haber utilizado sólo uno y el atributo `id` (que tendríamos que haberle asignado a `input`, `textarea` y `select`) y luego directamente hacer `[e.target.id]=e.target.value`  como veremos a continuación:

```jsx
handleValueChange = (event) => {
	this.setState({ [event.target.id]: event.target.value });
};
```