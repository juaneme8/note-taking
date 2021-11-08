# Sidebar Navigation Menu
> https://www.youtube.com/watch?v=CXa0f4-dWi4&t=149s&ab_channel=BrianDesign
> `md-sidebar-v1`

# Creación Proyecto
Trabajaremos con Create React App:
`npx create-react-app md-sidebar-v1` 

# React Router
`npm i react-router-dom`


# Limpieza Proyecto
En la carpeta `src` eliminar:
* `logo.svg`
* `serviceWorker.js` (en nuevas versiones no está) 
* `setupTests.js` (en el momento en que fue hecho el video no estaba `reportWebVitals.js`).
* `index.css`.

En `index.js` eliminar el import de `index.css` y la línea `serviceWorker.unregister();` (en caso de estar con la versión vieja que tiene `serviceWorker.js)`.  Además obviamente eliminar el import del `import * as serviceWorker from './serviceWorker';`.

En `App.js`dejar sólo un div y eliminar el import del logo.

# Componentes y Páginas
En `src` creamos las carpetas `components` (con `Navbar.js`, `Navbar.css` y `SidebarData.js`) y `pages` (con `Home.js`, `Products.js` y `Reports.js`) 

# Íconos
Utilizaremos React Icons `npm i react-icons` y para agregar el ícono del menú hamburguesa por ejemplo por tratarse de un ícono de Font Awesome podríamos ponerlo como `import  { FaBars }  from  "react-icons/fa";` pero lo que haremos será `import * as FaIcons from 'react-icons/fa';` y luego para utilizarlo haremos `<FaIcons.FaBars/>` y de esta manera si queremos utilizar luego otros íconos no hará falta importarlos de manera individual. 


# Primeras Rutas
En `App.js` importamos `import { BrowserRouter as Router, Switch, Route } from 'react-router-dom';` y luego:
```jsx
<Router>
	<Navbar />
	<Switch>
		<Route path='/' />
	</Switch>
</Router>
```


