## Módulos
En proyectos de magnitud es conveniente dividir el código en distintos archivos modulares. Por ejemplo un archivo para autenticación, otro para interacciones con la DB, otro para manipular el DOM. Esto nos permitiría tener un proyecto más fácil de leer y de actualizar en un futuro. La forma más fácil de realizar esto en TS es usando *ES6 Module System* que nos permite importar y exportar archivos de la manera deseada. Debemos tener presente que si bien los navegadores modernos soportan el sistema modular, TS no compilará en algo entendible por navegadores viejos, por lo que debemos asegurarnos estar utilizando uno moderno.

En `tsconfig.json` debemos realizar los siguientes cambios:
* `"module": "commonjs"` por `"module: es2015"`
* `"target": "es5"` por `"target: es6"`

Luego en el archivo `index.html` pasaremos de tener `<script src='app.js'></script>` a indicarle que se trata de un módulo con `<script type='module' src='app.js'></script>`

Supongamos que queremos tener en un archivo aparte la clase `Invoice`, para ello en `src` creamos una carpeta `classes` y dentro de ella un archivo `Invoice.ts`. En ese archivo pegamos la clase y le agregamos `export`. luego en `app.ts` agregamos el `import`

```ts
export class Invoice {
	constructor(
		readonly client: string, 
		private details: string, 
		public amount: number
	) {}
	
	format() {
		return `${this.client} owes ${this.amount} from ${this.details}`;
	}c
}
```

Luego en `app.ts`:

```ts
import {Invoice} from './classes/Invoice.js
```

Notar que a pesar de estar trabajando en TS ponemos `Invoice.js` y no `.ts` ya que es JavaScript ya compilado lo que estamos importando)

El uso de módulos si bien nos ayudará a tener un código mejor organizado y de mantenimiento más simple, tiene algunas desventajas. En primer lugar lo relacionado con el soporte por parte de los navegadores, ya que sólo aquellos más modernos soportan *module system* y en segundo lugar no se realiza el *bundle* en un único archivo (por lo que estaremos haciendo distintos *network requests*). Una posible solución a ambos problemas es utilizar **Webpack**.