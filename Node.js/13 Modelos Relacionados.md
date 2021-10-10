# Modelos Relacionados
Hasta el momento trabajamos con *single self contained documents* pero en la vida real trabajaremos con documentos que están relacionados entre sí como por ejemplo autores y cursos (cada curso tiene un autor y un autor puede tener más de un curso). En el manejo de modelos relacionales podemos adoptar distintos caminos:



## `.populate()`

Cuando tenemos relaciones entre dos colecciones es posible traer datos de una colección a la otra utilizando `populate()`.

Suponemos que tenemos una colección usuarios y una colección notas: 

* En `users` tenemos un campo `notes` que es una array con los ids de las notas de ese usuario.
* En `notes` tenemos un campo `user` con el id del autor de dicha nota.



> Debemos tener presente que `populate()` no es una transacción atómica, por lo que la información obtenida puede no estar actualizada.
>
> La opción `ref` en el Schema será la encargada de decirle a Mongoose qué modelo debe usar con el `populate()`.

### Guardado de ids

A la hora de crear una nota debemos recibir en `req.body` el id del autor. Luego buscamos ese usuario en la colección `Users`, extraemos su objectId (tenemos el id como string) y lo usamos para guardar la nueva nota. Por último le concatenamos un nuevo elemento al array `notes` con el id de la nota guardada.

```js
app.post('/api/notes', userExtractor, async (request, response, next) => {
  const {
    content,
    important = false,
    userId
  } = request.body;

  const user = await User.findById(userId);

  if (!content) {
    return response.status(400).json({
      error: 'required "content" field is missing'
    })
  }

  const newNote = new Note({
    content,
    date: new Date(),
    important,
    user: user._id
  })

  try {
    // En Note guardamos la nota junto con el id del usuario en "user"
    const savedNote = await newNote.save()

    //En User agregamos un nuevo id al array "notes"
    user.notes = user.notes.concat(savedNote._id)
    await user.save()

    response.json(savedNote)
  } catch (error) {
    next(error)
  }
})
```

Cuando queramos obtener todos los usuarios o notas, demandará un nuevo request para obtener los datos de ambas colecciones. Conceptualmente buscamos una herramienta similar al JOIN de SQL con el cual obtenemos todo en una petición, el método de mongoose `populate()` se encarga de hacer esto basado en las referencias establecidas en el Schema y en los ids especificados en los documentos.



### Populate de usuarios

Queremos obtener todas las notas con el agregado de los datos de `user` recordando que además del id en el Schema teníamos: 

```js
user: {
    type: Schema.Types.ObjectId,
    ref: 'User'
 }
```

Por eso en el controlador hacemos lo siguiente:

```js
app.get('/api/notes', async (request, response) => {
  const notes = await Note.find({}).populate('user', {
    username: 1,
    name: 1
  })
  response.json(notes)
})
```

> Notar que sólo estamos interesados en `username` y `name`.



#### Populate de Notas

Considerando que en el Schema de usuarios tenemos la siguiente referencia:

```js
notes: [{
    type: Schema.Types.ObjectId,
    ref: 'Note'
  }]
```

 Luego podremos obtener todas las notas de este modo:

```js
usersRouter.get('/', async (request, response) => {
  const users = await User.find({}).populate('notes', {
    content: 1,
    date: 1
  })
  response.json(users)
})
```



## Normalización

Esta metodología se caracteriza por el uso de referencias.
```js
let author = {
	name: 'Mosh'
}

let course = {
	author: 'id'
}
```
A diferencia de lo que ocurre en bases de datos relacionales donde esta garantiza la integridad de los datos, en bases de datos NoSQL como las colecciones no tienen relación esto no es así y podría por ejemplo colocar un id inválido.
Un curso también podría tener múltiples autores en cuyo caso tendríamos:
```js
let course = {
	authors: [
		'id1',
		'id2',
		'id3'
	]
}
```

## Desnormalización
Esta metodología se caracteriza por el uso de documentos embebidos
```js
let course = {
	author: {
		name: 'Mosh'
	}
}
```

## Normalización vs Desnormalización
Cada uno de estos enfoque tiene sus ventajas y desventajas. Debemos pensar que existe una relación de compromiso entre *query performance* y *data consistency*.
En la normalización si decido cambiar el nombre de un autor sólo tengo que hacerlo en un lugar y todos los cursos verán el cambio reflejado. Esto es un punto importante en cuanto a la **consistencia de datos**. Sin embargo cada vez que quiera buscar un curso deberé hacer un query extra para obtener el autor.
Con la desnormalización obtengo los datos del autor con un único query, es decir que este método se destaca en términos de **performance** pero en caso de que haya cambios en el futuro tendré que cambiar en muchos lugares y en caso de que la actualización no se complete correctamente podrá haber inconsistencia de datos.
La decisión deberá ser tomada en término de las características de las queries que debamos ejecutar.

