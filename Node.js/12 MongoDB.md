# Bases de Datos
Al trabajar con bases de datos existen de dos tipos **SQL** y **NoSQL** las cuales se caracterizan por estructurar los datos de manera distinta:

## SQL
SQL  (Structured Query Language) es el lenguaje estándar para trabajar con una base de datos relacional, es necesario contar con un  **RDBMS**  (**Relational Database Management System**) como por ejemplo **SQL Server**. 

Los  datos se guardan en **tablas**. Una tabla es un conjunto de entradas de datos relacionados que consiste en **filas** y **columnas**. 

# NoSQL
El término NoSQL (*not only SQL* o **no solo SQL**) hace referencia a bases de datos que trabajan con colecciones y documentos. 

# MongoDB
Dentro de las bases de datos NoSQL, probablemente una de las más famosas sea MongoDB. Es un **DBMS** (database management system) muy popular y es usado habitualmente en aplicaciones con Node.js y Express. Como en MongoDB almacenamos objetos (JSON)  en una colección, cuando hacemos el query obtenemos un objeto (JSON) y luego retornamos este objeto (JSON)  al cliente.

Las **colecciones** son similares a las tablas y los **documentos** son similares a los registros (*records*) en una arquitectura SQL. Los documentos tienen una apariencia JSON (JavaScript Object Notation) es decir *key/value pairs*.  Contamos con una propiedad *unique id* llamada **_id** la cual es autogenerada.

Desde nuestro código podremos conectarnos a una colección en MongoDB para guardar, leer, actualizar o eliminar documentos.

## Setup en PC
### Explicación 2021
>> Ver capturas en word Capturas MongoDB.docx

Ir a `C:\Program Files\MongoDB\Server\4.4\bin` y luego escribir en la barra de direcciones cmd para abrir la terminal en esa ruta. Luego ejecutar `mongo --version`  para obtener la versión recientemente instalada. Si intentamos hacer lo mismo desde otra ruta nos dirá:

>> mongo' is not recognized as an internal or external command,operable program or batch file. Es por esto que debemos asignar la variable de entorno **PATH**.

Ejecutar (`WIN+R`)  `sysdm.cpl` para ir a `Propiedades del Sistema`. Una vez allí ir a `Opciones avanzadas`, luego `Variables de entorno` y en la parte inferior en `Variables del sistema` seleccionar `Path`, click en `Editar` y luego `Nuevo` y pegar la ruta que contiene el MongoDB Server:  `C:\Program Files\MongoDB\Server\4.4\bin`

Luego si cerramos la terminal y la volvemos a abrir ejecutando mongo –versión veremos los datos del shell y si hacemos `mongod --versión` también obtendremos los datos dando cuenta que también la base de datos fue correctamente instalada.

Para conectar la terminal con MongoDB ejecutamos `mongo` veremos nuestra dirección IP y el número de puerto `27017`

* `show dbs` para ver las bases de datos instaladas por default. 
* `use myFirstDB` para crear una nueva base de datos.
* `db.students.insert({"FirstName":"Juan"})` para crear una colección especificamos el valor en formato JSON.
* `show collections` para ver todas las colecciones de esa db.
* `db.studends.find()` para ver todos las entradas de esa colleción.

# Pre 2021
Ejecutar en la consola `mongod` deberíamos ver un mensaje largo que al final dice `Not existing path`, por lo tanto para crearlo `md C:\data\db`. Luego volvemos a ejecutar `mongod` y no obtenemos la excepción sino que queda a la espera de conexiones.

A continuación en MongoDB Compass, `New Connection` y por último `Connect` (dejamos los valores default de localhost/27017)

> Si ya tenemos mongodb instalado y queremos saber qué versión tenemos ejecutar en la consola `mongod --version`. En caso de querer actualizar podemos seguir los mismos pasos. Una vez completado el proceso en MongoDB Compass no veremos los datos de la base de datos que teníamos sino sólo las que mongo utiliza para sus tareas.

