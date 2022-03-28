# NPM (Node Package Manager)
Hasta el momento hemos usado módulos y paquetes disponibles en el core de Node (built-in) como por ejemplo el módulo `fs` , pero si queremos contar otros features como por ejemplo una biblioteca de utilidades de JavaScript como `lodash`, un package para la lógica de validación, un framework como `express` podemos instalarlos mediante npm. 

NPM es una herramienta que se instala al instalar node, nos permitirá instalar, actualizar o eliminar paquetes tanto de nuestra computadora como a nivel proyectos.

> npmjs.com: Podremos buscar paquetes y veremos cómo instalarlos, cómo usarlos, y además enlace a página web y repositorio donde encontraremos más información.

## Versiones npm

Para conocer la versión que tenemos de npm hacemos lo mismo que antes 

```
npm -v
```



En caso de querer actualizar a la última versión. Si tenemos la 6.14.7 y ejecutamos este comando nos instala la 7.24.0

```
npm install -g npm
```



En cambio queremos actualizar pero en una determinada **versión mayor**. Supongamos que tenemos la 6.14.7 luego de actualizar tendremos la 6.14.15 podemos hacerlo con:

```
npm install -g npm@6
```

### `package.json`

El archivo **package.json** contiene metadata de la aplicación con datos como ser el nombre, versión, autor, repositorio git, scripts y además un registro de todos los paquetes instalados localmente en el proyecto.
Si tenemos pensado usar third party packages en nuestro proyecto debemos crear este archivo y más aún es considerado una buena práctica hacerlo al comenzar con un proyecto Node.

```bash
npm init
```

Nos hará una serie de preguntas respecto de las cuales podremos aceptar los valores default presionando enter o escribir el valor deseado. Otra opción es emplear el **método rápido** poniendo directamente `npm init -y` con lo cual tomará todos los valores default.

>  El archivo `package-lock.json` registra las diferentes versiones en las dependencias instaladas, no debemos editarlo.

En el archivo veremos como *key/value pairs* todas nuestras las respuestas del wizard (*name, version, description, author, license*)



#### `devDependencies`

 Son utilizadas durante el desarrollo y no deben ser incluidas en el **production environment** donde hagamos el deploy de la app (sin embargo estarán en **node_modules** junto al resto de las dependencias).

Por ejemplo `npm i jshint --save-dev` otra opción es hacerlo con `-D` será agregadas al `package.json` en la propiedad `devDependencies`

```json
 "devDependencies": {
	"jshint": "^2.11.1"
}
```

> En lugar de poner `--save-dev` podemos poner de manera reducida `-D`. En caso de que nos confundamos y no ponemos el `-D` podríamos moverlo en el `package.json` de `dependencies` a `devDependencies`. Esto es importante ya que en el deploy o en algún CI lograremos que sea más rápido (por ejemplo en caso de tener como dependencia Nodemon o ESLint, esto no tiene sentido que esté en producción ya que no le pasaremos el linter).



#### `scripts`

En `package.json` veremos la propiedad `"scripts":` y se trata de comandos a ejecutar en la terminal. Suponiendo que lo editemos de la siguiente manera:

```json
"scripts": {
		"start": "node index",
		"dev": "nodemon index"
	},
```

Esto nos permitirá luego ejecutar `npm run dev` y será lo mismo que poner `nodemon index`. A la hora de ejecutar el comando determinado por la propiedad `start` o `test` no hará falta poner run podremos poner directamente `npm test` o `npm start`.+



Para conocer todos los scripts disponibles debemos ingresar

```bash
npm run
```

### Instalación Paquetes

Al ejecutar `npm install lodash` npm descarga la versión más reciente del paquete del npm registry y la coloca en la carpeta `node_modules`. A su vez cada módulo tiene su archivo `package.json` con su objeto de dependencias (que también se encuentran en la misma carpeta en `node_modules` de la aplicación node).

En versiones anteriores las dependencias de un paquete eran colocadas dentro de la carpeta del paquete y a su vez dentro de una nueva carpeta node_modules. Como consecuencia de esto terminábamos con el mismo paquete instalado varias veces y además esto representaba un inconveniente pro el anidamiento y el límite de caracteres de windows en cuanto a los paths. Como excepción a la situación actual donde todos los paquetes están en la carpeta node_modules si un paquete usa una versión distinta entonces esa versión sí se almacenará localmente con ese paquete. La versión que requiere nuestra app estará en node_modules y la que requiera ese paquete estará en node_modules/nombrePaquete/node_modules



