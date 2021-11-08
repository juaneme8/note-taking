# LinkedIn Clone
https://www.youtube.com/watch?v=QaYts9sPmcY&ab_channel=SonnySangha

# Firebase Config
## Config Proyecto
1. Ir a https://console.firebase.google.com/
2. `Add project`: linkedin-clone-yt, luego click en `Continue`.
3. Destiladamos `Enable Google Analytics for this project` y presionamos `Create project`.

## Config Cloud Firestore
1. Una vez creado el proyecto ir a `Cloud Firestore`
2. `Create database`, luego tildamos `Start in test mode` y presionamos `Next`.
3. Click en `Enable` dejando la ubicación por default `nam5 (us-central)`.

### Evitar vencimiento Firestore:
En ocasiones obtendremos un email de Firebase indicándonos en el asunto **[Firebase] Pronto vencerá el acceso de clientes a tu base de datos de Cloud Firestore** para evitar esto debemos ir a `Rules` y donde dice:

```
allow read, write: if
	request.time < timestamp.date(2021, 1, 19);
```

Debemos poner:
```
allow read, write;
```
Por último hacemos click en `Publish`.

Esto desde luego no es algo a realizar en producción pero sí para una aplicación que vamos a usar para colocar en el Portfolio por ejemplo.

## Config Authentication
1. Hacer click en `Authentication`
2. Hacer click en `Get started`
3. Luego elegir el método `Email/Password`, presionamos `Enable` y luego `Save`.

## Config Deploy
1. En `Project Overview` hacemos click en el engranaje y luego en `Project Settings`.
2. En la parte inferior hacemos click en el botón `</>`
3. En primer lugar asignamos un nickname para la aplicación por ejemplo `linkedin-clone-yt` además tildamos `Also set up Firebase Hosting for this app` y presionamos `Register app`
4. Luego click en `Next`, no copiamos nada ya que vamos a usar React y JavaScript modules.
5. Luego instalamos `npm install -g firebase-tools` y luego click en `Next`

> Si ya tenemos `firebase-tools` instalado y queremos saber qué versión tenemos: `firebase -V`, con `firebase --version` o con `firebase tools --version` y podemos compararla con la versión actual que aparezca en https://www.npmjs.com/package/firebase-tools. En caso de querer actualizarla podemos hacerlo con `npm install -g firebase-tools`

7. Luego llegamos a la parte de `Deploy to Firebase Hosting` con los comandos que debemos utilizar para deployar cuando terminemos por lo que presionamos `Continue to console`
8. Donde dice `Firebase SDK snippet` hacemos click en `Config` y copiamos el contenido de `const firebaseConfig = {...}` 
9. Creamos un archivo `firebase.js` en `src` y pegamos ese contenido.

# App Config
Como será un proyecto en el que trabajaremos con Redux a la hora de crear la aplicación con CRA agregamos `--template redux`
`npx create-react-app linkedin-clone-yt --template redux`

Luego `cd linkedin-clone-yt` y si ejecutamos `npm start` en lugar del logo habitual de React veremos el logo de Redux y un contador en pantalla.

## Cleanup de la App
Eliminamos `App.test.js`, `logo.svg` y `setupTests.js`. 

Luego en `App.js` eliminamos el `header` de modo de dejar sólo el `div`, como vamos a utilizar **BEM naming convention** cambiamos `className="App"` por `className="app"`. 

 En las nuevas versiones no es necesario poner `import React from 'react` por lo que eliminamos esta línea y también eliminamos los imports de `logo.svg` y `Counter`

A continuación eliminamos todo el contenido de `App.css`.

## Material UI
```bash
npm install @material-ui/core
npm install @material-ui/icons
```

## Header
Como primer paso en `App` importamos un componente `Header`.
```jsx
import './App.css';
import Header from './components/Header';

function App() {
	return (
		<div className='App'>
			<Header />
			{/* Sidebar */}
			{/* Feed */}
			{/* Widgets */}
		</div>
	);
}

export default App;

```

