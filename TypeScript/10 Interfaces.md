## Interfaces
Las interfaces nos permiten hacer cumplir una cierta estructura para una clase o un objeto pudiendo establecer las propiedades y sus tipos, los métodos y sus valores que retornan. **Es diferente a una clase ya que no lo usamos para generar objetos sino para establecer los lineamientos que una clase u objeto deben tener**.
```ts
interface IsPerson {
	name: string;
	age: number;
	speak(a: string): void;
	spend(a: number): number;
}
```
En los métodos no importa cómo sea llamado luego el parámetro, por simplicidad usamos la letra `a`.
Si en un futuro tenemos una variable declarada como  `isPerson` tendrá que tener todas esas propiedades y métodos y además no podrá tener ninguna adicional.
```ts
const me: IsPerson = {
	name: 'shaun',
	age: 30,
	speak(text: string): void {
		console.log(text);
	},
	spend(amount: number): number {
		console.log('I spent', amount);
		return amount;
	},
};
```
Como ya dijimos los tipos retornados no es necesario ponerlos explícitamente ya que TS se dará cuenta sin la necesidad de que los pongamos.

Es posible declarar una variable que luego deberá respetar la estructura de la interface.

```ts
let someone: IsPerson;

someone.name = 'Juan';
someone.age = 34;
//another.extra = 'Propiedad Extra'; //error
```

Si en un futuro creamos una función que recibe como parámetro un objeto `isPerson`, el Intellisense nos sugerirá las propiedades y métodos definidos anteriormente.
```ts
const greetPerson = (person: isPerson) => {
	console.log('hello',person.name);
}
```
Luego esta función sólo podrá ser invocada con objetos que tengan la estructura de `isPerson`

## Interfaces con Clases
En la carpeta `src` de nuestro proyecto crearemos una carpeta `interfaces` (también trabajaremos con el archivo `Invoices.ts` que tenemos en la carpeta `classes`). En ella creamos un archivo `HasFormatter.ts` y exportaremos una interface para que sea utilizada en otros archivos).

En `/interfaces/HasFormatter.ts`
```ts
export interface HasFormatter {
	format(): string;
}
```

Luego en `/classes/Invoice.ts` donde inicialmente teníamos:
```ts
export class Invoice {
	constructor(
		readonly client: string, 
		private details: string, 
		public amount: number
	) {}
	
	format() {
		return `${this.client} owes ${this.amount} from ${this.details}`;
	}
}
```
Importamos este archivo e indicamos que la clase `Invoice` implementa la interface `HasFormatter` por lo que tendrá que tener el método `format()`
```ts
import { HasFormatter } from '../interfaces/HasFormatter.js';

export class Invoice implements HasFormatter {
  constructor(
    readonly client: string, 
    private details: string, 
    public amount: number,
  ){}

  format() {
    return `${this.client} owes £${this.amount} for ${this.details}`;
  }
}
```

En `classes` creamos otro archivo llamado `Payment.ts` donde también importamos la interface y creamos una clase que también la implementará.
```ts
import { HasFormatter } from '../interfaces/HasFormatter.js';

export class Payment implements HasFormatter{
  constructor(
    readonly recipient: string,
    private details: string,
    public amount: number,
  ){};

  format() {
    return`${this.recipient} is owed £${this.amount} for ${this.details}`;
  }
}
```

Luego en `app.ts` gracias al uso de la interface para implementar la clase, nos aseguramos que las instancias `docOne` y `docTwo` implementen la interface `HasFormatter` y por ende tengan el método `format`.

```ts
import { Invoice } from './classes/Invoice.js';
import { Payment } from './classes/Payment.js';
import { HasFormatter } from './interfaces/HasFormatter.js';

let docOne: HasFormatter;
let docTwo: HasFormatter;

docOne = new Invoice('yoshi', 'web work', 250);
docTwo = new Payment('mario', 'plumbing', 200);

let docs: HasFormatter[] = [];
docs.push(docOne);
docs.push(docTwo);
```
Con `let docs: HasFormatter[] = [];` creamos un array de objetos implementados con `HasFormatter`.