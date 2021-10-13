# Bases de Datos
Al trabajar con bases de datos existen de dos tipos **SQL** y **NoSQL** las cuales se caracterizan por estructurar los datos de manera distinta:

## SQL
SQL  (Structured Query Language) es el lenguaje estándar para trabajar con una base de datos relacional, es necesario contar con un  **RDBMS**  (**Relational Database Management System**) como por ejemplo **SQL Server**. 

Los  datos se guardan en **tablas**. Una tabla es un conjunto de entradas de datos relacionados que consiste en **filas** y **columnas**. 

# NoSQL
El término NoSQL (*not only SQL* o **no solo SQL**) hace referencia a bases de datos que trabajan con colecciones y documentos. 

# MongoDB
Dentro de las bases de datos NoSQL, probablemente una de las más famosas sea MongoDB. Es un **DBMS** (database management system) muy popular y es usado habitualmente en aplicaciones con Node.js y Express. Como en MongoDB almacenamos objetos (similares a JSON pero que en realidad son binarios BSON)  en una colección, cuando hacemos el query obtenemos un objeto (JSON) y luego retornamos este objeto (JSON)  al cliente.

MondoDB  tiene **escalabilidad horizontal**, dado que podremos agregar más máquinas(a diferencia de lo que ocurre en un RDBMS donde para mejorar el rendimiento debemos adquirir una máquina más potente, lo que se conoce como escalabilidad vertical).

En MongoDB en lugar de tener relaciones (cosa que podríamos hacer manualmente) lo que haremos será duplicar o triplicar datos cuando corresponda.

Las **colecciones** son similares a las tablas y los **documentos** son similares a los registros (*records*) en una arquitectura SQL. Los documentos tienen una apariencia JSON (JavaScript Object Notation) es decir *key/value pairs*.  Esta similitud hace que sea la opción más elegida cuando se hace backend con JavaScript.

Contamos con una propiedad *unique id* llamada **_id** la cual es autogenerada.

Desde nuestro código podremos conectarnos a una colección en MongoDB para guardar, leer, actualizar o eliminar documentos.

Para la visualización de los datos de la base de datos podemos utilizar Compass, Robo 3T, etc.

## Aplicación de Escritorio

>Capturas en MongoDB.docx

Ir a `C:\Program Files\MongoDB\Server\4.4\bin` y luego escribir en la barra de direcciones cmd para abrir la terminal en esa ruta. Luego ejecutar `mongo --version`  para obtener la versión recientemente instalada. Si intentamos hacer lo mismo desde otra ruta nos dirá:

>mongo' is not recognized as an internal or external command,operable program or batch file. Es por esto que debemos asignar la variable de entorno **PATH**.

Ejecutar (`WIN+R`)  `sysdm.cpl` para ir a `Propiedades del Sistema`. Una vez allí ir a `Opciones avanzadas`, luego `Variables de entorno` y en la parte inferior en `Variables del sistema` seleccionar `Path`, click en `Editar` y luego `Nuevo` y pegar la ruta que contiene el MongoDB Server:  `C:\Program Files\MongoDB\Server\4.4\bin`

Luego si cerramos la terminal y la volvemos a abrir ejecutando mongo –versión veremos los datos del shell y si hacemos `mongod --versión` también obtendremos los datos dando cuenta que también la base de datos fue correctamente instalada.

Para conectar la terminal con MongoDB ejecutamos `mongo` veremos nuestra dirección IP y el número de puerto `27017`

* `show dbs` para ver las bases de datos instaladas por default. 
* `use myFirstDB` para crear una nueva base de datos.
* `db.createCollection('posts')` por convención las colecciones se crean en plural.
* `db.students.insert({"name":"Juan"})` para crear una colección y a su vez insertar un documento en ella.
* `show collections` para ver todas las colecciones de esa db.
* `db.studends.find()` para ver todos las entradas de esa colleción.
* `db.studends.find({"name":"Juan"})` para ver los estudiantes con ese nombre en la colección.
* `db.students.update({"name: "Juan"}, {$set:{"user":"@j8"}})` como queremos conservar el docuemento original y cambiar sólo el campo `user` debemos utilizar `$set`. Si hiciéramos directamente `db.students.update({"name: "Juan"}, {"user":"@j8"})` perderíamos todos los campos que teníamos en ese documento.