## Setup en la Nube
En lugar de instalarlo en la PC, es posible usar un servicio en la nube como [MondoDB Atlas](https://www.mongodb.com/cloud/atlas).
Lo primero que hacemos es crear un **cluster** elegimos el plan gratis, luego el provedor y región (dejar los valores default) y le asignamos un nombre. Luego de unos minutos lo creará.

Podemos crear la base de datos y la colección yendo a **Collections** y luego a **Add My Own Data**.

A continuación vamos al menu **Database Access** y creamos un usuario para luego conectarnos desde el código: asignamos un nombre de usuario, contraseña y seleccionamos el nivel de privilegio **"Read and write to any database"**
Luego vamos a **Connect**, seleccionamos **Connect your Application** y luego copiamos el **connection string**.

```js
 const dbURI = 'mongodb+srv://**<username>**:**<password>**@cluster0-niujt.mongodb.net/**<dbname>**?retryWrites=true&w=majority';
```

Debemos reemplazar  \*\*\<username>\*\* por el usuario , \*\*\<password>** por la contraseña definidos anteriormente y \*\*\<dbname>** por el nombre de la base de datos (en caso de que no exista la creará).

Si bien podríamos conectarnos y realizar queries utilizando la API standard de MondoDB (`npm install mongodb` y luego `const MongoClient=require('mongodb').MongoClient) `por motivos de simplicidad utilizaremos **mongoose** que está construído sobre `MongoClient`.

Para almacenar el URI podemos crear un archivo `.env.local` en el cual ponemos:
`MONGO_URI=mongodb+srv://**<username>**:**<password>**@cluster0-niujt.mongodb.net/**<dbname>**?retryWrites=true&w=majority`

# MongoDB y Express:
En las aplicaciones Express podremos trabajar con SQL Server, MySQL, MongoDB, etc sólo será cuestión de cargar el driver de Node.js apropiado.

Más información en: [https://expressjs.com/en/guide/database-integration.html](https://expressjs.com/en/guide/database-integration .html).

Trabajaremos con **MongoDB** mediante el paquete **Mongoose**.

Supongamos que tenemos una colección como la siguiente:
```json
{
"_id": ObjectId(12345),
"title": "Opening Party",
"snippet": "all about the...",
"body": "blah blah blah..."
}
```
Desde nuestro código podremos conectarnos a esa colección en MongoDB para guardar, leer, actualizar o eliminar documentos.

# Mongoose
Mongoose es una **ODM Library** (object document mapping) y eso significa que toma la API standard de MongoDB y proporciona un modo mucho más simple de comunicarnos con la base de datos. Esto lo logra permitiéndonos crear **modelos de datos** con los cuales accedemos a métodos para crear, listar, actualizar y eliminar documentos. 

## Instalación Mongoose
```bash
 npm install mongoose
```

## Conexión a Mongoose
### Modo 1:
Si colocamos simplemente `mongoose.connect(dbURI)` obtendremos un deprecation warning por lo que le agregamos un segundo argumento que es un options object: `mongoose.connect(dbURI, { useNewUrlParser: true, useUnifiedTopology: true })` como se trata de una tarea asincrónica que toma cierto tiempo en realizarse y retorna una promesa.

Como queremos que la conexión con la base de datos sea única (y no una para cada API que tengamos en la carpeta `routes`) lo realizamos en `app.js`

```js
const mongoose = require('mongoose');

const dbURI = 'mongodb+srv...';

mongoose.connect(dbURI, { 
	useNewUrlParser: true, 
	useUnifiedTopology: true 
})
	.then(result => console.log('connected to db'))
	.catch(err => console.error(err));
```

> El uso del **paquete debug** nos posibilitaría filtrar los mensajes relacionados con la base datos para aquellos momentos en que no queremos mostrarlos.
> Notar que en el método catch utilizamos `console.error()`
> Para no obtener un warning al usar findByIdAndRemove() y findByIdAndUpdate, findOneAndRemove(), findOneAndUpdate() debemos agregar también al **option object** useFindAndModify: false,


### Conexión a Mongoose Modo 2:
```js
mongoose.connect(dbURI, {
	useNewUrlParser: true,
	useUnifiedTopology: true,
});

const db = mongoose.connection;
db.once('open', () => console.log('Conectado a MondoDB'));
db.on('error', (err) => console.log(err));
```

### Conexión en un archivo aparte:
Podemos crear un archivo `/config/connectMongoDB.js` y en el un método llamado `connectMongoDB()` pegar todas estas líneas de código. En la parte inferior exportamos dicho método  `module.exports = connectMongoDB;` De esta manera en `app.js` primero haremos el require `const connectMongoDB = require('./config/dbMongo');` y luego podremos usarla directamente `connectMongoDB();`

### Connection Strings:
1) Si trabajamos con MongoDB en la nube el connection string será de la forma:
```js
 const dbURI = 'mongodb+srv://**<username>**:**<password>**@cluster0-niujt.mongodb.net/**<dbname>**?retryWrites=true&w=majority';

```
> En Node.js Auth en el curso de the net ninja elimina la parte de `?retryWrites=true&w=majority`

2) Si trabajamos con MongoDB instalado en la PC el connection string será del tipo `mongodb://localhost/playground` siendo playground el nombre de la base de datos. 

3) Cuando hagamos el deploy de la aplicación tendremos un connection string distinto. En este caso debemos trabajar con archivos de configuración como por ejemplo con el **paquete config**.

