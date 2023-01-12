# Versionado

## Introducción

A la hora de trabajar con [semantic versioning](https://docs.npmjs.com/about-semantic-versioning) como sabemos tendremos versiones llamadas major, minor y patch.

Este número de versión tendremos que incorporarlo en el `package.json` en la propiedad `version`.

## Versionado Automático

Sin embargo, no tenemos que hacerlo de manera manual sino que podemos ejecutar el siguiente:

```bash
npm version patch
```

Como consecuencia de esto se incrementará el valor de patch, pasando por ejemplo de la 1.0.0 a 1.0.1.

Sin embargo esto nos agregará un tag a ese commit, para evitarlo debemos ejecutar:

```bash
npm version patch --no-git-tag-version
```



## Obtener datos de `package.json`

En ocasiones vamos a querer utilizar el número de versión dentro de nuestro código.

> Si bien la forma más de hacerlo es importando el `package.json`, esta no es del todo conveniente ya que estaríamos incorporandolo en el build todo el archivo y exponiéndonos a potenciales vulnerabilidades.
>
> ```js
> const pj = require('./package.json')
> console.log(pj.version)
> ```



## Creación de Script

Lo que haremos en cambio será contar con un script que se ejecutará antes del script de **build**, para lograr esto de manera automática como vemos en [npm docs](https://docs.npmjs.com/cli/v8/using-npm/scripts#pre--post-scripts) debemos ponerle el nombre **pre**build.

```json
"prebuild": "node -p \"'export const LIB_VERSION = ' + JSON.stringify(require('./package.json').version) + ';'\" > src/version.js",
```

De esta manera nos creará un archivo `version.js` con los datos extraídos. Con `-p/--print` le indicamos Node que evalúe el código entregado a continuación.



Gracias a esto en nuestro `Dockerfile` no tendremos que hacer ningún cambio ya que el comando que ejecutamos es `RUN npm run build` que implica el llamado previo a nuestro script.



## Mostrar versión

Luego podremos utilizarlo en otros archivos:

```jsx
import { LIB_VERSION } from '../version';
```



```jsx
const consoleFormat = 'color:#5aff81;font-weight:bold;font-family:Montserrat;';

// eslint-disable-next-line no-console
console.log(`%clabo-remito v: ${LIB_VERSION}`, consoleFormat);
```

Con `%c` logramos aplicar las reglas de css establecidas en el segundo parámetro.