### Not Existing Path

Ejecutar en la consola `mongod` deberíamos ver un mensaje largo que al final dice `Not existing path`, por lo tanto para crearlo `md C:\data\db`. Luego volvemos a ejecutar `mongod` y no obtenemos la excepción sino que queda a la espera de conexiones.

A continuación en MongoDB Compass, `New Connection` y por último `Connect` (dejamos los valores default de localhost/27017)

> Si ya tenemos mongodb instalado y queremos saber qué versión tenemos ejecutar en la consola `mongod --version`. En caso de querer actualizar podemos seguir los mismos pasos. Una vez completado el proceso en MongoDB Compass no veremos los datos de la base de datos que teníamos sino sólo las que mongo utiliza para sus tareas.

## MongoDB Atlas
En lugar de instalarlo en la PC, es posible usar un servicio en la nube como [MondoDB Atlas](https://www.mongodb.com/cloud/atlas) que cuenta con un free tier.
Lo primero que hacemos es crear un **cluster** elegimos el plan gratis, luego el provedor y región (dejar los valores default) y le asignamos un nombre. Luego de unos minutos lo creará.

Un cluster es un set de servidores los cuales estan dispuestos a almacenar las bases de datos. Maneja una serie de réplicas de modo tal que si el servidor primario falla es posible utilizar otro secundario como primario sin pérdidas de información. 

Esta implementación tiene la ventaja que desde el primer momento seremos conscientes de lo que tendremos que hacer para producción en términos de tener que contemplar la seguridad, por ejemplo poniendo contraseña.

Podemos crear la base de datos y la colección yendo a **Collections** y luego a **Add My Own Data**.

A continuación vamos al menu **Database Access** y creamos un usuario para luego conectarnos desde el código: asignamos un nombre de usuario, contraseña y seleccionamos el nivel de privilegio **"Read and write to any database"**
Luego vamos a **Connect**, seleccionamos **Connect your Application** y luego copiamos el **connection string**.

```js
 const connectionString = 'mongodb+srv://**<username>**:**<password>**@cluster0-niujt.mongodb.net/**<dbname>**?retryWrites=true&w=majority';
```

> Debemos reemplazar  `**<username>**` por el usuario , `**<password>**` por la contraseña definidos anteriormente y `**<dbname>**` por el nombre de la base de datos (en caso de que no exista la creará).


En la parte de Network Access debemos especificar las direcciones IP desde las cuales se puede acceder al clúster. Si queremos que sea accesible desde cualquier IP debemos poner `0.0.0.0`, de todos modos hay otras formas de aportar seguridad a la base de datos. En el caso de producción con un servidor propio, sabremos la IP y podemos especificarla ahí. Distinto será si vamos a deployar en Heroku pues no sabemos la IP.

# MongoDB y Express
En las aplicaciones Express podremos trabajar con SQL Server, MySQL, MongoDB, etc sólo será cuestión de cargar el driver de Node.js apropiado.



## Driver Oficial MongoDB

Es posible conectarnos y realizar queries utilizando la API standard de MondoDB (`npm install mongodb` y luego `const MongoClient=require('mongodb').MongoClient) ` pero trabaja muy a bajo nivel por lo que por motivos de simplicidad es conveniente utilizar **mongoose** que está construído sobre `MongoClient`.





## Mongoose

Mongoose es una **ODM Library** (object document mapping) y eso significa que toma la API standard de MongoDB y proporciona un modo mucho más simple de comunicarnos con la base de datos. Esto lo logra permitiéndonos crear **modelos de datos** con los cuales accedemos a métodos para crear, listar, actualizar y eliminar documentos. Nos permitirá tener esquemas y de esta manera validar los datos con los que crearemos los documentos.

```bash
 npm install mongoose
```

## Conexión a Mongoose
Como queremos que la conexión con la base de datos sea única (y no una para cada API que tengamos en la carpeta `routes`) lo realizamos en `index.js`

```js
const mongoose = require('mongoose');

const connectionString = 'mongodb+srv...';

mongoose.connect(connectionString, { 
	useNewUrlParser: true, 
	useUnifiedTopology: true,
    useFindAndModify: false,
    useCreateIndex: true
})
	.then(result => console.log('connected to db'))
	.catch(err => console.error(err));
```

> Si ponemos simplemente `mongoose.connect(connectionString)` veríamos que nos aparecen varios *Deprecation Warnings* y nos sugerirá agregar el objeto de opciones que hemos puesto como segundo parámetro.

> El uso del **paquete debug** nos posibilitaría filtrar los mensajes relacionados con la base datos para aquellos momentos en que no queremos mostrarlos.
>
> `useFindAndModify: false` lo usamos para no obtener un warning al usar `findByIdAndRemove()` y `findByIdAndUpdate()`, `findOneAndRemove()`, `findOneAndUpdate()`.
>
> `useCreateIndex: true` nos permite crear índices y de esta manera recuperar los datos de manera más rápida.



Sino quisiéramos que el server esté escuchando requests hasta tanto haber efectuado la conexión a la base de datos, podemos hacer recién el `app.listen('3000')` dentro del then method de `mongoose.connect()` que se ejecuta cuando la conexión se efectuó.

```js
mongoose.connect(dbURI, { useNewUrlParser: true, useUnifiedTopology: true })
	.then(result => app.listen('3000'))
	.catch(err => console.log(err));
```
### Archivo aparte

Hasta ahora el código lo hemos puesto en `index.js`, ahora en cambio queremos crear un archivo `/config/connectMongoDB.js` y allí pegar todas las líneas que vimos anteriormente para efectuar la conexión. No debemos exportar ninguna función y simplemente colocando en `index.js` `require('./config/connectMongoDB')` ejecutará todo el código de dicho archivo por lo que realizará la conexión.

Otra opción es crear en `connectMongoDB.js` un método llamado `connectMongoDB()` y luego exportar dicho método  `module.exports = connectMongoDB;` Finalmente en `index.js` primero haremos el require `const connectMongoDB = require('./config/dbMongo');` y luego ejecutamos esta función `connectMongoDB();`



## Connection Strings

1) Si trabajamos con MongoDB Atlas el connection string será de la forma:
```js
 const dbURI = 'mongodb+srv://**<username>**:**<password>**@cluster0-niujt.mongodb.net/**<dbname>**?retryWrites=true&w=majority';

```
> En Node.js Auth en el curso de the net ninja elimina la parte de `?retryWrites=true&w=majority`

2) Si trabajamos con MongoDB instalado en la PC el connection string será del tipo `mongodb://localhost/playground` siendo playground el nombre de la base de datos. 