## Enfoque híbrido
En este enfoque  no embebemos la totalidad de las propiedades sino sólo aquellas que nos interesan, para las restantes contamos con la referencia.
```js
let author = {
	name: 'Mosh'
	//otras 50 propiedades
}
let course = {
	author = {
		_id: 'ref',
		name: 'Mosh'
	}
}
```
En esta aproximación logramos performance ya que con un único query podemos leer tanto datos del curso como el autor.

La aproximación híbrida nos permite obtener un snapshot de los datos en un punto determinado.
Se usa en aplicaciones de ecommerce donde tenemos colecciones de productos, órdenes, etc. En cada orden queremos tener una *snapshot* del producto donde veamos el precio del producto en un momento específico o bien en aquellos casos que sabemos que la propiedad embebida no va a cambiar (si tengo una colección movies y dentro de ella embebo *genre* sé que el nombre del género difícilmente cambie)

## Referenciando Documentos 
A continuación mostraremos un ejemplo de cómo referenciar un documento en otro documento.
Contamos con dos modelos: Author y Course. 

```js
const mongoose = require('mongoose');

const Author = mongoose.model(
	'Author',
	new mongoose.Schema({
		name: String,
		bio: String,
		website: String,
	})
);
const Course = mongoose.model(
	'Course',
	new mongoose.Schema({
		name: String,
		author: {
			type: mongoose.Schema.Types.ObjectId,
			ref: 'Author', //target collection
		},
	})
);

mongoose
	.connect('mongodb://localhost/libreria', { useNewUrlParser: true, useUnifiedTopology: true })
	.then((result) => console.log('connected to db'))
	.catch((err) => console.error(err));

async function createAuthor(name, bio, website) {
	const author = new Author({
		name,
		bio,
		website,
	});
	const result = await author.save();
	console.log(result);
}

async function createCourse(name, author) {
	const course = new Course({
		name,
		author,
	});

	const result = await course.save();
	console.log(result);
}

async function listCourses() {
	const courses = await Course.find().select('name author');
	console.log(courses);
}

//createAuthor('Shaun', 'the net ninja', 'http://youtube.com');
createCourse('Node.js Crash Couse', '5f0dcfff5fdcb042bc83b2e1');
//listCourses();


```
A la hora de crear un nuevo curso veremos lo siguiente a la salida:
```bash
{
  _id: 5f0e7c478c928d55bc58cdd9,
  name: 'Node.js Crash Couse',
  author: 5f0dcfff5fdcb042bc83b2e1,
  __v: 0
}
```

A pesar de que indicamos que la propiedad author referencia a la colección Author con `ref: 'Author'` eso no asegura que el id sea válido, pero como veremos más adelante nos permite saber donde debe hacer el query. 

Al guardar un objeto en la base de datos sólo las propiedades definidas en el modelo serán persistentes.

Utilizamos `find().select('name author');` eligiendo todos los datos dado que lo que no queremos es la propiedad `__v: 0`

## Population:
Cuando llamamos a la función `listCourses()` obtenemos: 
```json
[
  {
    _id: 5f0e7c478c928d55bc58cdd9,
    name: 'Node.js Crash Couse',
    author: 5f0dcfff5fdcb042bc83b2e1
  }
]
```
Utilizando el método `populate('author')` podemos obtener el autor al que hace referencia ese id. Population es el proceso de remplazar automáticamente las rutas especificadas en el documento por documentos de otra colección. Esto es posible dado que cuando definimos el modelo Course especificamos la para author la referencia a la colección Author.

```js
async function listCourses() {
	const courses = await Course.find()
		.populate('author')
		.select('name author');
	console.log(courses);
}
```

Por lo que ahora obtenemos:
```json
[
  {
    _id: 5f0e7c478c928d55bc58cdd9,
    name: 'Node.js Crash Couse',
    author: {
      _id: 5f0dcfff5fdcb042bc83b2e1,
      name: 'Shaun',
      bio: 'the net ninja',
      website: 'http://youtube.com',
      __v: 0
    }
  }
]
```

Al llamar el método `populate()` podremos pasarle un segundo argumento con las propiedades que queremos incluir o excluir: `.populate('author', 'name')` por lo que veremos:
```json
[
	{
		_id: 5f0e7c478c928d55bc58cdd9,
		name: 'Node.js Crash Couse',
		author: { _id: 5f0dcfff5fdcb042bc83b2e1, name: 'Shaun' }
	}
]
```

Si queremos excluir la propiedad `_id` `.populate('author', 'name -_id')`
```json
[                                  
	{                                
		_id: 5f0e7c478c928d55bc58cdd9, 
		name: 'Node.js Crash Couse',   
		author: { name: 'Shaun' }      
	}                             
]
```

Es posible hacer populate de múltiples propiedades, supongamos que cada curso tiene una propiedad categoría que referencia a un documento categoría.

