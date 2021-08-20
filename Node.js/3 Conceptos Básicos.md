# Node.js Basics
## The Global Object
Existen algunas diferencias entre el código JavaScript que ejecutamos en front-end y el que ejecutaremos en back-end una de ellas es el Global Object.
El Global Object es similar al *Window Object* que tenemos en front-end y representa el contexto global en el entorno de Node.
```javascript
console.log(global)
```
Veremos una serie de métodos que están adjuntos a global como ser `console`, `setTimeOut()`, `setInterval()`,  `clearTimeout()`,  `clearInterval()`, etc. Para usarlos no será necesario poner por ejemplo `global.setTimeout()` eso está implícito y ponemos directamente `setTimeout()`

**Ejemplo de uso de setTimeout y setInterval** que muestra el mensaje "Intervalo" cada 0.5seg hasta que pasados 3 seg mostramos el mensaje "Timeout" y detenemos el intervalo.
```javascript
setTimeout(() => {
	console.log('Timeout');
	clearInterval(a);
}, 3000);

const  a  =  setInterval(() => {
	console.log('Intervalo');
}, 500);
```
A diferencia de lo que ocurre en el **navegador** donde las variables globales creadas con la sentencia o `statement var` son añadidas como miembros del objeto window, en **Node.js** esto no es así. De manera que no estarán disponibles por fuera del archivo en el cual fueron creadas. Como queremos apps confiables y de fácil mantenimiento recurrimos a la **modularidad**.
Cada archivo en una aplicación de Node es considerado un módulo y allí puede tener variables y funciones con el mismo nombre que otro módulo sin sobre escribirlas. Si queremos utilizar funciones de otro módulo debemos exportarlas.
Decimos que `app.js` es el **main module**.

### Nombre Directorio y Nombre Archivo
Con `__dirname` obtenemos el path absoluto del directorio donde está el archivo y con `__filename` obtenemos el path absoluto del archivo.
```javascript
console.log(__filename);
console.log(__dirname);
```

## Modules & Require
Como dijimos en ocasiones será conveniente dividir nuestro código en varios archivos para lograr modularidad, mantenimiento más fácil y posibilidad de reutilización de código.

### Importar Módulo:
Creamos los archivos `people.js` y `modules.js` 
En `people.js`
```javascript
const  people  = ['yoshi', 'ryu', 'chun-li', 'mario'];
console.log(people);
```

Luego en `modules.js`
```javascript
const  xyz  =  require('./people');
//Notar que podemos ponerle cualquier nombre y no necesariamente people
```
Al ejecutarlo veremos en la consola
`[ 'yoshi', 'ryu', 'chun-li', 'mario' ]`
Lo cual significa que cuando requerimos un archivo también lo ejecutamos.

Si hacemos un `console.log(xyz)` veremos que obtenemos un objeto vacío `{}`, esto se debe a que en todos los módulos por default la propiedad exports del **objeto module** es un objeto vacío.

### Export Simple:
Si quisiéramos acceder a la variable `people` desde `modules.js` no podríamos hacerlo, para poder hacerlo debemos indicarlo explícitamente que queremos exportar ciertos datos para lo cual debemos agregar en `people.js`, esto lo hacemos trabajando con el **objeto module**.
```js
module.exports = people
```
Si ahora hacemos console.log(xyz) veremos`[ 'yoshi', 'ryu', 'chun-li', 'mario' ]`

Notar que si hiciéramos: `module.exports = 'Hola'` al hacer `console.log(xyz)` veríamos `'Hola'`

De la misma manera si en un archivo `logger.js` tenemos una función `log` y hacemos `module.exports = log` en el archivo donde queremos usarla luego de hacer `const logger = require('./logger')` podremos utilizarla haciendo `logger ('texto')`. Mientras que si hubieramos hecho `module.exports.log = log` para usarla tendríamos que poner `logger.log('texto').`