Para instalar un paquete:

```
npm install lodash
```

> También podemos usar la forma reducida `npm i lodash`



Para instalar una versión específica de un paquete:

```
npm install lodash@4.17.4
```



Para desinstalar un paquete podemos hacerlo con:

```
npm uninstall mongoose
```

> O bien utilizando la forma reducida `npm un mongoose`



### Paquetes Locales

En la medida que instalemos paquetes los veremos agregados al `package.json` dentro del objeto dependencies.

#### lodash

```bash
npm i --save lodash
#En versiones modernas de node y npm no hace falta el --save
npm i lodash
```

>lodash documentation: https://lodash.com/docs/

Una vez que instalamos este paquete veremos que se agrega al objeto dependencies dentro del package.json y a su vez nos agrega una carpeta `node_modules` que almacenará los archivos necesarios para ese paquete junto con sus dependencias.

Por convención se le suele dar el nombre _

```js
const _ = require('lodash')
```

Este package cuenta con numerosos métodos mencionaremos algunos de ellos:

##### Método random()

Nos permite obtener un número random pasándole los límites como argumento.

```js
const num = _.random(0,20); 
```

##### Método once()

Nos permite que una función sólo sea ejecutada una vez, por ejemplo para `greet()`:
Inicialmente:

```js
const greet = () => {
	console.log('Hello');
}
greet(); //veremos Hello
greet(); //veremos Hello
```

```js
const greet = _once(() => {
	console.log('Hello');
});
greet(); //veremos Hello
greet(); //no veremos nada
```



### Paquetes Globales

Existen paquetes que no son específicos de una aplicación, sino que queremos utilizarlos en todos los proyectos (por lo general son *command line tools*) por eso debemos instalarlos de manera global esto lo hacemos con `npm install -g <name>`:  

#### Nodemon

La extensión `nodemon`  (abreviatura de node monitor) es una herramienta se encarga de cerrar y volver a abrir el servidor cada vez que realizamos un cambio en el directorio. Este proceso se conoce como live reload server.

Si bien la documentación recomienda instalarla globalmente, por mas que ocupe más espacio tenerlo instalado en varios proyectos **lo aconsejable es instalarlo como dependencia de desarrollo**. De esta manera aquel que se clone nuestro repositorio contará con todas las herrramientas necesarias para trabajar.

> ```
> npm install nodemon --save-dev
> ```



Podemos iniciar nuestra aplicación con *nodemon* así:

```
node_modules/.bin/nodemon index.js
```

Para simplificar esto definimos un script npm dedicado en el `package.json`

```
"dev": "nodemon index.js",
```

En el script no es necesario especificar la ruta *`node_modules/.bin/nodemon`*, porque *npm* automáticamente sabe buscar el archivo desde ese directorio.



###  `.gitignore`
La carpeta `node_modules` usualmente es muy pesada, por lo que no queremos incorporarla al repositorio del sistema de control de versiones (Github por ejemplo) ya que cada vez que alguien haga checkout (copia local desde el repositorio) debería bajar muchos datos. De todos modos, como las dependencias están listadas en `package.json` bastará con ejecutar `npm install` para instalarlas.

Podremos excluir del sistema de control de versiones git la carpeta `node_modules` creando un archivo **.gitignore** en el root del proyecto y luego dentro del archivo escribimos `node_modules/`.

 Podemos hacer `git status` antes y después de agregar esto al archivo para verificar su correcto funcionamiento (veremos primero la carpeta `node_modules` como pendiente de colocar en el staging area y luego ya no la veremos)



### Semantic Versioning ([semver](https://docs.npmjs.com/misc/semver))

La versión de un paquete está compuesta de tres elementos: MAJOR.MINOR.PATCH
* **MAJOR**: nuevas funcionalidades que podrían romper la API (rompen la retrocompatibilidad)
* **MINOR**: nuevas funcionalidades en la API existente (no rompen la retrocompatibilidad).
* **PATCH**: solución de bugs

#### Versión Específica
Cuando queremos asegurarnos que se instale una versión determinada:
```json
"dependencies": {
	"mongoose": "4.13.6"
}
```

#### Caret Character
Cuando en el número de versión vemos el caracter caret `^4.13.6` significa que nos interesan la major version 4 (si al instalar hay una nueva minor version o patch release se instalará).
Ejemplo:
```json
"dependencies": {
	"mongoose": "^4.13.6"
}
```
#### Tilde Character
Cuando en el número de versión vemos el caracter caret `~4.13.6` significa que nos interesan la major version 4  y la minor version 13 (si al instalar hay una nueva patch release se instalará).
Ejemplo:
```json
"dependencies": {
	"mongoose": "~4.13.6"
}
```