```js
async function listCourses() {
	const courses = await Course.find()
		.populate('author','name -_id')
		.populate('category','name')
		.select('name author');
	console.log(courses);
}
```

En MongoDB sin embargo no tenemos relaciones que garanticen la integridad de datos. Supongamos que en Compass en la colección de cursos modificamos el ObjectId de la propiedad author de modo de que este sea inválido, al ejecutar luego la función `listCourses()` al hacer el populate **obtendremos null**. 

## Embedding Documents:

Cuando referenciamos documentos a la hora de definir la propiedad `author` en `courseSchema` hicimos lo siguiente: 
```js
author: {
	type: mongoose.Schema.Types.ObjectId,
	ref: 'Author',
},
```
En cambio ahora como queremos embeber el documento directamente pondremos el nombre del Schema:
```js
author: authorSchema
```

```js
const authorSchema = new mongoose.Schema({
	name: String,
	bio: String,
	website: String,
});

const Author = mongoose.model(
	'Author',
	authorSchema
);

//Creamos el schema y el model en una misma línea
//No hago esto mismo para el authorSchema ya que lo necesitaré separado
const Course = mongoose.model(
	'Course',
	new mongoose.Schema({
		name: String,
		author: authorSchema,
	})
);
```
Luego para crear un nuevo curso `createCourse(name, author)` permanece igual pero cambia el modo en el que lo llamamos, en vez de hacerlo con `createCourse('Node Couse', '5f0dcfff5fdcb042bc83b2e1');` lo hacemos con `createCourse('Node Course, new Author({name:'Shaun',bio:'the net ninja',web:'youtube.com'}));'`

### Validación sub documentos 
Podemos pensar en los documentos embebidos como sub-documentos por lo que podremos hacer con ellos la mayoría de las cosas que hacemos con los documentos, por ejemplo validación. Entendiendo como tal por ejemplo establecer dentro del Schema de cursos que la propiedad author (el sub-documento en sí) sea required. No confundir con la validación propia de `authorSchema`)

```js
const Course = mongoose.model(
	'Course',
	new mongoose.Schema({
		name: String,
		author: {
			type: authorSchema,
			required: true,
		},
	})
);
```

### Actualización sub documentos
Sin embargo estos documentos sólo pueden ser guardados en el contexto de su padre. Supongamos que queremos actualizar datos del autor de un curso para ello creamos la función `updateAuthor(courseId)`

```js
async function updateAuthor(courseId) {
	const course = await Course.findById(courseId);
	course.author.name = 'Juan';
	const result = await course.save();
	console.log(result);
}
```

También es posible actualizar un sub-documento de manera directa:

> Mosh utiliza el método `update()` pero actualmente obtenemos *DeprecationWarning*.

```js
async function updateAuthor(courseId) {
	const result = await Course.updateOne(
		{
			_id: courseId,
		},
		{
			$set: {
				'author.name': 'John Smith',
			},
		}
	);
}
```
Para acceder a las propiedades anidadas usamos *dot notation*.

Para eliminar un sub-documento usamos el operador **unset**:

Si quisiéramos eliminar sólo la propiedad `name` del documento author podemos hacerlo con `'author.name' : ''`, mientras que en este caso eliminamos el documento completo poniendo `'author' : ''`

```js
async function deleteAuthor(courseId) {
	const result = await Course.updateOne(
		{
			_id: courseId,
		},
		{
			$unset: {
				author: '',
			},
		}
	);
}
```

### Array de sub-documentos:
Si quisiéramos que cada curso tenga más de un autor, deberíamos almacenar un array de autores. Para ello en lugar de tener `author: authorSchema` ahora ponemos `authors: [authorSchema]`
```js
const Course = mongoose.model(
	'Course',
	new mongoose.Schema({
		name: String,
		authors: [authorSchema],
	})
);
```

Luego al invocar a `createCourse(name,authors)` en vez de pasarle un autor le pasamos un array:

```js
createCourse('JavaScript', [
	new Author({ name: 'Shaun' }),
	new Author({ name: 'Mosh' }),
]);
```

También es posible agregar luego un sub-documento del array:

```js
async function addAuthor(courseId, author) {
	const course = await Course.findById(courseId);

	course.authors.push(author);

	course.save();
}

addAuthor(
	'5f0fd6aacdf08f3f40993038', 
	new Author({ name: 'JuanOcho' })
);
```

De la misma manera para eliminar un sub-documento del array:

```js
async function removeAuthor(courseId, authorId) {
	const course = await Course.findById(courseId);

	const author = course.authors.id(authorId);
	author.remove(); //?
	course.save();
}

removeAuthor('5f0fd6aacdf08f3f40993038', '5f0fdbb9109e2a0144c71cb5');
```
Los arrays de documentos cuentan con un método especial `.id()` de mongoose para buscar el _id deseado.
Notar que guardamos a `course` y no a `author`.

== Buscar alternativa a método remove() porque aparentemente está deprecated ==


