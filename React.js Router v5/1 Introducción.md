# React Router
> `md-react-router`
> https://www.youtube.com/watch?v=Law7wfdg_ls&t=904s&ab_channel=DevEd
> Agregados Curso React Mosh

A diferencia de lo que ocurre en Frameworks como Angular, en React no tenemos el concepto de Routing, por tratarse de una *lightweight library* sólo se encarga del renderizado de la vista.

La idea detrás de React Router consiste en que si visitamos `/about` en lugar de hacer el fetch de esa página del server, lo que hacemos es renderizar un componente en particular (el request no alcanza al server sino que React Router lo intercepta). De esta manera la página no refresca y obtenemos una experiencia similar a la de usar una aplicación.

`npx create-react-app md-react-router`
`cd md-react-router`
`npm i react-router-dom`

Creamos una carpeta `components` y en ella `Nav.js`, `About.js` y `Shop.js`. 

> Si cuando estamos haciendo un import ponemos por ejemplo `import Link from ''` y dentro de las comillas presionamos `rrd` nos autocompletará con `react-router-dom`
>
> La idea es mostrar siempre `Nav` y de acuerdo a la url mostrar un componente en particular. Es decir que si la dirección es `/about` renderizamos `About`.

Decimos que `Router` (es un alias de `BrowserRouter`) wrapea el *history object* y lo pasa por el *component tree* permitiéndonos usarlo en cualquier punto.  `Route` en cambio renderiza un componente de acuerdo a la URL.

`App` 
```jsx
import Nav from './components/Nav';
import About from './components/About';
import Shop from './components/Shop';

import { BrowserRouter as Router, Switch, Route } from 'react-router-dom';

function App() {
	return (
		<Router>
			<Nav />
			<Route path='/about' component={About} />
			<Route path='/shop' component={Shop} />
		</Router>
	);
}


export default App;
```
Si ingresamos a `/about` veremos al componente `About` (además de `Nav` que lo vemos siempre).

Supongamos que mismo en `App` creamos un componente `Home` (que podría estar en un archivo aparte pero por simplicidad lo hacemos ahí) y también agregamos una ruta para `/`

> Con emmet podríamos hacer `Route[path][component]*2`y nos autocompletaría las dos rutas.

```jsx
import Nav from './components/Nav';
import About from './components/About';
import Shop from './components/Shop';

import { BrowserRouter as Router, Switch, Route } from 'react-router-dom';

const Home = () => <h1>Home Page</h1>;

function App() {
	return (
		<Router>
			<Nav />
			<Route path='/' component={Home} />
			<Route path='/about' component={About} />
			<Route path='/shop' component={Shop} />
		</Router>
	);
}

export default App;

```
Si ingresamos a `/` mostraremos el componente `Home`, mientras que si ingresamos a `/about` mostraremos tanto a `About` como a `Home`. Esto ocurre dado que `/about` comienza con `/` y lo toma como un match. Para evitar esto debemos agregarle `exact`
```jsx
return (
	<Router>
		<Nav />
		<Route path='/' exact component={Home} />
		<Route path='/about' component={About} />
		<Route path='/shop' component={Shop} />
	</Router>
);
```
Otra opción es usando `Switch` que importamos de `react-router-dom` y en la medida que hay un matcheo con una URL renderiza un único componente e interrumpe la búsqueda. En ese caso además debemos poner a la ruta en último lugar de modo tal que al entrar a `/about` encuentre primero a la ruta asociada a `About` que la asociada a `Home`.

```jsx
return (
	<Router>
		<Nav />
		<Switch>
			<Route path='/about' component={About} />
			<Route path='/shop' component={Shop} />
			<Route path='/' component={Home} />
		</Switch>
	</Router>
); 
```

Si no queremos tener que mover las rutas podemos agregarle también el `exact`
```jsx
return (
	<Router>
		<Nav />
		<Switch>
			<Route path='/' exact component={Home} />
			<Route path='/about' component={About} />
			<Route path='/shop' component={Shop} />
		</Switch>
	</Router>
);
```
Notar que si por ejemplo tuviera además `<Route path='/shop/cart' component={Cart} />` e ingreso a la dirección `/shop/cart` vería tanto `Shop` como `Cart`, para evitar esto debemos ponerle `exact` también quedando así: `<Route path='/shop' exact component={Shop} />`

