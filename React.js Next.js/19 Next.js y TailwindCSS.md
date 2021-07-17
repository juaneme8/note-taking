# Pokedex
## Next.js
## TailwindCSS
> Basado en el [video](https://youtu.be/LMRAEUPkFXI) de James Quick. 
> Basado en el [blog post](https://medium.com/better-programming/how-to-set-up-next-js-with-tailwind-css-b93ccd2d4164) para la Introducción a Next.js con Tailwind - [repositorio](https://github.com/jamesqquick/nextjs-pokedex-with-tailwind-css)
>
> Para este proyecto utilizaremos Next.js tanto con **SS (*static site generation*) para el listado de los pokmones y **SSR** (*server side enerin*) para la página con los detalles de un okmon en particular, para evitar tener que hacer 15 páginas estáticas.

Utilizaremos la API: 
https://pokeapi.co/api/v2/pokemon
https://pokeapi.co/api/v2/pokemon?limit=150

`npm create-next-app pokedex`

## Limpieza Inicial
Como parte del clean up inicial
1.  Eliminamos la carpeta `api`.
2. Limpiamos `index.js` de modo que nos quede:
```jsx
import Head from 'next/head'

export default function Home() {
  return (
    <div>
      <Head>
        <title>Create Next App</title>
        <link rel="icon" href="/favicon.ico" />
      </Head>

      <main>
      <h1>Welcome to Next</h1>
      </main>

    
    </div>
  )
}

```
Estamos trabajando con `Head` que es un *built-in component* con el cual podemos agregar elementos al `head` de la página.

De esta manera al ejecutar `npm run dev` veremos en pantalla simplemente un encabezado `h1`.

3. En carpeta `styles` por un lado eliminamos `Home.module.css` y por otra parte limpiamos `globals.css` que utilizaremos más adelante.

## Configuración Tailwind CSS en Next.js
Nos vamos a basar en [este artículo](https://dev.to/notrab/get-up-and-running-with-tailwind-css-and-next-js-3a73), no al pie de la letra pero sí en varios aspectos:

1. Instalar dependencias
```bash
npm i tailwindcss postcss-preset-env
```

2. Crear archivo `tailwind.config.js` con el siguiente comando:
```bash
npx tailwindcss init
```

3. Crear el archivo `postcss.config.js` y pegar dentro:
```js
module.exports = {
  plugins: ['tailwindcss', 'postcss-preset-env'],
}
```

4. En `styles/globals.css` pegar:

```
@tailwind base;
@tailwind components;
@tailwind utilities;
```

Ahora veremos en pantalla que el `h1` ya no tiene el tamaño habitual producto del reseteo propio de Tailwind.

# Primeros Pasos
Creamos en la raíz una carpeta `components` y dentro de ella `Layout.js`

En `index.js` utilizamos `Layout` que es un contenedor que recibe como `props.title` un texto para mostrar en `title` mediante `Head` y también como `props.children` un `h1`
```jsx
import Layout from '../components/Layout';

export default function Home() {
  return (
		<div>
			<Layout title='NextJS Pokedex'>
				<h1>Next.js Pokedex</h1>
			</Layout>
		</div>
  );
}

```

En `Layout.js`
```jsx
import React from 'react';
import Head from 'next/head';

export default function Layout({ title, children }) {
	return (
		<div className='bg-gray-100'>
			<Head>
				<title>{title}</title>
				<link rel='icon' href='/favicon.ico' />
			</Head>
			<main className='container mx-auto max-w-xl pt-8 min-h-screen'>{children}</main>
		</div>
	);
}

```
Para el `main` utilizamos las clases:
* `container`: que equivale a `width: 100%`
* `mx-auto` que equivale a `margin-left: auto;` `margin-right: auto;`
* `max-w-xl` que equivale a `max-width: 36rem;`
* `pt-8` que equivale a `padding-top: 2rem;`
* `min-h-screen` que equivale a `min-heigth: 100vh`
* 

# `getStaticProps` y `getServerSideProps`
En las páginas de Next (en `pages/index.js` por ejemplo) podemos exportar dos tipos de funciones que indicarán si la página debe ser SSG o SSR.

Con `async function getStaticProps(context)` Next.js sabrá que debe ser una página estática y *at build-time* correrá esta función y buscará los datos de la API y se la pasará a la página `index` de modo que sea una página estática para cuando la descargue el cliente.

Realizamos el fetch en la url https://pokeapi.co/api/v2/pokemon?limit=150 y obtendremos algo similar a  esto:
```json
{
	"count": 1050,
	"next": "https://pokeapi.co/api/v2/pokemon?offset=150&limit=150",
	"previous": null,
	"results": [
		{
		"name": "bulbasaur",
		"url": "https://pokeapi.co/api/v2/pokemon/1/"
		},
		...
		...
	]
}
```

Como los datos deseados están en el array `results` hacemos object destucturing `const { results } = await res.json();`.
También necesitamos presentar una imagen y como vimos recién dentro de `results` sólo tenemos objetos con `name` y `url`. Si ingresamos a pokemon.com en particular a https://www.pokemon.com/us/pokedex/bulbasaur al copiar la dirección de la imagen veremos que es del tipo https://assets.pokemon.com/assets/cms2/img/pokedex/full/001.png. El número del final coincide con los obtenidos por en pokeapi.co. Debemos asegurar que el elemento 0 del array se corresponda con 001 y que el elemento (y a su vez si el número es mayor que 10 sólo quedarnos con los últimos 3 dígitos) eso lo hacemos con `const paddedIndex = ('00' + (index + 1)).slice(-3);`

```jsx
export async function getStaticProps(context) {
	try {
		const res = await fetch('https://pokeapi.co/api/v2/pokemon?limit=150');
		const { results } = await res.json();

		const pokemon = results.map((result, index) => {
			const paddedIndex = ('00' + (index + 1)).slice(-3);
			const image = `https://assets.pokemon.com/assets/cms2/img/pokedex/full/${paddedIndex}.png`;
			return {
				...result,
				image,
			};
		});
		return {
			props: { pokemon },
		};
	} catch (err) {
		console.error(err);
	}
}
```

En el componente recibimos esto como props: `export default function Home({ pokemon }) {...}` y si quisiéramos mostrar los nombres de los pokemones bastaría con hacer un `map()` como el que vemos a continuación:
```jsx
<ul>
	{pokemon.map((pokeman, index)  =>  (
		<li key={index}>
			{pokeman.name}
		</li>
	))}
