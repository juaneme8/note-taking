## React Router v6

:link: Basado en el [video de freeCodeCamp](https://youtu.be/59IXY5IDrBA).

:link: Basado en la [playlist](https://www.youtube.com/watch?v=OMQ2QARHPo0&list=PL4cUxeGkcC9iVKmtNuCeIswnQ97in2GGf&ab_channel=TheNetNinja) de The Net Ninja. (VIDEO 2 COMPLETO)

## Introducción

Cuando hacemos sitios utilizando React normalmente estaremos creando SPAs (single page applications). En este tipo de aplicaciones cuando navegamos a la aplicación, el servidor nos devolverá el HTML y código JavaScript de React necesario para mostrar los distintos componentes de la página. Luego cuando intentamos navegar hacia otra sección, esta petición será interceptada y no llegará al servidor. Esto será manejado por el frontend y React Router mostrará el componente acorde a la página deseada.

Como tendremos todo el código en un único `<div>` la navegación será instantánea.

La utilización de una biblioteca como React Router es necesaria ya que React no tiene un mecanismo de ruteo incorporado por defecto.

![image-20230105075625371](Introducción.assets/image-20230105075625371.png)





## Getting started

Utilizaremos **Create React App** para crear la SPA, por lo que primero que ejecutamos es:

```bash
npx create-react-app react-router-v6
```



Luego instalamos React Router propiamente dicho:

```bash
npm install react-router-dom@6
```



# Implementación previa a v6.4

## Pasando **contenido** directo a `element`

A continuación modificamos `App.js` para tener una primera implementación del ruteo.

```jsx
import { BrowserRouter, Routes, Route } from 'react-router-dom';

function App() {
  return (
    <BrowserRouter>
      <Routes>
        <Route path='/' element={<h1>Home Page</h1>} />
        <Route path='about' element={<h1>About Page</h1>} />
        <Route path='products' element={<h1>Products Page</h1>} />
      </Routes>
    </BrowserRouter>
  );
}

export default App;
```

Cuando nos referimos a la ruta principal en lugar de colocar  `<Route path='/' element={...}` podemos usar `<Route index element={...}>`

Para cada página debemos utilizar el componente `Route` con dos props `path` con la URL deseada y `element` con lo que queremos renderizar.

* En`path` podemos poner tanto `ruta` como `/ruta`.
* Es posible colocar `<BrowserRouter>` en `index.js` en lugar de hacerlo en `App.js`.
* Si cambiamos en al barra de dirección de **/** a **/about** por ejemplo y le damos ENTER si bien estaríamos visualizando uno u otro componente, en ese caso sí estaríamos haciendo un nuevo request al server, cosa que evitaremos con los componentes de RR `Link` o `NavLink`. 
* :vertical_traffic_light: Para darle un mayor nivel semántico podríamos colocar abarcando a `<Routes>` un `<main>`.



## Pasando **componentes** a `element`

En `element` hemos puesto directamente el contenido pero lo más normal y fácil de mantener es poner directamente un componente.

```jsx
import { BrowserRouter, Routes, Route } from 'react-router-dom';
import Home from './pages/Home';
import About from './pages/About';
import Products from './pages/Products';

function App() {
  return (
    <BrowserRouter>
      <Routes>
        <Route path='/' element={<Home />} />
        <Route path='about' element={<About />} />
        <Route path='products' element={<Products />} />
      </Routes>
    </BrowserRouter>
  );
}

export default App;
```



## Navegación

## `Link`

Utilizaremos el componente `Link` para navegar las distintas secciones de la SPA.

En el componente `Home` queremos poder navegar hacia `/about`.

```jsx
import { Link } from "react-router-dom";

const Home = () => {
  return (
    <section>
      <Link to="/about">About</Link>
      <h2>Home Page</h2>
    </section>
  )
}

export default Home;
```

> Es posible asignarle una clase `Link` para estilarlo, lo haremos con `<Link to="/about" className="btn">About</Link>`

> Si queremos navegar a una página externa debemos utilizar el anchor de HTML`<a href="">`



:tada: Como ya dijimos al presionar dicho elemento veremos que no tendremos un *full page refresh* sino que la navegación será instantánea.



## `NavLink`

`NavLink` es un tipo especial de `Link` que sabe si está "activo" o no. Esto es útil a la hora de crear menus de navegación como un breadcrumb o tabs donde nos resultaría útil mostrar cuál opción está actualmente seleccionada.

Si analizamos en las DevTools veremos que tenemos un elemento `<a>` al cual cuando está activo se le agrega `class="active"`.

En `index.css` establecemos los estilos que nos permitan diferenciar cuando un elemento de la navegación esté activo.



## Página de Error

En aquellas ocasiones donde la request no matchee con ninguna de las rutas queremos mostrar una página de error.

En ese caso creamos una ruta y como `path` colocamos `*`.

```jsx
import { BrowserRouter, Routes, Route } from 'react-router-dom';
import Home from './pages/Home';
import About from './pages/About';
import Products from './pages/Products';
import Error from './pages/Error';

function App() {
  return (
    <BrowserRouter>
      <Routes>
        <Route path='/' element={<Home />} />
        <Route path='about' element={<About />} />
        <Route path='products' element={<Products />} />
        <Route path='*' element={<Error />} />
      </Routes>
    </BrowserRouter>
  );
}

export default App;

```



En la versión 6 de React Router **el match mas específico aplica** a diferencia de lo que sucedía en versiones anteriores donde era importante el orden. En este caso podremos poner la ruta de manejo de errores primero y sin embargo nos seguirán funcionando el resto de las rutas.



## Layout

En ocasiones vamos a querer tener el mismo layout para todos los componentes. 

Un caso típico de uso será para implementar un navbar con todos los enlaces a las distinas secciones.

En ese caso **por fuera de `Routes`** debemos colocar aquellos elementos que queremos reutilizar.

```jsx
import { BrowserRouter, Routes, Route } from 'react-router-dom';
import Home from './pages/Home';
import About from './pages/About';
import Products from './pages/Products';
import Error from './pages/Error';

function App() {
  return (
    <BrowserRouter>
      <nav>Navbar</nav>
      <Routes>
        <Route path='/' element={<Home />} />
        <Route path='about' element={<About />} />
        <Route path='products' element={<Products />} />
        <Route path='*' element={<Error />} />
      </Routes>
      <footer>Footer</footer>
    </BrowserRouter>
  );
}

export default App;

```

* :vertical_traffic_light: Nuevamente para darle un mayor nivel semántico podríamos envolver el `<nav>` con un `<head>`.

  

## Nested Routes

En ocasiones vamos a querer tener un shared layout y para eso debemos configurar nested routes.

Por ejemplo si queremos que al ingresar a **/about** nos muestre el componente `Home` y `About` tendríamos que empezar creando nested routes como vemos a continuación en `App`:

```jsx
import { BrowserRouter, Routes, Route } from 'react-router-dom';
import Home from './pages/Home';
import About from './pages/About';
import Products from './pages/Products';
import Error from './pages/Error';

function App() {
  return (
    <BrowserRouter>
      <Routes>
        <Route path='/' element={<Home />} >
          <Route path='about' element={<About />} />
          <Route path='products' element={<Products />} />
          <Route path='*' element={<Error />} />
        </Route>
      </Routes>
    </BrowserRouter>
  );
}

export default App;
```



A continuación para poder visualizar el contenido de las rutas hijas debemos colocar en el componente de la ruta padre el componente `Outlet`, por lo tanto en nuestro caso en `Home`:

```jsx
import { Link, Outlet } from "react-router-dom";

const Home = () => {
  return (
    <section>
      <h2>Home Page</h2>
      <Link to="/about">About</Link>
      <Outlet />
    </section>
  )
}

export default Home;
```

Ahora si ingresamos a /about veremos ambos contenidos.

## Navbar Reutilizable

Un caso de utilidad podría ser mostrar un `Navbar` que incluiremos en `Home` de modo que se vea al entrar a todas las rutas (conservando el anidamiento de `App` visto anteriormente)

Vamos a crear un componente `Navbar` y lo hacemos `/components/Navbar.js`.

```jsx
import { Link } from "react-router-dom";

const Navbar = () => {
  return (
    <nav>
      <ul>
        <li>
          <Link to="/">Home</Link>
        </li>
        <li>
          <Link to="/about">About</Link>
        </li>
        <li>
          <Link to="/products">Products</Link>
        </li>
      </ul>
    </nav>

  )
}

export default Navbar;
```



Luego en `Home` importamos a `Navbar` y como esta es una ruta padre con otras rutas anidadas hijas, el contenido se mostrará en todos ellas.

```jsx
import { Outlet } from "react-router-dom";
import Navbar from "../components/Navbar";

const Home = () => {
  return (
    <>
      <Navbar />
      <section>
        <Outlet />
      </section>
    </>
  )
}

export default Home;
```



# Implementación desde v6.4

A partir de la versión 6.4 en lugar de utilizar el componente `<BrowserRouter>` utilizaremos otra metodología como veremos a continuación.

En `App.js`:

```jsx

```



Creamos una carpeta `layouts` y en ella el componente `RootLayout` que debe tener un `<Outlet/>` que garantiza que se muestre el contenido de las rutas anidadas.
