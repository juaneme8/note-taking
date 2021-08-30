# Control Flow
## Conditional Statements
* `if...else` suele brindar código más conciso.

```jsx
const adversario = 'Iron-Man';
let loki = '';

if(adversario === 'Iron-Man'){
	loki = 'Magneto';
}else if(adversario === 'Hulk'){
	loki = 'Thanos';
}else if(adversario === 'Thor'){
	loki = 'Odin';
}else{
	loki = 'Loki';
}
```

Muchas veces cuando tenemos muchos `if` pensamos que es necesario refactorizarlo utilizando `switch`

* `switch...case` suele utilizarse para lograr código más legible, pero tiene la contra de que por cada estado que queremos agregar tenemos que incorporar varias líneas:

```jsx
const adversario = 'Iron-Man';
let loki = '';

switch(adversario){
	case 'Iron-Man':
		loki='Magneto';
		break;
	case 'Hulk':
		loki='Thanos';
		break;
	case 'Thor':
		loki='Odin';
		break;
	default:
		loki='Loki';
}

```

> Recordar que en caso de utilizar `swich` para `default` no hace falta `break` por estar ubicado en el último lugar. 

#### Hash Table
Si queremos que este código sea más legible y escalable podemos recurrir a utilizando un objeto con una **estructura Hash Table** donde el string del adversario es el nombre de la propiedad y el disfraz de Loki es el valor.

```jsx
const adversario = 'Iron-Man';

const LOKI_DISFRACES = {
	'Iron-Man': 'Magneto',
	Thor:'Odin',
	Hulk: 'Thanos'
}

const LOKI_DEFAULT_DISFRAZ = 'Loki';

const loki = LOKI_DISFRACES[adversario] || LOKI_DEFAULT_DISFRAZ;
```

Para añadir un nuevo elemento bastará con agregar una propiedad al objeto lo cual es mucho más simple que agregarlo al `switch`.

> Notar que sólo hacen falta las comillas en el nombre de la key para `Iron-Man`
> Esto no está limitado a que las propiedades tengan como valor un `string` y podrían ser por ejemplo métodos: `'Iron-Man': () => (...)`
## Loops
* `for`
* `while`
* `do...while` no es muy usado en la práctica, se ejecuta al menos una vez.
* `for...in`
* `for...of`


### `for...in`
`for...in` nos permite iterar sobre las propiedades de un objeto.
#### Ejemplo 1:
```jsx
const person = {
	name: 'Mosh',
	age: 30
}

for(let key in person){
	console.log(key); //name
	console.log(person[key]); //'Mosh'
}
```

Regla mnemotécnica: forki --> for **key** in

#### Ejemplo 2:
```jsx
const circle = {
	radius: 1,
	draw(){
		console.log('Drawing');
	}
}

for(let key in circle){
	console.log(key, circle[key]);
	// radius	1
	// draw		f
}
```

### `for...of`
`for...of` sólo se puede utilizar sobre iterables (arrays, maps). 

Por ejemplo nos permite iterar sobre los valores de un array.
```jsx
const colors = ['red','blue','green'];
for (let color of colors){
	console.log(color);
}
```

Si en cambio quisiéramos usarlo para iterar sobre las propiedades de un objeto (como hicimos con `for...in`):
```jsx
const person = {
	name: 'Mosh',
	age: 30
}

for(let key of person){
	console.log(key); //ERROR!
```

Obtendríamos **Uncaught TypeError: person is not iterable** y tendríamos que 
utilizar el **built-in constructor** `Object()`: `Object.keys(person)` o bien `Object.entries(person)`

```jsx
for(let key of Object.keys(person)){
	console.log(key); 
	//name
	//age
}
```

```jsx
for(let entry of Object.entries(person)){
	console.log(entry); 
	//(2)["name","Mosh"]
	//(2)["age",30]
}
```

### `break` y `continue`
 Funcionan para todos los loops:
 `break`: interrumpe la ejecución del loop.

```jsx
for (let i=0;i<10;i++){
	if(i===5)
		break;
	console.log(i); //0 1 2 3 4
}
```

  `continue`: interrumpe el ciclo actual y continúa con el siguiente (no es muy conveniente usarlo).
```jsx
for (let i=0;i<10;i++){
	if(i===5)
		continue;
	console.log(i); //0 1 2 3 4 6 7 8 9
}
```

### Uso de Llaves:
Cuando tenemos un single statement no es necesario utilizar llaves, por ejemplo:
```jsx
for(let key in obj)
	if(...)
		console.log(...)
```
En este caso no necesido usar llaver ya que 

### FizzBuzz Algorithm
Suele ser una pregunta de entrevistas, desarrollar una función tal que:
* si el número es divisible por 3 devuelva Fizz
* si el número es divisible por 5 devuelva Buzz
* si el número es divisible por 3 y 5 devuelva FizzBuzz 
* si el número no es divisible por 3 ni por 5 devuelve el número ingresado
* si el input ingresado no es un número, retorna `NaN`

```jsx
function FizzBuzz(input){
    if(typeof(input)!=='number'){
        return NaN;
    }
    if(!(input%2) && !(input%5)){
        return 'FizzBuzz';
    }
    if(!(input%2)){
        return 'Fizz';
    }
    if(!(input%5)){
        return 'Buzz';
    }
    return input;
}
```
> Notar que en la solución implementada tratamos de lograr un código sin *scroll horizontal*, para ello en lugar de poner `if/else` pusimos `if` y `return`.

## Funciones vs Métodos
Las funciones son porciones de código que realizan una tarea específica mientras que los métodos también son funciones pero asociadas a un objeto o tipo de dato en particular. 
> Los métodos se invocan con *dot notation*

## Single Responsability Principle (SRP)
Debemos tratar de desarrollar funciones pequeñas y enfocadas sólo en una tarea.

### Nested Loops
Siempre que tenemos dos `for` anidados probablemente podamos reemplazar a uno de ellos por una función que realice una tarea simple.


## Operator Precedence
El comportamiento es similar al de la matemática.
```jsx
(2+3)*4 //20
2+3*4	//14
```

## Orden de Operación
El orden de operación podemos recordarlo como `BIDMAS` siendo:
* B: *bracket*
* I: *index* (potencia)
* D: *division*
* M: *multiplication*
* A: *addition*
* S: *substraction*

```jsx
let result = 5 * (10 - 3) ** 2;
```

En primer lugar resolvemos el paréntesis por lo que nos queda `5 * 7 ** 2`
En segundo lugar resolvemos la potencia por lo que nos queda `5 * 49`
Por último resolvemos la multiplicación por lo que nos queda `245`

## Clonar
### Clonar Objeto con `for...in`
Supongamos que tenemos el objeto `circle` descripto anteriormente.
```jsx
const circle = {
	radius: 1,
	draw(){
		console.log('Drawing');
	}
}

const another = {};
for(let key in circle){
	anocher[key] = circle[key];
}
```
Sin embargo esta forma no es muy conveniente ay que en caso de tener muchas propiedades **podría ocasionar problemas de performance** y en JavaScript moderno hay mejores formas de hacerlo por ejemplo con `Object.assign()`

### Clonar Objeto con `assign()`
El método `assign()` nos permite combinar o mergear dos o más objetos.
```jsx
const another = Object.assign({},circle);
```

El primer argumento no necesariamente tiene que ser un objeto vacío:
```jsx
const another = Object.assign({color:'yellow'},circle);
```

### Clonar Objeto con Spread Operator
```jsx
cons another = {...circle};
```
También es posible utilizarlo para concatenar y clonar arrays.