# Curso Platzi

A continuación se presentan algunos extractos del curso de Next.js de Platzi.

:thinking: ​Se abandonó en el **video 10**, evaluar si conviene conservar esto o eliminarlo.

# Proyecto con TypeScript

Antes de incorporar nuestra API el autor del curso decidió incorporar TypeScript y para obtener la puesta a punto inicial es posible clonar su repositorio y luego trabajar basándonos en la etiqueta `2-inicio-api`

```bash
mkdir md-nextjs-ts
cd md-nextjs-ts
git clone https://github.com/jonalvarezz/platzi-nextjs.git .
git fetch --tags
git tag
git checkout -b juaneme8-api 2-inicio-api 
yarn install
```

con `git clone https://github.com/jonalvarezz/platzi-nextjs.git .` indicamos que queremos clonar en la carpeta actual y que no cree una nueva llamada `platzi-nextjs`
con `git fetch --tags` obtenemos todos los tags (segun lo comprobado no es necesario hacerlo, quizás sea útil para situaciones en las cuales ya teníamos el proyecto y en el repo remoto hay un tag que no tenemos localmente, pero para una situación donde clonamos ya obtenemos todos).
con `git tag` los listamos
con `git checkout -b juaneme8-api 2-inicio-api` creamos un nuevo branch llamado `juaneme8-api` desde la etiqueta `2-inicio-api`

Para evitar la aparición de warnings vamos actualizar las dependencias:  
`yarn add next@latest` 
`yarn add react` 
`yarn add react-dom`

Veremos que tenemos una carpeta `database` y en ella dos archivos `data.ts` y `db.ts` 
En el archivo `data.ts` simulamos los datos devueltos por una base de datos:

```js
const data: Record<TProductId, TProduct> = {
	'2zd33b8c': {
		name: 'Maluma Hass Avocado',
		id: '2zd33b8c',
		sku: 'NUR72KCM',
		price: 1.15,
		image: '/images/maluma.jpg',
		attributes: {
			descriptio n:
			'A relatively new cultivar, it was, the pretty boy baby, discovered in South Africa in the early 1990s by Mr. A.G. (Dries) Joubert. Maluma Babyy. It is a chance seedling of unknown parentage',
			shape: 'Oval',
			hardiness: '1 °C',
			taste: 'Catchy, is an avocado',
		},
	},
	
	'6trfgkkj': {
	//...
	}
	
}
```

Donde cada hash tiene como `key` el `id` de un elemento de la base de datos.

Por otra parte tenemos el archivo `db.js` que es un manejador en el cual tememos los métodos para obtener los datos de esa base de datos ficticia.  

```jsx
// Oh you curious...
// This is not a real database,
// But let's imagine it is one :)
import allData from './data'

class Database {
  constructor() {}

  async getAll(): Promise<TProduct[]> {
    const asArray = Object.values(allData)
    await randomDelay()
    return asArray
  }

  async getById(id: string): Promise<TProduct | null> {
    if (!Object.prototype.hasOwnProperty.call(allData, id)) {
      return null
    }

    const entry = allData[id]
    await randomDelay()
    return entry
  }
}

// Let's also add a delay to make it a bit closer to reality
const randomDelay = () =>
  new Promise((resolve) => {
    const max = 350
    const min = 100
    const delay = Math.floor(Math.random() * (max - min + 1)) + min

    setTimeout(resolve, delay)
  })

export default Database

```

## Creación endpoint GET `/api/avo`

La idea es que cuando entremos a. `/` consumamos una API para renderizar todos los elementos llamando a `api/avo/`
Por lo que dentro de `pages` tendremos que crear una carpeta `api` y otra llamada `avo`, finalmente dentro de esta última un archivo `index.tx`.

>Otra opción hubiersa sido crear directamente dentro de `api` un archivo `avo.js` pero como vamos a tener más rutas dentro de `avo` optamos por esa opción.

En `index.ts` Next.js espera una función que recibe en el primer parámetro el `req` y en el segundo el `res` y los maneja como es habitual en Node.js, armando un ejemplo básico:

### Ejemplo Básico

```js
import {IncomingMessage, ServerResponse} from 'http';

const allAvos = async (req: IncomingMessage, res: ServerResponse) => {
	res.end(JSON.stringify({hello: 'world'}));
}

export default allAvos;
```

### Ejemplo usando mockData

Ahora utilizaremos el `db.ts` que lo importamos como `import DB from '@database`, esto es así ya que en `tsconfig.json` tenemos:

```json
"paths": {
	"@database": ["database/db.ts"]
},
```

Luego con `const db = new DB();` contamos con dos métodos `getAll()` y `getById(id)`

```js
import { NextApiRequest, NextApiResponse } from 'next'
import DB from '@database'

const allAvos = async (req: NextApiRequest, res: NextApiResponse) => {
  const db = new DB()
  const allEntries = await db.getAll()
  const lenght = allEntries.length

  res.status(200).json({ lenght, data: allEntries })
}

export default allAvos
```

Si ingresamos a `http://localhost:3000/api/avo` veremos los datos devueltos por la API. Se recomienda instalar una extensión de Chrome **JSON Formatter** o similar para visualizar mejor el JSON.

La idea es devolver un objeto que tenga las propiedades `length` y `data`. Luego veremos que en caso de erorr enviaremos `data: []` y un campo adicional `error`.


## Creación endpoint GET `/api/avo/[id]`

Creamos dentro de `avo` un archivo de una página dinámica `[id].ts`

Tomamos como punto de partida el código del endpoint anterior y con `request.query.id` capturamos el `id` .

```ts
import { NextApiRequest, NextApiResponse } from 'next'
import DB from '@database'

const singleAvo = async (req: NextApiRequest, res: NextApiResponse) => {
  const db = new DB()
  const entry = await db.getById(req.query.id as string)
  //Colocamos as string porque obteníamos Argument of type 'string | string[]' is not assignable to parameter of type 'string'.Type 'string[]' is not assignable to type 'string'.

  res.status(200).json({ data: entry })
}

export default singleAvo+
```

Ahora si entramos a `http://localhost:3000/api/avo/6trfgkkj` obtendremos un objeto con la propiedad `data` como un objeto con toda la información de esa entrada en particular.



# Debugging

Normalmente cuando tenemos algún problema en el backend comenzamos a hacer `console.log()` para saber qué es lo que está pasando. Sin embargo, como Next.js es una aplicación de Node si le pasamos el flag `--inspect` a ese proceso de Node.js podremos empezar en debug mode.

Para ello modificando`package.json` donde tenemos el script de `dev` en vez de `"dev": "next,"` poniendo `"dev": "NODE_OPTIONS='--inspect' next,"` luego ejecutamos `yarn dev`.

Si en chrome escribimos `about: inspect` y luego hacemos click en Open dedicated DevTools for Node podremos accede a la consola para la aplicación Node 