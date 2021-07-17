# Next.js

> Basado en el curso de Platzi de Next.js, es posible clonar un [repositorio](https://www.github.com/jonalvarezz/platzi-nextjs/tree/main) del autor con los pasos iniciales utilizando TypeScript.
> Basado en la lista de reproducción de [The Net Ninja](https://www.youtube.com/watch?v=A63UxsQsEbU&ab_channel=TheNetNinja).
> Basado en algunos videos del canal de [Self Teach Me](https://www.youtube.com/watch?v=DWCCkcJgvf0&ab_channel=SelfTeachMe).

> Contenido extra:
> [Next.js + TailwindCSS](https://betterprogramming.pub/how-to-set-up-next-js-with-tailwind-css-b93ccd2d4164)

# Introducción
Construir una aplicación en React demanda muchas decisiones por ejemplo **qué tipo de CSS vamos a utilizar** (CSS in JS o global CSS), **qué bundler vamos a utilizar** si usaremos Webpack o Rollup. Una vez decidido esto debemos configurar **Babel** para que funcione con ellos. Lo mismo si trabajamos con **TypeScript** debemos configurar todo lo anterior en función de esto. Si usamos **imágenes** también debemos tener en cuenta varios aspectos. 

Normalmente estas decisiones las tomamos buscando optimizar nuestro entorno de **desarrollo** pero quizás no son óptimas para **producción**. Por ejemplo tener Webpack y Babel configurado de cero por nosotros nos genera lo que se conoce como *total cost of ownership* que da cuenta de **cuanto le cuesta a tu empresa** el tiempo que gastas en mantener ese framework o arquitectura que vos mismo creaste **y si esto representa un valor agregado para el usuario**.

Resumiendo React al ser una **librería** de UI nos deja la responsabilidad de definir el entorno en el que la vamos a operar, lo cual también representa una debilidad pues ¿cómo sabemos si el entorno construido es el correcto? **Next.js** es un **framework** de React.js creado por Vercel que ya ha tomado estas decisiones por nosotros constituyendo un ambiente optimizado tanto para desarrollo como para producción. 


# Pre-rendering
Una de las características más destacadas de Next.js es el pre-rendering, es decir que nos permite crear sitios prerenderizados. Esto significa que los componentes de React fueron renderizados en HTML para el momento en que llegan al navegador. **A partir de ese momento la página se comporta como una SPA** y el routing lo podemos manejar del lado cliente en el navegador.  

Como contraste de esto tenemos el CSR *(client side rendering)* donde los componentes son renderizados en HTML en el navegador. Si en lugar de Next.js utilizamos create-react-app que tiene CSR, el usuario verá una pantalla en blanco durante unos segundos al actualizar la página, mientras que con Next.js verá el contenido de manera inmediata. 

Cuando utilizamos `useEffect()` es el navegador quien realiza el fetch de los datos (inicialmente recibimos una página en blanco lo cual podemos constatar en Network yendo a Preview), mientras que en Next.js gracias a `getStaticProps` y `getServerSideProps` obtendremos los datos en el servidor por lo que recibimos la página con información.

> Si en una aplicación creada con CRA deshabilitamos JavaScript veremos una página en blanco ya que se construye en el cliente. Sin embargo, si utilizamos Next.js como hacemos un pre-rendering, podremos verla sin tener JavaScript habilitado. 

Expresando esto mismo de manera técnica decimos que Next.js tiene un **FCP** (*first contentful paint*) más rápido. En este [artículo](https://web.dev/user-centric-performance-metrics/) encontramos más información al respecto. 

Next.js nos ofrece tanto SSR *(server side rendering)* , SSG *(static site generation)* y ISR (*Incremental static regeneration*).

# Ventajas
* **mejora en la performance** ya que los componentes están pre-renderizados eso implica menos trabajo en el navegador.

* **mejoras en términos de SEO** *(search engine optimization)*.

* **mejor información disponible a la hora de compartir enlaces** de las páginas. Con Next.js gracias al SSR tendremos información del título de la página, imágenes, etc que no tendríamos si usamos CRA. Si Googleamos a [facebook debugger](https://developers.facebook.com/tools/debug/) podremos simular cómo se verán los enlaces cuando sean compartidos.

* **Creación de REST API de manera sencilla**

* Como estamos ejecutando código en el servidor, estaremos trabajando en un **entorno seguro** (podemos conectarnos a la DB usando credenciales sin que sean visibles para el usuario). 

* Cuenta con **ISR** posibilita generar los sitios estáticos que pueden ser regenerados luego de un tiempo.

* Tenemos soporte para *CSS in JS* incorporado utilizando Style JSX.

* No requiere configuración para contar con *code splitting* (cada ruta la carga sólo cuando es necesitada) y muchas otras optimizaciones.

* **Simplicidad al deployar** en Vercel 

* **next/head** para hacer el SEO y modificar el `title`, `description`, etc

 * Soporte de **TypeScript**

* Sirve **archivos estáticos** de manera muy simple.

* **Variables de entorno** (tanto para el servidor como para el navegador).

* **Fast Refresh** nos permite que al hacer un cambio lo veremos reflejado casi de manera instantánea en la aplicación.

  
# SSR, SSG y ISR
Con Next.js podemos realizar SSR, SSG, ISR.

Podemos ver más información en esta [página](https://www.netlify.com/blog/2020/12/02/next.js-should-i-use-ssr-or-ssg/) 

## SSR
SSR es cuando los componentes de React son renderizados en páginas HTML en el servidor, luego de un request del navegador es decir  **at run-time**. 
Tenemos la **ventaja** de que los datos estarán siempre actualizados.



## SSG
SSG es cuando los componentes de React son renderizados en páginas HTML en el momento de desplegar la aplicación, es decir **at build-time**. Tenemos la ventaja de que el TTFB (*time to first byte*) es más rápido.



## ISR

ISR es una implementación híbrida que nos permite actualizar contenido estático sin la necesidad de hacer *rebuild* de todo el sitio cuando cambiamos algo. Es una alternativa ideal para aprovechar las ventajas de las páginas estáticas y a la vez escalar a millones de páginas.



> Resumen de [Guía de ISR por Lee Robinson](https://www.smashingmagazine.com/2021/04/incremental-static-regeneration-nextjs/). 
>
> Trabajar con SSG, nos aporta sitios estáticos pre-renderizados que pueden ser subidos a CDN y disponibles globalmente en poco tiempo y además tenemos contenido que puede ser accedido muy rápido e indexado también rápidamente a los crawlers. Sin embargo tiene algunas desventajas. En sitios estáticos de gran escala, podrá llevar horas esperar el *build* del sitio. En caso de duplicar la cantidad de páginas también duplicaría el tiempo requerido. Es por esto que fue necesaria una solución híbrida como ISR. 
>
> Considerando el caso de un equipo donde el código está desacoplado del contenido ya que usan un Headless CMS, el cambio de un precio en un artículo demandaría el *rebuild* de todo el sitio lo cual podría llevar horas. ISR apunta a interpretar donde fueron los cambios y realizar una actualización incremental sin la necesidad de hacer un *rebuild* entero.

# Instalación

Existen dos formas de instalar Next.js una automática y otra manual, ambas pueden ser vistas en la [documentación](https://nextjs.org/docs)

La manera **automática** es con `npx create-next-app stream-it` y luego de ingresar este comando Next nos va a crear una serie de carpetas y archivos con la estructura del proyecto. 

> El nombre del proyecto no podrá tener letras mayúsculas, es por eso que usamos **kebab-case**

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
> Si hacemos un cambio en el componente veremos que este se actualizará en el navegador gracias al **hot reloading** y al [**fast refresh**](https://nextjs.org/docs/basic-features/fast-refresh) (cambios visibles en menos de un segundo sin perder el estado del componente).



## Limpieza Inicial

* En `pages/index.js` borramos el `footer`, editamos `main` borrando el contenido y la clase de modo que el componente nos queda así:

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

* Luego en `styles` en `Home.module.css` borramos todo menos los estilos asignados a la única clase que tenemos es decir`.container`



## Levantar Proyecto

Contamos con dos builds uno de desarrollo y uno de producción: Usamos `npm run dev` cuando estemos trabajando de manera **local** y `npm start` para **producción**.




# Estructura de Archivos
## `pages`

* En Next.js trabajamos con un **routing basado en el file system**, por lo que cada archivo que tengamos en `pages` representará una ruta de la página.  Es decir que si creamos un archivo `about.js` en `pages` con el siguiente contenido:

```jsx
function About() {
  return <div>About</div>
}

export default About
```

Cuando ingresamos a una `/about`  podremos acceder a ese componente.

*  Carpeta `api` es para crear una API para eso cualquier archivo ubicado dentro de `pages/api` será mapeado con la url `/api/*` y tratado como un endpoint de la API en lugar de como una página.

    Por ejemplo si en `pages/api` tenemos el siguiente archivo `hello.js`:

  ```js
  export default (req, res) => {
    res.statusCode = 200
    res.json({ name: 'John Doe' })
  }
  ```

  Luego cuando ingresemos a `http://localhost:3000/api/hello` obtendremos el JSON `{ name: 'John Doe' }`

  

## `index.js`

`index.js` representa la página home.

## `_app.js`

`_app.js` que es el root component y los distintos *page componentes* son renderizados allí:

```jsx
const App= ({ Component, pageProps }) => {
	return (
			<Component {...pageProps} />
	);
};
export default App;
```

> Si tenemos el server corriendo y este archivo no existe y lo creamos, tendremos que reiniciarlo.

## `public`

Next.js puede [servir archivos estáticos](https://nextjs.org/docs/basic-features/static-file-serving) como imágenes, si las colocamos dentro de la carpeta `public` en el directorio raíz. Los archivos dentro de esta carpeta podrán ser accedidos desde el código comenzando con la URL base `/`
Por ejemplo para trabajar con `/public/img/logo.svg` en `components/Logo` directamente lo referenciamos como `'/img/logo.svg`.

## `styles`

Es donde tenemos los archivos `.css` y los `module.css`

En Next.js por default trabajamos con **CSS Modules** a fin de poder tener los mismos nombres de clases en múltiples componentes sin que se pisen. De esta manera puedo tener selectores menos específicos o utilizar nombres de clases comunes como `.container` más de una vez asignándole estilos distintos. Si a estos elementos los visualizamos en las DevTools veríamos que al nombre de la clase le agrega un **hash** de modo que sea único. 
Por otra parte si queremos reutilizar un estilo en más de un componente podemos utilizar la hoja de estilos `global.css`.

## `components`

Cuando tenemos componentes que no son páginas como [metodología de trabajo](https://www.youtube.com/watch?v=DWCCkcJgvf0&ab_channel=SelfTeachMe) podemos crear una carpeta `components` fuera de `pages` y en ella al luego una carpeta para cada componente por ejemplo `Tabs` donde tendremos `Tabs.js`, sus estilos `Tabs.css` y sus archivos de testing `Tabs.test.js`.




### Default Exports vs Named Exports
A la hora de trabajar con `components` una posible metodología sería utilizar **named exports** en todas partes para mantener la consistencia (para en un futuro no dudar si este componente es un **named export** o **default export**), por lo que el componente nos quedará así:

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

Si queremos simplificar un poco los imports esto podemos configurar **imports absoluto**s con el archivo `jsconfig.json`.

### `props.children` de elementos HTML:

Supongamos que tenemos un componente `Tabs` al cual le pasamos como `children` una serie de elementos HTML, en este caso tres `div` con contenido anidado dentro de ellos.

```jsx
<Tabs>
	<div label='Tab 1'>
		<h2>Tab 1</h2>
		<p>Lorem ipsum dolor sit amet, consectetur adipisicing elit, sed do eiusmod tempor incididunt.</p>
	</div>
	<div label='Tab 2'>
		<h2>Tab 2</h2>
		<p>Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat.</p>
	</div>
	<div label='Tab 3'>
		<h2>Tab 3</h2>
		<p>Sunt in culpa qui officia deserunt mollit anim id est laborum. Lorem ipsum dolor sit amet.</p>
	</div>
</Tabs>
```

Como es de esperar en `Tabs` recibimos este contenido via la prop `children` que será un array por lo que recorriéndolo con `map()` podremos acceder a cada uno de esos `div` y a sus props (por lo que con `one.props.label` accedemos a `<div label='Tab 3'>` mientras que con `one.props.children` accedemos al `h2` y `p`)

```jsx
<div>
	{children.map(one => (
		<div key={one.props.label}>{one.props.children}</div>
	))}
</div>
```



# Estilos en Next.js

Next.js tiene soporte para **CSS Modules** que nos permite tener los mismos nombres de clases en múltiples componentes sin que se pisen. De esta manera puedo tener selectores menos específicos o utilizar nombres de clases comunes como `.container` más de una vez asignándole estilos distintos. Si a estos elementos los visualizamos en las devtools veríamos que al nombre de la clase le agrega un **hash** de modo que sea único. 

> Si queremos darle formato a un componente  `Navbar` creamos en `styles` un archivo `Navbar.module.css`.
>
> Si queremos reutilizar un estilo en más de un componente podemos utilizar la hoja de estilos `global.css`.



# Routing
El routing es la navegación entre páginas y en Next.js tenemos una solución para no tener que preocuparnos por eso. 

Existen dos formas de hacer routing que se conocen como **rutas estáticas**  y  **rutas dinámicas** 

## Rutas Estáticas
Next.js va a hacer un **routing basado en el filesystem** es decir que si dentro de `pages` creamos una **página básica** llamada `about.js` y en ella un componente funcional, si vamos a `/about` lo veremos. 

## Rutas Dinámicas
Las rutas dinámicas son aquellas dependientes de variables como por ejemplo `/product/123`. 

Si queremos mostrar una página dinámica cuando el usuario vaya a `/product/123` debemos crear en `pages` la carpeta `product` y dentro de ella un archivo `[productId].js`.

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



Las **rutas dinámicas** también son aplicables a endpoints de la API, por ejemplo si creamos un archivo en `pages/api/user/[id].js`

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

Como consecuencia de esto al hacer un *request* a `http://localhost:3000/api/user/:id` obtendremos un json con el id ingresado.



## Routing *«under the hood»*

Cuando generamos el build para producción con `yarn build` veremos datos acerca de las páginas que hemos creado, pudiendo caer en las siguientes categorías:

* (Server) server-side renders at runtime
* (Static) **automatically rendered as static HTML**.
* (SSG): automatically generated as static HTML.

En nuestro caso todo lo ha generado con el algoritmo de forma estática (segunda categoría). Además veremos que Next.js nos crea múltiples archivos de JavaScript en lo que se conoce como **code splitting** y veremos la estrategia también de **hash assets** para solucionar el tema de caché.

:warning: Se recomienda realizar curso optimización web 

Corremos el servidor node para producción con `yarn start` si vamos a Network en las DevTools veremos que Next.js nos hace el code splitting y cada página tiene su chunk específico `2d57ec00e72a16...` además también veremos otros paquetes que son reutilizados por todas las páginas como `webpack-c2126667a5f965...` o `framework.c6faa22799416a6...`. Con esto notamos que que Next.js ya ha realizado optimizaciones de manera automática.

Otra optimización que hace Next.js es el **prerendering**  o **SSR** *(server side rendering)* lo cual podemos verificar (luego de ejecutar `yarn build` y `yarn start`). Si abrimos la dirección home `localhost:3000` y hacemos click derecho en "Ver código fuente" (o bien en Response en Network) veremos que el contenido principal de la página ya viene incluido en el HTML de la respuesta. Esto significa que no hay *client side rendering* y todo lo que estamos viendo ya viene del servidor. **Esto es muy importante en términos de SEO** (search engine optmization) como ya todo viene prerenderizado es mucho más amigable para los motores de búsqueda.

## Enlazando Páginas
Para vincular páginas entre sí utilizamos el componente `Link` proveniente de Next.js. 

Para la navegación creamos una carpeta `components`, dentro de ella una carpeta `Navbar` por último dentro de ella un archivo `Navbar.js` con el componente en si.

Si los enlaces los implementáramos con tags `<a>` cada vez que hagamos click en uno de ellos se producirá una solicitud al servidor (lo cual visualmente podremos detectar como una recarga o un parpadeo), como queremos evitar esto usamos el componente `Link`.

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
Con el agregado del componente `Link` se produce otra optimización que se conoce como **prefetching automático de recursos**, para ver esto `yarn build` y `yarn start`. Estando en la página `/` veremos que se cargan de manera anticipada recursos de `about` logrando de esta manera una navegación mucho más rápida hacia esa página. 







