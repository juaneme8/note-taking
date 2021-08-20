# NPM (Node Package Manager)
Hasta el momento hemos usado módulos y paquetes disponibles en el core de Node (built-in) como por ejemplo el `módulo fs` , pero si queremos contar otros features como por ejemplo una library de utilidades de JavaScript como `lodash`, un package para la lógica de validación, un framework como `express` podemos instalarlos mediante npm. Es una herramienta que se instala al instalar node, nos permitirá instalar, actualizar o eliminar paquetes tanto de nuestra computadora como a nivel proyectos.

> npmjs.com: Podremos buscar paquetes y veremos cómo instalarlos, cómo usarlos, y además enlace a página web y repositorio donde encontraremos más información.

### Actualizar npm a  la última versión estable:
`npm install -g npm@latest`

### Paquetes Globales:
Existen paquetes que no son específicos de una aplicación, sino que queremos utilizarlos en todos los proyectos (por lo general son *command line tools*) por eso debemos instalarlos de manera global esto lo hacemos con `npm install -g <name>`:  

#### nodemon: 
nodemon (node monitor) se trata de un paquete encargado del *live reload server* que nos permitirá no tener que hacer el restarting del server manualmente cada vez que realizamos un cambio.

```bash
npm install -g nodemon
#También podemos hacerlo reemplezando install por i:
npm i -g nodemon 
```
Luego para utilizarlo ejecutaremos `nodemon server` en lugar de `node server`

### npm install y node_modules:
Al ejecutar `npm install` npm descarga la versión más reciente del paquete del npm registry y la coloca en la carpeta **node_modules**. A su vez cada módulo tiene su archivo `package.json` con su objeto de dependencias (que también se encuentran en la misma carpeta en node_modules de la aplicación node).
En versiones anteriores las dependencias de un paquete eran colocadas dentro de la carpeta del paquete y a su vez dentro de una nueva carpeta node_modules. Como consecuencia de esto terminábamos con el mismo paquete instalado varias veces y además esto representaba un inconveniente pro el anidamiento y el límite de caracteres de windows en cuanto a los paths. Como excepción a la situación actual donde todos los paquetes están en la carpeta node_modules si un paquete usa una versión distinta entonces esa versión sí se almacenará localmente con ese paquete. La versión que requiere nuestra app estará en node_modules y la que requiera ese paquete estará en node_modules/nombrePaquete/node_modules

### package.json
El archivo **package.json** contiene metadata de la aplicación con datos como ser el nombre, versión, autor, repositorio git, scripts y además un registro de todos los paquetes instalados localmente en el proyecto.
Si tenemos pensado usar third party packages en nuestro proyecto debemos crear este archivo y más aún es considerado una buena práctica hacerlo al comenzar con un proyecto Node.
```bash
npm init
```
Nos hará una serie de preguntas respecto de las cuales podremos aceptar los valores default presionando enter o escribir el valor deseado. Otra opción es emplear el **método rápido** poniendo directamente `npm init -y` con lo cual tomará todos los valores default.

>  El archivo `package-lock.json` registra las diferentes versiones en las dependencias instaladas, no debemos editarlo.

En el archivo veremos como *key/value pairs* todas nuestras las respuestas del wizard (*name, version, description, author, license*)

#### Scripts:
En `package.json` veremos la propiedad `"scripts":` y se trata de comandos a ejecutar en la terminal. Suponiendo que lo editemos de la siguiente manera:
```json
"scripts": {
		"start": "node index",
		"dev": "nodemon index"
	},
```
Esto nos permitirá luego ejecutar `npm run dev` y será lo mismo que poner `nodemon index`. A la hora de ejecutar el comando determinado por la propiedad `start` o `test` no hará falta poner run podremos poner directamente `npm test` o `npm start`.

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

##### Método random():
Nos permite obtener un número random pasándole los límites como argumento.
```js
const num = _.random(0,20); 
```

##### Método once():
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
### Desinstalar Paquete:
Podemos hacerlo tanto con `npm uninstall mongoose` como con `npm un mongoose`

### Dependencias y .gitignore:
La carpeta node_modules usualmente es muy pesada, por lo que no queremos incorporarla al repositorio del sistema de control de versiones (Github por ejemplo) ya que cada vez que alguien haga checkout (copia local desde el repositorio) debería bajar muchos datos. A pesar de esto como todas las dependencias están listadas en `package.json` bastará con ejecutar `npm install` para instalarlas.
En caso de utilizar como *herramienta* del sistema de control de versiones a git podremos excluir la carpeta `node_modules` creando un archivo **.gitignore** en el root del proyecto y luego dentro del archivo escribimos `node_modules/` Podemos hacer `git status` antes y después de agregar esto al archivo para verificar su correcto funcionamiento (veremos primero la carpeta `node_modules` como pendiente de colocar en el staging area y luego ya no la veremos)

