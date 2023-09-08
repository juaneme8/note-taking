# Introducción

Prisma es un ORM open-source que nos permite comunicar el backend con una DB sin la necesidad de utilizar código SQL. Esta herramienta que facilita la conexión con la db, la obtención de datos, la cración de migraciones, validación de esquemas, etc sin la necesidad de utilizar código SQL. 



Prisma está formado por los siguientes módulos:

* Prisma Client: para generar archivos de configuración
* Prisma Migrate: para leer nuestros esquemas y crear los archivos de migración desplegar a producción
* Prisma Studio: interfaz gráfica



## Descripción del proyecto

Se trata de un proyecto utilizando JavaScript, Prisma y SQLite con la finalidad de mostrar los aspectos básicos de este ORM y luego crear una REST API.



## Extensiones

Para este proyecto en  Visual Studio Code podemos instalar las extensiones:

* Prisma para obtener highlighting y autocompletado.

* SQLite Viewer

   

## Getting Started

```
mkdir nodejs-prisma-restapi
npm init -y
```



## Instalación dependencias

Si queremos utilizar TypeScript en nuestro proyecto:

```
npm install typescript ts-node @types/node --save-dev
```



```
npm install prisma --save-dev
```



## Configurar Prisma

```
npx prisma init
```

Con esto lograremos la creación del directorio `prisma` con `schema.prisma` dentro.



En las notas que aparecen en la consola nos indica: 

* Configurar el `.env` con la variable de entorno `DATABASE_URL` y como utilizaremos sqlite que es una db que funciona en un archivo, será: `DATABASE_URL="file:./dev.db"`

  

* Establecer el provider en `schema.prisma` para lo cual debemos modificar las siguientes líneas:

  

```js
datasource db {
  provider = "sqlite"
  url      = env("DATABASE_URL")
}
```



## Prisma models

Los modelos en Prisma representan las entidades de la aplicación y tienen una correlación con las tablas (SQLlite, PostgreSQL) o colecciones (MongoDB).

En `schema.prisma` creamos los modelos deseados:

```js
model Product{
	id Int @id @default(autoincrement())
	name String @unique
	price Int @default(0)
	createadAt DateTime @default(now())
	categoryId Int 
}
```

