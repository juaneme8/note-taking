# state
Las `props` son **inmutables** esto significa que su valor lo determina el padre y no puede ser cambiado por el hijo. Lo cual nos lleva a preguntarnos como podemos mantener datos de componentes que pueden cambiar con el tiempo y la respuesta es mediante `state`. El `state` es manejado dentro del componente y puede ser cambiado. En componentes funcionales lo hacemos mediante el *hook* `useState` y en componentes de clase mediante `this.state`.

De esta manera describimos el estado de un componente tanto **en cuanto a datos como en cuanto a UI** (modal open o modal close)

## `state` en Componentes de Clase

En el siguiente ejemplo creamos el componente `Message.js` y queremos tener un botón que muestre un mensaje u otro dependiendo de si fue presionado o no.

> Para el manejo del evento `onClick` podemos ver que le pasamos una *arrow function* y el motivo por el cual lo hacemos será visto al estudiar *event handling*. 
> Otra opción sería poner `onClick={this.changeMessage}` y trabajar con una propiedad de la clase `changeMessage = () => {...}`

### 3) `state` definido en el constructor
 Debemos crear el objeto `state` e inicializarlo, esto usualmente lo hacemos en el `constructor()` de la clase y también debemos llamar al método `super()` ya que estamos extendiendo la clase `Component`. Podemos utilizar el snippet `rconst`.
 Luego para alterar el estado siempre debemos llamar a `this.setState()` con el objeto nuevo.

```jsx
import React, { Component } from 'react';

export default class Message extends Component {
	constructor() {
		super();
		this.state = {
			message: 'Welcome Visitor',
		};
	}
	changeMessage() {
		this.setState({ message: 'Thanks for subscribing' });
	}

	render() {
		return (
			<div>
				<h1>{this.state.message}</h1>
				<button onClick={() => this.changeMessage()}>Subscribe</button>
			</div>
		);
	}
}
```
Cuando presionamos el botón cambia el `state` por lo que el componente vuelve a renderizarse y mostrará el nuevo mensaje.
Al igual que con las `props` es posible hacer destructuring de las propiedades del `state` y lo hacemos también dentro del método `render()`  por lo que tendríamos:
```jsx
render() {
		const {message} = this.state;
		return (
			<div>
				<h1>{message}</h1>
				<button onClick={() => this.changeMessage()}>Subscribe</button>
			</div>
		);
	}
```

### 2) `state` definido como propiedad
Otra forma de crear el objeto `state` es definiendo una propiedad `state` dentro del componente.
```jsx
import React, { Component } from "react";

export default class Message extends Component {
  state = {
    message: "Welcome Visitor"
  };
  changeMessage() {
    this.setState({ message: "Thanks for subscribing" });
  }

  render() {
    return (
      <div>
        <h1>{this.state.message}</h1>
        <button onClick={() => this.changeMessage()}>Subscribe</button>
      </div>
    );
  }
}

```

### `setState`
Creamos un componente `<Counter>` el cual será un componente de clase, luego queremos inicializar `state` en el constructor.

Si intentáramos incrementar `this.state.count` sin hacer uso de `this.setState` (es decir por ejemplo con `this.state.count = this.state.count+1`) veríamos que el valor no se refleja en el UI mientras que en un eventual `console.log` luego de modificarlo sí lo veríamos. Esto nos demuestra que no se está produciendo el rerendering esperado por lo que **nunca debemos modificar el state de manera directa**.
```jsx
increment() {
	this.state.count = this.state.count + 1;
	console.log(this.state.count);
}
```
En cambio lo que debemos hacer es:
```jsx
increment() {
	this.setState({ count: this.state.count + 1 });
	console.log(this.state.count);
}
```
Al ejecutar este programa veremos que el valor devuelto por el `console.log` es una unidad menos respecto al valor en pantalla y esto es debido a que la llamada a `this.setState()` es **asincrónica**, por lo que el `console.log()` es llamado antes de que el state sea modificado. Si queremos asegurarnos que haya sido modificado podemos pasarle un *callback* como segundo parámetro (es una *arrow function*).

```jsx
increment() {
		this.setState({ count: this.state.count + 1 }, () => {
			console.log(this.state.count);
		});
	}
```

El componente completo:
```jsx
import React, { Component } from 'react';

class Counter extends Component {
	constructor(props) {
		super(props);

		this.state = {
			count: 0,
		};
	}

	increment() {
		this.setState({ count: this.state.count + 1 }, () => {
			console.log(this.state.count);
		});
	}

	render() {
		return (
			<div>
				Count {this.state.count}
				<button onClick={() => this.increment()}>Increment</button>
			</div>
		);
	}
}

export default Counter;
```

React puede agrupar múltiples  llamados a `setState` y realizarlos juntos por motivos de performance.
Por lo que si por ejemplo cada vez que presionamos el botón queremos un incremento de 5 unidades y lo implementamos con una función `incrementFive()` que llama 5 veces a `increment()`:

