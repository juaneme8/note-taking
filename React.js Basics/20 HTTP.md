# HTTP
A continuación veremos como se manejan los HTTP requests en React.
En una React App cuando queremos efectuar **HTTP requests** ya sea para  enviar o recibir datos de Servidor, debemos tener presente que React es una biblioteca para construir interfaces de usuario y no tiene implicancias en HTTP. Los componentes de React sólo leen _props_ y _state_ y renderizan la UI, por lo que para usar datos de un servidor tendremos que colocarlos allí.
Como React no estará involucrado en hacer los requests debemos utilizar una bibloteca HTTP como ser **axios**, **fetch API**

`npm i axios`

Utilizaremos como API a `https://jsonplaceholder.typicode.com/` y haremos un GET request a `https://jsonplaceholder.typicode.com/posts`

Creamos un componente de clase `PostList`, importamos `axios`, creamos el `state` donde guardaremos los posts y en `componentDidMount` (recordemos que este método sólo se ejecutará una vez cuando el componente se monta) realizamos el get request.

```jsx
import React, { Component } from 'react';
import axios from 'axios';

class PostList extends Component {
	constructor(props) {
		super(props);

		this.state = {
			posts: [],
			errors: '',
		};
	}

	componentDidMount() {
		axios
			.get('https://jsonplaceholder.typicode.com/posts')
			.then((response) => this.setState({ posts: response.data }))
			.catch((error) => this.setState({ errors: 'Error en get request' }));
	}
	render() {
		const { posts, errors } = this.state;
		return (
			<div>
				{posts.length
					? posts.map((post) => <h3 key={post.id}>{post.title}</h3>)
					: 'No hay posts para mostrar'}
				{errors ? <div>{errors}</div> : null}
			</div>
		);
	}
}

export default PostList;

```

Notar que inicialmente el state tiene al array posts vacío por lo que al renderizar veremos la leyenda "No hay posts para mostrar", luego en el lifecycle method `componentDidMount` cambiamos el estado por lo que se vuelve a renderizar el componente ahora sí con el agregado de los posts al estado.

## Post Request
Analizando los datos devueltos por la API de jsonplaceholder vemos:
```json
[
	{
		"userId": 
		"id":
		"title":
		"body":
	}
]
```
El `id` entregado es autoincremental y no debemos pasarlo como parte del POST request.
Creamos un componente `PostForm.js` y en él un formulario con tres input fields para `userId`, `title`, `body` acompañados de un submit button.
La URL a la cual hacemos el POST es la misma que la del GET y en `response.data` obtenemos el objeto "añadido" (en realidad sólo nos entrega un nuevo número de id y nos devuelve el mismo objeto pero no lo agrega a esta fake API).
Para lo relacionado con validación de formularios podemos utilizar la biblioteca **Formik** o en caso de utilizar Redux podemos usar Redux Axios Middleware.

```jsx
import React, { Component } from 'react';
import axios from 'axios';

class PostForm extends Component {
	constructor(props) {
		super(props);

		this.state = {
			userId: '',
			title: '',
			body: '',
		};
	}
	changeHandler = (e) => {
		this.setState({ [e.target.name]: e.target.value });
	};

	submitHandler = (e) => {
		e.preventDefault();
		axios
			.post('https://jsonplaceholder.typicode.com/posts', this.state)
			.then((response) => console.log(response));
	};

	render() {
		const { userId, title, body } = this.state;
		return (
			<form onSubmit={this.submitHandler}>
				<div>
					<input
						type="text"
						name="userId"
						value={userId}
						onChange={this.changeHandler}
					/>
				</div>
				<div>
					<input
						type="text"
						name="title"
						value={title}
						onChange={this.changeHandler}
					/>
				</div>
				<div>
					<input
						type="text"
						name="body"
						value={body}
						onChange={this.changeHandler}
					/>
				</div>
				<button type="submit"> Enviar </button>
			</form>
		);
	}
}

export default PostForm;

```

# Mock Data
En algunas ocasiones al desarrollar un prototipo puede que no esté disponible la  API o al menos un endpoint en particular. Para evitar que esto nos retrase en el desarrollo de la UI.
En esos casos resulta útil crear *mock data*.

En primer lugar creamos una carpeta `services` y en ella un archivo `mockUsers.js` suponiendo tenemos que trabajar con un endpoint que devuelve un array de usuarios, 

```jsx
export const getUsers = () => [
	{id: 1, name: 'Juan'},
	{id: 2, name: 'Ocho'},
	(...)
];
```

Luego en el archivo que queramos hacer uso de esos datos ponemos:
```jsx
import {getUsers} from '../services/mockUsers'

//Para obtener todos los elementos del array
const data = getInstantData1()
```

En paso más podría ser trabajar con una herramienta como **JSON Server** con lo cual haremos una solicitud http real por lo que la cantidad de código a modificar una vez tengamos la API a disposición será menor.

```jsx

```

```jsx

```