# Type Basics 
Dijimos que una de las características principales de TypeScript es que utiliza **strict types** por lo que si declaramos `let age = 30` estamos diciendo que es un `number` y no podrá tomar más adelante el valor de un `string` u otro tipo de dato. 

TypeScript infiere qué tipo de variable será de acuerdo al valor asignado. Si queremos hacerlo de manera explícita podemos poner `let age: number;` con lo cual le indicamos que `age` deberá tener números como valor.

## Funciones
Habrá situaciones en las que esperamos que una función reciba un tipo específico de datos. Por ejemplo si tenemos una función que a partir del diámetro calcula la circunferencia de un círculo:
```ts
const circ = diameter => {
	return diameter * Math.PI;
};

console.log(circ(2));
```

Si bien sabemos que esta sólo debe admitir números la realidad es que podríamos pasarle cualquier tipo de datos por ejemplo un `string` y no obtendríamos ningún error al compilar, sino que recién nos daríamos cuenta en tiempo de ejecución ya que veríamos en la consola `NaN`. Para indicarle que el valor esperado es un `number` hacemos lo siguiente:

```js
const circ = (diameter: number) => {
	return diameter * Math.PI;
};
```

Si ahora intentamos pasarle otro tipo de dato nos lo marcará como error. Este chequeo se realiza antes de compilar y no en tiempo de ejecución, es por eso que **en el archivo JavaScript compilado no notaremos nada extraño en el cuerpo de la función**. De esta manera logramos un código más limpio y menos propenso a errores.

## Arrays
Si definimos un array como `let names=['luigi','mario','yoshi']` sólo nos dejará poner elementos de tipo `string` por lo que si hacemos `names.push(3)` obtendremos un error.

De la misma manera que fijamos el tipo de datos dentro del `array`, tampoco podremos cambiar el tipo de la variable `names = 'hola'` para que esta deje de ser un `array` y se convierta a un `string`

Es decir que estamos fijando tanto el tipo de dato de la variable para que sea un array y el tipo de elementos que almacena dicho array.

Si queremos trabajar con **array con contenido mixto** podemos declararlo inicialmente con contenido de los distintos tipos deseados, por ejemplo `mixed = ['luigi',3,'mario']` y luego TypeScript está al tanto de que queremos poder agregar contenido tanto de números como de strings `mixed.push(10); mixed.push('yoshi')`. Siendo incluso posible modificar por un `string` una posición ocupada por un `number` o viceversa: `mixed[0] = 3`

## Objectos
```ts
let ninja = {
	name: 'mario',
	belt: 'black',
	age: 30,
};
```
Esta variable siempre deberá ser un objeto, pero sí podemos hacer que este sea un nuevo objeto

Las propiedades actúan del mismo modo que las variables, es decir que si tengo `name: 'mario'` esta propiedad sólo aceptará `string`, por lo que si hago `ninja.age = '30'` obtengo un error dado que debe ser un `number` y no un `string`.

Tampoco es posible definir nuevas propiedades que no existían al definir el objeto en primer lugar.


```ts
ninja = {
	name: 'yoshi',
	belt:'orange',
	age: 40
}
```
A la hora de hacer esto debemos incluir todas las propiedades del objeto inicial, no siendo posible agregar nuevas propiedades acá tampoco. 

# Tipos explícitos
Hasta el momento TypeScript infiere el tipo de la variable de acuerdo al valor que le damos, sin embargo, en ocasiones puede que queramos inicializar la variable sin darle un valor por lo que no podría inferirlo. En estos casos le damos a la variable un tipo de manera explícita.

```ts
let character:string;
let age:number;
let isLoggedIn:boolean;
```
La variable `character` será inicializada, no le dará un valor pero en un futuro sólo permitirá que sea un `string`.

En el caso de querer trabajar con un `array` de `string`:
```ts
let ninjas: string[];
// ninjas = [1, 2]; // Error (Type 'number' is not assignable to type 'string'.)
// ninjas.push('mario') // Eror ('ninjas' is used before being assigned)
ninjas = ['mario', 'luigi']; //ok
```

Si queremos trabajar con el método `.push()` será necesario inicializar este array como un array vacío.
```ts
let ninjas: string[] = [];
```