```jsx
	.
	.
	.
	increment() {
		this.setState({ count: this.state.count + 1 }, () => {
			console.log('callback value', this.state.count);
		});
	}

	incrementFive() {
		this.increment();
		this.increment();
		this.increment();
		this.increment();
		this.increment();
	}

	render() {
		return (
			<div>
				Count {this.state.count}
				<button onClick={() => this.incrementFive()}>Increment</button>
			</div>
		);
	}
	.
	.
	.
```
Al presionar en el UI veríamos uno y en la consola veríamos 5 veces el mensaje: `callback value 1`. Para evitar esto **siempre que queremos actualizar el estado basándonos el estado anterior** debemos pasarle a `this.setState()` una arrow function en vez de un objeto regular. Notar que tenemos `() => ({})` y no `() => {}` esto es asi debido a que queremos retornar un objeto. Esta función recibe `prevState` y `props` (pero también podríamos poner únicamente `prevState`)
```jsx
import React, { Component } from 'react';

class Counter extends Component {
	constructor(props) {
		super(props);

		this.state = {
			count: 0,
		};
	}

	increment() {
		this.setState((prevState,props) => ({
			count: prevState.count + 1,
		}));
	}

	incrementFive() {
		this.increment();
		this.increment();
		this.increment();
		this.increment();
		this.increment();
	}

	render() {
		return (
			<div>
				Count {this.state.count}
				<button onClick={() => this.incrementFive()}>Increment</button>
			</div>
		);
	}
}

export default Counter;
```




#  `state` como array
Es la lógica que utilizaremos en caso de tener que implementar una **Todo List** (más adelante mostramos un ejemplo de código completo) en un array en el estado. Tenemos un componente `App` con array de tareas en el state, ese estado es pasado a un componente `Tareas` que los muestra en pantalla (también recibe un método para eliminarlos). Un tercer componente `AddTodo` es el encargado de mediante los valores cargados en un formulario, llamar a un método (que recibe mediante props) para agregarla al state de `App` a la nueva tarea. Cuando cambia el state el componente `Todos` vuelve a renderizarse.

## Agregado de un elemento
Si tenemos un array `todos` en el `state` y queremos agregar un nuevo elemento debemos hacerlo de esta forma:
```jsx
addTodo= (todo) => {
	let todos= [...this.state.todos, tarea];
	this.setState({todos: todos}) //Podemos lo que es lo mismo this.setState({todos})
}
```
Como no podemos pushear al `state` el nuevo valor, lo que hacemos es crear una copia con el *spread operator* y agregar el nuevo valor. Una vez hecho esto ahí actualizamos el `state`.

## Borrado de un elemento
Supongamos que desde un componente padre le pasamos a un componente hijo como prop el método `deleteTodo` y luego este lo asocia a un botón de cada elemento de una lista de manera similar a esta: `<button onClick={()=> deleteTodo(todo.id)}/>`.

Luego en el padre tendríamos la siguiente función:
```jsx
deleteTodo= (id) => {
	let todos= this.state.todos.filter(todo=> todo.id!=id)

	this.setState({todos: todos})
}
```

## Ejemplo Completo To Do List
`App.js`
```jsx
import React, { Component } from 'react';
import Todos from './Todos'
import AddTodo from './AddTodo'

class App extends Component {
  state = {
    todos: [
      {id: 1, content: 'buy some milk'},
      {id: 2, content: 'play mario kart'}
    ]
  }
  deleteTodo = (id) => {
    const todos = this.state.todos.filter(todo => {
      return todo.id !== id
    });
    this.setState({
      todos
    });
  }
  addTodo = (todo) => {
    todo.id = Math.random();
    let todos = [...this.state.todos, todo];
    this.setState({
      todos
    });
  }
  render() {
    return (
      <div className="todo-app container">
        <h1 className="center blue-text">Todo's</h1>
        <Todos todos={this.state.todos} deleteTodo={this.deleteTodo} />
        <AddTodo addTodo={this.addTodo} />
      </div>
    );
  }
}

export default App;
```

`AddTodo.js`
```jsx
import React, { Component } from 'react'

class AddTodo extends Component {
  state = {
    content: ''
  }
  handleChange = (e) => {
    this.setState({
      content: e.target.value
    });
  }
  handleSubmit = (e) => {
    e.preventDefault();
    // call function to add a todo
    this.props.addTodo(this.state);
    this.setState({
      content: ''
    })
  }
  render() {
    return (
      <div>
        <form onSubmit={this.handleSubmit}>
          <label>Add a new todo:</label>
          <input type="text" onChange={this.handleChange} value={this.state.content} />
        </form>
      </div>
    )
  }
}

export default AddTodo
```

`Todos.js`
```jsx
import React from 'react';

const Todos = ({todos, deleteTodo}) => {

  const todoList = todos.length ? (
    todos.map(todo => {
      return (
        <div className="collection-item" key={todo.id}>
          <span onClick={() => {deleteTodo(todo.id)}}>{todo.content}</span>
        </div>
      )
    })
  ) : (
    <p className="center">You have no todo's left, yay!</p>
  );

  return (
    <div className="todos collection">
      {todoList}
    </div>
  )
}

export default Todos;
```