En caso de que la base de datos no esté creada, se creará cuando queramos escribir algo en ella.

## Schemas & Models
Los **schemas** definen la estructura de los datos que conforman el documento, es decir sus propiedades y el tipo de datos de ellas.

Antes de meternos en la sintaxis sabemos que nuestro Schema queremos que tenga las siguientes propieades y el siguiente tipo:

* title(string), required
* snippet(string), required
* body (string), required
> Tener en cuenta que _id es propiedad autogenerada

Luego creamos un **modelo** basado en ese **esquema** (decimos que el **model** envuelve al **schema**) con el cual podremos comunicarnos con una colección en particular, podrá contar con **métodos estáticos** (por ejemplo siendo Blog el modelo `Blog.find()`) y **métodos de instancias** (`const blog = new Blog({...}) y luego blog.save())`  con los cuales podremos leer, modificar, actualizar o eliminar datos de esa colección.

A la hora de trabajar con una colección en MongoDB debemos crear el Schema y el Model para dicha colección. Suponiendo que tenemos una colección `blogs` creamos una carpeta `models` y en ella una archivo `blog.js`.

En primer lugar debemos crear una instancia del `Schema` object y al constructor le pasamos como primer argumento un objeto con las características  de la estructura del documento y como segundo argumento para tener un registro automático de cuando se crea o actualiza un documento mediante un timestamp le pasamos algo similar a un options object en el cual ponemos `{ timestamp: true}` 

A continuación **crearemos un modelo basado en este schema**, típicamente se utiliza una letra mayúscula. `const Blog = mongoose.model('Blog', blogSchema);` notar que `Blog` está en singular y luego será pluralizado. 

```js
const mongoose = require('mongoose');

const Schema = mongoose.Schema;
const blogSchema = new Schema({
	// Si no fuera required podríamos poner directamente:
	// title: String,
	title: {
	 type: String,
	 required: true,
	},
	snippet: {
	 type: String,
	 required: true,
	},
	body: {
	 type: String,
	 required: true 
	},
 }, { timestamps: true });
 
const Blog = mongoose.model('Blog', blogSchema);

module.exports = Blog;
```
En este archivo también tendríamos que tener la función encargada de validar los datos de entrada mediante joi o similar. Luego en vez de `module.exports = Blog;` tendríamos por ejemplo `module.exports.Blog = Blog` y `module.exports.validate = validateBlog` y luego en `app.js` pondríamos `const {Blog, validate} = require('./models/blog');`

En `app.js` agregamos una serie de sandbox routes:


```js
const  Blog  =  require('./models/blog');

// mongoose & mongo tests
app.get('/add-blog', (req, res) => {
	const blog = new Blog({
		title: 'new blog',
		snippet: 'about my new blog',
		body: 'more about my new blog'
	})
	blog.save()
	.then(result => {
		res.send(result);
	})
	.catch(err => {
	console.log(err);
	});
});

app.get('/all-blogs', (req, res) => {
	Blog.find()
	.then(result => {
		res.send(result);
	})
	.catch(err => {
		console.log(err);
	});
});

app.get('/single-blog', (req, res) => {
	Blog.findById('5ea99b49b8531f40c0fde689')
	.then(result => {
		res.send(result);
	})
	.catch(err => {
		console.log(err);
	});
});
```

Si ingresamos a `/add-blog` agregamos un elemento a la base de datos y debido a que estamos enviando como respuesta al navegador el resultado obtenido luego de guardar: `res.send(result);` veremos en pantalla al documento agregado a la base de datos:
```json
{"_id":"5eb415667...", "title": "new blog", "snippet": "about my new blog",
"body": "more about my new blog", "createdAt": "2020-05-07T14:04:22.341Z", 
"updatedAt": "2020-05-07T14:04:22.341Z", "__v":0}
```
Si bien en este caso trabajamos con promesas, podríamos haber hecho lo mismo utilizando **async/await**, notar tres cosas: que el callback pasa a ser `async (req,res)`, el uso de `await` y el agregado del bloque `try/catch`.

```js
app.get('/add-blog', async(req, res) => {
	try{
		const blog = new Blog({
			title: 'new blog',
			snippet: 'about my new blog',
			body: 'more about my new blog'
		})
		result = await blog.save();
	}
	catch(err){
		console.log(err);
	}
});
```

Si ingresamos a `/all-blogs` veremos un array con todos los documentos de la colección utilizando el método `find()`

Con respecto a la ruta `/single-blog` en ella utilizamos el metodo `findById()` pero debemos tener presente que `_id` es un `ObjectId` y no un `string` en **MongoDB**. Cuando lo usamos en **mongoose** realiza la conversión automáticamente por lo que podemos trabajar directamente con el string. 

A la hora de querying documents también contamos con el método `findOne()`.


## Crear Documentos
En el siguiente ejemplo mostramos cómo crear un nuevo documento sin hacerlo desde el route handler y utilizando **async/await**, debemos tener presente que no lo ejecutamos con `nodemon app.js` sino con `node app.js` dado que sino cada vez que hagamos un cambio agregaría el elemento a la base de datos.
```js
async  function  createCourse() {
	try {
		const  course  =  new  Course({
			name: 'Node.js',
			author: 'Mosh',
			tags: ['node', 'backend'],
			isPublished: false,
		});
		const  result  =  await  course.save();
		console.log(result);
	} catch (ex) {
		console.log(ex.message);
	}
}

createCourse();
```

Cuando la creación de documentos forma parte de una API y estemos dentro de un route handler es buena práctica retornar aquello que fue agregado a la DB indicando el éxito de la operación, por lo que en ese caso pondríamos `res.send(result)`.

Existen dos formas de crear un documento una de ellas es con una instancia y save() (como vimos recién)
```js
const  course  =  new  Course({
			name: 'Node.js',
			author: 'Mosh',
			tags: ['node', 'backend'],
			isPublished: false,
		});
		const  result  =  await  course.save();
```
Y la otra forma es con del model y un método de mongoose.
```js
const course = await Course.create({
	name: 'Node.js',
	author: 'Mosh',
	tags: ['node', 'backend'],
	isPublished: false,
})

```

## Obtener Documentos
De ahora en más por simplicidad se omitirá el bloque `try/catch` por lo que sólo asumimos el escenario en que las promesas se cumplen (`fulfilled`) , si son rechazadas (`rejected`) no tenemos código para manejar la situación.

```js
async  function  getCourses() {
	const  courses  =  await  Course
		.find({ author: 'Mosh', isPublished: false })
	console.log(courses);
}
```

Estamos haciendo `console.log(courses)` dentro de `getCourses()` idealmente deberíamos tener otra función encargada de mostrar los cursos ya que el propósito de `getCourses()` debería ser sólo obtenerlos.

## Filtrar Documentos
```js
const courses = await Course
	.find({author:'Mosh',isPublished:true})
	.limit(10)
	.sort({name:1})
	.select({name:1, tags:1})
```

Con `.find({author:'Mosh',isPublished:true})` indicamos los elementos deseados.

Con `.limit(10)` la cantidad máxima a  mostrar.

Con `.sort({name:1})` que deseamos ascending order o alfabético también podemos hacerlo con un **string** `.sort('name')` para ascending order o `sort('-name')` descending order. 

Con `.select({name:1, tags:1})` las propiedades que nos interesan, también lo podemos expresar con un **string** `.select('name tags')`


##  Operadores de Comparación
* **eq** (equal)
* **ne** (not equal)
* **gt** (greater than)
* **gte** (greater than or equal to)
* **lt** (less than)
* **lte** (less than or equal to)
* **in**
* **not in**

Con el signo `$` indicamos que se trata de un **operador**.

##  Operadores Lógicos

* **or** usamos el método `or([{},{}])`:

```js
const courses = await Course
	.find()
	.or([{author:'Mosh'},{isPublished:true}])
```
---
* **and**: podemos hacerlo de dos formas
1) usando dos pares key/value como argumento de `find()`
```js
const courses = await Course
	.find({author:'Mosh',isPublished:true})
```

