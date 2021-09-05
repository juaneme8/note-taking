## Generics
Los genéricos nos permiten crear bloques de código reutilizables que pueden ser utilizados con diferentes tipos.

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
Se suele utilizar `<T>` pero bien podría usarse otra letra.

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

También es posible usar **generics** con **interfaces**.

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

Si en cambio queremos que `data` sea flexible (pudiendo ser un objeto, string, array, etc), podemos usar **generics**.  Donde tipo lo especificaremos a la hora de crear el objeto con la interface.
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