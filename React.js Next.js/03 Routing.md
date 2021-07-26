# Routing

El concepto de routing involucra lo relacionado con páginas y navegación. A diferencia de lo que ocurre cuando trabajamos con **create-react-app** donde debemos configurar un paquete de terceros, configurarlo y escribir código para cada ruta, con Next.js tenemos una solución para no tener que preocuparnos por eso.  

La convención establecida por Next.js consiste en que cuando un archivo es agregado dentro de la carpeta `pages` pasa a estar disponible como una ruta. Esto se conoce como routing basado en el filesystem.



## Rutas Estáticas   

Gracias a este **routing basado en el filesystem** si dentro de `pages` creamos una **página básica** llamada `about.js` y en ella un componente funcional, si vamos a `/about` la veremos. 

> Escribir los nombres de los archivos en minúscula, ya que las rutas son *case sensitive* (esto antes no era así fue incorporado en Next.js 10) por lo que si ponemos en la barra de direcciones otra capitalización obtendremos un 404.

Si queremos tener la ruta `/ninjas` tenemos dos opciones:

* Una opción es crear un archivo `ninjas.js`.
* Otra opción es crear un archivo `ninjas` y luego dentro de ella un archivo `index.js` de manera tal que para acceder a esa ruta sigue siendo necesario ingresar a `/ninjas`, pero tenemos la estructura ya armada para cuando trabajemos con rutas anidadas. 

>Esto es útil si tenemos pensado luego agregar más cosas en esa carpeta por ejemplo si tenemos pensado tener una ruta `/ninjas/1` veremos luego que para esto será necesario una archivo `[id].js` dentro de `ninjas`.



## Rutas Anidadas

Las rutas anidadas en Next.js pueden lograrse simplemente anidando carpetas dentro de `pages`.

Por ejemplo si queremos mostrar una página cuando el usuario navega a `localhost:3000/blog` y otra cuando navega a `localhost:3000/blog/first` y `localhost:3000/blog/second` como dijimos anteriormente tenemos dos formas de hacerlo:

* La primera es crear un archivo `blog.js` y una carpeta `blog` y dentro de ella dos archivos  `first.js` y `second.js`. 
* La segunda es crear una carpeta `blog` y en ella tres archivos `index.js`, `first.js` y `second.js`. Esta es la más recomendada porque nos quedan los archivos relacionados juntos.

## Rutas Dinámicas

En aplicaciones complejas, muchas veces no es posible definir las rutas con paths predefinidos. En aquellos casos en que las rutas son dependientes de variables como por ejemplo `/product/123` se las conoce como **rutas dinámicas**.

Si queremos mostrar un listado de productos en `/product` y detalles de un producto cuando va `/product/1`, `/product/2`, etc, lo cual se conoce como *list-detail pattern*

Lo primero que se nos puede ocurrir es crear la carpeta `product`, luego dentro de ella un archivo`index.js` (exportamos un componente `ProductList` ) y otro para cada página `1.js`, `2.js`, etc. Claramente si tenemos que mostrar muchos productos esto no será una alternativa eficiente. En Next.js podemos crear una ruta dinámica agregando corchetes al nombre del archivo. Es decir que creamos el archivo  `[productId].js` y dentro el un componente funcional como el que mostramos a continuación.

```jsx
import React from 'react';
import { useRouter } from 'next/router';

const ProductDetail = () => {
	const router = useRouter();
	return (
		<div>
			<h1>Producto {router.query.productId}</h1>
		</div>
	);
};

export default ProductDetail;
```

> Para utilizar dentro del componente el valor del id de la ruta dinámica debemos utilizar el hook `useRouter` 

> A la hora de mostrar el componente `ProductDetails` Next.js no hace distinción si el valor recibido luego de `/product/` es un número, sino que si ingresamos a `/product/sweater` también mostrará esta ruta dinámica a menos que dentro de `/product` tengamos un archivo `sweater.js` en cuyo caso matcheará primero con ese componente. 

> Si queremos trabajar con **query parameters** es decir que tenemos `/car/Juan?age=2021` recibiremos también `router.query.age`, mientras que si tenemos  `/car/Juan?age=2021&age=2022` recibiremos un array `age` con esos dos valores.

> Si queremos simplificar la notación y en lugar de tener `router.query.productId` usar `productId` podemos utilizar *nested object destructuring* 

```js
const {
	query: {productId}
} = useRouter();
```

> Con `useRouter` podremos realizar también navegaciones programáticas.

### Rutas dinámicas anidadas

Si queremos tener una ruta dinámica anidada o de  **mas de un nivel** del estilo `/product/1` y `/product/1/review/1` dentro de `pages` debemos crear una carpeta `product` y luego otra **carpeta** `[productId]`, dentro de ella una carpeta `review` y por último un archivo `[reviewId].js`. El archivo que antes se llamaba `[productId].js` lo metemos en `[productId]` y lo llamamos `index.js`.