## React Router 5
React Router 5 incorpora el uso de hooks.

Antes de React Router 5 utilizábamos la sintaxis de `component` vista hasta ahora donde las llamadas `route props` (de las cuales hablaremos en breve) `match`, `location` y `history` son pasadas implícitamete al componente.
> Para recordar: Note that adding an inline function to the `component` syntax would lead to the component re-mounting on every render.

`<Route path="/" component={Home}  />`
 Mientras que en caso de querer pasarle props extra (cosa que también veremos más adelante) deberíamos hacerlo con `render` y para contar con las **route props** debíamos hacer algo así:
`<Route path='/' render={(props) => <Profile {...props} sortBy='newest'/>} />`

Otra opción:
`<Route path="/" render={({ match })  =>  <Profile match={match} mine={true}  />}>` 

Con React Router 5 en cambio hacemos lo siguiente:
```jsx
<Route path="/">  
	<Home />
</Route>
```

O lo que es lo mismo: `<Route path="/" children={<Home/>} />`

De este modo no hay un pasaje implícito de props al componente `Home` pero como ventaja sin la necesidad de cambiar nada podremos agregar extra props a `Home`. Además ya no es posible cometer el error de re-montar el componente en cada renderizado.
Como ya no contamos con las **route props** pasadas de manera implícita podemos utilizar los hooks que nos ofrece React Router. Por ejemplo `useHistory` para acceder a `history`o `useParams()` para lograr lo que antes hacíamos con `props.params.id` ahora lo hacemos con `let { slug } = useParams();`

# Link
Tenemos registradas nuestras rutas y qué componente debe renderizarse de acuerdo a la URL pero para evitar tener que ingresar manualmente la dirección en la barra de direcciones vamos a implementar la navegación en el componente `Nav`
No la hacemos utilizando elementos `<a>` pues esto ocasionaría un refresco de pantalla cada vez que hacemos click en uno de estos enlaces y si observáramos en las dev tools en la pestaña Network veríamos que se producen *http requests*. Si bien en esta aplicación esto no es un inconveniente en aplicaciones de mayor magnitud puede llegar a serlo. 

Queremos lograr un comportamiento tal que al navegar de una página a la otra en vez de recargar toda la página con todos los assets sólo actualicemos lo que tenemos en un div `.content-area`. Para ello utilizamos el componente `Link` que tiene dentro un *anchor* normal `<a>` y a su vez tiene un handler para el onClick event. Es decir al hacer click en un `Link` el handler es invocado e impide el comportamiento por default del anchor (no enviará requests adicionales) y sólo actualizará la URL. Al coincidir esta URL con la del `path` de `<Route path='' componente=''/>` matchará y renderizará el componente indicado.
Este tipo de aplicaciones reciben el nombre de **SPA** (*single page applications*)
```jsx
import React from 'react';
import { Link } from 'react-router-dom';

function Nav() {
	return (
		<nav>
			<h3>Logo</h3>
			<ul>
				<li>
					<Link to='/'>Home</Link>
				</li>
				<li>
					<Link to='/about'>About</Link>
				</li>
				<li>
					<Link to='/shop'>Shop</Link>
				</li>
			</ul>
		</nav>
	);
}

export default Nav;
```

## Route Props
El componente `Route` actúa como wrapper del componente que le pasamos como prop.

```jsx
<Route path='' component=''/>
```
Si matchea la url va a renderizar el componente inyectándole a su vez las props `history`, `location` y `match`

* `history` podremos utilizarlo para enviar al usuario a una ubicación distinta.

* `location` nos permite saber donde está la app por ejemplo `pathname:'/products'` Es donde veremos los **query string parameters** `location.search`

* `match` nos entrega información de cómo la url matchea el path. `isExact: true` y también veremos los **route parameters** `match.params`

## Pasaje de Props
Si queremos pasarle props adicionales a un componente usando `Route`, en lugar del atributo `component`debemos usar `render`

Vamos a trabajar con un nuevo componente `Products`.

Agregamos un `Link` en el componente `Nav` 
```jsx
<nav>
	<h3>Logo</h3>
	<ul>
		<li>
			<Link to='/'>Home</Link>
		</li>
		<li>
			<Link to='/about'>About</Link>
		</li>
		<li>
			<Link to='/shop'>Shop</Link>
		</li>
		<li>
			<Link to='/products'>Products</Link>
		</li>
	</ul>
</nav>
```