### Listar Versiones Dependencias
Para conocer la versión instalada de un paquete una forma es ir a `node_modules` y fijarnos en la carpeta del paquete en cuestión el archivo `package.json` en la **propiedad version**. 



Con `npm ls` obtenemos las versiones instaladas de nuestras dependencias. Si además nos interesan las dependencias de las dependencias podemos ejecutar `npm ls --all`



:rotating_light: Esto puede ser útil si nos encontramos con un vulnerabilidad servera y con `npm audit` detectamos que es por culpa de un paquete y queremos saber quién depende de él por ejemplo `npm ls minimist`.



### Ver Metadata Paquetes
* `npm view express` nos permite obtener en consola el `package.json` de ese paquete.
* `npm view express dependencies` vemos las dependencias de ese paquete (no el árbol completo)
* `npm view express version`  vemos la versión instalada
* `npm view express versions` vemos todas las versiones existentes (útil para upgrades y downgrades)

### Actualizar Paquetes
* `npm outdated` **en caso de tener algún paquete desactualizado** nos mostrará la versión actual que tenemos (**current**), la versión deseada de acuerdo a la condición determinada por el carácter `caret (^)`  o `tilde (~)`   (**wanted**) y la última versión disponible del paquete (**latest**).

```bash
Package            Current  Wanted         Latest  Location
bcrypt               3.0.6   3.0.8          5.0.0  oauth
```

>En `package.json` veremos dentro del objeto dependencies: `"bcrypt": "^3.0.6"` por lo que la *major version* debe ser 3

> Notar que `location` es la ubicación de la dependencia en el árbol de dependencias pero como por default depth es 0, a menos que lo cambiemos siempre estaremos viendo las dependencias top-level que estén desactualizadas. Para cambiarlo por ejemplo a 1 podremos hacerlo del siguiente modo `npm outdated --depth=1`

* `npm update` Actualiza los paquetes de modo que alcancen la versión deseada (wanted)

Si queremos en cambio actualizar a la latest version debemos instalar un paquete con el cual actualizaremos `package.json`:
```bash
npm i -g npm-check-updates
```

Ejecutando `npm-check-updates` obtenemos los paquetes con una major version distinta de la **latest** major version por ejemplo:
```bash
mongoose	 ^2.9.10 -> ^4.13.6
```
Luego en caso de ejecutar `ncu -u` actualizará el `package.json` de modo que acepte la nueva major version a pesar de tener el caracter ^ (que indica que debería conservar la major versión 2). Luego para instalar la versión ahora especificada en el `package.json`  ejecutamos `npm install`.



### Conclusión sobre versiones

Es una buena práctica indicar en el `package.json` la versión exacta con la que queremos trabajar (en lugar de usar el *caret*) ya que esto nos dará un mayor control.

Existe la extensión **Version Lens** que nos muestra un cartel arriba de cada dependencia indicándonos si existe una versión más nueva y podremos modificarlo si así lo ++-deseamos.



### Publicar Paquete
Es posible crear nuestros propios paquetes y publicarlos en *npm registry*.
Supongamos que desarrollamos `module.exports.add= (a,b) => a+b;`

1) Crear cuenta en [npmjs.com](https://www.npmjs.com/signup)
2) `npm login` luego ingresar username, password y email.
3) `npm publish` el nombre (determinado por **name** en `package.json` ) del paquete debe ser único, en caso de que ya exista obtendremos un error y tendremos que cambiarlo.

### Utilizar Paquete Publicado
```bash
mkdir nombreProyecto
npm init -y
npm i nombrePaquete
```
Luego para utilizarlo:
```js
const nPaquete= require('nombrePaquete');

const resultado = nPaquete.add(1,2)
console.log(resultado) //3
```

### Actualizar Versión
Si bien podremos actualizar la versión manualmente en `package.json` también podemos incrementar automáticamente el número que corresponda (major, minor o patch) con:

```
npm version major
```

```
npm version minor
```

```
npm version patch
```



>  Si queremos luego publicar la nueva versión debemos ejecutar `npm publish`

Si luego de realizar cambios en nuestro paquete intentamos ejecutar `npm publish` obtendremos un error, ya que debemos especificar el nuevo número de versión. 