En `[reviewId].js` podemos acceder a los parámetros dinámicos con `router.query.productId` y `router.query.reviewId`

> Otra forma mucho más simple de ver esto es si queremos tener las rutas  `/car/Juan` o `/avion/Ocho` debemos crear dentro de `pages` una carpeta `[vehicle]` y dentro de ella un archivo `[person].js` y luego en `const router = useRouter();` tendremos `router.query.vehicle` y `router.query.person`. 



### Catch-all Route

Next.js tiene una característica de routing llamada *catch-all routes* ideal para cuando tenemos muchas urls que muestran el mismo componente, el caso típico es la documentación de un proyecto donde tenemos direcciones de tipo `/docs/feature1/concept1` con múltiples *features* y múltiples *concepts*  en cada *feature*. Lógicamente queremos tener una ruta única para cada tema. Otra situación en la cual resulta de utilidad es cuando queremos pasar *filter parameters* para una página, por ejemplo una página de inmuebles donde queremos filtrar por presupuesto, la url sería `/houses/100000/1000000` y recibiríamos ambos valores siendo posible el filtrado, es decir en una situación donde recibimos parámetros opcionales.

Suponiendo 20 *features* y 20 *concepts* serían necesarios 400 rutas, como hemos visto no serán necesarios 400 archivos sino que podemos utilizar rutás dinámicas y haría falta crear dentro de pages una carpeta`[featureId]` y un archivo `[conceptId].js` (sin contar los archivos para renderizar los componentes de las rutas incompleta). Además si queremos agregar un nivel por ejemplo `/docs/feature1/concept1/example1` hará falta un nuevo nivel de anidamiento carpeta `[featureId]`, carpeta `[conceptId]` y archivo `[exampleId].js` (también sin contar los archivos para poder resolver las rutas intermedias). En situaciones como estas donde todas las páginas tendrán el mismo layout podemos hacer uso de la característica llamada *catch-all routes* que es un archivo que matchea con todas las rutas. Creamos una carpeta `docs` y dentro un archivo `[...params].js` (por convención se le suele dar este nombre) y dentro definimos un componente. Como consecuencia de esto matcheará con todos los paths que comiencen con `/docs/...` 

Como ventaja de esta característica tenemos los distintos segmentos de la url para mejor organización y SEO, pero con un único layout.

Renderizaremos el mismo componente sin ingresamos a cualquiera de estas direcciones:

* `/docs/feature1`
* `/docs/feature1/concept1`
* `/docs/featuire1/concept1/example1`



El acceso a los distintos segmentos lo logramos a través de `router.query.params` que es una array con cada uno de los segmentos.

```
import {useRouter} from 'next/router'
function Doc(){
    const router = useRouter();
    const {params} = router.query;
    console.log(params);
    return(
    if (params.length === 2) {
        return (
        	<h1>Viewing docs for feature {params[0]} and concept {params[1]}</h1>
        )
  	} 
  	else if (params.length === 1) {
		return (
			<h1>Viewing docs for feature {params[0]}</h1>
  		)
  	}
	return <h1>Docs Home Page</h1>
    )
}
export default Doc;
```

Si ingresamos a `/docs/feature1/concept1/example1` el valor de `params` será `[feature1, concept1, example1]` y veremos como título "Docs Home Page". Como podemos ver en base al análisis de `length` del `params` mostrarmos un contenido u otro. 



Hasta el momento si navegamos a `/docs` obtendremos 404, si queremos mostrar también el componente debemos cambiar el nombre de `[...params].js` a `[[params]].js`.



> En la consola veremos que inicialmente `params` es `undefined` debido al pre-rendering. Para evitar inconvenientes podemos inicializarlo como una array vacío para evitar eventuales errores `const {params =[]} = router.query;`

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



## Enlazando Páginas
Hasta ahora implementamos el filesystem routing tanto con rutas estáticas, anidadas, dinámicas, catch-all routes, etc pero hasta ahora hemos navegado las páginas ingresando la url en la barra de direcciones. Típicamente el usuario realizará la navegación a una ruta distinta, haciéndole click a un elemento de la UI o bien será redirigido programáticamente luego de cierta acción.

Queremos realizar lo que se conoce como *client side navigation* sin enviar *requests* al servidor, para ello Next.js cuenta con el componente `Link`. Si los enlaces los implementáramos con tags `<a>` cada vez que hagamos click en uno de ellos se producirá una solicitud al servidor (lo cual visualmente podremos detectar como una recarga o un parpadeo), como queremos evitar esto usamos el componente `Link`. 

> Otra forma de entender que todo está renderizado antes de llegar al navegador es si agarramos las dev tools y le cambiamos el `background` al `body` por ejemplo, veremos que este permanecerá cuando naveguemos en todas las páginas (desapareciendo cuando refresquemos).
>
> Para la navegación creamos una carpeta `components`, dentro de ella una carpeta `Navbar` por último dentro de ella un archivo `Navbar.js` con el componente en si.

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

