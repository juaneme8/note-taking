# Introducción a Firebase 9
> Actualizado con Notas de Firebase 9 basado en la [playlist](https://www.youtube.com/watch?v=9zdvmgGsww0&list=PL4cUxeGkcC9jERUGvbudErNCeSZHWUVlb) de The Net Ninja.

Firebase se define como un *backend as a service* y nos permitirá realizar distintas tareas:  Authentication, Firestore Database, Realtime Database, File Storage, Hosting, Cloud Functions, etc y podremos utilizarlos directamente desde el frontend. Gracias a esto no tendremos que concentrarnos la lógica del lado del servidor y Firebase lo hará por nosotros. Podemos pensarlo como una alternativa a crear nuestra propia infraestructura backend por ejemplo con Node.js y MondoDB.

> Repositorio `md-firebase`



## Diferencias con Firebase 8

Firebase 9 se caracteriza por tener una filosofía **modular** donde tendremos que importar las funciones que necesitamos de las bibliotecas. A diferencia de Firebase 8 que tiene una mirada orientada en objetos y llamamos a esas funciones utilizando objetos de firebase. Esto nos permitirá contar con *tree-shaking* que significa que el código no utilizado será eliminado del bundle (para esto será necesario contar con un module bundler como webpack o rollup). Como resultado tendremos aplicaciones código más optimizado, archivos de menor tamaño y aplicaciones más rápidas



## Configuración Webpack

Como dijimos anteriormente para contar con el tree-shaking y así eliminar el código no utilizado debemos usar un module bundler como webpack. Será el Webpack quién empaquetará las diferentes partes del código importado en un bundle javascript final.

Los pasos que veremos a continuación serán útiles para proyectos en Vanilla JavaScript mientras que caso de estar utilizando una herramienta CLI como create-react-app no tendremos que configurarlo manualmente ya que viene con un module bundler como webpack u otro listo para usar.

![image-20211108084540803](C:\Users\juan.lauria\AppData\Roaming\Typora\typora-user-images\image-20211108084540803.png)



## Instalación Webpack

Tenemos que instalar webpack, para eso eso primero ejecutamso`npm init -y`de manera tal que tengamos un `package.json` y luego `npm init webpack webpack-cli -D`



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
	"build": "webpack "
}
```

Luego con `npm run build` podremos verificar que todo esté funcionando correctamente.



Luego en `index.html` tendremos que importar el archivo `bundle.js` (ambos están dentro de la carpeta `dist`) y para previsualizar los cambios podemos usar cualquier local development server como Live Server.



