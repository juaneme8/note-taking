# Creación de Proyecto

Lo primero que tenemos que hacer es crear un proyecto, para eso debemos ir a https://firebase.google.com/, iniciar sesión y luego hacer click en **Go to console**.

1. Hacemos click en **Create a project**
2. Le asignamos el nombre deseado, aceptamos los términos y presionamos **Continue**
3. Habilitamos Google Analytics para este proyecto.
4. Donde dice **Analytics location** puse Argentina (dice que debe ser la ubicación de la organización). Luego destilé **use the default settings for sharing Google Analytics data** (y todos los sub-items) y aceptamos los términos.
5. Luego nos dirá que el proyecto está listo y presionamos **Continue**.


# Obtener Objeto de Configuración
En primer lugar vamos a **Project Overview** (en la barra lateral)
Hacemos click en el botón de Web `</>` 
Registramos la aplicación poniéndole un nombre `proyecto-web` e indicamos que no queremos Firebase Hosting en caso de que tengamos en mente deployar en Vercel.
Luego en **Project Overview** hacemos click en el engranaje y luego en **Project settings**
En el apartado **Your apps** veremos una parte que dice **SDK setup and configuration** hacer click en **Config** y copiamos el objeto de configuración `const firebaseConfig = {...}`.

> Como este objeto de configuración `firebaseConfig` lo vamos a enviar con JavaScript este podrá ser visto por cualquier persona, por lo que como seguridad debemos indicar en Firebase desde qué dominios se puede utilizar la aplicación.

## Instalación Firebase
Instalar firebase `npm install firebase`

## Variables de Entorno
Anteriormente obtuvimos un objeto llamado  `firebaseConfig` en lugar de colocar las credenciales del cliente como un string vamos a utilizar variables de entorno, para ello creamos el archivo `.env.local` y dentro ponemos:
```jsx
NEXT_PUBLIC_FIREBASE_API_KEY=
NEXT_PUBLIC_FIREBASE_AUTH_DOMAIN=
NEXT_PUBLIC_FIREBASE_PROJECT_ID=
NEXT_PUBLIC_FIREBASE_STORAGE_BUCKET=
NEXT_PUBLIC_FIREBASE_MESSAGING_SENDER_ID=
NEXT_PUBLIC_FIREBASE_APP_ID=
NEXT_PUBLIC_FIREBASE_MEASUREMENT_ID=
```
Con los secretos que obtuvimos del objeto de configuración.

Luego accederemos a estos valores con `process.env.NEXT_PUBLIC_FIREBASE_...`

> En caso de tener problemas con la creación de archivos (los cuales se solucionan si usamos el string hardcodeado sin variables de entorno) reiniciar al servidor.

## Initialización Firebase
Crear carpeta `firebase` y dentro de ella un archivo `initFirebase.js`

> En el video de **midudev** importaba `import * as firebase from 'firebase'`pero debemos poner `import firebase from 'firebase/app'` y luego importar los módulos que utilizaremos, por ejemplo si queremos tener autenticación debemos poner `import '@firebase/auth';`, si vamos a trabajar con Firestore Database `import '@firebase/firestore';`, etc.

```jsx
import firebase from 'firebase/app';
import '@firebase/auth';
import '@firebase/firestore';

// For Firebase JS SDK v7.20.0 and later, measurementId is optional
const firebaseConfig = {
  apiKey: process.env.NEXT_PUBLIC_FIREBASE_API_KEY,
  authDomain: process.env.NEXT_PUBLIC_FIREBASE_AUTH_DOMAIN,
  projectId: process.env.NEXT_PUBLIC_FIREBASE_PROJECT_ID,
  storageBucket: process.env.NEXT_PUBLIC_FIREBASE_STORAGE_BUCKET,
  messagingSenderId: process.env.NEXT_PUBLIC_FIREBASE_MESSAGING_SENDER_ID,
  appId: process.env.NEXT_PUBLIC_FIREBASE_APP_ID,
  measurementId: process.env.NEXT_PUBLIC_FIREBASE_MEASUREMENT_ID,
};

export const initFirebase = () => {
  if(!firebase.apps.length){
    firebase.initializeApp(firebaseConfig);
    console.log('Firebase was successfully initialized');
  } 
}

```

Luego en `index.js` llamamos a `initFirebase` y veremos en la consola que se inició exitosamente. **Esto es provisorio** ya que luego cuando tengamos Authentication lo haremos en `components/auth/FirebaseAuth.js` porque los usuarios puede que no ingresen directamente a index y en ese caso no tendríamos la inicialización realizada.

