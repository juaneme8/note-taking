# E-Commerce
## Next.js
## TypeScript
## Chakra-UI
## Google Sheets

> Basado en el stream de [GonzyPozzo](https://www.twitch.tv/videos/9720258141782457)

Se trata de un ecommerce con TypeScript, Next.js, ChakraUI y Google Sheets como medio para proporcionar los productos y su precio.

# Elección de Next.js
* **Server Side Rendering (SSR)** significa que una instancia de nuestra aplicación va a correr en un servidor que es un entorno seguro (podemos conectarnos a nuestra db con las credenciales  sin que sean visibles para el usuario). Además uando entramos a una página y el servidor se encarga de procesar toda la información esta se ve mucho más rápido en pantalla que si fuera el cliente quién debe comunicarse con un backend, este le da los datos y el los debe bajar y recién ahi los muestra en pantalla.

* **Incremental static generation:** posibilita generar los sitios estáticos que pueden ser regenerados luego de un tiempo.
* 
# Google Sheets
Obtendremos la información de google sheets en formato `.csv`, luego tendremos que parsearla a `JSON`. Esa operación puede ser costosa en términos de procesamiento de datos, pero como esto corre en el servidor no debemos prestar demasiada atención al dispositivo con el cual el usuario visita la página cosa que sí sucedería si usáramos create-react-app.

Para poder acceder externamente ala información del Google Sheet debemos generar un enlace, yendo a Archivo -> Publicar en la web -> Todo el documento y formato `.csv`. Cuando se actualice algo de este documento y alguien ingrese a la página,  Next.js se encargará de hacer un request de modo que el cliente obtendrá la información actualizada.

# Crakra UI
Utilizaremos [chakra-ui](https://chakra-ui.com/) como librería de componentes que trabaja con CSS in JS, los estilos se generan en *real-time* (a diferencia de Tailwind por ejemplo que están precompilados y simplemente cargamos el CSS).

 Además de componentes ya fabricados cuenta con la posibilidad de extender los estilos de los componentes basado en **Style Props**, cada componente recibe todas las propiedades de CSS via props por lo que si queremos darle por ejemplo un margen a un botón, en lugar de tener que crear una nueva clase basará con poner `m={2}`


# Creación Proyecto
`npx create-next-app md-ecommerce-gsheets `
Otra opción es `create-next-app` y luego ingresar el nombre cuando nos pregunte a continuación.

Luego navegamos a la carpeta `cd md-ecommerce-gsheets` y ejecutamos `npm run dev` para correr la aplicación.

# Instalación TypeScript
Creamos el archivo `tsconfig.json`

Luego instamos las dependencias y los tipos:
`npm install typescript @types/react @types/react-dom --save-dev`

Como tenemos el archivo `tsconfig.json` cuando volvemos a ejecutar `npm run dev` nos aparece el mensaje **"We detected TypeScript in your project and created a tsconfig.json file for you."** y veremos que nos ha completado ese archivo.

Ahora para trabajar con TypeScript le cambiamos el nombre a `index.js` por `index.tsx` y `_app.js` por `_app.ts` y reiniciamos el server para que tome los cambios.

Creamos un Code Snippet, hacemos `CTRL+SHIFT+P`y escribimos **Preferences: Configure User Snippets**, luego **New Global Snippet File** y nos preguntará el nombre, en este caso lo llamamos `scomp` con el siguiente contenido 
```json
{
	// Place your global snippets here. Each snippet is defined under a snippet name and has a scope, prefix, body and
	// description. Add comma separated ids of the languages where the snippet is applicable in the scope field. If scope
	// is left empty or omitted, the snippet gets applied to all languages. The prefix is what is
	// used to trigger the snippet and the body will be expanded and inserted. Possible variables are:
	// $1, $2 for tab stops, $0 for the final cursor position, and ${1:label}, ${2:another} for placeholders.
	// Placeholders with the same ids are connected.
	// Example:
	"Stateless component": {
		"prefix": "scomp",
		"body": [
			"import React from \"react\";",
			"interface Props {}",
			"",
			"const ${1:Component}:React.FC<Props> = () => <div>{`<${1:Component} />`}</div>;",
			"",
			"export default ${1:Component};",
			""
		],
		"description": "Stateless component"
	}
}
```
Guiándonos por el comentario que inserta VSCode podremos entender fácilmente lo que representa cada propiedad.

Luego utilizamos ese snippet y nos queda:

```jsx
import React from 'react';
interface Props {}

const IndexRoute: React.FC<Props> = ({}) => <div>{`<IndexRoute/>`}</div>;

export default IndexRoute;
```

> Si nos posicionamos como para hacer destructuring de las props y presionamos `CTRL+SPACE` nos sugerirá las props de acuerdo a la interface props, en este caso nos dirá sólo `children`

# Instalación Chakra-UI:
De acuerdo a la [documentación](https://chakra-ui.com/docs/getting-started) instalamos `npm i @chakra-ui/react @emotion/react@^11 @emotion/styled@^11 framer-motion@^4`.
En un [apartado](https://chakra-ui.com/docs/getting-started#nextjs) también nos indica qué debemos hacer en las aplicaciones Next.js:

> Go to  `pages/_app.js`  or  `pages/_app.tsx`  (create if it doesn't exist) and add this:

```jsx
import  { ChakraProvider }  from  "@chakra-ui/react"
function  MyApp({ Component, pageProps })  {
  return  (
  <ChakraProvider>
	  <Component  {...pageProps}  />
  </ChakraProvider>
  )
}
export  default MyApp
```

Lo cual adaptándolo a TypeScript y como este componente recibe las props `Component` y `pageProps` importamos  `AppProps` por lo que nos queda:
```jsx
import { ChakraProvider } from '@chakra-ui/react';
import { AppProps } from 'next/app';

const App: React.FC<AppProps> = ({ Component, pageProps }) => {
	return (
		<ChakraProvider>
			<Component {...pageProps} />
		</ChakraProvider>
	);
};
export default App;
```
En `_app.tsx` debo colocar el Layout que quiero que forme parte de todas las páginas en nuestro ruteo basado en file system y reemplazará a `<Component/>` por el componente de la página en cuestión.

# `getStaticProps`
Para traernos los datos de Google Sheets vamos a trabajar en el ambiente del server (en una aplicación *client side* usaríamos `useEffect` o React Query) y lo hacemos con `getStaticProps`.

```jsx
import { GetStaticProps } from 'next';
import React from 'react';
import { Product } from '../product/types';

interface Props {
	products: Product[];
}

const IndexRoute: React.FC<Props> = () => <div>{`<IndexRoute! />`}</div>;

export const getStaticProps: GetStaticProps = async () => {
	return {
		props: {
			products: [],
		},
	};
};

export default IndexRoute;
```

En lugar de trabajar con una carpeta `components` en la raíz creamos una carpeta `product` propia de esta entidad y dentro colocaremos componentes, tipos, API helpers, etc. Creamos el archivo `types.ts` y allí la interface `Product` donde definimos qué tipos de datos nos vendrán desde el Google Sheet.


Si utilizamos `getStaticProps` sin `revalidate` veremos siempre la misma información cacheada en memoria (en vercel por lo que no tendremos que ir a busar ese recurso externo a otor lado) que fue obtenida cuando la primera persona entró a la tienda. Si coloco revalidate 10 y durante los primeros 9 segundos entran X personas, todas verán esa primera información. El que entra depsués de los 10 segundos también verá la info vieja pero dispara un request para traer la info nueva.

En cambio nosotros vamos a hacer uso de **ISR** con lo cual la información se irá actualizando. Como podemos ver en la  [documentación](https://nextjs.org/docs/basic-features/data-fetching)  se basa la filosofía de *stale while revalidate* la cual tiene ventajas y desventajas. La principal ventaja es la rapidez  mientras que tiene la desventaja que en ocasiones (por ejemplo ante un cambio en google sheets) habrá un momento en el cual el usuario verá la información desactualizada. Si cuando ese usuario entró se había cumplido el tiempo de revalidate, Next.js se fija si hay información nueva, la actualiza en su caché y el próximo usuario ya la verá actualizada. Esto permite ahorrar muchos costos, suponiendo que configuramos para que se actualice una vez por día y en ese día entran 5000 personas sólo estaremos yendo una vez al servidor por todos esos usuarios.

Si estamos trabajando con un stock de productos limitados y queremos mostrar siempre la información actualizada podemos recurrir a  `getServerSideProps` en lugar de `getStaticProps`, pero con la desventaja es que ya no será instantánea la carga ya que tendremos que esperar que la aplicación vaya hasta el server traiga la información, la parsee y luego recién ahí la verá el usuario.

# Instalar Dependencias
Para hacer los requests vamos a utilizar `axios` (no podemos utilizar fetch ya que los requests los vamos a hacer desde un entorno Node por lo que no tenemos acceso al objeto fetch como cuando estamos en el browser). Otra alternativa podría ser la librería `isomorphic-fetch`.

`npm install axios`
`npm install @types/axios --save-dev`

Para parsear el `.csv` a `json` vamos a utilizar [Papaparse](https://www.papaparse.com/)
`npm install papaparse` y `npm install @types/papaperse --save-dev`

# `api.ts`
En `product` creamos un archivo `api.ts` que tendrá métodos que nos permitirán interactuar con nuestros datos.

En el GET request que realizamos con axios indicamos como segundo parámetro `responseType: 'blob'`
```jsx
import { Product } from './types';
import axios from 'axios';

export default {
	list: async (): Promise<Product[]> => {
		return axios
			.get(
				'https://docs.google.com/spreadsheets/d/e/2PACX-1vQG1pOSbGxaJYzlOWF2t7KFyP5wtWo_EDS0crTllQtALvDDZBq7OwxvTDJwM4dmasW7wmNh6KFB443j/pub?output=csv',
				{
					responseType: 'blob',
				}
			)
			.then(response => {
				console.log(response.data);
				return response.data;
			});
	},
};
```

Si luego en `index.tsx` llamamos a esta función veremos en la consola (no en las DevTools los elementos del google sheet separados por comas). La primer fila es la fila de headers.

Ahora agregamos el parseo de csv a json que como dijimos haremos con papaparse.
Tenemos que tener presente que `Papa.parse()` no funciona con Promises, por lo que debemos usar `return new Promise`. A `Papa.parse()` le pasamos como segundo parámetro un objeto la primera propiedad es `header:true` con lo cual le decimos que la primera fila del csv (y el primer elemento del array en este caso) tienen todos los títulos de las columnas y la propiedad `complete` es una función que recibe `results` por lo que retornamos `resolve(results.data)`
```jsx
import { Product } from './types';
import axios from 'axios';
import Papa from 'papaparse';

export default {
	list: async (): Promise<Product[]> => {
		return axios
			.get(
				'https://docs.google.com/spreadsheets/d/e/2PACX-1vQG1pOSbGxaJYzlOWF2t7KFyP5wtWo_EDS0crTllQtALvDDZBq7OwxvTDJwM4dmasW7wmNh6KFB443j/pub?output=csv',
				{
					responseType: 'blob',
				}
			)
			.then(response => {
				return new Promise<Product[]>((resolve, reject) => {
					Papa.parse(response.data, {
						header: true,
						complete: results => {
							return resolve(results.data as Product[]);
						},
						error: error => {
							return reject(error.message);
						},
					});
				});
			});
	},
};
```

> Para no tener que poner la URL del csv podemos crear una carpeta `app` y dentro de ella un archivo `constants.ts` donde ponemos

```jsx
export const INFORMATION = {
	sheet: `https://docs...`
}
```

Luego en `api.ts` importamos dicho archivo con `import  {INFORMATION}  from  "../app/constants";` y hacemos referencia a la dirección con `INFORMATION.sheet`

Lo cual si queremos minimizarlo sacando los return podemos expresarlo de este modo:
```jsx
.then(
	response =>
		new Promise<Product[]>((resolve, reject) => {
			Papa.parse(response.data, {
				header: true,
				complete: results => resolve(results.data as Product[]),
				error: error => reject(error.message),
			});
		})
	);
```

# WhatsApp
Para enviar el mensaje por WhatsApp vamos a utilizar la siguiente [documentación](https://faq.whatsapp.com/general/chats/how-to-use-click-to-chat/?lang=en) básicamente utilizaremos la dirección `https://wa.me/1XXXXXXXXXX` y para pasarle un mensaje determinado usaremos `https://wa.me/1XXXXXXXXXX?text=I'm%20interested%20in%20your%20car%20for%20sale` notar que se trata de un mensaje URL-encoded.

Cuando utilizamos `text` para conformar el mensaje que será enviado por whatsapp lo hacemos con `React.useMemo(()=> {}, [cart])` de modo tal que si el componente se renderiza más de una vez solo se ejecute cuando cambia el valor de `cart`

```jsx
  const text = React.useMemo(() => {
    return cart
      .reduce(
        (message, product) =>
          message.concat(
            `* ${product.title} - $${product.price}\n`
          ),
        ``
      )
      .concat(
        `\nTotal: $ ${cart.reduce(
          (total, product) =>
            (total += Number(product.price)),
          0
        )}`
      );
  }, [cart]);
```

A la hora de hacer la suma de los precios hemos utilizado `total+= Number(product.price)` para evitar concatenar strings en lugar de hacer una suma pero otra posible solución sería castearlo en `api.ts` así ya nos llega como un número. 
```jsx
import axios from 'axios';
import Papa from 'papaparse';

import { Product } from './types';

export default {
  list: async (): Promise<Product[]> => {
    return axios
      .get(
        'https://docs.google.com/spreadsheets/d/e/2PACX-1vQG1pOSbGxaJYzlOWF2t7KFyP5wtWo_EDS0crTllQtALvDDZBq7OwxvTDJwM4dmasW7wmNh6KFB443j/pub?output=csv',
        {
          responseType: 'blob',
        }
      )
      .then(
        (response) =>
          new Promise<Product[]>((resolve, reject) => {
            Papa.parse(response.data, {
              header: true,
              complete: (results) => {
                const products = results.data as Product[];

                return resolve(
                  products.map((product) => ({
                    ...product,
                    price: Number(product.price),
                  }))
                );
              },

              error: (error) => reject(error.message),
            });
          })
      );
  },
};

```


```jsx

```

```jsx

```

```jsx

```

```jsx

```

```jsx

```

```jsx

```

```jsx

```

```jsx

```

```jsx

```

```jsx

```

```jsx

```

```jsx

```

```jsx

```

```jsx

```

```jsx

```

```jsx

```

```jsx

```

```jsx

```

```jsx

```

```jsx

```

```jsx

```

```jsx

```

```jsx

​```create-next-app almacency 
Otra opción es create-next-app y luego ingresar el nombre cuando nos pregunte a continuación.

cd almacency
Si queremos trabajar con yarn en lugar de npm podemos borrar el package-lock.json y ejecutar yarn para instalar las dependencias


# TypeScript
Creamos el archivo `tsconfig.json`

Suponiendo que estamos trabajando con yarn `yarn add typescript @types/react @types/react-dom -dev` mientras que si estamos con npm `npm install typescript @types/react @types/react-dom --save-dev
`
yarn dev

Como tenemos el archivo `tsconfig.json` nos aparece el mensaje "We detected TypeScript in your project and created a tsconfig.json file for you."