## GraphQL

> Basado en [curso](https://www.youtube.com/watch?v=Y0lDGjwRYKw&list=PL4cUxeGkcC9iK6Qhn-QLcXCXPQUov1U7f) The Net Ninja



## Características

* GraphQL es **lenguaje de consultas**. **Graph** significa grafo y **QL** query language.

* Es un lenguaje utilizado para obtener datos de una API.

* Es considerado una capa entre el frontend y el backend. A diferencia de SQL por ejemplo que es entre el backend y la DB.

* Permite la **comunicación de datos entre servidor y cliente** (navegador). 

* **Código abierto**.

* **Creado por Facebook**.

* Es **utilizable en múltiples lenguajes de programación**. 

* Es *platform agnostic* esto significa que no es necesario usarlo con una tecnología en particular (puede usarse con React, Angular, Vue, etc).

* Es **flexible y eficiente** comparado con el enfoque RESTful.

  

* Nos permite describir los datos.

* Nos permite pedir sólo los datos que queremos, lo cual genera una **optimización y mejora en el rendimiento** en los datos que viajan.

* Nos permite obtener datos predecibles ya que se corresponderán con la descripción.

* Haremos **una única petición** a un *supercharged endpoint* y será el servidor de GraphQL el encargado de recoger los datos de las distintas fuentes.




## Comparación con REST API

RestAPI es una arquitectura para obtener recursos de una fuente de datos como una DB mientras que GraphQL es un lenguaje de consulta. Si bien con GraphQL podemos llamar de manera directa a una base de datos, también es posible utilizarlo con una REST API de modo tal que seamos agnósticos de dónde estamos obteniendo los datos.

En una API REST de libros, si queremos obtener información sobre un libro en particular (cuyo `id` conocemos) y su autor deberíamos primero hacer un `GET` a este endpoint:

```
domain.com/books/:id
```

Donde obtendríamos parte de la información deseada `title`, `genre`, `reviews` y también el `authorId` con el cual obtendríamos la info del autor.

```
domain.com/authors/:id
```

De este último obtendríamos `name` ,`age`, `bio`, `booksIds` (un array con los id de cada libro).

Vemos que sólo para obtener info del libro y del autor hemos tenido que hacer dos requests, esto nos da la pauta que en ciertos casos puede volverse ineficiente. Imaginemos si además queremos mostrar todos los libros escritos por ese autor, en ese caso serían necesarios nuevos requests al endpoint mostrado en primer lugar.

Para obtener estos mismos datos con un enfoque GraphQL haríamos la siguiente query:

```
{
    book(id:123){
        title
        genre
        reviews
        author{
            name
            bio
            books{
				title
            }
        }
    }
}
```



> **Simplicidad**

Con un único HTTP request hemos obtenido toda la información anidada.



> **Selectividad**

Con GraphQL podemos ser **selectivos** en cuanto a los datos que queremos recibir, por ejemplo siguiendo con el ejemplo anterior, si no queremos recibir algunos datos podemos omitirlos en la query:

```
{
    book(id:123){
        title
        author{
            name
            books{
				title
            }
        }
    }
}
```



> **Supercharged Endpoint**

En GraphQL en vez de contar con muchos endpoints contamos con uno sólo recargado con el cual mapeamos todos los datos.

Podremos tener distintos puntos de entrada y nos movemos hacia los datos relacionados, como podemos ver en los siguientes dos ejemplos:

```
{
	book(id:1){
		title
		genre
		author{
			name
			age
			books{
				title
			}
		}
	}
}
```



```
{
	author(id:2){
		name
		books{
			title
			similar{
				title
			}
		}
	}
}
```



## Estructura Proyecto

En este proyecto tendremos un backend (servidor), un frontend y una base de datos.

**El servidor consistirá en una Express App + GraphQL Server** donde describiremos cómo está compuesto el graph, indicando sus relaciones y los distintos tipos de entrada.

El proyecto consistirá en una aplicación React y en ella utilizando **Apollo como cliente GraphQL** haremos queries al server GraphQL para obtener datos y mostrarlos en el navegador. 



> :information_source: **GraphiQL** es como una dummy frontend application que nos permite hacer requests al server GraphQL. Podremos ver un ejemplo de cómo se realizan las consultas con GraphiQL en https://api.spacex.land/graphql/



# Proyecto

Utilizaremos **Apollo** por un lado con **Apollo Server** y también **Apollo Client** (con React).

Desde la aplicación web haremos la petición utilizando GraphQL que llegará a Apollo y será este quien se encargará de buscar la información a una REST API, microservicios o base de datos.



# Servidor con Apollo Server

```
mkdir md-graphql-server
cd md-graphql-server
npm init -y
npm i apollo-server graphql 
```

> En `package.json` agregamos `"type":"module"` para utilizar ECMAScript Modules, de este modo podremos utilizar `import {gql} from 'apollo-server'`.



:link: En la documentación encontramos los primeros pasos explicados claramente: https://www.apollographql.com/docs/apollo-server/getting-started/



Creamos un archivo `index.js` con los siguientes elementos:

* `people`que es un array de objetos con los datos simulando una base de datos en memoria que luego vendrán de una API o una DB. 

* `typeDefs` que es la descripción de estos datos y de las peticiones que se pueden hacer. 

* `resolvers` donde indicamos cómo serán resueltas estas queries.

  Luego creamos el Apollo Server y comenzamos a escuchar peticiones entrantes.

```javascript
import {gql, ApolloServer} from 'apollo-server'

const people = [
	{
		name: 'Midu',
		phone: '034-1234567',
		stret: 'Calle Frontend',
		city:'Barcelona',
		id:"3d594650-3436"
	},
	{
		name: 'Youseff',
		stret: 'Calle Fullstck',
		city:'Ibiza',
		id:"56214650-3436"
	}
]

const typeDefs = gql`
	# Comentario en un String de GraphQL
	
	type Person{
        name: String!
        phone: String
        street:String!
        city:String!
        id: ID!
	}
	
	type Query{
		personCount: Int!
		allPeople: [Person]!
	}
`

const resolvers={
    Query:{
        personCount: () => people.length,
        allPeople: () => people
    }
}

const server= new ApolloServer({
    typeDefs,
    resolvers
})

server.listen().then(({ url }) => {
	console.log(`🚀 Server ready at ${url}`)
}); 
```

> :memo: Con el signo de exclamación indicamos que el campo es requerido.



Para leventar el servidor ejecutamos `node index.js` o bien si queremos correrlo con nodemon `npx nodemon index.js` (atención en las mutaciones si estamos trabajando con nodemon)

A partir de la versión 3 de `apollo-server` cuando vayamos http://localhost:4000/ a nos redireccionará a https://studio.apollographql.com/. Si queremos en cambio trabajar con **GraphQL Playground** podemos instalar las siguientes versiones: 

```
"apollo-server": "2.24.0",
"graphql": "15.5.0"
```



> :memo: Con `CONTROL+SPACE` tendremos un autocompletado del comando que podemos ejecutar. La primera vez nos sugerirá entre query, mutation, luego si ponemos `query {}` y volvemos a presionarlo nos sugerirá las posibles queries a ejecutar.



## Ejecutar Queries

### :balloon: `personCount`

```
query{
	personCount
}
```

Obtenemos un objeto con la propiedad `data` que también es un objeto que cuenta con una propiedad `personCount`



```
{
  "data": {
    "personCount": 2
  }
}
```



### :balloon: `allPeople`

En cuanto a `allPeople` no podríamos poner directamente 

```
query{
	allPeople
}
```

Cuando retorna un objeto (en este caso un array de objetos) debemos indicarle los campos que queremos extraer, esto para `personCount` no hacía falta pues solo retornaba un número.

```
query{
	allPeople{
		name
		phone
	}
}
```

:warning: Cuando un campo no está disponible para uno de los objetos, el valor obtenido es `null`.

```
{
  "data": {
    "allPeople": [
      {
        "name": "Midu",
        "phone": "034-1234567"
      },
      {
        "name": "Youseff",
        "phone": null
      }
    ]
  }
}
```



## Query con Parámetros

Es posible tener una query con parámetros por ejemplo un caso en el que recibimos un argumento String que será requerido y retornamos un objeto `Person`. Por lo tanto en `typeDefs` dentro de `Query`

```javascript
const typeDefs = gql`
	type Person{
	
	}
	type Query{
		findPerson(name:String!):Person
	}
`
```

> :memo: Notar que ponemos como tipo de dato devuelto `Person` y no `Person!` pues puede darse un caso en el que busquemos a alguien que no exista y nos debería retornar `null`.

En el resolver vemos que recibimos en primer lugar el parámetro `parent` (es para cuando una consulta está adentro de otra, luego `args`, `context` e `info`.

 Utilizando `args.name` buscamos en el array el valor deseado.

```javascript
const resolvers={
    Query:{
        findPerson: (parent, args, context, info) => {
    		const {name} = args;
			return people.find(person => peroson.name===name)
		}
    }
}
```



### :balloon:`findPerson`

Para probar el funcionamiento de esta query

```
query{
  findPerson(name:"Midu"){
    phone
  }
}
```

> :memo: Notar el uso de comillas dobles a la hora de pasar parámetros.



En lo que obtendremos 

```
{
  "data": {
    "findPerson": {
      "phone": "034-1234567"
    }
  }
}
```

Si buscamos una persona que no existe 

```
query{
  findPerson(name:"udiM"){
    phone
  }
}
```

Obtendremos en cambio

```
{
  "data": {
    "findPerson": null
  }
}
```

> En este caso podremos trabajar con optional chaining `person?.phone` para no tener inconvenientes. 



## Resolvers

Apollo utiliza los resolvers para cómo popular cada campo y así poder responder las consultas. 

Un resolver es una función responsable de popular la data de un campo del esquema.

Si no definimos ninguno utilizará el *default resolver* como podemos ver a continuación



```js
const resolvers = {
	Query:{
	
	}
	Person:{
		name: (parent) => parent.name
	}
}
```

Vemos `name: (parent) => parent.name` y lo mismo para cada campo es lo que sucede por defecto.

En este caso `parent` hace referencia a la persona que hemos encontrado en la query (ya sea `findPerson` o `allPeople`)



Esto podría ser útil para incorporar información

```js
const resolvers = {
	Query:{
	
	}
	Person:{
		address: (parent) => `${parent.stree} ${parent.city}`,
		check: () => 'test'
	}
}
```

Por ejemplo `address` es un cálculo sobre información que tengo (con lo que podemos extrar lógica que normalmente haríamos en el cliente) y `check` es un campo que devuelve un valor fijo.



Esto debemos agregarlo al tipo de Person.

```js
type Person{
        name: String!
        phone: String
        street:String!
        city:String!
        id: ID!
        address: String!
        check: String!
}
```

> Ambos parámetros son required uno porque es basado en otros required y el otro porque sabemos que estará siempre disponible.



Con GraphQL buscamos que los datos puedan ser consumidos de manera más simple independientemente de cómo están almacenados en la base de datos. Si queremos poder hacer una consulta de este tipo:



```
query{
	findPerson(name:"Itzi"){
		name
		phone
		id
		address{
			street
			city
		}
	}
}
```



Debemos modificar la descripción de Person

```js
const typeDefs = gql`
	type Address {
		street:String!
		city: String!
	}
	type Person{
		name:String!
		phone:String
		address: Address!
		id:ID!
	}
	
	type Query{
		findPerson(name:String!):Person
	}
`
```



Luego en resolvers:



```js
const resolvers={
	Query:{
		...
	}
	Person:{
		address: (parent) => {
			return {
				street: parent.street,
				city: parent.city
			}
		}
	}
}
```

En el ejemplo anterior queda en claro que los datos almacenados en la db y su forma no tiene por qué ser la misma que el modo en que son consultados y devueltos al cliente luego.



## Mutations

### :balloon: `addPerson`

```js
const typeDefs = gql`
	type Person{
	
	}
	type Query{
	
	}
	type Mutation{
		addPerson(
			name: String!
			phone: String
			stret: String!
			city: String!
		): Person
			
		
	}
`
```

> :memo: Notar que `street` y `city` se lo pasamos por parámetro al mismo nivel aunque luego la consulta la manejamos de otro modo (con `address`). 
>
> Estamos devolviendo la persona agregada. Esto es importante por el modo en que Apollo maneja la caché, lo guarda en la cache local mientras lo refleja en la base de datos.



El id lo generamos con la librería uuid

```
import {v1 as uuid} from 'uuid'
```



Luego en el `resolver`

```js
const resolver = {
	Query:{
	
	}
	Mutation:{
		addPerson: (parent, args) => {
			const person = [id: uuid(),...args]
			people.push(person) //simulamos una actualización de la DB
			return person
		}
	}

}
```

Con `return person` nos aseguramos devolver la persona como dijimos anteriormente.



Para probar esta mutación en el Playground.

```
mutation{
	addPerson(
		name: "Juan"
		phone: "123 123"
		stret: "Calle 1"
		city: "CABA"
		
	){
		name
		address{
			street
			city
		}
	}
}
```

Como hemos puesto que la mutación devuelva una persona debemos indicar también qué campos queremos extraer con `{}`. Notar que podemos obtener `address` pues estamos devolviendo un `Person` 



:memo: En el Playground podremos notar que tenemos un único endpoint si presionamos en `COPY CURL` sea cual sea la operación realizada query o mutación haremos un POST a ese endpoint.



## Validaciones

Queremos evitar que una persona con el mismo nombre sea añadida varias veces. Esto lo hacemos en el resolver.



## Manejo de Errores

https://www.apollographql.com/docs/apollo-server/data/errors/



# Servidor con Express y Apollo

> Basado en el [video](https://www.youtube.com/watch?v=fIZxZk_szWw) de Fazt

Si bien podríamos trabajar directamente con Apollo Server, utilizamos Express y Express Apollo Server ya que es útil tener la posibilidad de tener una REST API además de la GraphQL API en el mismo servidor.

```
npm i express apollo-server-express graphql
```



Adicionalmente instalamos

```
npm i dotenv mongoose
```



> En cuanto a la estructura en ocasiones trabajamos sobre una carpeta `src`, pero en este caso lo haremos en la raíz.

* `app.js`

* `db.js`
* `typeDefs.js`
* `resolvers.js` 



> :package: Instalamos la extensión GraphQL de GraphQL Foundation.



En `app.js`.

* Creamos un servidor de Express.

* En `/` creamos una ruta de la REST API que muestra un mensaje de bienvenida.
* `ApolloServer` es un wrapper utiliza el servidor que creamos y lo extiende. Creamos un nuevo objeto `ApolloServer` y le pasamos `typeDefs` y `resolvers` `const server = new ApolloServer({ typeDefs, resolvers });`. Luego con el objeto creado utilizamos el método `start` que devuelve una promesa (para usar `await` lo colocamos en una función `async`). `await server.start();` Por último lo asociamos al servidor express para que extienda sus funcionalidades `server.applyMiddleware({ app });`
* En caso de ingresar a otra dirección distinta de `/` o `/graphql` mostraremos un mensaje de "not found" y entregamos un 404. Lo hacemos después de `  server.applyMiddleware({ app });` para que nos permita ingresar a `/graphql`

```javascript
const express = require('express');

const { typeDefs } = require('./typeDefs');
const { resolvers } = require('./resolvers');
const { ApolloServer } = require('apollo-server-express');

const start = async () => {
  const app = express();

  app.get('/', (req, res) => {
    res.send('Hello World');
  });


  const server = new ApolloServer({ typeDefs, resolvers });

  await server.start();
  server.applyMiddleware({ app });

  app.use((req, res, next) => {
    res.status(404).send('404: Page not found');
  });

  app.listen(3000, () => {
    console.log('Listening on Port 3000');
  });
};

start();
```



## Integración con MongoDB

> :notebook: En lugar de instalarlo o usar MongoDB Atlas podemos utilizar una imagen Docker de Mongo.



En `db.js` creamos y exportamos la función asíncrona `connectDB` que utiliza el método `connect` de mongoose.

```javascript
const { connect } = require('mongoose')

const connectDB = async () => {
  try {
    await connect(
      "mongodb://localhost/tasksdb"
    );
  } catch (error) {
    console.log(error);
  }
}

module.exports = { connectDB };
```



Luego en `index.js` importamos `connectDb` y lo invocamos.

```
const app = express();
connectDB();
```

> Lo hacemos al principio para darle tiempo a conectar mientras hacemos las otras tareas de Express y Apollo.



## CRUD

En `models` creamos un archivo `Task.js` con el esquema de datos que almacenaremos en MongoDB y un modelo que luego exportamos ya que lo usaremos en el código para interactuar con la base de datos.

```js
const { Schema, model } = require('mongoose');

const taskSchema = new Schema({
  title: {
    type: String,
    required: true,
  },
  description: String
});

module.exports = model('Task', taskSchema);
```



### :rocket: Listado de tareas

Queremos generar una query `getAllTasks` que devuelva todas las tareas disponibles en la DB. Por lo tanto debemos editar `typeDefs.js` definiendo el tipo de datos `Task` y la query `getAllTasks` que devuelve `[Task]`

```js
const { gql } = require('apollo-server-express');

const typeDefs = gql`
  type Task {
    id: ID!
    title: String!
    description: String
  }

  type Query {
    hello: String
    getAllTasks: [Task]
  }
`;

module.exports = { typeDefs };
```



A continuación editamos `resolvers.js` agregando qué debemos hacer cuando recibamos la query `getAllTasks`. Importamos el modelo, con el buscamos todos los elementos de dicha colección y los retornamos.



```js
const Task = require('./models/Task.js');

const resolvers = {
  Query: {
    hello: () => 'Hello World',
    getAllTasks: async () => {
      return await Task.find({})
    }
  }
}

module.exports = {resolvers}
```

> Para probar que todo funciona podemos abrir MongoDB Compass y agregar un documento manualmente.



### :rocket: Agregado de tarea

Queremos crear una mutación `addTask` capaz de agregar una nueva tarea. Para ello primero editamos `typeDefs.js` y luego `resolvers.js`

```javascript
const { gql } = require('apollo-server-express');

const typeDefs = gql`
  type Task {
    id: ID!
    title: String!
    description: String
  }

  type Query {
    hello: String
    getAllTasks: [Task]
  }

  type Mutation {
    createTask(title:String, description:String):Task
  }
`;

module.exports = { typeDefs };
```

> :scream: A la hora de definir el tipo `Task` hemos dicho que tendrá un `id`. Tener presente que si bien en MongoDB contamos con `_id` gracias a Mongoose podremos usar `_id` o `id` ya que ambos tienen el mismo valor.

> El tipo `ID` nos da la flexibilidad de que puede ser un entero o un string.



En `resolvers.js`

```javascript
const Task = require('./models/Task.js');

const resolvers = {
  Query: {
    hello: () => 'Hello World',
    getAllTasks: async () => {
      return await Task.find({})
    }
  },
  Mutation: {
    createTask: async (_, args) => {
      const { title, description } = args;
      const newTask = new Task({ title, description })
      await newTask.save();
      return newTask;
    }
  }
}

module.exports = {resolvers}
```

> Notar que al parámetro  `parent` como no lo vamos a utilizar lo nombramos como `_`. Además a `context` e `info` tampoco los usaremos.
>
> En este caso hicimos destructuring de este modo `const { title, description } = args;` pero también podríamos haberlo hecho al recibir `args` es decir ` createTask: async (_, {title, description})`

Luego en el Sandbox indicamos las características del documento a agregar:

```
mutation{
  createTask(
    title:"My task", 
    description:"this is my task"
    )
    {
      id
    }
}
```

 

### :rocket: Listar una tarea

Queremos agregar la query `getTask` que recibe por parámetro el id de la tarea a listar. 

Lo primero que hacemos es modificar `typeDefs.js` agregando al tipo query a `getTask(id:ID):Task`

```js
const { gql } = require('apollo-server-express');

const typeDefs = gql`
  type Task {
    id: ID!
    title: String!
    description: String
  }

  type Query {
    hello: String
    getAllTasks: [Task]
    getTask(id:ID):Task
  }

  type Mutation {
    createTask(title:String, description:String):Task
  }
`;

module.exports = { typeDefs };

```

Luego modificamos `resolvers.js` agregando el método `getTask` del objeto `Query`.

```js
const Task = require('./models/Task.js');

const resolvers = {
  Query: {
    hello: () => 'Hello World',
    getAllTasks: async () => {
      return await Task.find({})
    },
    getTask: async (_, args) => {
      //console.log({args})
      const task = await Task.findById(args.id)
      return task;
    }
  },
  Mutation: {
    createTask: async (_, args) => {
      const { title, description } = args;
      const newTask = new Task({ title, description })
      await newTask.save();
      return newTask;
    }
  }
}

module.exports = {resolvers}
```

> Notar que colocamos `console.log({args})` para chequear que el parámetro esté llegando de manera correcta.
>
> En lugar de definir `getTask` como una propiedad, podemos utilizar ES6 method definition:

```
async getTask(id){
	return
}
```



Con el sandbox probamos esta query con:

```
query{
  getTask(id:"621ae5afa432eb01e861604a") {
    title
  }
}
```



### :rocket: Eliminar una tarea

Queremos agregar la query `removeTask` que recibe por parámetro el id de la tarea a eliminar. 

Lo primero que hacemos es modificar `typeDefs.js` agregando al tipo query a `removeTask(id:ID):Task`. Notar que además de eliminarla queremos retornarla. Otra opción sería retornar un mensaje de tipo string.

En `typeDefs.js` agregamos al tipo de mutación `removeTask(id:ID):Task`

```javascript
const { gql } = require('apollo-server-express');

const typeDefs = gql`
  type Task {
    id: ID!
    title: String!
    description: String
  }

  type Query {
    hello: String
    getAllTasks: [Task]
    getTask(id:ID):Task
  }

  type Mutation {
    createTask(title:String, description:String):Task
    removeTask(id:ID!):Task
  }
`;

module.exports = { typeDefs };

```



En `resolvers.js`

```js
const Task = require('./models/Task.js');

const resolvers = {
  Query: {
    hello: () => 'Hello World',
    getAllTasks: async () => {
      return await Task.find({})
    },
    getTask: async (_, args) => {
      // console.log({args})
      const { id } = args;
      const task = await Task.findById(id)
      return task;
    },
  },
  Mutation: {
    createTask: async (_, args) => {
      const { title, description } = args;
      const newTask = new Task({ title, description })
      await newTask.save();
      return newTask;
    },
    removeTask: async (_, args) => {
      const { id } = args;
      const task = await Task.findByIdAndDelete(id)
      return task;
    }
  }
}

module.exports = {resolvers}
```



Luego para probar la mutación en el sandbox:

```
mutation{
  removeTask(id:"621ae5afa432eb01e861604a") {
    title
  }
}
```



#### Uso de type

En `typeDefs` hemos indicado que por ejemplo a la hora de crear una tarea en `createTask` esperamos recibir un `title` y una `description`, podríamos haber utilizado `input` como veremos a continuación.

```js
const { gql } = require('apollo-server-express');

const typeDefs = gql`
  type Task {
    id: ID!
    title: String!
    description: String
  }

  type Query {
    hello: String
    getAllTasks: [Task]
    getTask(id:ID):Task
  }

  input TaskInput{
    title: String
    description: String
  }

  type Mutation {
    createTask(task:TaskInput):Task
    removeTask(id:ID):Task
  }
`;

module.exports = { typeDefs };

```



Será necesario cambiar el modo en que recibimos datos en `resolvers.js` principalmente cambiamos `const { title, description } = args` por `const { title, description } = args.task;`

```js
const Task = require('./models/Task.js');

const resolvers = {
  Query: {
    hello: () => 'Hello World',
    getAllTasks: async () => {
      return await Task.find({})
    },
    getTask: async (_, args) => {
      // console.log({args})
      const { id } = args;
      const task = await Task.findById(id)
      return task;
    },
  },
  Mutation: {
    createTask: async (_, args) => {
      const { title, description } = args.task;
      const newTask = new Task({ title, description })
      await newTask.save();
      return newTask;
    },
    removeTask: async (_, args) => {
      const { id } = args;
      const task = await Task.findByIdAndDelete(id)
      return task;
    }
  }
}

module.exports = {resolvers}
```

También tendremos que cambiar el modo en que hacemos la mutación en el sandbox:

```
mutation{
	createTask(
        task:{
            title:"My last task", 
            description:"this is my task"
        }
	)
    {
      id
    }
}
```



### :rocket: Actualizar una tarea

Vamos a crear `updateTask` que recibe por un lado un `id` y además un objeto de tipo `task` como el definido anteriormente.

En `typeDefs.js`

```js
const { gql } = require('apollo-server-express');

const typeDefs = gql`
  type Task {
    id: ID!
    title: String!
    description: String
  }

  type Query {
    hello: String
    getAllTasks: [Task]
    getTask(id:ID):Task
  }

  input TaskInput{
    title: String
    description: String
  }

  type Mutation {
    createTask(task:TaskInput):Task
    removeTask(id:ID!):Task
    updateTask(id:ID!, task:TaskInput!):Task
  }
`;

module.exports = { typeDefs };
```

> El tipo de dato devuelto no es `Task!` ya que puede que no encuentre la tarea buscada.

En `resolvers.js`

```js
const Task = require('./models/Task.js');

const resolvers = {
  Query: {
    hello: () => 'Hello World',
    getAllTasks: async () => {
      return await Task.find({})
    },
    getTask: async (_, args) => {
      // console.log({args})
      const { id } = args;
      const task = await Task.findById(id)
      return task;
    },
  },
  Mutation: {
    createTask: async (_, args) => {
      const { title, description } = args.task;
      const newTask = new Task({ title, description })
      await newTask.save();
      return newTask;
    },
    removeTask: async (_, args) => {
      const { id } = args;
      const task = await Task.findByIdAndDelete(id)
      return task;
    },
    updateTask: async (_, {id, task}) => {
      const { title, description } = task;
      console.log(description)
      const updatedTask = await Task.findByIdAndUpdate(id, { title, description }, { new: true })
      return updatedTask;
    }
  }
}

module.exports = {resolvers}
```

Como queremos que nos devuelva la tarea nueva agregamos `{ new: true }` pues de lo contrario nos enviaría la vieja.

Notar que podemos hacer **actualizaciones parciales** ya que de acuerdo a la documentación de Mongoose nos agrega el operador `$set` por defecto.

```
Model.findByIdAndUpdate(id, { name: 'jason bourne' }, options, callback)

// is sent as
Model.findByIdAndUpdate(id, { $set: { name: 'jason bourne' }}, options, callback)
```

>Esto significa que si estamos actualizando una tarea y solo le pasamos la descripción, el título no se verá afectado y seguirá siendo el que era.



En el sandbox colocamos:

```
mutation{
  updateTask(
    id:"621aece0cfd39446c40096d0",
    task:{
      title:"Task updated 2",
    }
  ){
    title
    description
  }
}
```