Este componente `Header` estará dividido en `.header__left` donde tendremos el logo, ícono de buscar y un `input` y `.header__right` donde tendremos una serie de íconos para los cuales emplearemos un componente reutilizable llamado `HeaderOption`.

A su vez a modo de práctica para evitar tener que usar CSS y `!important` trabajaremos con `classes` utilizando `makeStyles()` de Material UI.

```jsx
import React from 'react';
import './Header.css';
import HeaderOption from './HeaderOption';
import SearchIcon from '@material-ui/icons/Search';
import HomeIcon from '@material-ui/icons/Home';
import SupervisorAccountIcon from '@material-ui/icons/SupervisorAccount';
import BusinessCenterIcon from '@material-ui/icons/BusinessCenter';
import ChatIcon from '@material-ui/icons/Chat';
import NotificationsIcon from '@material-ui/icons/Notifications';
import { Avatar, makeStyles } from '@material-ui/core';

const useStyles = makeStyles({
	searchIcon: {
		height: '18px',
	},
	avatar: {
		height: '25px',
		width: '25px',
		opacity: 0.75,
		marginTop: '-0.75em',

		'&:hover': {
			opacity: 1,
		},
	},
});

function Header() {
	const classes = useStyles();
	return (
		<div className='header'>
			<div className='header__left'>
				<img src='https://www.flaticon.com/svg/static/icons/svg/174/174857.svg' alt='logo' />
				<div className='header__search'>
					<SearchIcon className={classes.searchIcon} />
					<input type='text' placeholder='Buscar' />
				</div>
			</div>
			<div className='header__right'>
				<HeaderOption title='Home' Icon={HomeIcon} />
				<HeaderOption title='My Network' Icon={SupervisorAccountIcon} />
				<HeaderOption title='Jobs' Icon={BusinessCenterIcon} />
				<HeaderOption title='Messaging' Icon={ChatIcon} />
				<HeaderOption title='Notificaciones' Icon={NotificationsIcon} />
				<Avatar alt='me' className={classes.avatar} src='https://i.pinimg.com/originals/91/43/e3/9143e3831c2877c36c972d15f706721c.jpg' />
			</div>
		</div>
	);
}

export default Header;

```
En cuanto a los estilos en `Header.css` podemos destacar algunos aspectos en primer lugar el `.header` con `position: sticky` acompañado de `top: 0px`.
Luego la imagen con `object-fit: contain` de modo tal que rellene la caja contenedora al mismo tiempo que conserva su relación de aspecto.

En `.header` tenemos `z-index: 1` para evitar que en un futuro el `Sidebar` quede arriba del `Header`.

```css
.header {
	position: sticky;
	top: 0px;
	display: flex;
	justify-content: space-evenly;
	border-bottom: 0.1px solid lightgray;
	padding: 10px 0;
	background-color: white;
	height: 53px;
	width: 100%;
	z-index: 1;
}

.header__left {
	display: flex;
	align-items: center;
}

.header__left > img {
	object-fit: contain;
	height: 34px;
	margin-right: 10px;
}

.header__search {
	padding: 10px;
	display: flex;
	align-items: center;
	border-radius: 5px;
	background-color: #eef3f8;
	color: gray;
	height: 34px;
}

.header__search > input {
	background: none;
	outline: none;
	border: none;
	color: inherit;
}

.header__right {
	display: flex;
	align-items: center;
}

```
Luego en el componente `HeaderOption`

```jsx
import React from 'react';
import './HeaderOption.css';

function HeaderOption({ Icon, title }) {
	return (
		<div className='headerOption'>
			{Icon && <Icon className='headerOption__icon' />}
			{title && <h3 className='headerOption__title'>{title}</h3>}
		</div>
	);
}

export default HeaderOption;

```

