# Node.js Authentication con JWT
> Basado en videos  [The Net Ninja - Node Auth Tutorial (JWT)](https://www.youtube.com/watch?v=SnoAwLP1a-0) | [Repositorio](https://github.com/iamshaunjp/node-express-jwt-auth)

En este proyecto realizaremos un sistema de autenticación usando Node.js, Express, MongoDB y JWT. La aplicación tendrá como alcance la posibilidad de realizar *Sign Up*, *Login*, *Logout* y proteger determinadas rutas ante el acceso sin estar autenticados.
JWT significa JSON Web Tokens y es una de las formas de implementar autenticación otra forma sería usar *sessions*.

* Utilizaremos la implementación MVC (model view controller) explicada en *"Express Router & MVC"*

* Usaremos los siguientes paquetes: `npm i express ejs mongoose`

* Trabajemos con las siguientes rutas:

|      |             |
| :--: | :---------: |
| GET  | **/signup** |
| GET  | **/login**  |
| POST | **/signup** |
| POST | **/login**  |
| GET  | **/logout** |

## `app.js`
```js
const express = require('express');
const mongoose = require('mongoose');
const authRoutes = require('./routes/authRoutes');

const app = express();

// middleware
app.use(express.static('public'));

// view engine
app.set('view engine', 'ejs');

// database connection
const dbURI = 'mongodb+srv://shaun:test1234@cluster0.del96.mongodb.net/node-auth';
mongoose.connect(dbURI, { useNewUrlParser: true, useUnifiedTopology: true, useCreateIndex:true })
  .then((result) => app.listen(3000))
  .catch((err) => console.log(err));

// routes
app.get('/', (req, res) => res.render('home'));
app.get('/smoothies', (req, res) => res.render('smoothies'));
app.use(authRoutes);
```

## `/routes/authRoutes.js`
```js
const { Router } = require('express');
const authController = require('../controllers/authController');

const router = Router();
router.get('/signup', authController.signup_get);
router.post('/signup', authController.signup_post);
router.get('/login', authController.login_get);
router.post('/login', authController.login_post);
module.exports = router;
```

## `/controllers/authController.js`
Creamos un directorio `controllers` y en el un archivo `authController.js` donde colocaremos toda la lógica de las rutas.

```js
// controller actions
module.exports.signup_get = (req, res) => {
	res.render('signup');
}

module.exports.login_get = (req, res) => {
	res.render('login');
}

module.exports.signup_post = async (req, res) => {
	res.send('new signup');
}

module.exports.login_post = async (req, res) => {
	res.send('user login');
}
```

## `/views/partials/header.ejs`
```html
<html lang="en">
	<head>
		<meta charset="UTF-8">
		<meta name="viewport" content="width=device-width, initial-scale=1.0">
		<title>Document</title>
		<link rel="stylesheet" href="/styles.css">
	</head>
	<body>
		<nav>
			<h1><a href="/">Ninja Smoothies</a></h1>
		</nav>
```

## `/views/partials/footer.ejs`
```html
		<footer>Copyright 2020 Ninja Smoothies</footer>
	</body>
</html>
```

## `views/home.ejs`
```html
<%- include('partials/header'); -%>

<header>
  <div class="smoothie">
    <img src="/smoothie.png" alt="">
  </div>
  <div class="headings">
    <h2>Smoothie Recipes</h2>
    <h3>By Ninjas For Ninjas</h3>
    <a href="/smoothies" class="btn">View Recipes</a>
  </div>
</header>


<%- include('partials/footer'); -%>
```

## `views/login.ejs`
```html
<%- include('partials/header'); -%>
<h1>Login</h1>

<%- include('partials/footer'); -%>
```
**
## `/views/signup.ejs`
```html
<%- include('partials/header'); -%>

<h1>Sign up</h1>

<%- include('partials/footer'); -%>
```

## `/views/smoothies.ejs`
```html
<%- include('partials/header'); -%>

<ul class="recipes">
  <li class="recipe">
    <img src="/smoothie.png" alt="smoothie recipe icon">
    <h4>Banana Boost</h4>
    <p>Banana, Vanilla ice cream, Milk</p>
  </li>
  <li class="recipe">
    <img src="/smoothie.png" alt="smoothie recipe icon">
    <h4>Tropical Twist</h4>
    <p>Peach, Pinapple, Apple juice</p>
  </li>
  <li class="recipe">
    <img src="/smoothie.png" alt="smoothie recipe icon">
    <h4>Protein Packer</h4>
    <p>Oats, Peanut butter, Milk, Banana, Blueberries</p>
  </li>
  <li class="recipe">
    <img src="/smoothie.png" alt="smoothie recipe icon">
    <h4>Banana Boost</h4>
    <p>Banana, Vanilla ice cream, Milk</p>
  </li>
  <li class="recipe">
    <img src="/smoothie.png" alt="smoothie recipe icon">
    <h4>Tropical Twist</h4>
    <p>Peach, Pinapple, Apple juice</p>
  </li>
  <li class="recipe">
    <img src="/smoothie.png" alt="smoothie recipe icon">
    <h4>Protein Packer</h4>
    <p>Oats, Peanut butter, Milk, Banana, Blueberries</p>
  </li>
</ul>

<%- include('partials/footer'); -%>
```