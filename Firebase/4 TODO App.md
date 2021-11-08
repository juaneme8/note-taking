# TODO App
> Basado en el [stream](https://youtu.be/VqgTr-nd7Cg) de Clever Programmer.
> Los archivos de este proyecto estarán en `md-todo-app`

# Configuración Firebase
## Configurar Proyecto
1. https://firebase.google.com/
2. Ir a la consola
3. Agregar proyecto
4. Asignar un nombre de proyecto: `todo-app`
5. Dejar seleccionado **Habilitar Google Analytics para este proyecto**
6. Elegir una cuenta de google analytics y hacer click en **Crear Proyecto**

## Configurar App
1. Hacer click en el botón `</>` para agregar una App Web.
2. Elegir un sobrenombre para la app `todo-app-react` y dejar tildado **Firebase Hosting** y hacer click en **Registrar App**
3. A continuación veremos el paso llamado **Agregar el SDK de Firebase** pero omitimos ese paso ya que como vamos a trabajar con React haremos uso de node modules, por lo que presionamos **Siguiente**.
4.  Hacemos click en **Ir a la Consola**. En el apartado de nuestra App que dice **Firebase SDK snippet** hacemos click en Config y veremos el objeto `firebaseConfig` que utilizaremos para conectar nuestra aplicación React a Firebase.

## Instalación Firebase CLI
1. Instalar Firebase CLI de manera global con `npm install -g firebase-tools`. 

> Podemos chequear si un paquete determinado fue instalado anteriormente de manera global con `npm list -g firebase-tools` sin embargo si ponemos `npm i -g firebase-tools` lo actualizará en caso de ya tenerlo instalado.

2. En la línea de comandos escribimos `firebase login` y nos pedirá que iniciemos sesión con nuestra cuenta.


# Creación App
Creamos el template de la aplicación con `npx create-react-app md-todo-app` y luego la iniciamos con `npm start`.

Hacemos la limpieza habitual del del template: 

En `src` eliminar `App.test.js`, `setupTests.js` y`logo.svg`

En app.js eliminar `import logo from './logo.svg';` y limpiar el contenido de  el contenido de `<div className="app"></div>` también cambiarle el nombre de `App` a `app` de manera de respetar *BEM naming convention*.

`App.js`
```jsx
import React, { useState } from 'react';
import './App.css';

function App() {
	const [todos, setTodos] = useState([]);
	const [input, setInput] = useState('');

	const addTodo = e => {
		e.preventDefault();
		setTodos([...todos, input]);
		setInput('');
	};

	return (
		<div className='App'>
			<h1>TODO App</h1>
			<form>
				<input type='text' value={input} onChange={e => setInput(e.target.value)} />
				<button type="submit" onClick={addTodo}>Add Todo</button>
			</form>
			<ul>
				{todos.map((todo, index) => (
					<li key={index}>{todo}</li>
				))}
			</ul>
		</div>
	);
}

export default App;

```

Contamos con un `input` y un `button` y como queremos ser capaces de agregar el contenido con la tecla `ENTER` los colocamos dentro de un `form` e indicamos que el `button` es de `type="submit"`.

# Material UI
`npm install @material-ui/core`

De acuerdo a la documentación https://material-ui.com/components/buttons/ remplazamos el botón que teníamos:

## Button con Material UI
```jsx
<button onClick={addTodo}>Add Todo</button>
```

Por este otro:
```jsx
<Button type='submit' variant='contained' color='primary' onClick={addTodo}>
	Add Todo
</Button>
```

Para ello debemos hacer el siguiente import: `import { Button } from '@material-ui/core';`

Como queremos que sólo sea posible agregar elementos cuando haya contenido agregamos `disabled={!input}`


## Input con Material UI
En cuanto al input en el stream utilizan un snippet de código similar a este:

```jsx
<FormControl>
  <InputLabel htmlFor="mi-campo">Write a Todo</InputLabel>
  <Input id="mi-campo" aria-describedby="mi-texto-de-ayuda" />
  <FormHelperText id="mi-texto-de-ayuda">Nunca compartiremos tu email.</FormHelperText>
</FormControl>
```
En la documentación de text fields podemos encontrarlo bajo el título **Accesibilidad**:
 https://material-ui.com/es/components/text-fields/
Sin embargo, luego lo modifican hasta dejarlo así:
```jsx
<FormControl>
  <InputLabel>Write a Todo</InputLabel>
  <Input/>
</FormControl>
```
Por lo que agregando los atributos que teníamos hasta ahora en el input nos queda:
```jsx
<FormControl>
  <InputLabel>Write a Todo</InputLabel>
  <Input value={input} onChange={e => setInput(e.target.value)}/>
</FormControl>
```

## Componente `Todo`
En `src` creamos una carpeta `components` y en ella un archivo `Todo.js`. Utilizamos el snippet `rfce` (*react functional component with an export*).

```jsx
import React from 'react';

function Todo({ text }) {
	return (
		<div>
			<li>{text}</li>
		</div>
	);
}

export default Todo;

```
Luego en `App.js` lo utilizamos de la siguiente forma:
```jsx
<ul>
	{todos.map(todo => (
		<Todo todo={todo} />
	))}
</ul>
```

## List con Material UI
Basándonos en la documentación: https://material-ui.com/components/lists/
Copiamos el siguiente snippet (borrando algunas partes del medio que usan otra dependencia) de la sección **Lista de Carpetas**

```jsx
<List className={classes.root}>
	<ListItem>
		<ListItemText primary="Photos" secondary="Jan 9, 2014" />
	</ListItem>
</List>    
```

Adaptándolo a nuestra aplicación:

```jsx
<List>
	<ListItem>
		<ListItemText primary={text} secondary={new Date().toLocaleString()} />
	</ListItem>
</List>
```
Pero como `<List>` es el equivalente a `<ul>` y `<ListItem>` el equivalente a `<li>` dentro de `Todo` sólo debemos poner `<List>` y `<ListItem>` en `App.js`

Por lo que `Todo` nos queda:
```jsx
import { ListItem, ListItemText } from '@material-ui/core';
import React from 'react';

function Todo({ text }) {
	return (
		<ListItem>
			<ListItemText primary={text} secondary={new Date().toLocaleString()} />
		</ListItem>
	);
}

export default Todo;

```

Mientras que `App.js` nos queda:
```jsx
import React, { useState } from 'react';
import './App.css';
import { Button, FormControl, InputLabel, Input, List } from '@material-ui/core';
import Todo from './components/Todo';

function App() {
	const [todos, setTodos] = useState([]);
	const [input, setInput] = useState('');

	const addTodo = e => {
		e.preventDefault();
		setTodos([...todos, input]);
		setInput('');
	};

	return (
		<div className='App'>
			<h1>TODO App</h1>
			<form>
				<FormControl>
					<InputLabel>Write a Todo</InputLabel>
					<Input value={input} onChange={e => setInput(e.target.value)} />
				</FormControl>

				<Button disabled={!input} type='submit' onClick={addTodo} variant='contained' color='primary'>
					Add Todo
				</Button>
			</form>
			<List>
				{todos.map(todo => (
					<Todo text={todo} />
				))}
			</List>
		</div>
	);
}

export default App;

```

#### Autoimport en VS Code
Por ejemplo si queremos importar `ListItem`, lo usamos y nos posicionamos `<ListItem>` justo antes del `>` y presionamos `CTRL + SPACE` y luego nos aparecerá un mensaje indicando el auto import.

## Estilos en `Todo`
Creamos en `components` un archivo `Todo.css` donde colocamos los estilos de este componente. Podríamos tener `<ListItem className="todo__list">` y luego modificar sus estilos en el css por ejemplo poniendo `.todo__list {background-color: red;}`

## Conexión a Firebase
Instalamos `npm i firebase`
Creamos un archivo `firebase.js` con el objeto `firebaseConfig` que copiamos de firebase, hacemos lo siguiente.

Según el stream debemos importar `import firebase from 'firebase';` pero esto ocasiona un warning por lo que ponemos `import firebase from 'firebase/app` y `import 'firebase/firestore';`
De manera similar en `App.js` ponemos ``import firebase from 'firebase/app``

```jsx
import firebase from 'firebase/app';
import 'firebase/firestore';

const firebaseConfig = {
	apiKey: '',
	authDomain: '',
	databaseURL: '',
	projectId: '',
	storageBucket: '',
	messagingSenderId: '',
	appId: '',
	measurementId: '',
};

const firebaseApp = firebase.initializeApp({
	firebaseConfig,
});

const db = firebaseApp.firestore();

export default db;
```

Estamos exportando `db` que será utilizado para realizar la lectura y escritura en `App.js`

# Firestore
Luego en la página de Firebase vamos a **Cloud Firestore**, luego a **Crear Base de Datos** y elegimos la opción **Comenzar en modo de prueba** por último click en **Habilitar**.
A continuación debemos **Iniciar una colección** y **Agregar un documento** a modo de prueba, para ellos debemos hacer click en **ID Automático** y luego indicar el **Campo** y el **Valor**.

## Lectura de DB
Ahora queremos que cuando la aplicación inicie, con el hook `useEffect()` obtener los valores de esa colección.
```jsx
useEffect(() => {
	db.collection('todos').onSnapshot(snapshot => {
		const todosArr = snapshot.docs.map(doc => doc.data().todo);
		setTodos(todosArr);
	});
}, []);
```
Como estamos utilizando `onSnapshot` cada vez que se produzca un cambio en la base de datos vamos a actualizar el valor del estado con `setTodos()`

## Escritura de DB
Para la escritura en la base de datos en `addtodo` hacemos lo siguiente:
```jsx
db.collection('todos').add({
	todo: input,
});
```
Como esto ocasionará un nuevo `snapshot` el cambio será alcanzado por `setTodos()` por lo que ya no lo agegamos más a `todos` en `addTodo`.

```jsx
db.collection('todos').add({
	todo: input,
	timestamp: 			firebase.firestore.FieldValue.serverTimestamp(),
});
```

## Ordenamiento de TODOs
Veremos que las entradas se ordenan de acuerdo al id que les fue asignada por Firebase y no de manera tal que el último aparezca abajo. Queremos incorporar un campo `timestamp`  de modo que el más reciente aparezca en la parte inferior.
Cuando trabajamos con timestamps debemos tener presente que no debemos utilizar un timestamp propio sino el del server de firebase para evitar inconvenientes debido a que gente de distintas zonas horarias agregue elementos. Esto lo hacemos con `timestamp: firebase.firestore.FieldValue.serverTimestamp()` es decir que tendremos que importar `firebase` con `import firebase from 'firebase';`

Para lograr un ordenamiento por timestamp debemos poner ahora.

```jsx
useEffect(() => {
	db.collection('todos')
	  .orderBy('timestamp', 'desc')
	  .onSnapshot(snapshot => {
		const todosArr = snapshot.docs.map(doc => doc.data().todo);
		setTodos(todosArr);
	});
}, []);
```

## Eliminar un TODO
Para poder eliminar un TODO necesitamos almacenar el `id`, por lo que la variable de estado será un array de objetos con las propiedades `id`,`todo`. Luego le pasamos a `Todo` un elemento de ese array y agregamos un botón debajo para eliminarlo.

```jsx
	db
	.collection('todos')
	.doc(props.todo.id)
	.delete()
```

## Editar un TODO
Así como antes utilizamos el método `delete()` ahora utilizamos un  `set()`

```jsx
const updateTodo = () => {
	db
	.collection('todos')
	.doc(props.todo.id)
	.set(
		{
			todo: input,
		},
		{ merge: true }
	);
	setOpen(false);
};
```

Como cada componente `TODO` recibe sólo los datos de esa tarea, podremos editarla fácilmente pues su `id` estará disponible en las props.

Cuando actualizamos utilizamos `{merge: true}` nos permite no perder los otros fields que tenemos asociados a esa entrada, en este caso el timestamp. 

La edición de tareas la haremos mediante un [modal](https://material-ui.com/es/components/modal/) usando Material UI. Trabajamos además con el estado `const [open, setOpen] = useState(false)`;
de manera tal que inicialmente está en `false` y a través de la prop del modal `open` le indica que no debe abrirse (`<Modal open={open} onClose={handleClose}></Modal>`). En tanto `handleClose()` se encarga de poner el estado en `false` y al hacer click sobre el ícono de edición  `handleOpen()` lo pone en `true`.

La forma más simple de implementar un modal en Material UI es con el componente `Modal`.

Decimos que es la versión más simple  del modal porque carece prácticamente de estilos, si queremos algo más avanzado podemos usar el componente `Dialog`.

```jsx
function Todo(props) {

	const [open, setOpen] = useState(false);
	const [input, setInput] = useState(props.todo.todo);
	const handleOpen = () => {
		setOpen(true);
	};
	const handleClose = () => {
		setOpen(false);
	};

	const classes = useStyles();

	const updateTodo = () => {
		db.collection('todos').doc(props.todo.id).set(
			{
				todo: input,
			},
			{ merge: true }
		);
		setOpen(false);
	};

	return (
		<>
			<Modal open={open} onClose={handleClose} aria-labelledby='simple-modal-title' aria-describedby='simple-modal-description'>
				<div className={classes.paper}>
					<h2>Editar TODO</h2>
					<input value={input} onChange={e => setInput(e.target.value)} />
					<button onClick={updateTodo}>Actualizar</button>
				</div>
			</Modal>
			<ListItem>
				<ListItemText primary={props.todo.todo} secondary='Just another task' />
				<DeleteIcon className='deleteBtn' onClick={e => db.collection('todos').doc(props.todo.id).delete()}></DeleteIcon>
				<EditIcon onClick={handleOpen}></EditIcon>
			</ListItem>
		</>
	);
}

export default Todo;

```

# Deploy
1. `firebase login`
2. `firebase init`
3.  **Are you ready to proceed? (Y/n)**
4. Elegimos la opción **(*) Hosting: Configure and deploy Firebase Hosting sites** la seleccionamos con `SPACE` y luego presionamos `ENTER`.
5. Use an existing project
6. Elegimos  todo-app-77371 (todo-app)
7. **What do you want to use as your public directory? (public)** ingresamos build
8.  **Configure as a single-page app (rewrite all urls to /index.html)?** Y
9.  Set up automatic builds and deploys with GitHub? (y/N) ==No== (sin referencia en tutorial)
10. File build/index.html already exists. Overwrite? ==No== (sin referencia en tutorial)

NOTA: Si al llegar al paso 4 obtenemos el siguiente mensaje: `Error: Failed to list Firebase projects. See firebase-debug.log for more info.` debemos ejecutar `firebase logout` y luego `firebase login`

11. Ejecutamos `npm run build` para obtener un paquete más pequeño y eficaz y será este el que haremos el deploy. Veremos que aparece una carpeta `build`
12. `firebase deploy`

> **NOTA**: Una forma de hacer los pasos 11 y 12 juntos es `npm run build && firebase deploy`


# Material UI Icons
https://material-ui.com/es/components/material-icons/

`npm i @material-ui/icons`

Luego por ejemplo para trabajar con un ícono de Delete, tendremos que importar: 
```js
import DeleteIcon from '@material-ui/icons/Delete';
```

Y luego en lugar de tener:
```jsx
<Button onClick={e => db.collection('todos').doc(props.todo.id).delete()}>X</Button>
```

Ponemos:
```jsx
<DeleteIcon onClick={e => db.collection('todos').doc(props.todo.id).delete()}></DeleteIcon>
```

# Solucionar Warnings:
* 1. Warning:
```bash
It looks like you're using the development build of the Firebase JS SDK.
When deploying Firebase apps to production, it is advisable to only import
the individual SDK components you intend to use.

For the module builds, these are available in the following manner
(replace <PACKAGE> with the name of a component - i.e. auth, database, etc):

CommonJS Modules:
const firebase = require('firebase/app');
require('firebase/<PACKAGE>');

ES Modules:
import firebase from 'firebase/app';
import 'firebase/<PACKAGE>';

Typescript:
import * as firebase from 'firebase/app';
import 'firebase/<PACKAGE>';
```
Solución:
En `App.js` debemos cambiar `import firebase from 'firebase'` por `import firebase from 'firebase/app';`

En `firebase.js` como también usamos el módulo firestore, debemos cambiar `import firebase from 'firebase'` por:
```jsx
import firebase from 'firebase/app';
import 'firebase/firestore';
```