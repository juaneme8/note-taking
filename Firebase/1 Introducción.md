# Introducción a Firebase 9
> Actualizado con Notas de Firebase 9 basado en la [playlist](https://www.youtube.com/watch?v=9zdvmgGsww0&list=PL4cUxeGkcC9jERUGvbudErNCeSZHWUVlb) de The Net Ninja.

Firebase se define como un *backend as a service* y nos permitirá realizar distintas tareas:  Authentication, Firestore Database, Realtime Database, File Storage, Hosting, Cloud Functions, etc y podremos utilizarlos directamente desde el frontend. Gracias a esto no tendremos que concentrarnos la lógica del lado del servidor y Firebase lo hará por nosotros. Podemos pensarlo como una alternativa a crear nuestra propia infraestructura backend por ejemplo con Node.js y MondoDB.

> Repositorio `md-firebase9`



## Diferencias con Firebase 8

Firebase 9 se caracteriza por tener una filosofía **modular** donde tendremos que importar las funciones que necesitamos de las bibliotecas. A diferencia de Firebase 8 que tiene una mirada orientada en objetos y llamamos a esas funciones utilizando objetos de firebase. Esto nos permitirá contar con *tree-shaking* que significa que el código no utilizado será eliminado del bundle (para esto será necesario contar con un module bundler como webpack o rollup). Como resultado tendremos aplicaciones código más optimizado, archivos de menor tamaño y aplicaciones más rápidas



## Configuración Webpack

Como dijimos anteriormente para contar con el tree-shaking y así eliminar el código no utilizado debemos usar un module bundler como webpack. Será el Webpack quién empaquetará las diferentes partes del código importado en un bundle javascript final.

Los pasos que veremos a continuación serán útiles para proyectos en Vanilla JavaScript mientras que caso de estar utilizando una herramienta CLI como create-react-app no tendremos que configurarlo manualmente ya que viene con un module bundler como webpack u otro listo para usar.

![diagrama webpack](https://blog.pleets.org/img/articles/webpack-process.png)



## Instalación Webpack

Tenemos que instalar webpack, para eso eso primero ejecutamso`npm init -y`de manera tal que tengamos un `package.json` y luego 

```
npm init webpack webpack-cli --save-dev
```



## Estructura Proyecto

* Creamos una carpeta `src` donde colocaremos todo el código javascript empezando por `index.js`

* Creamos una carpeta `dist` donde tendremos el código javascript del bundle final. En esta carpeta creamos un archivo `index.html` y en este archivo referenciaremos al bundle.
* Creamos un archivo `webpack.config.js` en el directorio raíz.

```js
const path = require('path')

module.exports = {
  mode: 'development',
  entry: './src/index.js',
  output: {
    path: path.resolve(__dirname, 'dist'),
    filename: 'bundle.js'
  },
  watch: true
}
```

> Con `mode: 'development'` indicamos que estamos en desarrollo, podría ser `mode: 'production'`
>
> Con `entry: './src/index.js'` indicamos que queremos que tome ese archivo y los imports realizados en el.
>
> Con`path: .resolve(__dirname, 'dist')` estamos estableciendo una ruta absoluta.
>
> Con `watch: true` nos aseguramos que cada vez que realicemos cambios en `index.js` se vean reflejados en el archivo de salida`bundle.js`



Creamos un script en `package.json`

```json
scripts:{
	"build": "webpack"
}
```

Luego con `npm run build` podremos verificar que todo esté funcionando correctamente.



Luego en `index.html` tendremos que importar el archivo `bundle.js` (ambos están dentro de la carpeta `dist`) y para previsualizar los cambios podemos usar cualquier local development server como Live Server.



# Firebase

## Configuración Firebase

Lo primero que tenemos que hacer es crear un proyecto, para eso debemos ir a https://firebase.google.com/, iniciar sesión y luego hacer click en **Go to console**.

1. Hacemos click en **Create a project.**
2. Le asignamos el nombre deseado, aceptamos los términos y presionamos **Continue.**
3. Deshabilitamos Google Analytics para este proyecto.
4. Hacemos click en **Create project** y luego en **continue** para ir al Dashboard del proyecto.

Una vez que creamos un proyecto debemos crear una aplicación front-end y esto lo hacemos haciendo click en el botón `</>`, luego debemos darle un nombre y hacemos click en **Register app**. A continuación podemos el código que nos aparece ya que lo copiaremos de otro lado y clickeamos **continue to console**. 

Luego en **Project Overview** hacemos click en el engranaje y luego en **Project settings**
En el apartado **Your apps** veremos una parte que dice **SDK setup and configuration** hacer click en **Config** y copiamos el objeto de configuración `const firebaseConfig = {...}`.

Este objeto contiene información sobre nuestro proyecto y lo usaremos para conectarnos desde el front-end.

> Como este objeto de configuración `firebaseConfig` lo vamos a enviar con JavaScript este podrá ser visto por cualquier persona, por lo que como seguridad debemos indicar en Firebase desde qué dominios se puede utilizar la aplicación.
>
> 

```js
const firebaseConfig = {
	apiKey: "",
	authDomain: "",
	projectId: "",
	storageBucket: "",
	messagingSenderId: "",
	appId: ""
}
```



## Instalación Firebase

Instalar firebase `npm install firebase`



## Conexión a Firebase

Lo primero que debemos hacer es inicializar la aplicación Firebase

```js
import { initializeApp } from 'firebase/app'

const firebaseConfig = {...}

initializeApp(firebaseConfig)
```



# Firestore Database

## Configuración Firestore

Lo primero que debemos hacer es crear la base de datos y para eso en **Project Overview** dentro de **Build** vamos a **Firestore Database** y luego hacemos click en **Create database**. 

En el modal de creación elegimos **Start in test mode** y presionamos Next. 

Luego dejamos **nam5 (us-central)** como Cloud Firestore location y presionamos **Enable**.



Para crear una colección hacemos click en **Start collection** y le damos un nombre donde dice **Collection ID** y luego nos dará la opción de crear documentos manualmente.



## Conexión a Firestore

En primer lugar debemos inicializar el servicio firestore con `getFirestore()`

```js
const db = getFirestore();
```



Luego debemos obtener una referencia a una colección en particular con `collection()`

```js
const colRef = collection(db, 'books')
```



## Obtener Documentos

Con `getDocs()` obtenemos un array con todos los documentos de la colección que le pasamos como parámetro y con cada uno de ellos armamos un objeto donde almacenamos los datos junto con el id.

```js
import {getDocs} from 'firebase/firestore'

getDocs(colRef)
  .then(snapshot => {
    // snapshot.docs.forEach(doc => console.log(doc.data()))
    let books = [];
    snapshot.docs.forEach(doc => {
      books.push({ ...doc.data(), id: doc.id })
    })
    console.log(books)
  })
  .catch(err => {
    console.log(err.message)
  })
```

> Podemos pensar que con `getDocs` obtenemos un array de documentos y cada uno de ellos tiene un `id` y `data()`.



## Agregar Documentos

Con `addDoc()` agregamos un nuevo documento a la colección que recibe como primer parámetro, el objeto que recibe como segundo.

```js
import {addDoc} from 'firebase/firestore'

addDoc(colRef, {
  title: 'Learning GIT',
  author: 'Midu'
})
  .then(() => {
    console.log('Document created')
  })
  .catch(err => {
    console.log(err.message)
  })
```



## Eliminar Documentos

Para eliminar un documento tenemos que usar `deleteDoc()` pasándole como parámetro `docForm` siendo esta una referencia al documento que obtenemos con `doc()`

```js
import {deleteDoc, doc} from 'firebase/firestore'


const docRef = doc(db, 'books', "sRQV7axYdu8umr7VOhTn")

deleteDoc(docRef)
  .then(() => {
    console.log('Document deleted')
  })
  .catch(err => {
    console.log(err.message)
  })
```



## Real Time Listener