Si queremos tener un array con contenido mixto podemos usar **uniones de tipos**
```ts
let mixed: (string|number)[] = [];
mixed.push('hello');
mixed.push(20);
//mixed.push(false); //error
```
> Los paréntesis al hacer uniones de tipos sólo hacen falta al estar adelante de un array.

Las uniones de tipo también pueden ser usadas en variables normales, por ejemplo:
```ts
let uid: string|number;
uid = '123';
uid = 123;
//uid = false; //error
```

Al trabajar con objetos:
```ts
let ninjaOne: object;
ninjaOne = {name: 'yoshi', age: 30}
//ninjaOne = 'string'; //error
ninjaOne = [] //esto funcionaría ya que un array es una clase de objeto
```

Es posible ser más específicos con:
```ts
let ninjaTwo: {
	name: string,
	age: number,
	beltColour: string
}
```
Notar que no estamos poniendo `=` sino `:` por lo que nuevamente estamos declarando el tipo de manera explícita.
Como consecuencia de esto nos obligará a que sea un objeto con todas esas propiedades.

## Tipo `any`
Utilizamos el tipo `any` para indicar que una variable puede ser de cualquier tipo en el futuro.
```ts
let age: any;

age = 1;
age = '1';
age = true;

console.log(age);
```

Esto básicamente revierte los beneficios de usar TypeScript por sobre JavaScript y no nos marcará errores y al pasar el mouse por encima de las variables no obtendremos información útil acerca de por ejemplo las propiedades de un objeto. Es recomendable usarlo lo menos posible y sólo en aquellos casos donde sea imprescindible. 

Si queremos indicar que el tipo es `any` y a su vez inicializar la variable podemos hacerlo así:
```ts
let age: any = 1;
let obj: any = { name: 'Juan' };
let mixed: any[] = []
```

Si queremos indicar que obj2 debe ser un objeto pero que sus propiedades pueden ser de cualquier tipo:
```ts
let obj2: {name:any, age:any}
```

## Funciones
### Tipo de función inferido
Cuando creamos una función y la almacenamos en una variable, de la misma vista para los otros tipos de datos TypeScript inferirá que de ahora en más sólo debe poder almacenar en ella funciones por lo que si intentamos almacenar otro tipo de dato nos dará error.
```ts
let greet = () => {
	console.log('hello, world!');
};

//greet = 'hello'; //error
```

### Tipo de función explícito
Si queremos indicar explícitamente que será una función podemos hacerlo con:
```ts
let greet: Function

greet = () => {
	console.log('hello, world!');
}
```
Notar la capitalización con F mayúscula.

### Funciones con parámetros
Pasaje de parámetros a una función:
```ts
const add = (a: number, b: number) => {
	console.log(a + b);
};

add(5, 10);
//add(5, '10'); //error
```

### Funciones con parámetros opcionales
Si queremos trabajar con parámetros opcionales, por ejemplo en la función anterior tener un tercer parámetro que pueda ser `number` o `string` podemos hacer lo siguiente:
```ts
const add = (a: number, b: number, c?: number | string) => {
	console.log(a + b);
	console.log(c);
};

add(5, 10);
```

En la consola veremos `15` y `undefined`.

Si queremos darle un valor por default a este parámetro opcional, en ese caso el signo `?` deja de ser necesario
```ts
const add = (a: number, b: number, c: number | string = 10) => {
	console.log(a + b);
	console.log(c);
};

add(5, 10);
```
En la consola veremos `15` y `10`.

Notar que ponemos los parámetros requeridos primero y por último los opcionales.

### Funciones que retornan un valor
```ts
const minus = (a: number, b: number) => {
	return a - b;
};

let result = minus(5, 1);
```

TypeScript infiere que `result` es de tipo `number` (lo podemos chequear colocando el mouse por encima) y si intentamos cambiarle el tipo obtendremos un error.

También es posible indicar explícitamente el tipo de valor retornado. Si bien esto no es necesario porque TypeScript lo infiere de manera automática pero en ocasiones puede resultar conveniente usarlo a modo de documentación para indicarle a otros developers el tipo de valor devuelto.
```ts
const minus = (a: number, b: number): number => {
	return a - b;
};

let result = minus(5, 1);
```

La función `add` descripta anteriormente retorna `void` lo cual podemos ver al pasar el mouse por encima de la palabra `add` veremos lo siguiente:
`const add : (a: number, b: number, c?: number | string) => void` como veremos más adelante esto se trata de una **Function Signature**