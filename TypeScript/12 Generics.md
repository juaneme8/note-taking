## Generics

> :link: Complementado con el [video de Ben Awad](https://youtu.be/nViEqpgwxHE).

Para adentrarnos en los généricos, comenzaremos trabajando con el tipo más simple que es el array. Con ellos podemos expresar un array de números de otra forma distinta a la cual venimos haciéndo, en lugar de `number[]` podemos poner `Array<number>`.

Es decir que si quiero especificar un tipo de estas características, lo haremos así:

```typescript
type arr = Array<number>
```
Con esto estamos diciendo que `arr` será un tipo de array de `number`.



## Motivación

Supongamos que tenemos una función `lastElement` que recibe un array y retorna el último elemento.

Inicialmente lo planteamos para recibir un array de números:

```typescript
const lastElement = (arr: Array<number>) => {
	return arr[arr.length-1]
}
```

Luego lo utilizamos de este modo:

```typescript
const l = lastElement([1,2,3])
```

Si hacemos hover sobre el tipo veremos  `const lastElement: (arr: Array<number>) => number`

Si quisiéramos ahora recibir un array de strings y retornar un string, nos mostraría un error. Pero como en nuestro caso queremos que funcione con cualquier tipo de array, podemos sacar provecho de los genéricos.

Los genéricos nos permiten crear bloques de código reutilizables que pueden ser utilizados con diferentes tipos.

```typescript
const lastElement = <T>(arr:Array<T>) => {
	return arr[arr.length-1];
}
```

> Notar que en este caso el genérico es `T` y el valor recibido es `T[]`.
>
> En lugar de `Array<T>` podríamos poner `T[]`

Ahora al hacer hover sobre la función veremos `const lastElement: <T>(arr:Array<T>) => T`

Al llamar a la función con números veremos `const lastElement : <number>(arr:Array<number>) => number` y al hacerlo con strings `const lastElement: <string>(arr:Array<string>) => string`. 

TypeScript inferirá qué tipo de dato es `T` sin embargo podemos sobrescribirlo indicando qué representa como vemos a continuación `const l = lastElement<string | null>([null,'b','c'])`.

> Notar que no fue necesario indicar el tipo de dato devuelto pues lo infirió pero podríamos haberlo hecho explícitamente con:
>
> ```typescript
> const lastElement = <T>(arr: T[]):T => {
> 	...
> }
> ```
>



## Múltiples Genéricos

Como primer paso como ejemplo una función que a partir de elemento recibido devuelve un array con ese elemento. Esto mismo planteado sin genéricos suponiendo que vamos a trabajar con números sería:

```typescript
const makeArray = (x:number) => {
	return [x]
}
```

Luego la utilizamos de esta forma:

```typescript
const v = makeArr(5);
```

Si queremos poder recibir cualquier tipo de dato recurrimos a los genéricos:

```typescript
const makeArr = <T>(x: T) => {
	return [x]
}
```

> Estará infiriendo que el valor retornado es `T[]`



También es posible tener más de un tipo genérico:

```typescript
const makeArr = <T, Y>(x: T, y:Y) => {
	return [x,y]
}
```

Si invocamos esta función con números o strings el tipo devuelto será `number` o `string` respectivamente. Sin embargo, también será posible mezclar los tipos:

```typescript
const v1 = makeArr(3,2)
const v2 = makeArr('a','b')
const v3 = makeArr('b',3)
```

En el último caso veremos que el tipo inferido devuelto al hacer hover es `(string | number)[]`. Si queremos algo similar a un tuple o un array de dos valores donde el primero es de un tipo y el segundo de otro, debemos especificar el tipo devuelto:

```typescript
const makeArr = <T, Y>(x: T, y:Y):[T,Y] => {
	return [x,y]
}
```

Para que tenga más sentido, le cambiamos el nombre a `T` y le ponemos `X`.

```typescript
const makeArr = <X, Y>(x: X, y:Y):[X,Y] => {
	return [x,y]
}
```

Ahora el valor devuelto será `[string, number]`



### Tipos por Defecto

Cuando sobrescribimos los tipos de los genéricos tendremos que hacerlo para todos, pero si sólo queremos hacerlo en uno podremos asignarle al otro un valor por defecto:

```typescript
const makeArr = <X, Y=number>(x: X, y:Y):[X,Y] => {
	return [x,y]
}
```

De manera que al usarlo podamos indicar solo las caracteristicas de `X` sin obtener un error.

```typescript
const v3 = makeArr<string | null>('a',5)
```



## Capturar Tipo de Dato

Si tenemos una función `addUID` que recibe un objeto, le agrega una propiedad `uid`y luego lo retorna.

```ts
const addUID = (obj: object) => {
	let uid = Math.floor(Math.random() * 100);
	return { ...obj, uid };
};

let docOne = addUID({ name: 'yoshi', age: 40 });

console.log(docOne.uid);
console.log(docOne.name); //Property name does not exist

```
Veremos que al intentar mostrar en pantalla una de las propiedades recibidas en el objeto obtenemos un error ya que TS no sabe acerca de ellas.

La forma más simple de solucionar esto sería especificar el tipo de objeto que vamos a recibir, con `const addUID = (obj: {name:string, age:number})`, pero eso nos impediría en un futuro llamar a la función con una propiedad adicional.

Para solucionar esto podemos recurrir a generics que capturan el tipo de dato que le pasamos a la función permitiéndonos en caso de pasarle un objeto referenciar todas las propiedades. Así mismo bastará con escribir `docOne.` y el intellisense nos dará a conocer las propiedades disponibles.

```ts
const addUID = <T>(obj: T) => {
	let uid = Math.floor(Math.random() * 100);
	return { ...obj, uid };
};

let docOne = addUID({ name: 'yoshi', age: 40 });

console.log('docOne.name', docOne.name); 
console.log('docOne.uid', docOne.uid);
```


Sin embargo ahora no estamos especificando que el tipo recibido debe ser un objeto por lo que podríamos poner `let docTwo = addUID('hello');` y no nos arrojaría ningún error.
Para solucionar esto debemos poner `<T extends object>` con lo cual nos marcará error si intentamos pasarle otra cosa que no sea un objeto.

```ts
const addUID = <T extends object>(obj: T) => {
	let uid = Math.floor(Math.random() * 100);
	return { ...obj, uid };
};

let docOne = addUID({ name: 'yoshi', age: 40 });

console.log('docOne.name', docOne.name);
console.log('docOne.uid', docOne.uid);
```
También podríamos ser más específicos acerca del objeto que debe aceptar por ejemplo indicando que debe tener una propiedad `name`. 
```ts
const addUID = <T extends { name: string }>(obj: T) => {
	let uid = Math.floor(Math.random() * 100);
	return { ...obj, uid };
};

let docOne = addUID({ name: 'yoshi', age: 40 });

```
Mientras le pasemos un objeto con una propiedad `name` de tipo `string` a pesar de que le pasemos `age` lo aceptará también.



## Genéricos en Interface

Supongamos inicialmente que tenemos una interface como la siguiente:
```ts
interface Resource {
	uid: number;
	resourceName: string;
	data: object;
}

const docThree: Resource = {
	uid: 1,
	resourceName: 'person',
	data:{name:'shaun'}
}
```

Hemos especificado que `data` será de tipo `object`, si en cambio queremos que su tipo sea flexible (pudiendo ser un objeto, string, array, etc), podemos usar **generics**.  Donde tipo lo especificaremos a la hora de crear el objeto con la interface.
```ts
interface Resource<T> {
	uid: number;
	resourceName: string;
	data: T;
}

const docThree: Resource<object> = {
	uid: 1,
	resourceName: 'person',
	data: { name: 'shaun' },
};

const docFour: Resource<string[]> = {
	uid: 2,
	resourceName: 'shoppingList',
	data: ['bread','mill','toilet roll'],
};

console.log(docThree, docFour);
```

Le estamos indicando explícitamente el tipo pero debería inferirlo.



## Con type

Otra forma de utilizar las interfaces con genéricos podría ser definiendo tipos nuevos:

```typescript
interface Resource<T> {
	uid: number;
	resourceName: string;
	data: T;
}

type ResourceNumber = Resource<number>;
type ResourceString = Resource<string>;

```