### Module Wrapper
Antes de que el código de un módulo se ejecute, Node.js lo wrapeará con una function wrapper como la siguiente:
```js
(function(exports, require, module, __filename, __dirname) {
// Module code actually lives in here
});
```
Se trata de una "**immediately invoked function expression**"

Con esto logra que las variables globales (definidas con  `var`,  `const`  o  `let`) tengan al módulo como scope y no al global object.
Nos permite tener variables específicas al módulo como los objetos `module` y `exports` o `__filename`  y  `__dirname`. 
    

### Export Múltiple:
Supongamos que en `people.js` tengo otros datos con los que deseo contar en `modules.js`
```js
const  people  = ['yoshi', 'ryu', 'chun-li', 'mario'];
const  ages  = [20, 25, 30, 35];
console.log(people);

module.exports  = {
	gente: people,
	ages: ages,
};
```

Notar que el nombre de la *propiedad* no tiene que ser el mismo que del *valor*, pero normalmente se suele poner el mismo `people:people`, que puede simplificarse gracias al  **Object Property Value Shorthand de ES6/ES2015**.
```js
module.exports  = {
	people,
	ages,
};
```

Luego al hacer `console.log(xyz)` veremos 
`
{
  people: [ 'yoshi', 'ryu', 'chun-li', 'mario' ],
  ages: [ 20, 25, 30, 35 ]
}
`

Por lo que podremos acceder a `xyz.people` o `xyz.ages`.

Utilizando *destructuring*
```javascript
const  {people, ages}  =  require('./people');
```
Por lo que podríamos trabajar directamente con las variable `people` y `ages`.

Suponiendo ahora que no necesito de `ages`.
```javascript
const  {people}  =  require('./people');
```
Lo visto anteriormente esta tanto válido para variables como para funciones.

## Built-In Core Modules
Además de importar y exportar nuestros propios archivos, podemos utilizar los módulos provistos por Node.js que reciben el nombre de **built-in core modules**. Algunos de ellos son: 
* os (operating system) 
* fs (file-system)
* events
* http

#### os module
Podemos utilizar os para obtener datos del sistema operativo y del usuario.
```js
const os = require('os');
console.log(os.platform(), os.homedir()) //win32 C:\//Users\JuaNeMe
console.log(os.freemem(), os.totalmem()); //2586308608 8459923456/Shaun
```

#### path module
Este módulo nos proporciona utilidades para trabajar con rutas de carpetas y archivos.
```js
const  path  =  require('path');

console.log(path.parse(__filename));
```
Con `path.parse(__filename)` obtenemos un objeto con todos los datos de esa ruta, por ejemplo para el archivo `server.js`:
```bash
{
  root: 'C:\\',
  dir: 'C:\\Users\\JuaNeMe\\Documents\\Code\\ninja-node',
  base: 'server.js',
  ext: '.js',
  name: 'server'
}
```

#### event module
Muchos objetos en Node emiten eventos por ejemplo `fs.readStream` emite un evento cuando se abre un fichero. Todos los objectos que emiten eventos son instancias de `events.EventEmitter`. Puedes usar este módulo haciendo `require("events");`
##### Ejemplo Custom Event:
```js
const EventEmitter = require('events'); 
//PascalCase ya que es una clase

class MyEmitter extends EventEmitter {}

//register event listener
const myEmitter = new MyEmitter();
myEmitter.on('event', () => {
	//se ejecutará el callback cuando sucede el evento
	console.log('an event occurred!');
});
//raise event
myEmitter.emit('event');
```
##### Event Arguments:
Cuando generamos un evento (raise an event) puede que queramos agregar argumentos adicionales.
```js
myEmitter.emit('messageLoaded', 1, 'https://nodejs.org')
```
Sin embargo, tener presente que lo ideal es evitar los valores mágicos y es una buena práctica encapsular los valores en un objeto.
```js
myEmitter.emit('messageLoaded', {id: 1, url:'https://nodejs.org')
```
Esto debe corresponderse con el event listener (que debe ir antes de generar el evento).
```js
myEmitter.on('event', (arg) => {
	//también suele usarse e, eventArg, en lugar de arg
	console.log('an event occurred!', arg);
});
```

