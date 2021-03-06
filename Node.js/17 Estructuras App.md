# Express Router & MVC
Si miramos el archivo `app.js` cuando desarrollamos varias rutas, veremos que se vuelve bastante largo, desordenado y por ende difícil de mantener. Con la ayuda de **Express Router** buscaremos dividir al código en distintos archivos.

## Express Router:
Nos permite manejar las rutas de manera más eficiente y ordenada. Lo que haremos será dividir nuestras rutas en pequeños grupos y colocar cada uno en un archivo. Con esto logramos modularidad y un código más fácil de mantener.

Lo primero que hacemos será crear una carpeta `routes`. 
Luego analizamos nuestras rutas y si hay varias que tienen una url similar por ejemplo como veremos a continuación las colocaremos en un archivo aparte. 

|        |                   |
| :----: | :---------------: |
|  GET   |    **/blogs**     |
|  POST  |    **/blogs**     |
|  GET   |  **/blogs/:id**   |
|  PUT   |  **/blogs/:id**   |
| DELETE |  **/blogs/:id**   |
|  GET   | **/blogs/create** |

> ==Importante:== Como tenemos las rutas /blogs/:id y /blogs/create es importante que /blogs/create esté ubicada arriba, sino 'create' será considerado un id.

Lo que hacemos será colocarlas en un archivo aparte llamado `blogs.js` dentro de la carpeta `routes` y colocamos en ese archivo todos los handlers.

> También será necesario utilizar en este nuevo archivo la referencia al modelo Blog en este caso.

Al estar en un archivo independiente los handlers no hacen más uso de `app` sino que deben hacer referencia a `router`.  Este objeto lo creamos con `const router = express.Router()` de manera similar a cuando creamos `const app = express(); ` En la parte inferior debemos exportar `router` para luego importarlo en `app.js`

En `blogs.js` tomando como ejemplo una única ruta:
```js
const express = require('express');
const router = express.Router(); 
//a la instancia router se la conoce como miniaplicación

router.get('/blogs', (req, res) => {
	Blog.find().sort({ createdAt: -1 })
	.then(result => {
		res.render('index', { blogs: result, title: 'All blogs' });
	})
	.catch(err => {
		console.log(err);
	});
});

module.exports = router;

```



Otra opción sería en lugar de poner:

```
const express = require('express');
const router = express.Router(); 
```

Poner directamente `const blogsRouter = require('express').Router()` usando `blogsRouter` en lugar de `router` ya que aunque no hace falta que sea el mismo nos da la pauta del nombre con el que lo importaremos en `app.js`

A continuación en `app.js` en primer lugar importamos el archivo `const blogRoutes = require('./routes/blogRoutes')` y luego donde teníamos definidos los handlers, ponemos `app.use(blogRoutes)` . 
También es posible establecer el scope o alcance de estas rutas a una url específica: `app.use('/blogs',blogRoutes)`. Una vez realizado este cambio también tendremos que modificar las rutas ya que donde antes teníamos `router.get('/blogs', (req,res))` tenemos que poner `router.get('/', (req,res))`


## MVC:
MVC **(Model, View, Controller)** es una metodología de estructuración del código y los archivos, con al finalidad de que sea modular, reutilizable y de fácil mantenimiento fundamentalmente en proyectos de un tamaño considerable.
Hasta el momento hemos trabajado con **model** (descripción de estructura de datos) y **view** (html templates). Los **controladores** son el eslabón en el medio entre modelos y vistas. Utilizan los modelos para obtener datos y luego pasan esos datos a las vistas. Hasta ahora hemos hecho esto en los route handlers pero la idea es separarlos en un archivo controlador  independiente. Luego en el archivo de rutas referenciaremos a ese controlador. Entonces proceso completo es el siguiente: el archivo de rutas, matchea los requests que recibe y se los pasa al controlador indicado, el cual se comunica con el modelo apropiado para pasar datos a la vista. La vista renderiza los datos en el template y luego es enviada al navegador. 

### Controllers:
Crearemos la carpeta `controllers` y en ella creamos el archivo `blogController.js`
Tomando como ejemplo dos rutas crearemos una función `blog_index` para obtener todos los blogs e inyectarlos en la vista `index.ejs`
```js
const Blog = require('../models/blog');

const blog_index = (req, res) => {
	Blog.find().sort({ createdAt: -1 })
	.then(result => {
		res.render('index', { blogs: result, title: 'All blogs' });
	})
	.catch(err => {
		console.log(err);
	});
}
const blog_details = (req, res) => {
	const id = req.params.id;
	Blog.findById(id)
	.then(result => {
		res.render('details', { blog: result, title: 'Blog Details' });
	})
	.catch(err => {
		console.log(err);
	});
}

module.exports = {
blog_index,
blog_details
}
```

Luego en `blogRoutes.js`

```js
const blogController = require('../controllers/blogController')

router.get('/', blogController.blog_index)
```
Es conveniente estructurar las vistas en carpetas, ya que normalmente tendremos varios tipos de recursos (blog, comentarios, usuarios, etc) así internamente también podremos darle los nombres  `index.ejs`, `details.ejs`, `create.ejs` 

```
+-- views
|   +-- blogs
|	   +-- create.ejs
|	   +-- details.ejs
|	   +-- index.ejs
```



## Estructura Route-Controller-Service

En cuanto a la estructura de las aplicaciones nos basamos en el [siguiente](https://sodocumentation.net/node-js/topic/10785/route-controller-service-structure-for-expressjs) modelo:

```bash
├───models
│   ├───user.model.js
├───routes
│   ├───user.route.js
├───services
│   ├───user.service.js
├───controllers
│   ├───user.controller.js
```



### user.model.js

```js
const mongoose = require('mongoose')

const UserSchema  = new mongoose.Schema({
    name: String
})

const User = mongoose.model('User', UserSchema)

module.exports = User;
```

### user.routes.js

```js
const express = require('express');
const router = express.Router();

var UserController = require('../controllers/user.controller')

router.get('/', UserController.getUsers)

module.exports = router;
```

### user.controllers.js

```js
const UserService = require('../services/user.service')    

exports.getUsers = async function (req, res, next) {
    // Validate request parameters, queries using express-validator
    
    var page = req.params.page ? req.params.page : 1;
    var limit = req.params.limit ? req.params.limit : 10;
    try {
        var users = await UserService.getUsers({}, page, limit)
        return res.status(200).json({ status: 200, data: users, message: "Succesfully Users Retrieved" });
    } catch (e) {
        return res.status(400).json({ status: 400, message: e.message });
    }
}
```

### user.services.js

```js
const User = require('../models/user.model')

exports.getUsers = async function (query, page, limit) {

    try {
        var users = await User.find(query)
        return users;
    } catch (e) {
        // Log Errors
        throw Error('Error while Paginating Users')
    }
}
```