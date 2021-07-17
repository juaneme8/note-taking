# Routing

El concepto de routing involucra lo relacionado con páginas y navegación. A diferencia de lo que ocurre cuando trabajamos con create-react-app por ejemplo en Next.js tenemos una solución para no tener que preocuparnos por eso.  Cada página en Next.js está representada por un componente React.

Existen dos formas de hacer routing que se conocen como **rutas estáticas**  y  **rutas dinámicas** 

## Rutas Estáticas
Next.js va a hacer un **routing basado en el filesystem** es decir que si dentro de `pages` creamos una **página básica** llamada `about.js` y en ella un componente funcional, si vamos a `/about` la veremos. 

> Escribir los nombres de los archivos en minúscula, ya que las rutas son *case sensitive* (esto antes no era así fue incorporado en Next.js 10) por lo que si ponemos en la barra de direcciones otra capitalización obtendremos un 404.

Si queremos tener la ruta `/ninjas` una opción es crear un archivo `ninjas.js`, sin embargo también es posible trabajar con una carpeta dentro de `pages` que llamamos `ninjas` y luego dentro de ella un archivo `index.js` de manera tal que para acceder a esa ruta ingresemos a `/ninjas`. 

>Esto es útil si tenemos pensado luego agregar más cosas en esa carpeta por ejemplo si tenemos pensado tener una ruta `/ninjas/1` veremos luego que para esto será necesario una archivo `[id].js` dentro de `ninjas`.

## Rutas Dinámicas
Las rutas dinámicas son aquellas dependientes de variables como por ejemplo `/product/123`. 

Si queremos mostrar una página dinámica cuando el usuario vaya a `/product/123` debemos crear la carpeta `product` y dentro de ella un archivo `[productId].js`.

> Las páginas dinámicas se crean utilizando la siguiente sintaxis `[variable].js` siendo variable el nombre con el que quiero representar a ese parámetro.

Para capturar lo que le estemos pasando como id de la ruta dinámica debemos utilizar el hook `useRouter` 

```jsx
import React from 'react';
import { useRouter } from 'next/router';

const ProductItem = () => {
	const router = useRouter();
	return (
		<div>
			<h1>Producto {router.query.productId}</h1>
		</div>
	);
};

export default ProductItem;
```

> Si queremos tener una ruta dinámica con mas de un nivel del estilo `/car/Juan` o `/avion/Ocho` debemos crear dentro de pages una carpeta `[vehicle]` t dentro de ella un archivo `[person].js` y luego en `const router = useRouter();` tendremos `router.query.vehicle` y `router.query.person`. 

> Si queremos trabajar con query parameters es decir que tenemos `/car/Juan?age=2021` recibiremos también `router.query1.age`, mientras que si tenemos  `/car/Juan?age=2021&age=2022` recibiremos un array `age` con esos dos valores.

> Si queremos simplificar la notación y en lugar de tener `router.query.productId` usar `productId` podemos utilizar *nested object destructuring* 

```js
const {
	query: {productId}
} = useRouter();
```

> Con `useRouter` podremos realizar también navegaciones programáticas.

### Rutas Dinámicas y APIs

Es posible trabajar con **rutas dinámicas** por ejemplo la API route `pages/api/user/[id].js`

```js
export default (req, res) => {
	const { id } = req.query;
	if (id) {
		res.statusCode = 200;
		res.json({ id });
	} else {
		res.sendStatus(400);
	}
};
```

Como consecuencia de esto al hacer un *request* a `http://localhost:3000/api/user/:id` obtendremos un json con el id.

## Routing *«under the hood»*
Cuando generamos el build para producción con `yarn build` veremos datos acerca de las páginas que hemos creado, pudiendo caer en las siguientes categorías:

* (Server): server-side renders at runtime (uses `getInitialProps` or `getServerSideProps`)
* (Static): **automatically rendered as static HTML (uses no initial props)**.
* (SSG): automatically generated as static HTML + JSON (uses `getStaticProps`)
* (ISR): incremental static regeneration (uses revalidate in `getStaticProps`)



En nuestro caso todo lo ha generado con el algoritmo de forma estática (segunda categoría). Además veremos que Next.js nos crea múltiples archivos de JavaScript en lo que se conoce como **code splitting** y veremos la estrategia también de **hash assets** para solucionar el tema de caché.