En tanto en `HeaderOption.css`
```css
.headerOption {
	display: flex;
	flex-direction: column;
	align-items: center;
	margin-right: 20px;
	color: gray;
	cursor: pointer;
}

.headerOption:hover {
	color: black;
}

.headerOption__title {
	font-size: 12px;
	font-weight: 400;
}

```

A continuación debemos encarar los componentes `Sidebar`, `Feed` y `Widgets` los cuales irán en tres columnas.

En `App` tendremos entonces:

```jsx
<div className='App'>
		<Header />
		<div className='app__body'>
			<Sidebar />
			{/* Feed */}
			{/* Widgets */}
		</div>
	</div>
```
Notar que tenemos un contenedor `.app__body` con la finalidad de que tenga `display:flex` y que luego los tres componentes hijos tengan `flex:0.2`, `flex: 0.6` y `flex: 0.2` así ocupan el 20%, 60% y 20% respectivamente de la pantalla.

Crearemos un componente `Sidebar` que a su vez estará dividido en parte superior `.sidebar__top`, `.sidebar__stats` y `.sidebar__botton`.

A la hora de darle estilo a `.sidebar` utilizamos `height: fit-content` 
== REVISAR ESTO ==

Tomando `.sidebar__top`
```jsx
<div className='sidebar__top'>
	<img
		src='https://images.unsplash.com/photo-1519046904884-53103b34b206?ixid=MXwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHw%3D&ixlib=rb-1.2.1&auto=format&fit=crop&w=1050&q=80'
		alt='Beach House'
	/>
	<Avatar className='sidebar__avatar' />
	<h2>Juan Ocho</h2>
	<h4>juaneme8@gmail.com</h4>
</div>
```

Le daremos estilos de la siguiente forma:
```css
.sidebar__top > img {
	width: 100%;
	margin-bottom: -20px;
	height: 60px;
	border-top-left-radius: 10px;
	border-top-right-radius: 10px;
	object-fit: cover;
}
```
Lo más destacable es el `margin-bottom` negativo de modo tal que el avatar suba y se mezcle con la imagen y `object-fit: cover` de modo tal que la imagen no se distorsione.

El resto son estilos sin mayores complicaciones.

En `Sidebar` en particular en `sidebar__bottom` a la hora de establecer los hashtags recientes en lugar de usar un componente reutilizable, implementamos una función que retorna JSX.

Esta función será:
```jsx
	const recentItem = topic => (
		<div className='sidebar__recentItem'>
			<span className='sidebar__hash'>#</span>
			<p className='sidebar__topic'>{topic}</p>
		</div>
	);
```

Luego la invocamos dentro del `return` con:
```jsx
{recentItem('reactjs')}
{recentItem('softwareengineering')}
```

Continuamos con el componente `Feed` como en el jsx tenemos;

```jsx
<div className='feed__inputContainer'>
	<div className='feed__input'>
		<CreateIcon />
		<form onSubmit={handleSubmit}>
			<input type='text' />
			<button type='submit'>Send</button>
		</form>
	</div>
	<div className='feed__inputOptions'>
		(...)
	</div>
</div>
```

Como luego en los estilos queremos que el `input` ocupe todo el espacio posible menos el espacio del botón, le asignamos a `.feed__input > form > input` `flex-grow: 1` (teniendo `.feed__input > form` la propiedad  `display: flex`).

Creamos el componente `InputOption` en el cual crearemos los íconos que van debajo del campo de texto para ingresar el posteo.

A continuación creamos el componente `Post` que será el que mostrará los mensajes ingresados por el usuario en el `Feed`. 

Para evitar que ante un mensaje largo se nos distorsione la caja contenedora hacemos lo siguiente:
```css
.post__body {
	overflow-wrap: anywhere;
}
```

Para mostrar los botones de acción (Like, Love, etc) reutilizaremos el mismo componente `InputOption` que usamos en el componente `Feed`.