Agregamos la `Route` en `App`
```jsx
<Router>
	<Nav />
	<Switch>
		<Route path='/' exact component={Home} />
		<Route path='/about' component={About} />
		<Route path='/shop' exact component={Shop} />
		<Route path='/products' component={Products} />
	</Switch>
</Router>
```

Creamos un componente `Products` y al navegar a `/products` vemos que recibe las **route props** implícitamente: `{history: {…}, location: {…}, match: {…}, staticContext: undefined}`
```jsx
import React from 'react';

function Products(props) {
	console.log(props);
	return (
		<div>
			<h1>Productos</h1>
		</div>
	);
}

export default Products;
```

Como queremos que reciba adicionalmente el método de ordenamiento de los productos hacemos lo siguiente:
```jsx
<Router>
	<Nav />
	<Switch>
		<Route path='/' exact component={Home} />
		<Route path='/about' component={About} />
		<Route path='/shop' exact component={Shop} />
		<Route path='/shop/:id' component={ItemDetails} />
		<Route path='/products' render={() => <Products sortBy='newest' />} />
	</Switch>
</Router>
```

Si bien logramos recibir correctamente `{sortBy: newest}`, en el console.log podremos notar que dejamos de recibir las otras tres props `history`, `match` y `location`.

Para solucionar esto debemos hacer lo siguiente: `<Route path='/products' render={(props) => <Products sortBy='newest' {...props}/>} />` por lo que nos queda:
```jsx
<Router>
	<Nav />
	<Switch>
		<Route path='/' exact component={Home} />
		<Route path='/about' component={About} />
		<Route path='/shop' exact component={Shop} />
		<Route path='/products' render={() => <Products sortBy='newest' {...props}/>} />
	</Switch>
</Router>
```

## Route Parameters
En ocasiones queremos pasarle parámetros al componente a través de la ruta.

Esto podría ser usado por ejemplo en una tienda si queremos tener una url para cada item `/shop/:id` donde muestre detalles de ese producto en particular.

Utilizando la API de Fortnite https://docs.fortniteapi.com/ y específicamente la ruta `GET Upcoming items` https://fortnite-api.theapinetwork.com/upcoming/get Queremos mostrar en `Shop` una serie de enlaces que al hacer click nos llevan a una página con más información.

La estructura del JSON devuelto por la API es similar a esta:
```json

  "lastUpdate": 0,
  "lanuage": "en",
  "data": [
    {
      "itemId": "ba237c08-321f-4130-a070-03089844c1e9",
      "lastUpdate": 1604588101,
      "item": {
```

En el componente `Shop` queremos mostrar un enlace para elemento de la tienda.
Hacemos uso de los hooks `useState` y `useEffect`

```jsx
import React, { useEffect, useState } from 'react';
import { Link } from 'react-router-dom';
function Shop() {
	const fetchItems = async () => {
		const res = await fetch('https://fortnite-api.theapinetwork.com/upcoming/get');
		const data = await res.json();
		setArr(data.data);
	};

	const [arr, setArr] = useState([]);
	useEffect(() => {
		fetchItems();
	}, []);

	return (
		<div>
			<h1>Shop Page</h1>
			{arr.length > 0? arr.map(item => <li key={item.itemId}>{item.item.name}</li>) : 'CARGANDO'}
		</div>
	);
}

export default Shop;
```

Hasta ahora mostramos en una lista cada uno de los elementos, ahora queremos agregar el componente `Link` (nuevamente no usamos `<a>` para evitar la recarga de la página) y generar una ruta dinámica de modo que al hacer click sobre cada elemento obtengamos más información.

```jsx
return (
	<div>
		<h1>Shop Page</h1>
		{arr.length > 0
			? arr.map(item => (
					<li key={item.itemId}>
						<Link to={`/shop/${item.itemId}`}>
							{item.item.name}
						</Link>
					</li>
			  ))
			: 'CARGANDO'
		}
	</div>
);
```

Debemos definir las rutas de modo que al ingresar por ejemplo a `/shop/ba237c08-321f-4130-a070-03089844c1e9` nos muestre información sobre ese item en el componente `ItemDetails`

En `Nav`

