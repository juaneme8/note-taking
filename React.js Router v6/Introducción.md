## React Router v6

> :link: Basado en el [video de freeCodeCamp](https://youtu.be/59IXY5IDrBA).

React Router es una biblioteca para crear SPA con las cuales a diferencia de las aplicaciones tradicionales multipágina no haremos una solicitud al servidor para cada página. En cambio, tendremos un único div con todo  el código y esto traerá como consecuencia una navegación instantánea.

Como React no tiene una solución de ruteo incorporada utilizaremos esta librería que de hecho es la más popular para tal fin.



## Configuración

Utilizaremos Create React App para crear la SPA.

```bash
npx create-react-app react-router-v6
```



```bash
npm install react-router-dom@6
```



### Pasando contenido directo a `element`

En `App.js` vemos una primera implementación del ruteo.

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



Para cada página debemos utilizar el componente `Route` con dos props `path` con la URL deseada y `element` con lo que queremos renderizar.

* En`path` podemos poner tanto `ruta` como `/ruta`.
* Es posible colocar ``<BrowserRouter>` en `index.js` en lugar de hacerlo en `App.js`



### Pasando componentes a `element`

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

Utilizaremos el componente `Link` para navegar las distintas secciones de la SPA.

En el componete `Home` queremos poder navegar hacia /about.

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



Al presionar dicho elemento veremos que no tendremos un *full page refresh* sino que la navegación será instantánea



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



> En v6 el match mas específico aplica a diferencia de lo que sucedía en versiones anteriores donde era importante el orden. En este caso podremos poner la ruta de manejo de errores primero y sin embargo nos seguirán funcionando el resto de las rutas.



## Layout

En ocasiones vamos a querer tener el mismo layout para todos los componentes. En ese caso **por fuera de `Routes`** debemos colocar aquellos elementos que queremos reutilizar.

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



## Nested Routes

En ocasiones vamos a querer tener un shared layout y para eso debemos configurar nested routes.

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



Sin embargo si ahora navegamos a /about veremos el contenido de `Home` dado que debemos modificar algo mas.



## Shared Layout

Para poder visualizar el contenido de las rutas hijas debemos colocar en el componente de la ruta padre el componente `Outlet`

En nuestro caso queremos que al entrar a /about nos muestre el contenido del componente `Home` y de `About` ya que es una ruta anidada y por ende queremos que compartan el layout.



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



## Navbar Reutilizable

Podemos utilizar entonces las rutas anidadas para generar compartir un layout y esto podemos utilizarlo para generar un Navbar.

Vamos a crear un componente `Navbar` y lo hacemos `/components/Navbar.js`

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

