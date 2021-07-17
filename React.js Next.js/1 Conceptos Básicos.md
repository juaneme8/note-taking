:blue_heart: Falta revisar bastante

# Next.js

> Basado en el curso de Platzi de Next.js, es posible clonar un [repositorio](https://www.github.com/jonalvarezz/platzi-nextjs/tree/main) del autor con los pasos iniciales utilizando TypeScript.
> Basado en la lista de reproducción de [The Net Ninja](https://www.youtube.com/watch?v=A63UxsQsEbU&ab_channel=TheNetNinja).
> Basado en algunos videos del canal de [Self Teach Me](https://www.youtube.com/watch?v=DWCCkcJgvf0&ab_channel=SelfTeachMe).
> Basado en el [blog post](https://medium.com/better-programming/how-to-set-up-next-js-with-tailwind-css-b93ccd2d4164) para la Introducción a Next.js con Tailwind
> Basado en el [directo de Twitch](https://www.youtube.com/watch?v=q2nVV5OYnl4&t=1087s&ab_channel=JuaniGallo) de Juani Gallo

> Contenido extra:
> [Next.js + TailwindCSS](https://betterprogramming.pub/how-to-set-up-next-js-with-tailwind-css-b93ccd2d4164)

# Introducción
Construir una aplicación React demanda muchas decisiones por ejemplo **qué tipo de CSS vamos a utilizar** (CSS in JS o global CSS), **qué bundler vamos a utilizar** si usaremos Webpack o Rollup. Una vez decidido esto debemos configurar **Babel** para que funcione con ellos. Lo mismo si trabajamos con **TypeScript** debemos configurar todo lo anterior en función de esto. Si usamos **imágenes** también debemos tener en cuenta varios aspectos. 
Normalmente estas decisiones las tomamos buscando optimizar para nuestro entorno de desarrollo pero quizás no son óptimas para producción (por ejemplo tener un Webpack y Babel configurado de cero por nosotros, que nadie mas tiene). Existe el concepto de *total cost of ownership* que da cuenta de **cuanto le cuesta a tu empresa** el tiempo que gastas en mantener ese framework o arquitectura que vos mismo creaste **y si esto representa un valor agregado para el usuario**.
**React** al ser una librería nos deja la responsabilidad de definir el entorno en el que la vamos a operar, lo cual también representa una debilidad pues ¿cómo sabemos si el entorno construido es el correcto? **Next.js** es un framework de React.js creado por Vercel que ya ha tomado estas decisiones por nosotros constituyendo un ambiente optimizado tanto para desarrollo como para producción. 


# Pre-rendering
Nos permite crear sitios en React prerenderizados. 

Gracias a esto lo componentes de React fueron renderizados en HTML para el momento en que llegan al navegador. **A partir de ese momento la página se comporta como una SPA** y el routing lo podemos manejar del lado cliente en el navegador. 

Como contraste de esto tenemos el *client side rendering* donde los componentes son renderizados en html en el navegador. Si comparamos a lo que sucede con create-react-app donde el usuario verá una pantalla en blanco durante unos segundos al actualizar la página, utilizando Next.js verá el contenido de manera inmediata. Decimos que tiene un **FCP** (*first contentful paint*) más rápido. En este [artículo](https://web.dev/user-centric-performance-metrics/) encontramos más información al respecto. De hecho si deshabilitamos JavaScript en las aplicaciones creadas con CRA veremos una página en blanco ya que se construye en el cliente. Con Next.js hacemos un pre-rendering por lo que podremos verla sin tener JavaScript habilitado. 
Cuando utilizamos `useEffect()` es el navegador quien realiza el fetch de los datos (inicialmente recibimos una página en blanco lo cual podemos constatar en Network yendo a Preview), mientras que en Next.js gracias a `getStaticProps` y `getServerSideProps` obtendremos los datos en el servidor por lo que recibimos la página con información.

Next.js nos ofrece tanto SSR (*server side rendering)* como SSG (*static site generation*).

# Ventajas
* Dentro de las ventajas de SSR y SGG nos encontramos con una **mejora en la performance** (ya están pre-renderizados eso implica menos trabajo en el navegador) y **mejoras en términos de SEO** (*search engine optimization).
* Tendremos información disponible a la hora de compartir enlaces de las páginas. Con Next.js gracias al SSR tendremos información del título de la página, imágenes, etc que no tendríamos si usamos CRA. Si Googleamos a [facebook debugger](https://developers.facebook.com/tools/debug/) podremos simular cómo se verán los enlaces cuando sean compartidos.
* No es necesario configurar Express.

* Como estamos ejecutando código en el servidor, estaremos trabajando en un **entorno seguro** (podemos conectarnos a la DB usando credenciales sin que sean visibles para el usuario). 

* Cuenta con ISR (*Incremental static regeneration*) posibilita generar los sitios estáticos que pueden ser regenerados luego de un tiempo.

* Tenemos soporte para *CSS in JS* incorporado utilizando Style JSX.

* No requiere configuración inicial para contar con *code splitting* (cada ruta la carga sólo cuando la necesita cosa que si quisiéramos implementar manualmente demandaría cierto trabajo) y muchas otras optimizaciones. Si realizamos SSR con Express tendríamos que hacerlas manualmente no lo cual no sería nada fácil.

* Creación de REST API de manera sencilla

* Excelente integración al deployar en Vercel

* next/head para hacer el SEO y modificar el `title`, `description`, etc

 * Soporte de TypeScript

* Sirve archivos estáticos de manera muy simple.

* Variables de entorno (tanto para el servidor como para el navegador).

* Fast Refresh nos permite que al hacer un cambio lo veremos reflejado casi de manera instantánea en la aplicación.

  
# SSR vs SSG
Con Next.js podemos realizar tanto SSR como SSG (también veremos una tercera forma que es un híbrido de estas dos llamada **ISR** (*incremental static regeneration*).

Podemos ver más información en esta [página](https://www.netlify.com/blog/2020/12/02/next.js-should-i-use-ssr-or-ssg/) 

## SSR
SSR es cuando los componentes de React son renderizados en páginas HTML en el servidor, luego de un request del navegador es decir  **at runtime**. 
Tenemos la **ventaja** de que los datos estarán siempre actualizados.



## SSG
SSG es cuando los componentes de React son renderizados en páginas html en el momento de desplegar la aplicación, es decir **at build time**. Tenemos la ventaja de que el TTFB (*time to first byte*) es más rápido.

# Instalación

Existen dos formas de instalar Next.js una automática y otra manual, ambas pueden ser vistas en la [documentación](https://nextjs.org/docs)

La manera **automática** es con `npx create-next-app stream-it` y luego de ingresar este comando Next nos va a crear una serie de carpetas y archivos con la estructura del proyecto. El nombre del proyecto no podrá tener letras mayúsculas, es por eso que usamos **kebab-case**

La forma **manual** consiste en crear un directorio sobre el cual queremos trabajar `md-nextjs`, navegar hacia el `cd md-nextjs` y luego crear un proyecto de npm con `npm init -y` para así obtener un `package.json` y por último instalar las dependencias o paquetes que vamos a utilizar `yarn add next react react-dom`

Luego agregamos los scripts que necesita Next:

```json
"scripts": {
  "dev": "next dev",
  "build": "next build",
  "start": "next start"
}
```

A continuación creamos un directorio `pages` y dentro de el una página `index.js` allí creamos un componente funcional que llamamos `Home`. Luego si ejecutamos `yarn dev`  y vamos a `localhost:3000` veremos esa página en pantalla.

```jsx
import React from 'react';

const Home = () => {
	return (
		<div>
			<h1>Hola Mundo!</h1>
		</div>
	);
};

export default Home;
```

> Con el snippet `rafce` creamos un componente funcional basado en una *arrow function*, como regla mnemotécnica si escribimos `race` nos lo sugerirá.
>
> Si hacemos un cambio en el componente veremos que este se actualizará en el navegador gracias al **hot reloading** y al [**fast refresh**](https://nextjs.org/docs/basic-features/fast-refresh) (incorporado recientemente por React y ya soportado por Next.js)



## Limpieza Inicial

En `pages/index.js` borramos el `footer`, el contenido y la clase de `main`casi todo de modo que nos quede así:

```jsx
import Head from 'next/head'
import styles from '../styles/Home.module.css'

export default function Home() {
  return (
    <div className={styles.container}>
      <Head>
        <title>React Tabs</title>
        <link rel="icon" href="/favicon.ico" />
      </Head>
      <main></main>
    </div>
  )
}
```

Luego en `styles` en `Home.module.css` borramos todo menos los estilos asignados a la única clase que tenemos `.container`



## Levantar Proyecto

Contamos con dos builds uno de desarrollo y uno de producción: Usamos `npm run dev` cuando estemos trabajando de manera **local** y `npm start` para **producción**.




# Estructura
## `pages`

En Next.js trabajamos con un **routing basado en el file system**, por lo que cada archivo que tengamos en `pages` representará una ruta de la página

* Carpeta `api` es para endpoints de la API. Es posible crear una API con Next.js para eso cualquier archivo ubicado dentro de `pages/api` será mapeado a a la url `/api/*` y tratado como un endpoint de la API en lugar de como una `page`.

  Por ejemplo si en `pages/api` tenemos el siguiente archivo `hello.js`:

```js
export default (req, res) => {
  res.statusCode = 200
  res.json({ name: 'John Doe' })
}
```

​		Luego cuando ingresemos a `http://localhost:3000/api/hello` obtendremos el JSON `{ name: 'John Doe' }`

* `index.js` representa la página home.

* `_app.js` que es el root component:

```jsx
const App= ({ Component, pageProps }) => {
	return (
			<Component {...pageProps} />
	);
};
export default App;
```

> Si tenemos el server corriendo y este archivo no existe y lo creamos, tendremos que reiniciarlo.



* `components`

Creamos una carpeta `components` en la raíz del proyecto. Dentro de esa carpeta podemos crear los componentes que necesitemos.



* `public`

Next.js como indica la [documentación](https://nextjs.org/docs/basic-features/static-file-serving) puede **servir archivos estáticos** como ser imágenes, si las colocamos dentro de la carpeta `public` en el directorio raíz. Los archivos dentro de esta carpeta podrán ser accedidos desde el código comenzando con la URL base `/`
Por ejemplo para trabajar con `/public/img/logo.svg` en `components/Logo` directamente lo referenciamoso como `'/img/logo.svg`

```jsx
export default function Logo() {
	return <img src="/img/logo.svg" />;
}
```

Luego usamos el componente `Logo` dentro del componente `Navbar`

```jsx
import Logo from './Logo';

export default function Navbar() {
	return (
		<nav>
			<Logo />
		</nav>
	);
}

```

Por último en `pages/index.js`

```jsx
import Navbar from '../components/Navbar';
export default function Home() {
	return (
		<>
			<Navbar />
		</>
	);
}

```



## Static File Serving

https://nextjs.org/docs/basic-features/static-file-serving

**Because a site is bundled and generated at build-time, your users don’t have to wait for a page to load** or generate at runtime. They simply navigate to the page, and no code has to be run for the page to show up!

The biggest downside to static sites (and where SSR shines) **is the long build times when you have hundreds (if not thousands) of pages** that you have to render. If you have a ton of pages and the content is dynamic, it is something that can lead to both many builds, and long builds.



En Next.js por default trabajamos con **CSS Modules** a fin de poder tener los mismos nombres de clases en múltiples componentes sin que se pisen. De esta manera puedo tener selectores menos específicos o utilizar nombres de clases comunes como `.container` más de una vez asignándole estilos distintos. Si a estos elementos los visualizamos en las DevTools veríamos que al nombre de la clase le agrega un **hash** de modo que sea único. 
Por otra parte si queremos reutilizar un estilo en más de un componente podemos utilizar la hoja de estilos `global.css`.

Creamos una carpeta `components` y luego una carpeta por ejemplo `Tabs` donde tendremos los archivos del componente en sí, sus estilos y archivos de testing.
A continuación creamos el archivo `Tabs.js`  y con `rfce` creamos un componente funcional, podemos borrar `import React from 'react'` ya que Next incluye React de manera automática.  SelfTeachMe sugiere utilizar **named exports** en todas partes para mantener la consistencia (para en un futuro no dudar si este componente es un **named export** o **default export**), por lo que el componente nos quedará así:

```jsx
import React from 'react'

function Tabs() {
    return (
        <div>
          Tabs  
        </div>
    )
}

export { Tabs }

```

Luego lo importamos en `/pages/index.js` con `import { Tabs } from '../components/Tabs/Tabs';` **notar que esto es muy engorroso** mientras que si al archivo en lugar de `Tabs.js` lo hubiésemos llamado `index.js` directamente podríamos haberlo importado con `import { Tabs } from '../components/Tabs';` pero tendríamos la desventaja de tener **muchos archivos con el mismo nombre** y al tenerlos abiertos no saber cuál es cual.

Como solución a esto podemos crear en `/components/Tabs` un archivo `index.js` con el siguiente contenido:

```jsx
import { Tabs } from './Tabs';
export { Tabs };
```

De esta manera luego en `index.js` podremos importar con `import { Tabs } from '../components/Tabs'`

Si queremos simplificar un poco los imports esto podemos configurar imports absolutos con el archivo `jsconfig.jsonLuego utilizamos el componente:

```jsx
<Tabs>
	<div label='Tab 1'>
		<h2>Tab 1</h2>
		<p>
			Lorem ipsum dolor sit amet, consectetur adipisicing elit, sed do eiusmod tempor incididunt ut labore et dolore magna
		</p>
	</div>
	<div label='Tab 2'>
		<h2>Tab 2</h2>
		<p>
			Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur.
		</p>
	</div>
	<div label='Tab 3'>
		<h2>Tab 3</h2>
		<p>
			Sunt in culpa qui officia deserunt mollit anim id est laborum. Lorem ipsum dolor sit amet, consectetur adipisicing elit.
		</p>
	</div>
</Tabs>
```

Luego en `Tabs` recibimos esto via la prop `children`

```jsx
<div>
	{children.map(one => (
		<div key={one.props.label}>{one.props.children}</div>
	))}
</div>
```

En este caso veremos que`children` son los elementos `div` y con el map referenciamos a cada uno de ellos con `one`, por lo que con `one.props.label` accedemos a `<div label='Tab 3'>` mientras que con `one.props.children` accedemos al `h2` y `p`

Luego creamos `Tabs.module.css` donde colocaremos los estilos es este componente.




# Estructura de Archivos
## `pages`

En Next.js trabajamos con un routing basado en el file system, por lo que cada archivo que tengamos en `pages` representará una ruta de la página.  Es decir que si creamos `pages/about.js` con el siguiente contenido:

```jsx
function About() {
  return <div>About</div>
}

export default About
```

Cuando ingresamos a una `/about`  podremos acceder a ese componente.

Carpeta `api` es para endpoints de la API.

`index.js` representa la página home.

`_app.js` que es el root component y los distintos *page**CSS Modules** a fin de poder tener los mismos nombres de clases en múltiples componentes* soin renderizados allí:

```jsx
const App= ({ Component, pageProps }) => {
	return (
			<Component {...pageProps} />
	);
};
export default App;
```


## `public`

Next.js puede [servir archivos estáticos](https://nextjs.org/docs/basic-features/static-file-serving) como imágenes, si las colocamos dentro de la carpeta `public` en el directorio raíz. Los archivos dentro de esta carpeta podrán ser accedidos desde el código comenzando con la URL base `/`
Por ejemplo para trabajar con `/public/img/logo.svg` en `components/Logo` directamente lo referenciamoso como `'/img/logo.svg`.

## `styles`
Es donde tenemos los archivos `.css` y los `module.css`

En `package.json` nos encontramos que tenemos el script `dev` que utilizaremos para correr un *local development server* por lo que ejecutamos `npm run dev`

## `components`
Creamos en una carpeta `components` fuera de `pages` y luego una carpeta para cada uno de ellos por ejemplo `Tabs` donde tendremos los archivos del componente en sí, sus estilos y archivos de testing.

## `components`

> Basado en [este video](https://www.youtube.com/watch?v=DWCCkcJgvf0&ab_channel=SelfTeachMe)

Aquellos componentes  que no son páginas los colocque se pisen. De esta manera puedo tener selectores menos específicos o utilizar nombres de clases comunes como `.container` más de una vez asignándole estilos distintos. Si a estos elementos los visualizamos en las DevTools veríamos que al nombre de la clase le agrega un **hash** de modo que sea único. 
Por otra parte si queremos reutilizar un estilo en más de un componente podemos utilizar la hoja de estilos `global.css`.

Creamos en una carpeta `components` fuera de `pages` y luego una carpeta para cada uno de ellos por ejemplo `Tabs` donde tendremos los archivos del componente en sí, sus estilos y archivos de testing. 




### Default Exports vs Named Exports
> Basado en [este video](https://www.youtube.com/watch?v=DWCCkcJgvf0&ab_channel=SelfTeachMe)

A la hora de trabajar con `components` una posible metodología sería utilizar **named exports** en todas partes para mantener la consistencia (para en un futuro no dudar si este componente es un **named export** o **default export**), por lo que el componente nos quedará así:
```jsx
export default function Logo() {
	return <img src="/img/logo.svg" />;
}
```
Luego usamos el componente `Logo` dentro del componente `Navbar`

```jsx
import ReactLogo from 'react'

./Logo';

export default function Navbar() {
    	return (
        <div>
          Tabs  
        </div>
    )
}

export { Tabs }

```

Luego lo importamos		<nav>
			<Logo />
		</nav>
	);
}

Por último en `/pages/index.js` con `import { Tabs } from '../components/Tabs/Tabs';` **notar que esto es muy engorroso** mientras que si al archivo en lug

```jsx
import Navbar from '../components/Navbar';
export default function Home() {
	return (
		<>
			<Navbar />
		</>
	);
}

```

# Estilos en Next.js

Para darle estilos a `Navbar` creamos en `styles` un archivo `Navbar.module.css` Trabajamos con **CSS Modules** a fin de poder tener los mismos nombres de clases en múltiples componentes sin que se pisen. De esta manera puedo tener selectores menos específicos o utilizar nombres de clases comunes como `.container` más de una vez asignándole estilos distintos. Si a estos elementos los visualizamos en las devtools veríamos que al nombre de la clase le agrega un **hash** de modo que sea único. 
Por otra parte si queremos reutilizar un estilo en más de un componente podemos utilizar la hoja de estilos `global.css`.



# Estilos

Para de `Tabs.js` lo hubiésemos llamado `index.js` directamente podríamos haberlo importado con `import { Tabs } from '../components/Tabs';` pero tendríamos la desventaja de tener **muchos archivos con el mismo nombre** y alarle estilos a `Navbar` creamos en `styles` un archivo `Navbar.module.css` Trabajamos con **CSS Modules** a fin de poder tener los abiertos no saber cuál es cual.

Como solución a esto podemos crear en `/components/Tabs` un archivo `index.js` con el siguiente contenido:
```jsx
import { Tabs } from './Tabs';
export { Tabs };
```
mismos nombres de clases en múltiples componentes sin que se pisen. De esta manera lpuegdo ten `index.js` podremos importar con `import { Tabs } from '../components/Tabs'`

Si queremos simplificar un poco los imports esto podemos configurar imports absolutos con el archivo `jsconfig.json`

# `props.children` de elementos HTML:
Supongamos que tenemos un componente `Tabs` al cual le pasamos como `children` una serie de elementos html, en este caso tres `div` con contenido anidado dentro de ellos.
```jsx
<Tabs>
	<div label='Tab 1'>
		<h2>Tab 1</h2>
		<p>
			Lorem ipsum dolor sit amet, consectetur adipisicing elit, sed do eiusmod tempor incididunt.
		</p>
	</div>
	<div label='Tab 2'>
		<h2>Tab 2</h2>
		<p>
			Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat.
		</p>
	</div>
	<div label='Tab 3'>
		<h2>Tab 3</h2>
		<p>
			Sunt in culpa qui officia deserunt mollit anim id est laborum. Lorem ipsum dolor sit amet.
		</p>
	</div>
</Tabs>
```

Como es de esperar en `Tabs` recibimos este contenido via la prop `children` que será un array por lo que recorriéndolo con `map()` podremos acceder a cada uno de esos `div` y a sus props (en este caso `label`).

```jsx
<div>
	{children.map(one => (
		<div key={one.props.label}>{one.props.children}</div>
	))}
</div>
```
En este caso veremos que`children` son los elementos `div` y coner selectores menos específicos o utilizar nombres de clases comunes como `.container` más de una vez asignándole estilos distintos. Si a estos elementos los visualizamos en las devtools veríamos que al nombre de la clase le agrega un **hash** de modo que sea único. 
Por otra parte si queremos reutilizar un estilo eln map referenciamos a cada uno de ellos con `one`, por lo que con `one.props.label` accedemos a `<div label='Tab 3'>` mientras que con `one.props.children` accedemos al `h2` y `pás de un componente podemos utilizar la hoja de estilos `global.css`.Durante **desarrollo** ante cada request los componentes serán pre-renderizado en el servidor y luego se nos envía el html al navegador es decir SSR, en **producción** en cambio podemos tener las páginas renderizadas *at build time* en lo que se conoce como SSG.



# Routing
Este concepto involucra lo relacionado con páginas y navegación y en Next.js tenemos una solución para no tener que preocuparnos por eso. 

Existen dos formas de hacer routing que se conocen como **rutas estáticas**  y  **rutas dinámicas** 

## Rutas Estáticas
Next.js va a hacer un **routing basado en el filesystem** es decir que si dentro de `pages` creamos una **página básica** llamada `about.js` y en ella un componente funcional, si vamos a `/about` la veremos. 

## Rutas Dinámicas
Las rutas dinámicas son aquellas dependientes de variables como por ejemplo `/user/juaneme8`. 

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

> Si queremos simplificar la notación y en lugar de tener `router.query.productId` usar `productId` podemos utilizar *nested object destructuring* 

```js
const {
	query: {productId}
} = useRouter();
```

## Rutas Dinámicas

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

* (Server) server-side renders at runtime
* (Static) **automatically rendered as static HTML**.
* (SSG): automatically generated as static HTML.

En nuestro caso todo lo ha generado con el algoritmo de forma estática (segunda categoría). Además veremos que Next.js nos crea múltiples archivos de JavaScript en lo que se conoce como **code splitting** y veremos la estrategia también de **hash assets** para solucionar el tema de caché.

== Se recomienda realizar curso optimización web ==

Corremos el servidor node para producción con `yarn start` si vamos a Network en las DevTools veremos que Next.js nos hace el code splitting y cada página tiene su chunk específico `2d57ec00e72a16...` además también veremos otros paquetes que son reutilizados por todas las páginas como `webpack-c2126667a5f965...` o `framework.c6faa22799416a6...`. Con esto notamos que que Next.js ya ha realizado optimizaciones de manera automática.

Otra optimización que hace Next.js es el **prerendering**  o **SSR** *(server side rendering)* lo cual podemos verificar (luego de ejecutar `yarn build` y `yarn start`). Si abrimos la dirección home `localhost:3000` y hacemos click derecho en "Ver código fuente" (o bien en Response en Network) veremos que el contenido principal de la página (el texto Hola Mundo) ya viene incluido en el html de la respuesta. Esto significa que no hay *client side rendering* y todo lo que estamos viendo ya viene del servidor. **Esto es muy importante en términos de SEO** (search engine optmization) como ya todo viene prerenderizado es mucho más amigable para los motores de búsqueda.

## Enlazando Páginas
Hasta el momento tenemos páginas sin posibilidad de comunicarse entre sí, por lo que colocaremos los enlaces para vincularlas. Utilizaremos como API o componente principal a `Link` proveniente de Next.js. 

Para la navegación creamos una carpeta `components`, dentro de ella una carpeta `Navbar` por último dentro de ella un archivo `Navbar.js` con el componente en si.

Si los enlaces los implementáramos con tags `<a>` cada vez que hagamos click en uno de elos se producirá una solicitud al servidor (lo cual visualmente podremos detectar como una recarga o un parpadeo), como queremos evitar esto usamos el componente `Link`.

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
Vemos entonces la diferencia que existe entre el routing con el servidor y el routing del lado del cliente que se conoce como SPA *(single page application)*.

Con el agregado del componente `Link` se produce otra optimización que se conoce como **prefetching automático de recursos**, para ver esto `yarn build` y `yarn start`. Estando en la página `/` veremos que se cargan de manera anticipada recursos de `about` logrando de esta manera una navegación mucho más rápida hacia esa página. 