3) Cuando hagamos el deploy de la aplicación tendremos un connection string distinto. En este caso debemos trabajar con archivos de configuración como por ejemplo con el **paquete config**.

En caso de que la base de datos no esté creada, se creará cuando queramos escribir algo en ella.



> Para almacenar el URI podemos crear un archivo `.env.local` en el cual ponemos:
> `MONGO_URI=mongodb+srv://**<username>**:**<password>**@cluster0-niujt.mongodb.net/**<dbname>**?retryWrites=true&w=majority`
### Uso de Variables de Entorno

En lugar de colocar el `connectionString` hardcodeado podemos ponerlo como una **variable de entorno**

Para ello lo primero que tenemos que hacer es crear un archivo `.env`

```
MONGO_DB_URI=mongodb+srv://**<username>**:**<password>**@cluster0-niujt.mongodb.net/**<dbname>**?retryWrites=true&w=majority+
```

La idea luego sería colocar: `const connectionString = process.env.MONGO_DB_URI` pero para que el valor de dicha variable esté disponible tenemos que hacer dos cosas:

* Instalamos `dotenv`

```
npm install dotenv
```

* Luego en `index.js` 

```
require('dotenv').config()
```



Esta técnica nos permitirá luego usar distintas bases de datos según el entorno en el cual nos encontremos trabajando.

