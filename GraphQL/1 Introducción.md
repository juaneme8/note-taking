# GraphQL

> Basado en [curso](https://www.youtube.com/watch?v=Y0lDGjwRYKw&list=PL4cUxeGkcC9iK6Qhn-QLcXCXPQUov1U7f) The Net Ninja



## Características

* GraphQL es **lenguaje de consultas**. **Graph** significa grafo y **QL** query language.

* Permite la **comunicación de datos entre servidor y cliente** (navegador).

* **Código abierto**.

* **Creado por Facebook**.

* Es **utilizable en múltiples lenguajes de programación**. 

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

GraphQL es *platform agnostic* esto significa que no es necesario usarlo con una tecnología en particular (puede usarse con React, Angular, Vue, etc). En nuestro caso consistirá en una aplicación React y en ella utilizando **Apollo como cliente GraphQL** haremos queries al server GraphQL para obtener datos y mostrarlos en el navegador. 



> :information_source: **Graphiql** es como una dummy frontend application que nos permite hacer requests al server GraphQL



# Proyecto

Utilizaremos Apollo por un lado con **Apollo Server** y también Apollo Client (con React).

Desde la aplicación web haremos la petición utilizando GraphQL que llegará a Apollo y será este quien se encargará de buscar la información a una REST API, microservicios o base de datos.



# Servidor

```
mkdir md-graphql-server
cd md-graphql-server
npm init -y
npm i apollo-server graphql 
```



En `package.json` agregamos `"type":"module"` para utilizar ECMAScript Modules.



Creamos un archivo `index.js` donde tenemos array de objetos `people` con los datos que luego serán de una API o una DB. `typeDefs` que es la descripción de estos datos y de las peticiones que se pueden hacer. Por último en `resolvers` indicamos cómo serán resueltas estas queries.

```javascript
import {gql} from 'apollo-server'

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
		findPerson(name:String!):Person
	}
`

const resolvers={
    Query:{
        personCount: () => people.length,
        allPeople: () => people
        findPerson: (root, args) => {
    		const {name} = args;
			return people.find(person => peroson.name===name)
		}
    }
}

const server= new ApolloServer({
    typeDefs,
    resolvers
})

server.listen().then(({ url }) => {
	console.log(`Server ready at ${url}`)
}); 
```

> :memo: Con el signo de exclamación indicamos que el campo es requerido.





Para leventar el servidor ejecutamos `node index.js` y si ingresamos a `localhost:4000` podremos acceder al **GraphQL Playground**. 

> A partir de la versión 3 de `apollo-server` nos redireccionará a https://studio.apollographql.com/.



## Sandbox

> :memo: Con `CONTROL+SPACE` tendremos un autocompletado del comando que podemos ejecutar. La primera vez nos sugerirá entre query, mutation, luego si ponemos `query {}` y volvemos a presionarlo nos sugerirá las posibles queries a ejecutar.



```
query{
	personCount
}
```

Obtenemos un objeto con la propiedad `data` que también es un objeto que cuenta con una propiedad `personCount`



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
	}
}
```

:warning: Cuando un campo no está disponible para uno de los objetos, el valor obtenido es `null`.



## Query con Parámetros

Es posible tener una query con parámetros por ejemplo un caso en el que recibimos un argumento String que será requerido y retornamos un objeto `Person`. 

```
findPerson(name:String!):Person
```

En el resolver vemos que recibimos en primer lugar el parámetro `root` y luego `args`. Utilizando `args.name` buscamos en el array el valor deseado.

```
findPerson: (root, args) => {
    		const {name} = args;
			return people.find(person => person.name===name)
		}
```

Para probar el funcionamiento de esta query

```
query{
  findPerson(name:"Midu"){
    phone
  }
}
```

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

Si buscamos algo que no existe 

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



En cambio si buscamos a alguien que si existe pero que no tiene un teléfono cargado:

```
{
  "data": {
    "findPerson": {
      "phone": null
    }
  }
}
```