2) usando el método `and([{},{}])`
```js
const courses = await Course
	.find()
	.and([{author:'Mosh'},{isPublished:true}])
```

### Ejemplos:
Si queremos obtener precios que sean mayores o iguales a 10 y menores o iguales a 20
```js
const courses = await Course
	.find({price:{$gte:10,$lte:20}})
```
Si queremos obtener los cursos cuyo `price` es de `10`, `15` o `20` usamos el concepto de JavaScript para múltiples valores: **array**.
```js
const courses = await Course
	.find({price:{$in:[10,15,20]}})
```

Si queremos obtener todos los cursos que contienen el tag `'frontend'` o `'backend'` podemos hacerlo de dos formas:
```js
const courses = await Course
	.find({tags:{$in:['frontend','backend']}})
```
```js
const courses = await Course
	.find()
	.or([{tags:'frontend'},{tags:'backend'}])
```

Si queremos obtener todos los cursos cuyo `price` es mayor o igual a `15` o que contienen la palabra `by` en el `title`:

```js
const courses = await Course
	.find()
	.or([
		{price: {$gte:15}},
		{name: /.*by.*/i}
	])
```
En la *regular expression* utilizada, colocamos `.*` ya que con el `.` representamos un caracter y con el `*` indicamos que puede haber 0 o +. Como queremos que la búsqueda sea *case insensitive* agregamos la letra `i` al final.