```js
const { NODE_ENV,MONGO_DB_URI,MONGO_DB_URI_TEST} = process.env;
const connectionString = NODE_ENV === 'test'
? MONGO_DB_URI_TEST
: MONGO_DB_URI;
```

Debemos agregar el `.env` a `.gitignore` ya que como tiene datos sensibles no queremos que forme parte del sistema de control de versiones.



## Schemas & Models
Con MongoDB no estamos obligados a seguir un esquema de los documentos que almacenamos en una colección. Esto significa que por ejemplo en una colección `students` podremos crear un documento con la propiedad `name` y otra `studentName` y ambas serán aceptadas. Incluso un documento podría tener más propiedades que otro o almacenar datos de distinto tipo. 

Debemos asegurarnos evitar crear documentos con esquemas distintos, aunque esta flexibilidad resultó muy llamativa a muchas startups ya que podían tener distintos esquemas que en definitiva son distintas versiones de los datos almacenados.

Los **schemas** definen la estructura de los datos que conforman el documento, es decir sus propiedades y el tipo de datos de ellas.

Antes de meternos en la sintaxis sabemos que nuestro Schema queremos que tenga las siguientes propieades y el siguiente tipo:

* `title(string), required`
* `snippet(string), required`
* `body (string), required`
> Tener en cuenta que _id es propiedad autogenerada