```jsx
<Router>
	<Nav />
	<Switch>
		<Route path='/' exact component={Home} />
		<Route path='/about' component={About} />
		<Route path='/shop' component={Shop} />
		<Route path='/shop/:id' component={ItemDetails} />
	</Switch>
</Router>
```
Sin embargo notamos que si entramos a `/shop/ba237c08-321f-4130-a070-03089844c1e9` muestra en pantalla al componente `Shop` debemos agregar el atributo `exact` `<Route path='/shop' exact component={Shop} />`

Utilizaremos el endpoint `GET Specific Item Data` https://fortnite-api.theapinetwork.com/item/get?id=376c7d95-1316-42a2-90a9-e554711192a0 y suponiendo que quisiéramos obtener los detalles para ese id independientemente de cuál fuera el `/:id` crearíamos un componente `ItemDetails` similar a este:
```jsx
import React, { useEffect, useState } from 'react';

function ItemDetails() {
	const [item, setItem] = useState({});
	useEffect(() => {
		fetchItem();
	}, []);

	const fetchItem = async () => {
		const res = await fetch('https://fortnite-api.theapinetwork.com/item/get?id=376c7d95-1316-42a2-90a9-e554711192a0');
		const data = await res.json();
		setItem(data.data.item);
		console.log(item);
	};

	return <h1>{item.name}</h1>;
}

export default ItemDetails;

```

Sin embargo este hace siempre un request al mismo id independientemente del parámetro `/:id` de la url acorde al elemento clickeado, por lo que debemos trabajar con `props.match.params.id`
```jsx
import React, { useEffect, useState } from 'react';

function ItemDetails(props) {
	const [item, setItem] = useState({});
	useEffect(() => {
		fetchItem();
	}, []);

	const fetchItem = async () => {
		const res = await fetch(`https://fortnite-api.theapinetwork.com/item/get?id=${props.match.params.id}`);
		const data = await res.json();
		setItem(data.data.item);
	};

	return (
		<>
			<h1>{item.name}</h1>;
		</>
	);
}

export default ItemDetails;

```

Si a partir de la variable de estado `item` que por ahora mostramos `item.name` también queremos mostrar una imagen que se encuentra dentro del objeto `item.images` y en particular la imagen `background` es decir `item.images.background` obtendríamos un error `Uncaught TypeError: Cannot read property 'background' of undefined`. 

Lo solucionamos en lugar de poner `const [item, setItem] = useState({});` poniendo `const [item, setItem] = useState({images:{}});` 

Otra posible solución sería dejar la variable de estado como estaba `const [item, setItem] = useState({});` y agregar un operador ternario en el JSX `{item.images ? <img src={item.images.background} /> : ''}`
```jsx
import React, { useEffect, useState } from 'react';

function ItemDetails(props) {
	const [item, setItem] = useState({});
	useEffect(() => {
		fetchItem();
	}, []);

	const fetchItem = async () => {
		const res = await fetch(`https://fortnite-api.theapinetwork.com/item/get?id=${props.match.params.id}`);
		const data = await res.json();
		setItem(data.data.item);
	};

	return (
		<>
            <h1>{item.name}</h1>;
            {item.images ? <img src={item.images.background} /> : ''}
		</>
	);
}

export default ItemDetails;

```

## Parámetros Opcionales
Cuando tenemos 
`<Route path='/post/:year/:month' component={PostDetails} />` 

Estos parámetros son **required** por lo que si no ponemos uno de ellos **la url no matcheará con el path** y no renderizaremos el componente deseado.

Si queremos que estos parámetros sean opcionales debemos agregarle un signo `?` luego del parámetro:

`<Route path='/post/:year?/:month?' component={PostDetails} />` 

## Query String Parameters
A excepción de casos como el mostrado anteriormente donde a pesar de usar parámetros opcionales conservamos una **buena estructura de nuestras urls**, generalmente no es muy recomendable utilizarlos.
Lo aconsejable es usar en cambio **query string paramers** donde a partir de la ruta básica `<Route path='/post' component={PostDetails} />` 

Si ingresamos a `/posts?sortBy=newest&approved=true` en `location.search`
podremos ver un string `"?sortBy=newest&approved=true"`

Para evitar manipular el string manualmente para extraer los valores podremos recurrir a: https://www.npmjs.com/package/query-string

`npm i query-string`

En `Products`
```jsx
import React from 'react';
import queryString from 'query-string';
function Products(props) {
	const result = queryString.parse(props.location.search);
	console.log(result);
	return (
		<div>
			<h1>Productos</h1>
		</div>
	);
}

