### Modificadores
El comportamiento default es equivalente a si tuviéramos el modificador de acceso `public` antes de cada propiedad.
```ts
class Invoice {
	public client: string;
	public details: string;
	public amount: number;

	constructor(c: string, d: string, a: number) {
		this.client = c;
		this.details = d;
		this.amount = a;
	}

	format() {
		return `${this.client} owes ${this.amount} from ${this.details}`;
	}
}
```
En aquellos casos donde no sea aceptable el comportamiento default podemos agregar el modificador `private` de modo tal que esa propiedad solo pueda ser accedida tanto para lectura y escritura desde adentro de la clase (en el ejemplo anterior en el método `format()`) y no desde afuera.

Otro *access modifier* es `readonly` que indica que es posible leer la propiedad tanto desde adentro de la clase como desde afuera pero no es posible de ningún modo cambiarla.

```ts
class Invoice {
	readonly client: string;
	private details: string;
	public amount: number;

	constructor(c: string, d: string, a: number) {
		this.client = c;
		this.details = d;
		this.amount = a;
	}

	format() {
		return `${this.client} owes ${this.amount} from ${this.details}`;
	}
}

let invoices: Invoice[] = [];

invoices.push(invOne);
invoices.push(invTwo);

invoices.forEach(inv => {
	console.log(inv.client, inv.amount, inv.format());
	//inv.client = 'juan'; //error por ser read only
	//console.log(inv.details) //error por ser private
});
```

Existe otra forma abreviada de hacer esto mismo y consiste en definir las propiedades y asignarles como valor a los parámetros recibidos al instanciar esta clase en un mismo paso. Este método sólo puede realizarse cuando tenemos *access modifiers* 
```ts
class Invoice {
	constructor(
		readonly client: string, 
		private details: string, 
		public amount: number
	) {}
	
	format() {
		return `${this.client} owes ${this.amount} from ${this.details}`;
	}
}

const invOne = new Invoice('mario', 'work on the mario website', 250);
const invTwo = new Invoice('luigi', 'work on the luigi website', 300);

let invoices: Invoice[] = [];

invoices.push(invOne);
invoices.push(invTwo);

invoices.forEach(inv => {
	console.log(inv.client, inv.amount, inv.format());
	//inv.client = 'juan'; //error por ser read only
	//console.log(inv.details) //error por ser private
});
```