</ul>
```

## Client-side Routing
Como queremos mostrar el nombre y un enlace a los detalles del Pokemon importamos el componente `Link`.

```jsx
<ul>
	{pokemon.map((pokeman, index)  =>  (
		<li key={index}>
			<Link href={`/pokemon?id=${index+1}`}>
				<a>
					<img src={pokeman.image} alt={pokeman.name}/>
					<span>{index+1}</span>
					{pokeman.name}
				</a>
			</Link>
		</li>
	))}
</ul>
```

Cuando hacemos click en alguno de estos enlaces vemos que la página no existe por lo que obtenemos 404.

## Aplicando Estilos
Queremos obtener cada pokemon ocupando toda una fila con el siguiente orden: foto - número - nombre.
```jsx
<ul>
	{pokemon.map((pokeman, index) => (
		<li key={index}>
			<Link href={`/pokemon?id=${index + 1}`}>
				<a className='flex border border-gray-400 p-4 my-2 capitalize items-center bg-gray-200 rounded-md'>
					<img className='w-20 h-20 mr-3' src={pokeman.image} alt={pokeman.name} />
					<span className='mr-2 font-bold'>{index + 1}</span>
					{pokeman.name}
				</a>
			</Link>
		</li>
	))}
</ul>
```

## Página Detalles
Creamos las páginas a las cuales nos redireccionará al presionar una imagen. En `pages` creamos el componente `pokemon.js` se trata de una página en la cual vamos a mostrar los detalles y será SSR.

La url con la que vamos a trabajar será del tipo `/pokemon?id=1` y utilizamos la función `getServerSideProps` que recibe el objeto `context` y hacemos destructuring para quedarnos con el objeto `query`

Esto es distinto a si quiéramos trabajar con la url `/pokemon/1` ya que en ese caso tendríamos una carpeta `pokemon` y dentro un archivo `[id].js` en `getServerSideProps` recibiríamos también `context` pero estaríamos interesados en la propiedad `params` en particular en `context.params.id`

```jsx
import React from 'react';
import Layout from '../components/Layout';
import Link from 'next/Link';
export default function pokemon({ pokeman }) {
    return (
        <Layout title={pokeman.name}>
            <h1 className="text-4xl mb-2 text-center capitalize">
                {pokeman.id}. {pokeman.name}
            </h1>
            <img className="mx-auto" src={pokeman.image} alt={pokeman.name} />
            <p>
                <span className="font-bold mr-2">Weight:</span> {pokeman.weight}
            </p>
            <p>
                <span className="font-bold mr-2">Height:</span>
                {pokeman.height}
            </p>
            <h2 className="text-2xl mt-6 mb-2">Types</h2>
            {pokeman.types.map((type, index) => (
                <p key="index">{type.type.name}</p>
            ))}
            <p className="mt-10 text-center">
                <Link href="/">
                    <a className="text-2xl underline">Home</a>
                </Link>
            </p>
        </Layout>
    );
}

export async function getServerSideProps({ query }) {
    const id = query.id;
    try {
        const res = await fetch(`https://pokeapi.co/api/v2/pokemon/${id}`);
        const pokeman = await res.json();
        const paddedId = ('00' + id).slice(-3);
        pokeman.image = `https://assets.pokemon.com/assets/cms2/img/pokedex/detail/${paddedId}.png`;
        return {
            props: { pokeman },
        };
    } catch (err) {
        console.error(err);
    }
}

```

Si ejecutamos `npm run build` nos indicará que en `/` tenemos SSG y en `/pokemon` SSR