### Semantic Versioning ([semver](https://docs.npmjs.com/misc/semver))
La versión de un paquete está compuesta de tres elementos: MAJOR.MINOR.PATCH
* MAJOR: nuevas funcionalidades que podrían romper la API (rompen la retrocompatibilidad)
* MINOR: nuevas funcionalidades en la API existente (no rompen la retrocompatibilidad).
* PATCH: solución de bugs

#### Versión Específica:
Cuando queremos asegurarnos que se instale una versión determinada:
```json
"dependencies": {
	"mongoose": "4.13.6"
}
```

#### Caret Character:
Cuando en el número de versión vemos el caracter caret `^4.13.6` significa que nos interesan la major version 4 (si al instalar hay una nueva minor version o patch release se instalará).
Ejemplo:
```json
"dependencies": {
	"mongoose": "^4.13.6"
}
```
#### Tilde Character:
Cuando en el número de versión vemos el caracter caret `~4.13.6` significa que nos interesan la major version 4  y la minor version 13 (si al instalar hay una nueva patch release se instalará).
Ejemplo:
```json
"dependencies": {
	"mongoose": "~4.13.6"
}
```

### Listar Versiones Dependencias:
Para conocer la versión instalada de un determinado paquete una forma es ir a node_modules y fijarnos en la carpeta del paquete en cuestión el archivo `package.json` en la **propiedad version**. Como esto puede resultar tedioso si tenemos múltiples dependencias  que revisar por lo que podemos `npm list` con lo cual obtenemos los números de versiones instaladas de nuestras dependencias y un árbol de las dependencias de las dependencias.
Si sólo queremos ver las dependencias de nuestra aplicación `npm list --depth=0`
#### Instalación Versión específica:
`npm install lodash@4.17.4`

### Ver Metadata Paquetes:
* `npm view express` nos permite obtener en consola el `package.json`.
* `npm view express dependencies` vemos las dependencias de ese paquete (no el árbol completo)
* `npm view express version`  vemos la versión instalada
* `npm view express versions` vemos todas las versiones existentes (útil para upgrades y downgrades)

### Actualizar Paquetes:
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
Luego en caso de ejecutar `ncu -u` actualizará el `package.json` de modo que acepte la nueva major version a pesar de tener el caracter ^ (que indica que debería conservar la major versión 2). Luego para instalar la versión ahora especificada `npm i`

### DevDependencies:
 Son utilizadas durante el desarrollo y no deben ser incluidas en el **production environment** donde hagamos el deploy de la app (sin embargo estarán en **node_modules** junto al resto de las dependencias)
 Por ejemplo `npm i jshint --save-dev`será agregadas al `package.json` en la propiedad `devDependencies`

```json
 "devDependencies": {
	"jshint": "^2.11.1"
}
```

> Si nos confundimos y no ponemos el -D podríamos moverlo en el `package.json` de `dependencies` a `devDependencies`. Esto es importante ya que en el deploy o en algún CI lograremos que sea más rápido (por ejemplo en caso de tener como dependencia ESLint, esta no tiene sentido que esté en producción ya que no le pasaremos el linter). 

> Las peerDependencies son dependencias que tiene que tener el proyecto en el cual utilicemos ese paquete. No son dependencias del proyecto en sí porque eso nos haría depender de una versión específica.

### Publicar Paquete:
Es posible crear nuestros propios paquetes y publicarlos en *npm registry*.
Supongamos que desarrollamos `module.exports.add= (a,b) => a+b;`

1) Crear cuenta en [npmjs.com](https://www.npmjs.com/signup)
2) `npm login` luego ingresar username, password y email.
3) `npm publish` el nombre (determinado por **name** en `package.json` ) del paquete debe ser único, en caso de que ya exista obtendremos un error y tendremos que cambiarlo.

### Utilizar Paquete Publicado:
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

### Actualizar Paquete Publicado:
Si luego de realizar cambios en nuestro paquete intentamos ejecutar `npm publish` obtendremos un error, ya que debemos especificar el nuevo número de versión. Esto podremos hacerlo manualmente en `package.json` o mediante comando de consola: `npm version major`, `npm version minor` o bien `npm version patch` donde incrementará el número automáticamente. Una vez hecho esto sí debemos ejecutar `npm publish`