## Counting
Si queremos conocer la cantidad de documentos devueltos por una query:
```js
const  courses  =  await  Course
	.find({ author: 'Mosh', isPublished: false })
	.countDocuments();
```
> En el curso de Mosh se usaba el método `.count()` pero actualmente arroja un DeprecationWarning.


## Pagination
Anteriormente usamos el método `.limit()` para limitar la cantidad de documentos a mostrar, este trabaja a menudo en conjunto con el método `skip()`. 
En una **RESTful API** recibiremos `pageNumber` y `pageSize` por *query string parameters* desde por ejemplo `/api/courses?pageNumber=2&pageSize=10` luego con esos valores determinaremos la cantidad de documentos que queremos saltarnos con `skip()` para luego mostrar la cantidad determinada por `limit()`.
Asumimos que `pageNumber` comienza en 1
```js
const pageNumber = 2;
const pageSize = 10;
const  courses  =  await  Course
	.find({ author: 'Mosh', isPublished: false })
	.skip((pageNumber-1)*pageSize)
	.limit(pageSize)
```


## Importar JSON en MongoDB
Supongamos que tenemos un archivo llamado `exercise-data.json` en el cual tenemos un array de objetos que queremos incorporar en una base de datos.

```json
[
	{"_id": "5a68..", "tags":["express", "backend"],"date":"..."},
	{"_id": "5a68..", "tags":["express", "backend"],"date":"..."},
	{"_id": "5a68..", "tags":["express", "backend"],"date":"..."}
]
```

En la terminal navegamos hasta la carpeta en la cual tenemos el archivo: 

```bash
mongoimport --db mongo-exercises --collection courses 
--file exercise-data.json --jsonArray
```

Primero debemos poner `mongoimport` luego con `--db mongo-example` especificamos la base datos en la cual queremos importar los datos, con `--collection courses` el nombre de la colección luego con `--file exercise-data.json` le pasamos el nombre del archivo. Por último ponemos el flag `--jsonArray` ya que los datos están en ese formato.


## Queries con RegEx 
Si queremos buscar todos los cursos que tengan como autor la palabra Mosh en mayúscula o minúscula y en cualquier posición:
```js
const  courses  =  await  Course
	.find({ author: /.*Mosh.*/i})
```

## Actualizar documentos
Existen dos modos de actualizar documentos, uno de ellos es **Query First** en el utilizaremos el método estático `findById()`, luego modificaremos la propiedad y por último emplearemos el método de instancia `save()`. La otra metodología se conoce como **Update First** y consiste en actualizar directamente utilizando el método `updateOne()` [^1] y  opcionalmente obtener el documento actualizado con `findByIdAndUpdate()`.

[^1]: Mosh utiliza el método `update()` pero actualmente obtenemos *DeprecationWarning*.

### Query First:
```js
async  function  updateCourse(id) {
	const  course  =  await  Course.findById(id);

	//el curso no existe
	if (!course)
		return;
	
	//opción 1 para actualizar
	course.isPublished  =  true;
	course.author  =  'Another Author';
	//opción 2 con el método set()
	/*
	course.set({
		isPublished: true,
		author: 'Another Author'
	});
	*/
	const  result  =  await  course.save();
	console.log(result);
}
updateCourse('5f0643a77261c712080228f5');
```
Esta metodología será útil por ejemplo cuando queramos actualizar o no dependiendo del valor de alguna propiedad del documento. Podríamos agregar `if(course.isPublished) return` con lo cual no permitiríamos cambiar el autor de un curso ya publicado.


### Update first
```js
async  function  updateCourse(id) {
	const  result=  await  Course.updateOne(
	{
		_id: id,
	},
	//update object -> MongoDB update operators
	{
		$set: {
			author: 'Juan',
			isPublished: false,
		},
	});
	console.log(result); //{ n: 1, nModified: 1, ok: 1 }
}
updateCourse('5f0643a77261c712080228f5');
```
Notar que al llamar a `updateOne()` no obtenemos el curso actualizado.

 **También es posible no utilizar el operador `$set`.**