En la [documentación](https://www.prisma.io/docs/reference/api-reference/prisma-schema-reference#model-field-scalar-types) podemos encontrar información de los types y su vinculación con cada tipo de base de datos, por ejemplo `String` es equivalente en SQL Server a `nvarchar(1000)`.



Queremos tener una tabla de productos y una de categorías que conformen una relación de uno a muchos (una categoría tiene muchos productos).

```js
model Product{
	id Int @id @default(autoincrement())
	name String @unique
	price Int @default(999)
	createadAt DateTime @default(now())
	category Category @relation(fields:[categoryId], references: [id])
	categoryId Int
}

model Category{
	id Int @id @default(autoincrement())
	name String @unique
	products Product[]
}
```



Con `category Category @relation(fields:[categoryId], references: [id])` indicamos que el campo `categoryId` está relacionado con el `id` de la otra tabla.



## Prisma Migrate

Ejecutaremos una migración para lograr dos cosas por un lado generar el código de javascript y también para crear las tablas de la base de datos

```
npx prisma migrate dev
```



```
Environment variables loaded from .env
Prisma schema loaded from prisma\schema.prisma
Datasource "db": SQLite database "dev.db" at "file:./dev.db"

SQLite database dev.db created at file:./dev.db

? Enter a name for the new migration: » first
```



Cuando nos pregunta el nombre podremos ponerle `first` o directamente ejecutar `npx prisma migrate dev --name first`.



```
Applying migration `20230908162515_first`

The following migration(s) have been created and applied from new schema changes:

migrations/
  └─ 20230908162515_first/
    └─ migration.sql

Your database is now in sync with your schema.

Running generate... (Use --skip-generate to skip the generators)
```



* En la carpeta migrations podremos ver código sql y en caso de modificar el schema tendremos que volver a ejecutar la migración.

* Además notaremos que nos ha instalado `@prisma/client`.

* Como tenemos la extensión SQLite Viewer podremos hacer click en `dev.db` y luego en Open Anyway para ver el contenido de la base de datos.





# Rest API

Creamos una carpeta `src` y dentro `index.js`

```
npm install express
```



> :bulb: Para poder utilizar import/export tenemos que modificar el `package.json` y añadimos la propiedad `type:"module"`



## Aplicación Express

```js
import express from 'express';

const app = express();
app.use(express.json());

app.listen(3000);

console.log('Server on port', 3000);
```



## Creación script `dev`

Queremos que la aplicación se reinicie cada vez que introduzcamos nuevos cambios, para ello vamos a instalar Nodemon (aunque las versiones más modernas de Node.js incluyen un flag que posibilita esto sin una dependencia aparte)

```
npm install nodemon --save-dev
```

Modificamos el script en `package.json`

```js
"scripts":{
	"dev": "nodemon src/index.js"
}
```

Luego ejecutamos el proyecto con `npm run dev` y veremos en la consola "Server on port 3000".



## Creación de rutas

Creamos una carpeta `routes` dentro de `src` y dentro de ella tendremos los archivos correspondientes a las rutas: `categories.routes.js` y `products.routes.js`



En `categories.routes.js` lo primero que hacemos es crear un endpoint simple.

```js
import { Router } from "express";

const router = Router();

router.get('/', (req, res) => {
  res.send('categories')
})

export default router;
```



Hacemos lo mismo en `products.routes.js` y luego los importamos en `index.js`

```js
import express from 'express';
import categoriesRouter from './routes/categories.routes.js'
import productsRouter from './routes/products.routes.js'

const app = express();
app.use(express.json());
app.use('/api/categories',categoriesRouter)
app.use('/api/products',productsRouter)

app.listen(3000);

console.log('Server on port', 3000);
```

> Notar que en Node.js debemos utilizar el import con la extensión `.js`



## GET

Ahora comenzamos a trabajar con `@prisma/client` (que si no lo instalamos manualmente se habrá instalado al momento de hacer la migración). Veremos que gracias a la migración también tenemos

```js
import { Router } from "express";
import { PrismaClient } from '@prisma/client'

const router = Router();
const prisma = new PrismaClient()

router.get('/', async (req, res) => {
  const products = await prisma.product.findMany()
  res.json(products)
})

export default router;
```

> Notar que al escribir `prisma.` nos aparece el autocompletado asociado a product y category producto del typing que logramos con el migrate.

Si ahora visitamos http://localhost:3000/api/products obtendremos un array vacío.



## POST

Creamos una ruta POST para insertar un elemento,

```js
import { Router } from "express";
import { PrismaClient } from '@prisma/client'

const router = Router();
const prisma = new PrismaClient()

router.get('/', async (req, res) => {
  const products = await prisma.product.findMany()
  res.json(products)
})

router.post('/', async (req, res) => {
  const newProduct = await prisma.product.create({
    data: req.body
  })
  res.json(newProduct)
})

export default router;
```

En lugar de `data: req.body` podríamos haber puesto `data: {name: req.body.name, price: req.body.price, ...}`



Podemos probarlo utilizando la extensión Thunder Client, hacemos click en New request





A continuación en lugar de `@prisma/client` importaremos un archivo `db.js` con el fin de reutilizar el código.

```js
import { Router } from "express";
import { prisma } from "../db.js";

const router = Router();

router.get("/categories", async (req, res, next) => {
	try {
		const categories = await prisma.category.findMany({
			include: {
				products: true,
			},
		});
		res.json(categories);
	} catch (error) {
		next(error);
	}
});

export default router;
```

Cuando ejecutamos `prisma migrate` se instaló como dependencia de desarrollo `@prisma/client`



En `products.routes.js`

```js
import { Router } from "express";
import { prisma } from "../db.js";

const router = Router();

router.get("/products", async (req, res) => {
	try {
		const products = await prisma.product.findMany({
			include: {
				category: true,
			},
		});
		res.json(products);
	} catch (error) {
		next(error);
	}
});

router.post("/products", async (req, res) => {
	try {
		const product = await prisma.product.create({
			data: req.body,
		});
		res.json(product);
	} catch (error) {
		next(error);
	}
});

router.get("/products/:id", async (req, res) => {
	const product = await prisma.product.findUnique({
		where: {
			id: Number(req.params.id),
		},
		include: {
			category: true,
		},
	});
	res.json(product);
});

router.delete("/products/:id", async (req, res) => {
	const product = await prisma.product.delete({
		where: {
			id: Number(req.params.id),
		},
	});
	res.json(product.quantity);
});

router.patch("/products/:id", async (req, res) => {
	try {
		const product = await prisma.product.update({
			where: {
				id: Number(req.params.id),
			},
			data: req.body,
			include: {
				category: true,
			},
		});
		res.json(product);
	} catch (error) {
		next(error);
	}
});

export default router;
```