```jsx
...
import {initFirebase} from '../firebase/initFirebase'

initFirebase();

export default function Home() {
  return (
    <>
      ...
    </>
  )
}

```



# Cloud Firestore
Cloud Firestore nos permite trabajar con bases de datos en Firebase.

## Creación DB
En **Project Overview** dentro de **Build** vamos a **Firestore Database** y luego hacemos click en **Create database**. 

En el modal de creación elegimos **Start in test mode** y presionamos Next. 

Luego dejamos **nam5 (us-central)** como Cloud Firestore location y presionamos **Enable**.

## Agregar al Proyecto
Haciendo uso de Firestore vamos a escribir en una colección y a leer de ella. Para eso en `components` creamos una carpeta `firestore` y dentro de ella dos archivos `Write.js` y `Read.js`.

## Escritura en DB
En `Write.js` realizaremos una escritura de cada uno de los tipos de datos que podemos utilizar en Firestore.

```jsx
import firebase from 'firebase/app'
import 'firebase/firestore'

const WriteToFirestore = () =>{
    const sendData = () =>{
        try{
            firebase
                .firestore()
                .collection('myCollection')
                .doc('my_document')
                .set({
                    string_data: 'Juan Ocho',
                    number_data: 2,
                    boolean_data: true,
                    map_data: { stringInMap: 'Hi', numberInMap: 7 },
                    array_data: ['text', 4],
                    null_data: null,
                    time_stamp: firebase.firestore.Timestamp.fromDate(new Date('December 17, 1995 03:24:00')),
                    geo_point: new firebase.firestore.GeoPoint(34.714322, -131.468435)
                })
                .then(alert('Data was successfully sent to firestore'))
        }   
        catch(error){
            console.log(error)
        }
    }

    return (
        <button onClick={sendData}>Send data to Firestore</button>
    )
}
export default WriteToFirestore;
```
Buscará la colección `myCollection` y la creará si no existe.
Hemos especificado `doc('my_document')`, por lo cual buscará un documento con ese nombre y si no existe lo creará. Si hubiéramos puesto `doc()` le asignaría un *unique name*, pero como queremos leerlo luego es que le ponemos ese nombre.

Si volviéramos a hacer un `.set()` en ese mismo documento sólo con un valor de `string_data` actualizado esto sobrescribiría todos los otros datos dejando sólo ese campo. Para evitar eso debemos utilizar como segundo argumento del set `{ merge: true }` de modo tal que sólo modifique esa propiedad y deje intacta las otras.

Luego en `index.js` importamos este componente `import WriteToFirestore  from '../components/firestore/Write'` y luego lo utilizamos.

## Lectura de DB
La lectura la efectuamos usando `.onSnapshot()` del siguiente modo  `firebase.firestore().collection('myCollection').doc('myDocument').onSnapshot(doc => console.log(doc.data())`  notar que no retorna una promesa sino que trabajamos con callback.

```jsx
import firebase from 'firebase/app'
import 'firebase/firestore'

const ReadFromFirestore = () => {
    const readData = () => {
        try {
            firebase.
            firestore()
            .collection('myCollection')
            .doc('my_document')
            .onSnapshot(doc => {
                
                console.log(doc.data())
                alert('Data was successfully fetched')
            });
            
        } catch (error) {
            console.log(error)
        }
      
    }
    return (
        <button onClick={readData}>Read data from Firestore</button>
    )
}

export default ReadFromFirestore;

```