En el ejemplo anterior utilizamos el update operator `$set` podemos ver una lista completa en [https://docs.mongodb.com/manual/reference/operator/update/](https://docs.mongodb.com/manual/reference/operator/update/).
Por ejemplo si quisiéramos incrementar el precio un 21% podríamos utilizar también el update operator `$mul`.

```js
{
	$set: {
		author: 'Juan',
		isPublished: false,
		},
	$mul: {
		price: 1.21,
		}
}
```

Si queremos obtener el documento actualizado podemos utilizar el método `findByIdAndUpdate()` (ver más adelante qué argumento debemos pasarle para obtenerlo)

```js
async  function  updateCourse(id) {
	const  course  =  await  Course.findByIdAndUpdate(
	id,
	{
		author: 'Jack',
		isPublished: true,
	});
	console.log(course); 
}
updateCourse('5f0643a77261c712080228f5');
```

Sin embargo en pantalla veremos el valor del documento previo a la actualización:
```bash
{
  tags: [ 'node', 'backend' ],
  _id: 5f0643a77261c712080228f5,
  name: 'Node.js',
  author: 'Juan',
  price: 200,
  isPublished: false,
  date: 2020-07-11T20:41:41.215Z,
  __v: 0
}
```
Para obtener el valor actualizado debemos agregar otro argumento a `findByIdAndUpdate(id, updateObject, {new: true})`

## Eliminar documentos
De la misma manera que cuando actualizamos documentos contamos con el método `deleteOne()` que no devuelve el documento eliminado y `findByIdAndDelete()` que sí lo entrega.

```js
async  function  removeCourse(id) {
	const  result  =  await  Course.deleteOne({ _id: id });
	console.log(result); //{ n: 1, ok: 1, deletedCount: 1 }
}
```

Si hay más de un documento que reúne los criterios, será elimnado el primero.
Si el id que buscamos no existe `deleteOne()` entregará `{ n: 0, ok: 1, deletedCount: 0 }`

```js
async  function  removeCourse(id) {
	const  course  =  await  Course.findByIdAndDelete(id);
	console.log(course); //curso eliminado
}
```
Si el id que buscamos no existe `findByIdAndDelete()` entregará `null`.

Si queremos eliminar varios documentos podemos utilizar el método `deleteMany(filterObject)`

## Validación
Por default todas las propiedades de un Schema son opcionales por lo que podremos crear un documento sin ninguna propiedad `const course = new Course({})` sin inconvenientes.

```js
const courseSchema = new mongoose.Schema({
	name: String,
	author: String,
	price: Number,
	tags: [String],
	date: {
		type: Date,
		default: Date.now,
	},
	isPublished: Boolean,
});
```

Con Mongoose podemos implementar validación.

### Built-in Validators:

**Required Validator**
1) Validación automática
Si queremos que la propiedad name por ejemplo sea requerida podemos hacerlo con 
```js
const courseSchema = new mongoose.Schema({
	name: {
		type:String,
		required: true
	},
	//...
```
Cuando en vez de poner `name: String` hacemos `name: { type: String, required: true}` estamos haciendo uso del **SchemaType object**.
Luego si intentamos guardar un elemento sin el campo required obtendremos una excepción: `Course validation failed: name: Path 'name' is required.`  Si no tuviéramos el bloque `try/catch` obtendríamos `UnhandledPromiseRejectionWarning`.
Si queremos obtener un mensaje personalizado en caso de que falle la validación de required podemos hacerlo del siguiente modo:  `name: { type: String, required: [true,'mensaje personalizado']}`

2) Validación manual
Anteriormente vimos como la validación se activa automáticamente cuando intentamos guardar en la base de datos, pero también podemos dispararla manualmente con `await course.validate();` y si hay un error de validación obtendremos una excepción por lo que terminaremos en el bloque catch. 

```js
async function createCourse() {
	const course = new Course({
		//name: 'JavaScript',
		author: 'The Net Ninja',
		price: 9.99,
		tags: ['javascript', 'frontend'],
		isPublished: true,
	});
	try {
		await course.validate();
		//const result = await course.save();
		//console.log(result);
	} catch (error) {
		console.log(error);
	}
}
```

El método `validate()` retorna una `Promise <Void>` por lo que no entrega un resultado booleano con el cual podríamos implementar cierta lógica. Si queremos hacerlo podemos cambiar de metodología y en vez de usar async/await usar un callback que recibe error como argumento.  

```js
try{
	course.validate((err) => {
		if(err){
			//lógica a ejecutar en caso de error de validación
		}
	}
}
catch(ex){
	console.log(ex.message);
}
```
Si bien esta lógica podríamos colocarla en el bloque catch eso en oportunidades podría resultar confuso.

#### Validación de Mongoose + joi
Es conveniente utilizar el paquete **joi** (para chequear que los datos que nos pasa el cliente son válidos) de manera complementaria con las validaciones de **mongoose** (para volver a chequear antes de guardar por si hubo un error en el manejo de los datos). 
Ambas validaciones no necesariamente serán iguales por ejemplo si tenemos una coleccción movies que en su interior embebe datos de autores, para **joi** validaremos `genreId` que es lo que nos pasa el cliente mientras que para mongoose validaremos un objeto `genre`.

