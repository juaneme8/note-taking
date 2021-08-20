# Variables de Entorno:
Cuando en Express utilizamos `app.listen(3000)` estamos estableciendo un valor arbitrario que si bien funciona en **desarrollo** puede que no lo haga en **producción**, ya que cuando hacemos el deploy el puerto es asignado por el hosting utilizando la variable de entorno PORT. Para asegurarnos un correcto funcionamiento en ambos casos:

```js
const port = process.env.PORT || 3000;
app.listen(port);
```

#### dotenv
Otra opción es usar el paquete dotenv (`npm install dotenv --save-dev` o lo que es lo mismo y más corto `npm i -D dotenv`) que carga en  `process.env.` las variables de entorno establecidas en un archivo `.env` creado en el directorio raíz del proyecto.
Para ello agregamos variables de entorno cada una en una nueva línea con la forma `NAME=VALUE` por convención en mayúsculas y con guión bajo para separar palabras.
Notar que al instalarlo lo agregamos como dependencia de development por lo que no lo tendremos en producción. 

Luego al colocar `require('dotenv').config();`automáticamente cargará los valores especificados en el archivo `.env`.   También es posible usar el comando `--require (-r)` para hacer un **preload de dotenv**, por lo que no tendremos que colocar el require mencionado en la aplicación. `node -r  server`

**Nota:** Tener presente que con este método ==no es posible sobrescribir variables de entorno ya definidas==.

**Nota2:** No debemos utilizar archivos `.env` en producción sino setear los valores directamente en el host.

Volviendo al ejemplo del puerto en el archivo `.dontenv` ponemos `PORT = 3000` y luego:

```js
if (process.env.NODE_ENV !== 'production') {
 require('dotenv').config();
}

app.listen(process.env.PORT);
```

Normalmente no queremos que formen parte del source control por lo que debemos agregar `.env` al `.gitignore`.  Sin embargo algunos desarrolladores incorporan un archivo `.env.example` al repositorio donde listan todas las variables de entorno utilizadas por la aplicación, veremos:
```
HOST=
PORT=
```

Cuando trabajamos con *passwords, tokens, connection strings* también es conveniente trabajar con variables de entorno así podremos reutilizarlas, modificarlas en un único lugar y no correr el riesgo de que se filtren en los repositorios remotos.

### Seteo de Variables de Entorno en Windows:
Podremos setear las variables de entorno en Windows con `set PORT=5000` (sin espacios) utilizando cmd o Cmder (desde la terminal de VS Code no funciona).

En este caso suponiendo que seteamos la variable PORT en 5000, si luego en el `.env` le damos otro valor, no será posible reemplazar el 5000 ya asignado. Para borrar la variable de entorno `set PORT=` y ahí sí podremos establecerla desde el `.env`

`set` modifica el valor de la variable de entorno de manera temporal y únicamente en la consola actual.
`setx` modifica el valor de manera permanente y se verá reflejado este valor en todas las consolas que se abran en un futuro (no así en las que ya están ejecutando)

# Environments:
Podremos habilitar y deshabilitar porciones de código según el environment o entorno en el cual nos encontremos, este podrá ser *development*, *production*, *testing*, etc.

## Detectar Entorno:
El entorno podemos leerlo de la variable `NODE_ENV` desde la aplicación podremos hacerlo de dos formas:
1) `process.env.NODE_ENV` donde si no está configurada obtendremos `undefined` 

2) `app.get('env')` a diferencia del anterior método, si la variable de entorno no está seteada devuelve `development`

## Cambiar Entorno:
Si queremos por ejemplo que el paquete morgan (explicado en sección 3rd Party Middleware) sólo esté habilitado en development environment:

```js
if(app.get('env')==='development'){
	app.use(morgan('tiny'));
	console.log('Morgan Habilitado');
}
```
Podemos cambiar la variable de entorno que determina el environment desde la terminal:
 `set NODE_ENV=production` desde **consola externa**.

Si queremos setear la variable y ejecutar la aplicación: `set NODE_ENV=production & node app.js`.

Otra forma desde la  es establecer `NODE_ENV` para la aplicación actual desde la **terminal de Visual Studio Code (bash)**:
`NODE_ENV=production nodemon app.js`

### Setear entorno con script
Si queremos hacerlo mediante un script de `package.json` no podremos ejecutar directamente `NODE_ENV=development nodemon app.js` sino que tendremos que instalar una dependencia de desarrollo llamada `cross-env` y poner por ejemplo para `npm run dev` `"dev": "cross-env NODE_ENV=development nodemon app",`