> También sería posible navegar a rutas dinámicas colocando `href="/product/1"`o si recibiéramos el id como prop podríamos hacer *string interpolation* ``href={`/product/${productId}`} ``
>
> En versiones antiguas de Next.js además de `href` era necesario poner `as`

Vemos entonces la diferencia que existe entre el routing con el servidor y el routing del lado del cliente en el cual logramos un comportamiento que se conoce como SPA *(single page application)*.

Con el agregado del componente `Link` se produce otra optimización que se conoce como **prefetching automático de recursos**, para ver esto `yarn build` y `yarn start`. Estando en la página `/` veremos que se cargan de manera anticipada recursos de `about` logrando de esta manera una navegación mucho más rápida hacia esa página. 

El componente Link se utiliza sólo para client side routing es decir navegación dentro de la aplicación mientras que si queremos navegar a un sitio externo debemos usar `<a>`.

### `replace`

En el componente `<Link>` tenemos la prop `replace` que reemplaza la historia en lugar de agregar una url en el stack, por lo que si navegamos desde `/products` a `/products/3` y presionamos **back** iremos a home en lugar de volver a `/products`.



## Navegación Programática

La mayoría de las veces realizaremos la navegación entre las distintas páginas utilizando el componente `Link`. Sin embargo, en ocasiones puede que necesitemos redireccionar al un usuario programáticamente hacia alguna página por algún motivo.

Una opción podría ser un redireccionamiento unos segundos después de mostrar la página 404 cuando el usuario ingresa a una ruta inexistente.

A continuación implementamos esa situación en el archivo `pages/404.js` y para ello usamos el hook `const router = useRouter()` de next y luego en el `useEffect` utilizamos un timer con `setTimeOut` y con `router.push('/')` hacemos el redireccionamiento a `/`. 

Si quisiéramos navegar hacia atrás en la historia podríamos utilizar `router.go(-1)` o bien `router.go(1)` si quisiéramos navegar hacia adelante.

```jsx
import Link from 'next/link'
import { useEffect } from 'react'
import { useRouter } from 'next/router'

const NotFound = () => {
  const router = useRouter()

  useEffect(() => {
    setTimeout(() => {
      // router.go(-1)
      // router.go(1)
      router.push('/')
    }, 3000)
  }, [])

  return (
    <div className="not-found">
      <h1>Ooops...</h1>
      <h2>That page cannot be found :(</h2>
      <p>Going back to the <Link href="/"><a>Homepage</a></Link> is 3 seconds...</p>
    </div>
  );
}
 
export default NotFound;
```



Otra opción podría ser en un sitio de e-commerce cuando el usuario presiona el botón "Finalizar Compra" y es redirigido a una ruta con el listado de los artículos comprados.

```jsx
	<button onClick={handleClick}>Finalizar Compra</button>
```



Luego en `handleClick`:

```
const handleClick = () => {
	router.push('/product')
}
```



> Es posible navigar programáticamente a rutas estáticas, dinámicas, anidadas e incluso a *catch-all routes*.



### `replace`

De manera similar a lo visto con `<Link>` donde teníamos la prop `replace` , en la navegación programática contamos con el método `router.replace('/product')`que reemplaza la historia en lugar de agregar una url en el stack. 



# Página 404

Si intentamos navegar a cualquier ruta que no esté definida en la carpeta `pages`, veremos una página 404 por default. Es posible mostrar una página personalizada creando en `pages` el archivo `404.js`. Luego como es de esperar este componente se mostrará en pantalla automáticamente cada vez que ingresemos a una url que no tenga una página asociada:

```jsx
import React from 'react';
import Link from 'next/link';
const NotFound = () => {
  return (
    <div>
      <h1>Ooooops...</h1>
      <h2>Página No Encontrada</h2>
      <p>
        Ir al{' '}
        <Link href="/">
          <a>Inicio</a>
        </Link>
      </p>
    </div>
  );
};

export default NotFound;

```



## Custom Page Extensions

Cuando trabajamos con unit tests necesitaremos crear archivos similares a `index.spec.js` y no queremos que Next.js haga una ruta a partir de ellos como por ejemplo http://localhost:3000/index.spec

Es posible configurar las extensiones que serán analizadas en el directorio `pages` a la hora de resolver páginas. Para eso en `next.config.js` agregamos `pageExtensions`

```js
module.exports = {
  pageExtensions: ['mdx', 'jsx', 'js', 'ts', 'tsx'],
}
```

Como indica la [documentación](https://nextjs.org/docs/api-reference/next.config.js/custom-page-extensions) tener presente que esto afecta a `_document.js`, `_app.js` y a los archivos dentro de `pages/api/`.


Por lo tanto si en cambio colocamos como `pageExtensions: ['page.js']` los archivos deberían ser renombrados a `_document.page.tsx`, `_app.page.tsx`, `about.page.js`, etc  y lo mismo en `api` 