```js
router.post('/', async (req,res)=>{
	//1) validación de datos de entrada con joi
	const {error} = validateCustomer(req.body);
	if(error)
		return res.status(400).send('error.details[0].message);
	
	//creación de objeto
	//2) validación antes de guardar por parte de mongoose
	//save()
}
```

**Otros built-in validators:**
A la propiedad required no sólo podemos setearla como boolean sino que también podemos setearla con una función que retorna un boolean. Esto es útil cuando queremos que sea required o no dependiendo de alguna condición.
Supongamos que `price` es required si el curso tiene `isPublished = true`, en el Schema tendremos lo siguiente:
```js
	const courseSchema = new mongoose.Schema({
	name: {
		type: String,
		required: true,
	},
	author: String,
	price: {
		type: Number,
		required: function () {
			return this.isPublished;
		},
	},
	tags: [String],
	date: {
		type: Date,
		default: Date.now,
	},
	isPublished: Boolean,
});
```
Notar que no podemos usar arrow functions ya que no tienen su propio this, sino que usan el valor del *enclosing execution context* y queremos en cambio que `this` referencie al objeto `course`.

```js
const course = new Course({
	//name: 'JavaScript',
	author: 'The Net Ninja',
	tags: ['javascript', 'frontend'],
	isPublished: true,
	//price: 100
});
```
Al intentar guardar en la base de datos obtendremos: `Course validation failed: price: Path 'price' is required., name: Path 'name' is required.`

**Built-in Validators Adicionales:**
Dependiendo del tipo de propiedad podremos contar con validaciones adicionales.
Por ejemplo para **strings**:
```js
name:{
	type: String,
	required: true,
	minlength: 5,
	maxlength: 255,
	match: /javascript/i,,
},
category:{
	type: String,
	enum: ['web','mobile','network']
}
```
Como ya dijimos cuando en vez de poner `name: String` hacemos `name: { type: String, required: true}` estamos haciendo uso del **SchemaType object**. 

Con `minlength` establecemos la cantidad mínima de caracteres, con `maxlength` la máxima, con `match` una **regEx** que debe matchear. Con `enum` debemos usar uno de los valores del array.

`lowercase: true` convierte el valor de la propiedad en minúscula, `uppercase: true` convierte el valor en mayúscula, `trim: true`elimina los espacios en blanco a ambos lados del string.

Para **números y fechas** contamos con las validaciones  `min` y `max`
```js
price: {
	type: Number,
	required: true,
	min: 10,
	max: 200
}
```

Independientemente del tipo de propiedad, mongoose cuenta con getters y setters que nos permitirán ejecutar cierta lógica cuando obtenemos o seteamos una propiedad en un documento.
Por ejemplo si queremos que cuando cargamos un determinado precio este sea redondeado podemos implementar un setter `set: (v) => Math.round()` si tengo almacenados precios sin redondear y queremos que cuando **accedamos a esa propiedad** (si imprimimos en pantalla todo el objeto veremos el valor sin alterar) obtenerlos redondeados `get: (v) => Math.round()`
```js
price: {
	type: Number,
	required: true,
	min: 10,
	max: 200,
	get: v => Math.round(v),
	set: v => Math.round(v)
}
```

### Custom Validators:
En ciertas oportunidades con los built-in validators no lograremos la validación deseada.
Si quisiéramos que un documento con una propiedad array de strings `tags: [String]`tenga al menos un elemento, esto no podríamos hacerlo con required ya que para mongoose será válido aunque nos pasen una array vacío:
```js
tags: {
	type: [String],
	required: true,
}
```
Luego aceptaría un objeto con la propiedad tags igual a un array vacío `tags: []`, por lo que necesitamos de un custom validator. Agregamos la propiedad `validate` que es un objeto que tiene dos propiedades, una de ellos es `validator` (que recibe el valor a validar como argumento y retorna false en caso de error) y el otro es el mensaje que mostraremos si falla la validación.

```js
tags: {
	type: [String],
	validate: {
		validator: function (v) {
			return v.length > 0;
		},
		message: 'Debe haber por lo menos un tag',
	},
}
```

Si le pasáramos `tags: null` obtendríamos en cambio `Course validation failed: tags: Cannot read property 'length' of null` es por eso que en lugar de `return v.length > 0;` ponemos `return v&&v.length > 0;` 

##  Async Validators:
En ocasiones para generar nuestros *custom validators* tendremos que realizar una operación asincrónica como ser leer algo de una base de datos o de un servicio http remoto, por lo que no vamos a tener una respuesta inmediata es por eso que necesitamos de un validador asincrónico.
> La técnica explicada por Mosh en video 98 está desactualizada ya que arroja un *DeprecationWarning* que sugiere utilizar lo explicado en: https://mongoosejs.com/docs/validation.html#async-custom-validators