Luego creamos un **modelo** basado en ese **esquema** (decimos que el **model** envuelve al **schema**) con el cual podremos comunicarnos con una colección en particular, podrá contar con **métodos estáticos** (por ejemplo siendo Blog el modelo `Blog.find()`) y **métodos de instancias** (`const note = new Note({...}) y luego note.save())`  con los cuales podremos leer, modificar, actualizar o eliminar datos de esa colección.

A la hora de trabajar con una colección en MongoDB debemos crear el Schema y el Model para dicha colección. Suponiendo que tenemos una colección `blogs` creamos una carpeta `models` y en ella una archivo `blog.js`.

En primer lugar debemos crear una instancia del `Schema` object y al constructor le pasamos como primer argumento un objeto con las características  de la estructura del documento y como segundo argumento para tener un registro automático de cuando se crea o actualiza un documento mediante un timestamp le pasamos algo similar a un options object en el cual ponemos `{ timestamp: true}` sin embargo debemos tener presente que el objeto `_id` también almacena información del momento en que dicho documento fue creado.

A continuación **crearemos un modelo basado en este schema**, típicamente se utiliza una letra mayúscula. `const Note= mongoose.model('Note', noteSchema);` notar que `Note` está en singular y luego será pluralizado. 

Conceptualmente debemos tener presente que **los esquemas son a nivel aplicación** (a diferencia de SQL donde son a nivel DB). Esto significa que con Compass o Robo 3T podríamos crear un documento que no cumpla con ese contrato. Si tenemos dos backends que ingresan datos debemos asegurarnos que utilicen el mismo modelo.



Por el momento vamos a crear/leer documentos en este archivo que llamaremos `mongo.js` 

```js
const mongoose = require('mongoose');

const {Schema, model} = mongoose;
const noteSchema = new Schema({
	content: String,
    date: Date,
    important: Boolean
});
 
const Note = model('Note', noteSchema);
```


Para crear un nuevo blog podríamos agregar abajo:

```js
const note = new Note({
	title: 'Aprendiendo Node.js',
	snippet: 'En este blog explicaremos todo sobre Node.js',
	body: 'Capítulo 1: Node.js es un entorno de ejecución de JavaScript...'
});
```

A continuación para guardarlo en la base de datos utilizamos el método `save()` que nos retorna una promesa:

```js
note.save()
 .then(result => {
    console.log(result)
	mongoose.connection.close();	 
 })
 .catch(error => console.error(error))
```

> `result` nos devuelve el objeto que ha sido guardado a la base datos.

Es una buena práctica cerrar la conexión luego de utilizarla y lo hacemos con `mongoose.connection.close();` En producción esto puede salvarnos de fallos por límite de memoria o límite de conexiones abiertas.

El código anterior lo podríamos ejecutar con `node mongo` y nos mostraría en pantalla el elemento agregado. Debemos tener presente que no lo ejecutamos con `nodemon index.js` sino con `node index.js` dado que sino cada vez que hagamos un cambio agregaría el elemento a la base de datos.

Cuando trabajemos en un proyecto real le agregaremos `module.exports = Note` y lo pondremos en una carpeta `models` y por último lo importaremos en `index.js`.  Si también tuvieramos allí la función encargada de validar los datos de entrada mediante joi  en vez de `module.exports = Note;` tendríamos por ejemplo `module.exports.Note = Note` y `module.exports.validate = validateNote` y luego en `index.js` pondríamos `const {Note, validate} = require('./models/note');`

Si queremos verificar que lo anterior funcionó podríamos comentar la parte de `blog.save()`  y agregar `blog.find()` que devuelve todos los elementos también en una promesa.

```js
note.find({})
 .then(result => {
    console.log(result)
	mongoose.connection.close();	 
 })
 .catch(error => console.error(error))
```



Ahora veríamos que además de las propiedades que pusimos manualmente tenemos `_id` y `__v` que son generadas automáticamente. Suponiendo que queremos tener `id` en lugar de `_id` y que `__v` directamente no la queremos podemos personalizarlo.

Cuando ejecutamos `res.json(result)` estamos llamando a `toJSON()` entonces podemos modificar este método para que realice también esta transformación:

```js
const mongoose = require('mongoose');

const {Schema, model} = mongoose;
const noteSchema = new Schema({
	content: String,
    date: Date,
    important: Boolean
});

noteSchema.set('toJSON', {
    transform: (document, returnedObject) =>{
        returnedObject.id = returnedObject._id;
        delete returnedObject._id;
        delete returnedObject.__v;
    }
})

const Note = model('Note', noteSchema);
```

 Entonces ahora cada vez que llamemos a `req.json()` veremos la propiedad `id`.



A continuación mostramos otro ejemplo de modelo con algunas validaciones y el agregado de timestamp de creación.

```js
const mongoose = require('mongoose');

const {Schema, model} = mongoose;
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
 
const Blog = model('Blog', blogSchema);

module.exports = Blog
```



## CRUD sin API

### Crear Documentos

En el siguiente ejemplo mostramos cómo crear un nuevo documento sin hacerlo desde el route handler y utilizando **async/await**. Sin embargo, en este caso no representa ninguna mejora sustancial. Incluso frente al uso de promesas, puede que el código se vuelva menos legible y también el incorporar el bloque `catch` nos genera dudas de si debemos incluir en él solo la parte de `save()` o el resto también. Por último decimos que cambia el sentido de la función que retornará una promesa.
```js
async  function createCourse() {
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

### Obtener Documentos
De ahora en más por simplicidad se omitirá el bloque `try/catch` por lo que sólo asumimos el escenario en que las promesas se cumplen (fulfilled) , si son rechazadas (rejected) no tenemos código para manejar la situación.

```js
async  function  getCourses() {
	const  courses  =  await  Course
		.find({ author: 'Mosh', isPublished: false })
	console.log(courses);
}
```

Estamos haciendo `console.log(courses)` dentro de `getCourses()` idealmente deberíamos tener otra función encargada de mostrar los cursos ya que el propósito de `getCourses()` debería ser sólo obtenerlos.

### Filtrar Documentos
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

### Actualizar documentos
Existen dos modos de actualizar documentos, uno de ellos es **Query First** en el utilizaremos el método estático `findById()`, luego modificaremos la propiedad y por último emplearemos el método de instancia `save()`. La otra metodología se conoce como **Update First** y consiste en actualizar directamente utilizando el método `updateOne()` [^1] y  opcionalmente obtener el documento actualizado con `findByIdAndUpdate()`.

[^1]: Mosh utiliza el método `update()` pero actualmente obtenemos *DeprecationWarning*.

#### Query First
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


#### Update first
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

### Eliminar documentos
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

## CRUD en API

Las mismas operaciones que hemos hecho hasta ahora podemos agregarlas en `index.js` dentro de algunas rutas de prueba:


```js
const  Note  =  require('./models/note');

app.get('/api/notes', (req, res) => {
	Note.find()
	.then(note => {
		res.json(note);
	})
	.catch(err => {
		console.log(err);
	});
});

app.get('/api/notes/:id', (req, res) => {
    const {id} = req.params;
	Note.findById(id)
	.then(note => {
        if(note){
        	res.json(note);    
        }
        else{
            res.status(404).end();
        }
		
	})
	.catch(err => {
		console.log(err);
        res.status(400).end()
	});
});

app.post('/api/notes', (req, res) => {
    const {note} = req.body;
    
	const newNote = new Note({
		content: note.content,
		date: new Date(),
		important: note.important || false
	})
	newNote.save()
	.then(result => {
		res.json(result);
	})
	.catch(err => {
		console.log(err);
	});
});

app.get('/api/notes/:id', (req, res) => {
    const {id} = req.params;
	Note.findById(id)
	.then(note => {
        if(note){
        	res.json(note);    
        }
        else{
            res.status(404).end();
        }
		
	})
	.catch(err => {
		console.log(err);
        res.status(400).end()
	});
});

app.delete('/api/notes/:id', (req, res) => {
    const {id} = req.params;
	Note.findByIdAndDelete(id)
	.then(note => {
        if(note){
        	res.status(204).end()
        }
        else{
            res.status(404).end();
        }
		
	})
	.catch(err => {
		console.log(err);
        res.status(400).end()
	});
});

app.put('/api/notes/:id', (req, res) => {
    const {id} = req.params;
    const {content, important} = req.body;
    const newNoteInfo = {
        content,
        important
    }
	Note.findByIdAndUpdate(id,newNoteInfo)
	.then(oldNote => {
        if(oldNote){
        	res.json(oldNote)
        }
        else{
            res.status(404).end();
        }
		
	})
	.catch(err => {
		console.log(err);
        res.status(400).end()
	});
});
```

En la ruta `'/api/notes/:id'` buscamos diferenciar entre un recurso no encontrado (devolvemos un 404) y el caso en que el id que recibimos como parámetro no sea correcto (devolvemos un 400 dentro del catch). Sin embargo, dentro del catch nos conviene colocar `next()` y luego crear un middleware para el manejo de errores como veremos mas adelante.

Si bien en este caso trabajamos con promesas, podríamos haber hecho lo mismo utilizando **async/await**, notar tres cosas: que el callback pasa a ser `async (req,res)`, el uso de `await` y el agregado del bloque `try/catch`.

En las rutas donde recibimos el id como parámetro `/api/notes/:id` y luego lo utilizamos para `findById()` debemos tener presente que `_id` es un `ObjectId` y no un `string` en **MongoDB**. Cuando lo usamos en **mongoose** realiza la conversión automáticamente por lo que podemos trabajar directamente con el string. 

El método `findByIdAndUpdate` retorna el elemento viejo a menos que le pasemos como tercer parámetro `{new: true}`

```js
Note.findByIdAndUpdate(id,newNoteInfo,{new:true})
```



Si queremos optimizar esta porción de código

```
if(note){
	res.json(note);    
}
else{
	res.status(404).end();
}
```



Podemos poner

```
if(note) return res.json(note);
res.status(404).end
```

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

### Ejemplos
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


## Paginación
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

En la terminal navegamos hasta la carpeta en la cual tenemos el archivo que deseamos importar y luego ejecutamos:

```bash
mongoimport --db mongo-exercises --collection courses 
--file exercise-data.json --jsonArray
```

Primero debemos poner `mongosimport` luego con `--db mongo-example` especificamos la base datos en la cual queremos importar los datos, con `--collection courses` el nombre de la colección luego con `--file exercise-data.json` le pasamos el nombre del archivo. Por último ponemos el flag `--jsonArray` ya que los datos están en ese formato.



## Importar JSON en Compass

Si queremos importar un JSON utilizando Compass es mucho más simple, debemos entrar en la colección donde queremos agregar esos documentos y luego hacer click en la parte superior en el menú Collection y luego Import Data.




## Queries con RegEx 
Si queremos buscar todos los cursos que tengan como autor la palabra Mosh en mayúscula o minúscula y en cualquier posición:
```js
const  courses  =  await  Course
	.find({ author: /.*Mosh.*/i})
```


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

### Validaciones de Mongoose

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
Cuando en vez de poner `name: String` hacemos `name: { type: String, required: true}` estamos haciendo uso del 

**SchemaType object**.
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

### Validación Personalizada
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

### Validación Unique

La librería **mongoose-unique-validator** nos permite validar que un campo del schema sea unique.

**No confundir con la opción `unique` de Mongoose que como indica en su [documentación](https://mongoosejs.com/docs/validation.html) no es para validación**

```
const { Schema, model } = require('mongoose')
const uniqueValidator = require('mongoose-unique-validator')

const userSchema = new Schema({
  username: {
    type: String,
    unique: true
  },
  name: String,
})

userSchema.plugin(uniqueValidator)

const User = model('User', userSchema)

module.exports = User
```

###  Validaciones Asincrónicas

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

### Validaciones con joi
Es conveniente utilizar el paquete **joi** (para chequear que los datos que nos pasa el cliente son válidos) de manera complementaria con las validaciones de **mongoose** (para volver a chequear antes de guardar por si hubo un error en la manipulación que hicimos de los datos).

Ambas validaciones no necesariamente serán iguales por ejemplo si tenemos una coleccción `movies` que en su interior embebe datos de autores, para **joi** validaremos `genreId` que es lo que nos pasa el cliente mientras que para **mongoose** validaremos un objeto `genre`.

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

### Errores de Validación
 Como ya dijimos cuando utilizamos el método `save()` o `validate()` y se genera un error de validación, ejecutamos el código del bloque catch. En `ex.errors` tenemos una *propiedad* para cada *propiedad inválida* del objeto. Por ejemplo si tenemos un error en la propiedad *category* y *tags*, tendremos información sobre los errores acontecidos en `ex.errors.category` y `ex.errors.tags`. Por lo que podremos recorrer el objeto `ex.errors` utilizando **for-in** con el propósito de obtener más detalles sobre los errores de validación.

```js
catch (ex) {
	for (field in ex.errors) {
		console.log(ex.errors[field].message);
	}
}
```


## Renderizar Documentos en Vistas

Cuando ingresemos a la home `/` seremos redireccionados a `blogs` que es una nueva ruta que creamos para mostrar todos los blogs ordenados por fecha de creación (del más nuevo al más viejo) `Blog.find().sort({ createdAt: -1 })` notar que cuando renderizamos como le pasamos `res.render('index', { blogs: result, title: 'All blogs' });`la propiedad `blogs` que es el mismo nombre que tenía cuando lo renderizábamos a través de la ruta `/` por lo que no tendremos que cambiar nada en `index.ejs`

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
