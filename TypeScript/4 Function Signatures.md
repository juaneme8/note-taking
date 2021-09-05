## Function Signatures
Anteriormente vimos cómo asignar un tipo de función a una variable, de modo que sólo pueda en el futuro almacenar una función de cualquier tipo:
```ts
let greet: Function;
```

Es posible ser todavía más específicos acerca de qué tipo de función puede almacenar esa variable y esto lo hacemos especificando una **function signature**. Básicamente consiste en describir la estructura de la función con los argumentos que acepta y que tipo de datos retorna.
```ts
let greet: (a:string, b:string) => void;
```

De esta manera la variable `greet` sólo podrá almacenar una variable que siga esa estructura.
Si bien hemos especificado `a` y `b` podríamos haberlos llamado de cualquier forma y al asignar la función también podremos darles cualquier nombre.
```ts
let greet: (a: string, b: string) => void;

greet = (name: string, greeting: string) => {
	console.log(`${name} says ${greeting}`);
};

```
Como veremos no es necesario poner `void` en `greet = (name: string, greeting: string):void =>` ya que TypeScript lo infiere automáticamente.

Por otra parte si le asignamos otro tipo a uno de los parámetros nos marcará el error:
```ts
greet = (name: string, greeting: number) => {
	console.log(`${name} says ${greeting}`);
};
```

A continuación mostramos otro ejemplo:
```ts
let calc: (a: number, b: number, c: string) => number;

calc = (numOne: number, numTwo: number, action: string) => {
	if (action === 'add') {
		return numOne + numTwo;
	} else {
		return numOne - numTwo;
	}
};

```

Por último podemos mostrar este ejemplo que recibe un objeto:
```ts
let logDetails: (obj: { name: string; age: number }) => void;

logDetails = (ninja: { name: string; age: number }) => {
	console.log(`${ninja.name} is ${ninja.age} years old`);
};

```
Si bien el nombre del parámetro recibido podemos cambiarlo por cualquier otro, las propiedades deberán respetar ese nombre `name` y `age`

También es posible utilizar type aliases y así mismo satisfacer la function signature
```ts
let logDetails: (obj: { name: string; age: number }) => void;

type person = { name: string; age: number };
logDetails = (ninja: person) => {
	console.log(`${ninja.name} is ${ninja.age} years old`);
};
```