## Configuraciones Entorno:
Es posible almacenar configuraciones de la app específicas para cada environment.

### rc 
`npm install rc`

### config
Recomendado por Mosh.
`npm install config`

Creamos una carpeta `config` y dentro de ella podremos crear archivos `default.json`, `development.json`, `production.json`, etc. El modo de funcionamiento es el siguiente en `default.json` colocamos aquellos parámetros genéricos para todos los entornos y luego se los puede reemplazar y agregar nuevos en el el archivo específico del environment.

Por ejemplo supongamos que en `default.json` tenemos:
```json
{
	"name": "Default"
	"port": "3000" 
		
}
```
Luego en  `production.json` colocamos sus configuraciones específicas y también aquellas que queremos reemplazar de las default, por ejemplo:
```json
{
	"name": "Production"
	"mail": {
		"host":"prod-mail-server"
		//tenemos una base de datos diferente para el servicio de mail
	}
}
```

Luego si el valor de `NODE_ENV=production`, cargará los valores específicos de `production.json` por lo que `name:"Production"` y `mail.host="prod-mail-server"` mientras que `port=3000` ya que no fue redefinido.

Luego en `index.js`
```js
const config = require('config');

//Obtener valores de configuración:
console.log(config.get('name')); //Production
console.log(config.get('mail.host')); //prod-mail-server
```
**Nota:** Si `NODE_ENV` toma un valor para el cual no existe ningún archivo `.json` obtendremos el siguiente mensaje: 
*WARNING: NODE_ENV value of 'production' did not match any deployment config file names*


#### config + Custom Environment Variables:
No debemos almacenar contraseñas de la base de datos ni de mail en los archivos de **config** ya que al hacer check in en un repositorio estos quedarían visibles para todo aquel con acceso al repositorio. Por el contrario debemos almacenarlos en variables de entorno.

En desarrollo las variables las seteamos manualmente `set APP_PASSWORD=1234` mientras que en producción seguramente tengamos un panel de configuración para configurar las variables de entorno.

> En Linux en shell bash las creamos con `export FOO=bar`

Para habilitar variables de entorno personalizadas, crear un archivo de configuración: `config/custom-environment-variables.json` mapeamos configuraciones con variables de entorno.
**Nota:** Si tenemos una variable definida en este archivo que no seteamos obtendremos un error.
```json
{
	"mail":
		{
		"password:"APP_PASSWORD"
		}
}
```

En caso de que estemos en el entorno de desarrollo y tengamos `mail.password` también definido en `development.json` ==tendrá prioridad lo definido mediante variable de entorno==. A menos que  la variable de entorno esté vacía en cuyo caso valdrá lo especificado en el archivo del environment.


|        |                                                              |
| ------ | ------------------------------------------------------------ |
| `NOTA` | Notar que con el package **config** a diferencia de lo que hacemos con **dotenv** no cargamos valores de variables de entorno, sino que las asociamos a propiedades de configuración. |



# Debugging:

## debug:
Utilizar `console.log()` con propósitos de debug puede volverse tedioso cuando tenemos que comentar y descomentar estas sentencias.
Con el paquete **debug** (`npm install debug`) logramos mostrarlos o no, de acuerdo al valor de la variable de entorno llamada `DEBUG`.

Por ejemplo suponiendo que tenemos distintos namespaces:
```js
const debug = require('debug')('app:startup'); 
const debugDb = require('debug')('app:db'); 
const debugDb2 = require('debug')('app:db2'); 
//tiene un default module que es una función y le pasaremos un nombre

//cuando queremos enviar un mensaje
debug('mensaje');
debugDb('mensaje2');
debugDb2('mensaje3');
```

Sólo se verán estos mensajes si la variable de entorno `DEBUG` tiene el valor del namespace, es decir:

### Si queremos no ver ningún mensaje:
```bash
set DEBUG= 
```
### Si queremos ver algunos mensajes:
```bash
set DEBUG=app:startup,app:db
```
### Si queremos ver todos los mensajes:
Esto incluirá no sólo nuestros mensajes sino también los usados por los paquetes por ejemplo.
```bash
set DEBUG=app:*
```

### Si queremos ver todos los mensajes menos algunos:
Veremos todos los debuggers menos los que comiencen con "connect:"
```bash
set DEBUG=*,-connect:*
```
Veremos cada mensaje identificando el namespace con un color y al final el tiempo transcurrido desde el anterior mensaje del namespace.