# Authentication
> Basado en [video midudev](https://youtu.be/UlYGGCNFcWo?list=PLV8x_i1fqBw1VR86y4C72xMGJ8ifjBwJ6) con algunos cambios en el modo en que se importa `firebase`.
> Basado en el [video](https://youtu.be/SYnu6CLKD70)

Para agregar autenticación a nuestros proyectos debemos ir a [firebase.google.com](firebase.google.com), luego **Go to console** y por último hacer click en el proyecto al que queramos agregarle esa característica.

En la barra de la izquierda hacemos click en **Build** y luego en **Authentication**.

Una vez en esa sección hacemos click en **Get started** y en la pestaña **Sign-in method** nos dará a elegir las distintas opciones de autenticación.

> Por más que hayamos definido varios proveedores por default sólo será posible tener una cuenta para cada correo electrónico.

## Habilitar Autenticación con GitHub
Activamos la llave **Enable** y nos pedirá el Client ID y Client secret.

Para obtener estos datos vamos en GitHub a **Settings**, luego **Developer Settings** y por último **OAuth Apps**. 

A continuación hacemos click en **Register a new application**.

Nos preguntará el nombre de la aplicación, la url (que probablemente no tengamos todavía, ponemos cualquiera) y una descripción y por último el **Authorization callback URL** donde pegamos la información que nos entrega Firebase. En ese momento nos entregará el Client ID y Client secret que ingresamos en Firebase logrando así habilitar la autenticación mediante GitHub.

## Habilitar Autenticación con Email/Password
Activamos la llave **Enable** (y nos aseguramos que la de abajo esté deshabilitada, que sería permitir un ingreso sin contraseña) y hacemos click en **Save**.

## Agregar Authentication al Proyecto
En `components` creamos la carpeta `auth` y dentro un archivo `FirebaseAuth.js`
En ese archivo vamos a llamar a `initFirebase()` cosa que hacíamos provisoriamente en `index.js` pero puede que el usuario no ingrese a través de esa ruta y nos quedaríamos sin inicializarlo.












```jsx
export const loginWithGitHub = () => {
  const githubProvider = new firebase.auth.GithubAuthProvider();

  return firebase
    .auth()
    .signInWithPopup(githubProvider)
    .then((user) => {
       const { displayName,email, photoURL } = user;

      return ({
	      avatar: photoURL,
	      username: displayName,
	      email
      })
    });
};

```

En este `client.js` exportamos una función para hacer login con GitHub esta luego la pasaremos en el `onClick` del botón que usemos para loguearnos.

> Frecuentemente nos toparemos con un mensaje de error ** FirebaseError: Firebase: Firebase App named '[DEFAULT]' already exists** para evitarlo debemos agregar `!firebase.apps.length && firebase.initializeApp(firebaseConfig);` con lo que sólo inicializará la aplicación si no hay ninguna otra. 

## Cambio de Estado
Independientemente de la variable de estado `user` queremos tener una forma de saber si el usuario está autenticado o no, sino bastará con refrescar para que user sea `null` y nos considere no autenticados.

Es posible saber en qué momento el usuario cambia de estado entre autenticado y no autenticado con un método llamado `onAuthStateChanged`. Hasta ahora en `loginWithGithub` usamos `firebase.auth().signInWithPopup(githubProvider)` y obteníamos el usuario. Ahora gracias a este método esto no será necesario, ya que accederemos a `onAuthStateChanged` y ahí mapearemos los datos obtenidos para lograr sólo un objeto con `avatar`, `url` y `username`.

Luego `onAuthStateChanged` nos queda de esta forma:

```jsx
export const onAuthStateChanged = (onChange) => {
  return firebase
    .auth()
    .onAuthStateChanged(user => {
      const { displayName,email, photoURL } = user;

      const normalizedUser = {
      avatar: photoURL,
      username: displayName,
      email
  }
      onChange(normalizedUser);
    });
}
```

Para utilizar `onAuthStateChanged` en `index.js` creamos un nuevo efecto con `useEffect`:

```jsx
useEffect(()=> {
	onAuthStateChanged(user => setUser(user)
},[]);
```

O lo que es lo mismo cambiando `	onAuthStateChanged(user => setUser(user)` por `	onAuthStateChanged(setUser)`  dado que las funciones son de primera clase y por lo tanto se pueden utilizar como parámetros. Por lo tanto nos queda:

```jsx
useEffect(()=> {
	onAuthStateChanged(setUser)
},[]);
```
Con esto cuando montemos el componente nos fijaremos si tiene sesión autentificada y en ese caso la variable de estado dejará de ser null y pasará a ser un objeto con `username`, `avatar` y `email`. 

En `onAuthStateChanged` recibo `null` si el usuario no está logueado mientras que si está logueado recibo un objeto `user`. 

La variable de estado `user` tiene tres estados `undefined` (el valor inicial para cuando refrescamos y no sabemos si está logueado o no, por lo que queremos mostrar un placeholder), `null` (para cuando no está autenticado) y un valor distinto con los datos del usuario para cuando está autenticado.

> Una vez que iniciemos sesión quedarán datos guadados en IndexedDB



# TODO App
> Basado en el [stream](https://youtu.be/VqgTr-nd7Cg) de Clever Programmer.
> Los archivos de este proyecto estarán en `md-todo-app`

## Configuración Firebase
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


## Creación App
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

## Material UI
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

## Autoimport en VS Code
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

# Solucionar Warnings
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