#### http module
Permite crear webservers que escuchan pedidos HTTP en cierto puerto. Con lo que podremos crear un servicio backend para atender a nuestras aplicaciones clientes.

#### fs module
fs nos proporciona una API para interacturar con el file system permitiéndonos leer, escribir, eliminar archivos. Esta capacidad utilizando JavaScript no podría hacerse si no fuera por Node.js.
Veremos que contamos con los mismos métodos tanto **sincrónicos** como **asincrónicos** (por ejemplo `access() ` y `accessSync() `). El uso de los sincrónicos no es recomendable ya que son bloqueantes.

**Lectura de Archivos:**
En este caso utilizaremos `fs.readFile()` el cual toma 2 argumentos:
El primer argumento es la ruta relativa al archivo que queremos leer (si no existe obtendremos error)
El segundo argumento se trata de un callback que se ejecuta cuando terminamos de efectuar la lectura. Notar que es asincrónico o no bloqueante, por lo que continuará la ejecución mientras se efectúa la lectura y veremos el `console.log('last line')` antes que el contenido del archivo.
Creamos una carpeta docs y en ella un archivo `blog1.txt` que le ponemos un texto `"Hello World!"`
```js
const  fs  =  require('fs');
fs.readFile('./docs/blog1.txt', (err, data) => {
	if(err){
	console.log(err);
	}
	console.log(data); // <Buffer 48 65 6c 6c 6f 20 57 6f 72 6c 64 21>
	console.log(data.toString()) // Hello, World!
});
console.log('last line');
```

* **Escritura de Archivos:**

En este caso utilizaremos `fs.writeFile()` el cual toma 3 argumentos:
* El primero es la ruta relativa del archivo que quremos escribir (si no existe lo creará).
* El segundo es el contenido a agregar (reemplazará todo el contenido)
* El tercero es un callback a ejecutar cuando termine la escritura
```js
// writing files
fs.writeFile('./docs/blog.txt', 'Hello, Ninjas!', () => {
console.log('file was written');
});
```
En caso de que el archivo no exista lo creará
```js
fs.writeFile('./docs/blog2.txt', 'Hello, Again', () => {
console.log('file was written');
});
```
#### Creación y Borrado de Directorios
Supongamos que queremos crear una carpeta llamada `assets` y esto lo hacemos con el método `fs.mkdir()` el cual también es asincrónica, lleva un tiempo en realizar la creación y dispara un callback al terminar.

```js
fs.mkdir('./assets', err => {
	if (err) {
		console.log(err);
	}
	console.log('folder created');
});
```
En caso de que el directorio ya esté creado obtendremos un error: `file already exists`, por lo que antes de ejecutar ese código verificaremos si existe la carpeta. Esto lo hacemos utilizando `fs.existsSync()` se trata de un método sincrónico o bloqueante (demora sólo una pequeña porción de tiempo) y se fijará si existe la carpeta que la pasamos como argumento. En caso de que no exista la creará y si existe la eliminará con `fs.rmdir()` que recibe como primer parámetro el directorio a eliminar y como segundo el callback a disparar cuando termine de hacerlo.
```js
if (!fs.existsSync('./assets')) {
	fs.mkdir('./assets', err => {
		if (err) {
			console.log(err);
		}
		console.log('folder created');
	});
} else {
	fs.rmdir('./assets', err => {
		if (err) {
			console.log(err);
		}
		console.log('folder deleted');
	});
}
```
> Tener presente que **fs.rmdir()** sólo funciona cuando el directorio está vacío, por lo que en esos casos tendremos que usar previamente **fs.unkink()**
#### Borrado de Archivos
El borrado del archivo lo llevamos a cabo con `fs.unlink()` cuyo primer parámetro es la ruta relativa del archivo a eliminar y el segundo el callback a ejecutar al terminar de hacerlo. Si el archivo no existe obtendremos un error por lo que antes de ejecutar ese código nos aseguramos que exista nuevamente utilizando `fs.existsSync()`
```js
if (fs.existsSync('./docs/deleteme.txt')) {
	fs.unlink('./docs/deleteme.txt', err => {
		if (err) {
			console.log(err);
		}
		console.log('file deleted');
	});
}
```