export default Products;
```

Si ingresamos a `http://localhost:3000/products?sortBy=newest&approved=true` en `result` obtenemos un objeto: `{approved: "true", sortBy: "newest"}` lo que hace que la manipulación sea mucho más sencilla.
Notar que son strings por lo que si esperamos booleans o números tendremos que parsear.

Otra posible solución sin la necesidad de instalar un paquete para manipular los query string parameters es con:
`const sortBy= new URLSearchParams(props.location.search).get('sortBy');`

# Redireccionamiento
Queremos lograr que si el usuario ingresa a una ruta inválida por ejemplo http://localhost:3000/xyz redireccionarlo a otra url donde le digamos que no existe. 

> En nuestro caso mostrará sólo el `Nav`, mientras que si no tuviéramos el atributo `exact` en `<Route path='/' exact component={Home} />` mostraría también el componente `Home`.

Importamos `Redirect` de `react-router-dom` y en el último lugar (sabiendo que si llegó hasta esa posición es que la url no matcheó con ninguna ruta) redireccionamos a `/not-found` y asociamos un `Route` a esa dirección que renderiza el componente `NotFound`
```jsx
<Router>
	<Nav />
	<Switch>
		<Route path='/' exact component={Home} />
		<Route path='/about' component={About} />
		<Route path='/shop' exact component={Shop} />
		<Route path='/shop/:id' component={ItemDetails} />
		<Route path='/products' component={Products} />
		
		<Route path='/not-found' component={NotFound} />
		<Redirect to='/not-found' />
	</Switch>
</Router>
```

`Redirect` también tiene utilidad a la hora de mover recursos de una url a otra:
```jsx
<Redirect from='/messages' to='/posts'/>
```
De modo tal que cuando ingresemos a `/messages` nos redirija a `/posts`

> Con emmet `Redirect[from][to]`

## Programmatic Routing
> En versiones actuales existe el hook [useHistory](https://reactrouter.com/web/api/Hooks/usehistory)
> En ocasiones queremos redireccionar a un usuario cuando hace submit de un formulario por ejemplo si se encuentra en `/products/1` hacia `/products`


Creamos un componente `HomeButton` que al hacer click en un botón, haciendo uso del hook `useHistory()` nos redirecciona a `/`

```jsx
import { useHistory } from 'react-router-dom';

function HomeButton() {
	let history = useHistory();

	function handleClick() {
		history.push('/');
	}

	return (
		<button type='button' onClick={handleClick}>
			Go home
		</button>
	);
}

export default HomeButton;
```

Si nos encontramos en `/products` y hacemos click en el botón `Go Home` nos redirecciona a `/` pero si presionamos back podremos volver a `/products`

En cambio si utilizamos `history.replace('/')` ante la misma situación no podremos volver a `/products`

## Rutas Anidadas
Debemos entender que no debemos usar `Route` sólo en `App.js` sino que podemos usarlo en cualquier componente.

```jsx
const App = () => (
  <BrowserRouter>
    <div>
      <Route path="/tacos" component={Tacos} />
    </div>
  </BrowserRouter>
);

// when the url matches `/tacos` this component renders
const Tacos = ({ match }) => (
  // here's a nested div
  <div>
    {/* here's a nested Route,
        match.url helps us make a relative path */}
    <Route path={match.url + "/carnitas"} component={Carnitas} />
  </div>
);
```

# NavLink
Si cambiamos los elementos `Link` por `NavLink` observando en las *DevTools* tendremos:
```jsx
		<nav>
			<h3>Logo</h3>
			<ul>
				<li>
					<NavLink to='/'>Home</NavLink>
				</li>
				<li>
					<NavLink to='/about'>About</NavLink>
				</li>
				<li>
					<NavLink to='/shop'>Shop</NavLink>
				</li>
				<li>
					<NavLink to='/products'>Products</NavLink>
				</li>
			</ul>
		</nav>
```

```html
<a href="/about">About</a>
```

Si luego hacemos click en uno de ellos veremos que se le aplica automáticamente la clase `active` al link
```html
<a href="/about" aria-current="page" class="active">About</a>
```