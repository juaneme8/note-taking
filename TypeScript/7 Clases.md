## Clases
Las clases en TS son similares a las clases en JS, se trata de un *blueprint* de un objeto. 

En `src` creamos una carpeta `classes`, dentro un archivo `Invoices.ts` Creamos un `constructor` para inicializar las variables con el valor recibido al instanciar el objeto y un método `format()` que retorna un mensaje con las propiedades.
```ts
class Invoice {
	client: string;
	details: string;
	amount: number;

	constructor(c: string, d: string, a: number) {
		this.client = c;
		this.details = d;
		this.amount = a;
	}

	format() {
		return `${this.client} owes ${this.amount} from ${this.details}`;
	}
}

const invOne = new Invoice('mario', 'work on the mario website', 250);
const invTwo = new Invoice('luigi', 'work on the luigi website', 300);
console.log(invOne.format());
console.log(invTwo.format());
```

La ventaja de utilizar clases para crear objetos es que si en el futuro queremos crear un array que sólo permita almacenar este tipo de objetos podemos hacerlo del siguiente modo:
```ts
let invoices = Invoice[] = []
```
De esta manera solo podrán agregarse objetos creados usando la clase `Invoice`, al haberlo inicial izado con [] podremos usar el método push. 

```ts
// invoices.push('hello'); //error
invoices.push(invOne);
invoices.push(invTwo);
```

Por default cuando creamos objetos usando clases todas las propiedades son públicas, por lo que podremos modificarlas haciendo por ejemplo `invOne.client='yoshi';` (siempre respetando el tipo de dato indicado anteriormente).