Los procedimientos vistos hasta ahora de lectura y escritura de archivos funcionan bien si se trata de archivos pequeños. En caso de que sean muy grandes será más eficiente utilizar Streams para escribir y leer archivos, dado que tardarán mucho en leerlos y no podremos hacer nada hasta que complete la lectura.

## Streams & Buffers
Los streams nos permitirán comenzar a utilizar la data aún cuando esta no ha sido leída por completo.  
En lugar de esperar a leer todo la data y recién ahí utilizarla lo cual podría tardar bastante tiempo, lo que hacemos es pasar los datos un poco a la vez, a través de un stream. Es decir que pequeñas porciones de datos son troceadas dentro de un buffer (almacenamiento temporal) y cuando este se llena es enviado a través del stream por lo que podremos usarlo en el browser. Un ejemplo práctico es cuando estamos haciendo streaming Youtube/Netflix donde pequeñas porciones de datos son enviadas a la vez por lo que no tenemos que esperar que cargue el video completo para empezar a ver. 

Existen **read streams** con los cuales leeremos data, **write streams** para escribirla y **duplex streams** para leer y escribir datos.

### Read Stream
Creamos un archivo `blog3` con un bastante cantidad de contenido (unas 300 líneas).

Con `fs.createReadStream()` creamos el read stream y le pasamos como primer parámetro el lugar desde el cual queremos leer.
Luego  con `readStream.on` es un event listener de un `data` event (que lo hereda de *EventEmitter*), lo cual sucede cada vez que recibimos un buffer de datos y en ese momento disparamos el callback.
```js
const readStream = fs.createReadStream('./docs/blog3.txt')
readStream.on('data', chunk => {
	console.log('---- NEW CHUNK ----');
	console.log(chunk);
});
```
Cuando hacemos el `console.log(chunk)` obtenemos datos del tipo `<Buffer 6e 63 69 75...>` si queremos obtenerlo como texto podemos hacer `console.log(chunk.toString()) ` o bien pasarle un segundo argumento a `createReadStrem` indicáandole el encoding de modo que será legible así como viene.
```js
 const readStream = fs.createReadStream('./docs/blog3.txt', { encoding: 'utf8'});
```

### Write Stream
También es posible crear `writeStreams` para escribir datos en un archivo un poco a la vez *(chunks of data)*
A continuación queremos leer del archivo `blog3` y escribir esos mismos datos  en `blog4` lo cual hacemos con  el método `writeStream.write(contenido)`
```js
const readStream = fs.createReadStream('./docs/blog3.txt', { encoding: 'utf8'});
const writeStream = fs.createWriteStream('./docs/blog4.txt');

readStream.on('data', chunk => {
	// console.log('---- NEW CHUNK ----');
	// console.log(chunk);
writeStream.write('\nNEW CHUNK:\n');
writeStream.write(chunk);
});

```
### Pipe
En aquellos casos donde estemos pasando datos directamente de un read stream a un write stream, podemos utilizar el método `readStream.pipe()` logrando realizar lo mismo de manera mucho más concisa.
Anteriormente de manera manual escuchabamos al evento data para saber si habíamos recibido un chunk de datos y una vez logrado esto, lo escribíamos manualmente en el write stream para luego enviarlo. Los pipes se encargan de enviar el chunk recibido al write stream. 

Es decir que el siguiente código:
```js
readStream.on('data', chunk => {
	// console.log('---- NEW CHUNK ----');
	// console.log(chunk);
writeStream.write('\nNEW CHUNK:\n');
writeStream.write(chunk);
});
```
Ahora lo hacemos de este modo:
```js
readStream.pipe(writeStream);
```