```js
tags: {
		type: [String],
		validate: {
			validator: function (v) {
				return new Promise(function (resolve, reject) {
					resolve(false);
				});
			},
			message: 'Debe haber por lo menos 1 tag',
		},
	},
```

## Errores de Validación
 Como ya dijimos cuando utilizamos el método `save()` o `validate()` y se genera un error de validación, ejecutamos el código del bloque catch. En `ex.errors` tenemos una *propiedad* para cada *propiedad inválida* del objeto. Por ejemplo si tenemos un error en la propiedad *category* y *tags*, tendremos información sobre los errores acontecidos en `ex.errors.category` y `ex.errors.tags`. Por lo que podremos recorrer el objeto `ex.errors` utilizando **for-in** con el propósito de obtener más detalles sobre los errores de validación.

```js
catch (ex) {
	for (field in ex.errors) {
		console.log(ex.errors[field].message);
	}
}
```

## Manejo de Erores:
Una alternativa a mostar el error en los bloques try/catch es hacerlo en un custom middleware para el manejo de errores.

En `app.js`
```js
app.use(express.json());
app.use('/api/genres', genres);

app.use((err, req, res, next) => {
	console.log('Error:' + err);
	res.status(422).send({ error: err.message });
	//422 unprocessable entity
});
```

**NOTA:** Como queremos probar la validación de mongose con postman (sin pasarle un parámetro required) comentamos por el momento la validación de joi, sino esta nos impediría llegar a la parte de guardar el documento.

Luego en `/routes/genres.js`
```js
router.post('/', async (req, res, next) => {
	const { error } = validate(req.body);
	//validacion de joi comentada
	//if (error) return 	res.status(400).send(error.details[0].message);

	const { name } = req.body;

	const genre = new Genre({ name });
	try {
		await genre.save();
		res.send(genre);
	} catch (err) {
		next(err);
	}
});

```

## Ejemplo express+mongoose (promises):
Como no queremos que el server esté escuchando requests hasta tanto haber efectuado la conexión a la base de datos, recién hacemos el `app.listen('3000')` dentro del callback del then method de `mongoose.connect()` que se ejecuta cuando la conexión se efectuó.

Por lo que nos quedará:

```js
mongoose.connect(dbURI, { useNewUrlParser: true, useUnifiedTopology: true })
	.then(result => app.listen('3000'))
	.catch(err => console.log(err));
```

Si colocamos simplemente `mongoose.connect(dbURI)` obtendremos un deprecation warning por lo que le agregamos un segundo argumento que es un options object: `mongoose.connect(dbURI, { useNewUrlParser: true, useUnifiedTopology: true })` como se trata de una tarea asincrónica que toma cierto tiempo en realizarse y retorna una promesa.

```js
const mongoose = require('mongoose');

const dbURI = 'mongodb+srv...';
mongoose.connect(dbURI, { useNewUrlParser: true, useUnifiedTopology: true })
	.then(result => console.log('connected to db'))
	.catch(err => console.log(err));
```

Como no queremos que el server esté escuchando requests hasta tanto haber efectuado la conexión a la base de datos, recién hacemos el `app.listen('3000')` dentro del callback del then method de `mongoose.connect()` que se ejecuta cuando la conexión se efectuó.

Por lo que nos quedará:

```js
mongoose.connect(dbURI, { useNewUrlParser: true, useUnifiedTopology: true })
	.then(result => app.listen('3000'))
	.catch(err => console.log(err));
```

## Outputting Documents in Views:
Cuando ingresemos a la home `/` seremos redireccionados a `blogs` que es una nueva ruta que creamos para mostrar todos los blogs ordenados por fecha de creación (del más nuevo al más viejo) `Blog.find().sort({ createdAt: -1 })` notar que cuando renderizamos como le pasamos `res.render('index', { blogs: result, title: 'All blogs' });`la propiedad blogs que es el mismo nombre que tenía cuando lo renderizábamos a través de la ruta `/` por lo que no tendremos que cambiar nada en `index.ejs`

```js
app.get('/', (req, res) => {
    res.redirect('/blogs');
});

app.get('/about', (req, res) => {
	res.render('about', { title: 'About' });
});

// blog routes
app.get('/blogs/create', (req, res) => {
	res.render('create', { title: 'Create a new blog' });
});

app.get('/blogs', (req, res) => {
	Blog.find().sort({ createdAt: -1 })
	.then(result => {
		res.render('index', { blogs: result, title: 'All blogs' });
	})
	.catch(err => {
		console.log(err);
	});
});

// 404 page
app.use((req, res) => {
	res.status(404).render('404', { title: '404' });
});
```