:warning: Se recomienda realizar curso optimización web

Corremos el servidor node para **producción** con `yarn start` si vamos a Network en las DevTools veremos que Next.js nos hace el **code splitting** y cada página tiene su chunk específico `2d57ec00e72a16...` además también veremos otros paquetes que son reutilizados por todas las páginas como `webpack-c2126667a5f965...` o `framework.c6faa22799416a6...`. Con esto notamos que que Next.js ya ha realizado optimizaciones de manera automática.
En modo de **desarrollo** también veremos este proceso ya que cada página tiene su propio *javascript bundle* que obtenemos cuando la navegamos por primera vez. 

Otra optimización que hace Next.js es el **prerendering**  o **SSR** *(server side rendering)* lo cual podemos verificar (luego de ejecutar `yarn build` y `yarn start`). Si abrimos la dirección home `localhost:3000` y hacemos click derecho en "Ver código fuente" (o bien en Response en Network) veremos que el contenido principal de la página (el texto Hola Mundo) ya viene incluido en el html de la respuesta. Esto significa que no hay *client side rendering* y todo lo que estamos viendo ya viene del servidor. **Esto es muy importante en términos de SEO** (search engine optmization) como ya todo viene prerenderizado es mucho más amigable para los motores de búsqueda y también a la hora de compartir la página en redes sociales ya que la información estará disponible.



## Enlazando Páginas
Para vincular páginas entre sí utilizamos el componente `Link` proveniente de Next.js. 

Para la navegación creamos una carpeta `components`, dentro de ella una carpeta `Navbar` por último dentro de ella un archivo `Navbar.js` con el componente en si.

Queremos realizar lo que se conoce como *client side navigation* sin enviar *requests* al servidor. Si los enlaces los implementáramos con tags `<a>` cada vez que hagamos click en uno de ellos se producirá una solicitud al servidor (lo cual visualmente podremos detectar como una recarga o un parpadeo), como queremos evitar esto usamos el componente `Link`. 

> Otra forma de entender que todo está renderizado antes de llegar al navegador es si agarramos las dev tools y le cambiamos el `background` al `body` por ejemplo, veremos que este permanecerá cuando naveguemos en todas las páginas (desapareciendo cuando refresquemos).

```jsx
import React from 'react';
import Link from 'next/Link';
const Navbar = () => {
	return (
		<ul>
			<li>
				<Link href='/'>
					<a>Home</a>
				</Link>
			</li>
			<li>
				<Link href='/about'>
					<a>About</a>
				</Link>
			</li>
		</ul>
	);
};

export default Navbar;
 
```

> En versiones antiguas de Next.js además de `href` era necesario poner `as`

Vemos entonces la diferencia que existe entre el routing con el servidor y el routing del lado del cliente en el cual logramos un comportamiento deque se conoce como SPA *(single page application)*.

Con el agregado del componente `Link` se produce otra optimización que se conoce como **prefetching automático de recursos**, para ver esto `yarn build` y `yarn start`. Estando en la página `/` veremos que se cargan de manera anticipada recursos de `about` logrando de esta manera una navegación mucho más rápida hacia esa página. 

## Custom Page Extensions
Cuando trabajamos con unit tests necesitaremos crear archivos similares a `index.spec.js` y no queremos que Next.js haga una ruta a partir de ellos : http://localhost:3000/index.spec

Es posible configurar las extensiones que serán analizadas en el directorio `pages` a la hora de resolver páginas. Para eso en `next.config.js` agregamos `pageExtensions`

```js
module.exports = {
  pageExtensions: ['mdx', 'jsx', 'js', 'ts', 'tsx'],
}
```

Como indica la [documentación](https://nextjs.org/docs/api-reference/next.config.js/custom-page-extensions) tener presente que esto afecta a `_document.js`, `_app.js` y a los archivos dentro de `pages/api/`.


Por lo tanto si en cambio colocamos como `pageExtensions: ['page.js']` los archivos deberán tener ese nombre `about.page.js`  y lo mismo en `api` 

```js
module.exports = {
  pageExtensions: ['page.js'],
}
```