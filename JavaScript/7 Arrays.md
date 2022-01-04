# Arrays
`const numbers = [1,2];`
Al haber declarado el array como `const` no podremos reasignarlo a otro array pero sí podemos agregar o quitar elementos de ese array.

Como los Arrays son objetos con la notación de punto podremos acceder a sus métodos y propiedades.

## Agregar o quitar elementos
Los los métodos que veremos a continuación producen una mutación sobre el array original:
* `push()` y `pop()` nos permiten agregar/quitar elementos al final
* `unshift()` y `shift()` nos permiten agregar/quitar elementos al inicio
* `splice()` nos permite agregar o quitar elementos en cualquier parte.

### `push()`
El método `push()` nos permite agregar uno más elementos al final del array. 
Este método produce una mutación del array original. 
Retorna el `length` del nuevo array
```jsx
const numbers = [1,2];
numbers.push(3) //retorna 3 y numbers será [ 1, 2, 3 ]
numbers.push(4,5) //retorna 5 y numbers será [ 1, 2, 3, 4, 5 ]
```

### `pop()`
El método `pop()` nos permite eliminar el último elemento de un array. 
Este método produce una mutación del array original. 
Retorna el elemento que eliminó.
```jsx
const numbers = [1,2];
numbers.pop() //retorna 2 (el elemento que eliminó)
```

### `unshift()`
El método `unshift()` nos permite agregar elementos al principio del array. 
Este método produce una mutación del array original. 
Al igual que `push()` retorna el `length` del nuevo array
```jsx
const numbers = [1,2];
numbers.unshift(0); //retorna 3 y numbers será [ 0, 1, 2 ]
numbers.unshift(-1,-2); //retorna 5 y numbers será [ -2,-1, 0, 1, 2 ]
```
### `shift()`
El método `shift()` nos permite eliminar el primer elemento de un array.
Este método produce una mutación del array original.
Al igual que `pop` retorna el elemento que eliminó.
```jsx
const numbers = [1,2];
numbers.shift() //retorna 1 (el elemento que eliminó)
```
Regla Mnemotécnica: Pensarlo como ASM que con SHIFT quitamos el número

### `splice()`
El método `splice()` nos permite agregar o quitar elementos de la posición deseada de un array. 
Este método produce una mutación del array original.
Retorna un array con los elementos eliminados.

Si queremos eliminar elementos debemos indicar el índice del primer elemento a eliminar y la cantidad de elementos `numbers.splice(1,2);` indicamos que a partir de la posición 1 queremos eliminar 2 elementos
```jsx
const numbers = [1,2,3,4,5];
numbers.splice(1,2); // retorna [2,3] y numbers será [1, 4, 5] 
```

Si queremos agregar elementos sin quitar ninguno, indicamos en primer lugar el índice donde queremos agregarlos, luego un 0 indicando que no queremos borrar nada y como parámetros siguientes los elementos a insertar:
```jsx
const numbers2 = [1,2];
numbers2.splice(1,0,3,4); // devuelve [] y numbers será [ 1, 3, 4, 2 ]
```
Si queremos borrar un elemento e insertar otro en su lugar. Por ejemplo supongamos que tenemos `[1, 2, 3, 4, 5]` y queremos que el `3` pase a ser el string `'tres'`.
```jsx
const numbers = [1,2,3,4,5];
numbers.splice(2,1,'tres') // devuelve [3] y numbers será [1, 2, 'tres', 4, 5]
```

**Indice negativo:** El índice también puede ser negativo (considerando que es -1 el último elemento)
* Si queremos eliminar un elemento con índice `-2`
```js
let myFish = ['angel', 'clown', 'mandarin', 'sturgeon']
let removed = myFish.splice(-2, 1) //devuelve ['mandarin'] y myFish será ["angel", "clown", "sturgeon"]
```

**Eliminar todos desde índice:** Si queremos eliminar todos los elementos a partir de un cierto índice.
```js
let myFish = ['angel', 'clown', 'mandarin', 'sturgeon']
let removed = myFish.splice(2) //devuelve ["mandarin", "sturgeon"] y myFish será ["angel", "clown"]
```

## Buscar elementos
### `indexOf()`
El método `indexOf()` nos permite buscar la primera ocurrencia de un elemento y retorna el índice en el cual se encuentra. En caso de no encontrarlo devuelve -1.
```jsx
const numbers = [1,2,3,4,2];
numbers.indexOf(2); // 1
numbers.indexOf(5); // -1
```

### `lastIndexOf()`
El método `lastIndexOf()` nos permite buscar la última ocurrencia de un elemento y retorna el índice en el cual se encuentra. En caso de no encontrarlo devuelve -1.
```jsx
const numbers = [1,2,3,4,2];
numbers.lastIndexOf(2); // 4
numbers.lastIndexOf(5); // -1
```

### `includes()`
El método `includes()` nos permite buscar si un elemento forma parte de un array
```jsx
const numbers = [1,2,3,4];
numbers.includes(2); //true
numbers.includes(5); //false
```

## Vaciar Array

### Reasignación a array vacío: 
```jsx
let numbers =[1,2,3,4];
numbers = [];
```
> Si hubiéramos utilizado `const` no podríamos realizarlo de este modo.

Debemos tener presente que `[1,2,3,4]` seguirá en memoria, pero en caso de que no esté siendo usado por otras variables será eliminado eventualmente por el *garbage collector*. 

A continuación vemos un ejemplo en el cual estamos seguros que no será recogido por el *garbage collector* porque tiene otras referencias.

```jsx
let numbers =[1,2,3,4];
let another = numbers;
numbers = [];
console.log(numbers); // []
console.log(another); // [1,2,3,4]
```

### Truncado
```jsx
let numbers =[1,2,3,4];
let another = numbers;
numbers.length = 0;
console.log(numbers); // []
console.log(another); // []
```

### `splice()` total
```jsx
let numbers =[1,2,3,4];
let another = numbers;
numbers.splice(0,numbers.length); //retorna los elementos eliminados (todo el array)
console.log(numbers); // []
console.log(another); // []
```

### `pop()` en loop
Si tengo muchos elementos en el array tendré un costo en cuanto a performance.
```jsx
let numbers =[1,2,3,4];
let another = numbers;
while(numbers.length){
	numbers.pop();
}
console.log(numbers); // []
console.log(another); // []
```
 > Pensar que no puedo usar `for..of` porque estaría mutando el objeto con `pop()`

## Combinar Arrays

### `concat()`
El método `concat()` nos permite mergear dos o mas arrays y retorna un nuevo array (no modifica los arrays existnetes).

```jsx
const first = [1,2,3];
const second = [4,5,6];
const third = first.concat(second); //[ 1, 2, 3, 4, 5, 6 ] y first y second siguen igual.
```

### `slice()`
El método `slice()` retorna una *shallow copy* de una porción de un array en un nuevo array basado en los valores que le pasemos de `start` y `end` (este último no está incluído). **El array original no será modificado**. 

Tiene el mismo comportamiento que `slice` al utilizarlo con strings.

Es muy similar a `splice` en su comportamiento salvo que este no modifica el array original sino que retorna una copy (`splice` retorna los elementos eliminados)

Ejemplos:
Dado el siguiente array `const animals = ['ant', 'bison', 'camel', 'duck', 'elephant'];`

Tanto `start` como `end` son opcionales. 

```js
// Si sólo le pasamos start incluría 
console.log(animals.slice(2));
// expected output: Array ["camel", "duck", "elephant"]

console.log(animals.slice(2, 4));
// expected output: Array ["camel", "duck"]

console.log(animals.slice(1, 5));
// expected output: Array ["bison", "camel", "duck", "elephant"]

console.log(animals.slice(-2));
//[ 'duck', 'elephant' ]

console.log(animals.slice(-2, -1));
//[ 'duck' ]
```
En caso de utilizar índices negativos -1 corresponde al último elemento.

Si queremos hacer una copia de `animals` podemos ejecutar `const animals2 = animals.slice()`

#### Array Retornado
Decimos que el array retornado es una *shallow copy* y como tal que se comporta del siguiente modo:
* En caso de tratarse de un array de objetos `slice` copia las referencias en un nuevo array y tanto el array original como el nuevo refieren al mismo objeto. Si un objeto cambia los cambios son visibles en ambos arrays.
```js
const data = [
	{ id: 1, name: 'Juan' },
	{ id: 2, name: 'Ocho' },
];

const data2 = data.slice();
data2[1].name = 'Nueve';

console.log(data); //[ { id: 1, name: 'Juan' }, { id: 2, name: 'Nueve' } ]
```

* En caso de tratarse de un array de strings, numbers o booleans `slice` copia los valores en un nuevo array. Los cambios en un array no afectan al otro.

```js
let names = ['Barney', 'Marshall', 'Lily', 'Robin', 'Ted'];
let names1 = names.slice();
names1[0] = 'Superman';
console.log(names); //Seguimos viendo [ 'Barney', 'Marshall', 'Lily', 'Robin', 'Ted' ]
console.log(names1); //[ 'Superman', 'Marshall', 'Lily', 'Robin', 'Ted' ]
```

### Comparación `slice()` vs `splice()`
`splice()` retorna los elementos eliminados mientras que `slice()` retorna los elementos seleccionados.
`splice()` cambia el array original mientras que `slice()` no modifica el array original.

**Regla Mnemotécnica:** Si comparamos las palabras **slice** y **splice** vemos que splice tiene una letra p extra, podemos asociar esto al hecho de **agregar al array original** lo cual nos da una idea de la mutación que hace del array original.

## Verificar es Array
```js
Array.isArray([1, 2, 3]);  // true
Array.isArray({foo: 123}); // false
```

## Iterar un array:
A la hora de iterar un array podemos utilizar `for...of` o bien `forEach()`:

Dado un array `const numbers=[1,2,3];`

* con `for...of`:
```jsx
for(let number of numbers){
	console.log(number); //1 //2 //3
}
```

* con `forEach()`
Para cada elemento del array se ejecuta un *callback*
```jsx
numbers.forEach(function(number){
	console.log(number);
});
```

Con *arrow function*:
```jsx
numbers.forEach(number => console.log(number));
```

Con *function expressions*:
```jsx
const logNumber = (number) => {
	console.log(number)
}

numbers.forEach(logNumber);
```


El uso de **forEach()** tiene dos ventajas:
> `forEach()` admite el uso de manera encadenada.
> `forEach()` tiene la ventaja que admite un segundo parámetro con el índice de la iteración.
```jsx
numbers.forEach((number, index) => console.log(number, index));
```

### `join()`
El método `join()` nos permite **convertir un array en un string**. Podremos pasarle un parámetro con el separador deseado como string, sino por default utilizará una coma.

```jsx
const numbers = [1,2,3];
const joined = numbers.join();
console.log(joined); //"1,2,3"

const joined2 = numbers.join('-');
console.log(joined2); //"1-2-3"

console.log(numbers); 
//No se ve afectado el array original
//[1,2,3]
```

> Si lo que queremos es hacer lo inverso es decir **convertir un string en un array**:
```js
console.log(Array.from('foo'));
// expected output: Array ["f", "o", "o"]
```

### `filter()`
El método `filter()` nos permite crear un nuevo array a partir de los elementos 
que reúnen la condición especificado. **Es un método no destructivo ya que no modifica el array original sino que entrega uno nuevo.**

Suponiendo que tenemos un **array de números** y queremos filtrar los positivos:

```jsx
const numbers = [1,-1,2,3];
const filtered = numbers.filter(function(number){
	return number >=0;
});
```
Ejecuto un callback para cada elemento del array y si retorno `true` (condición superada) formará parte del array de salida.

Lo mismo usando funciones flecha:
```jsx
const numbers = [1,-1,2,3];
const filtered = numbers.filter(number => number>=0);
```

Suponiendo que tenemos un **array de objetos** con usuarios y queremos obtener una lista de aquellos que tienen cuenta premium:
```jsx
const users = [
	{name: 'Juan', premium: true},
	{name: 'Ocho', premium: false}
];

const premiumUsers = users.filter(user => user.premium);
```

### `map()`
El método `map()` nos permite crear un nuevo array a partir de mapear cada elemento de un array con algo distinto. Esto puede resultar útil a la hora de actualizar precios, agregar IVA, crear un template html o jsx, etc. **Es un método no destructivo ya que no modifica el array original sino que entrega uno nuevo.**

#### Ejemplo1: 
Dado un array de números `const numbers =[1,2,3,4];`  armar un html template para mostrarlos como una lista desordenada:
```html
<ul>
	<li>1</li>
	<li>2</li>
	<li>3</li>
	<li>4</li>	
</ul>
```
En ese caso podemos utilizar primero el método `map()` para obtener los `li` (en un array) y luego `join()` para convertirlo en un string.
```jsx
const numbers =[1,2,3,4];
const items = numbers.map(n => '<li>'+n+'</li>')
const html = '<ul>'+items.join(" ")+'</ul>'
```

> Notar que en el `join()` estamos separando los elementos con un espacio ya que si no especificamos nada los separará con comas.

#### Ejemplo2: 
Aplicar un descuento del 50% a los productos que tengan un precio superior a $20.
```jsx
const products = [
	{name: 'TV Led', price:10000},
	{name: 'Hoja A4', price:1}
]

const saleProducts = products.map(product => {
	if(product.price > 20){
		return {name: product.name, price: product.price*0.5}
	}
	return product;
});
```

> Notar que retorno un nuevo objeto porque si hiciera `product.price*=0.5` o `product.price = product.price *0.5` estaría modificando al elemento del array original `products` y esto no es lo deseado.
> Notar que no hace falta el `else` ya que si ingresé al primer if ya retorné cuando llego al segundo `return`.


### `reduce()`
El método `reduce()` lo utilizamos cuando tenemos una array y queremos un único valor como resultado.

### Ejemplo 1:
Dado un array `const numbers = [1,-1,2,3];` obtener la suma de todos los sus números.
```jsx
const numbers = [1,-1,2,3];
const sum = numbers.reduce(function(accumulator, currentValue){
	return accumulator+=currentValue;
},0);
```

> Como segundo parámetro le pasamos el valor inicial. En este caso es lo mismo pasárselo o no, ya que si no se lo pasamos tomará como valor inicial el primer elemento y comenzará las llamadas al callback a partir del segundo, lo cual es lo mismo.

Esto mismo con funciones flecha:
```jsx
const numbers = [1,-1,2,3];
const sum = numbers.reduce((acc, value) => acc+=value);
```

#### Ejemplo 2:
Dado un array de puntajes de un juego sumar todos los puntos obtenidos por  'mario':
```jsx
const scores = [
	{player: 'mario', score: 50},
	{player: 'luigi', score: 10},
	{player: 'mario', score: 20}
]

const marioTotal = scores.reduce((acc,curr) => {
	if(curr.player==='mario'){
		return acc+=curr.score;
	}
	return acc;
},0);
```

### `find()`
El método `find()` retorna el valor del primer elemento de un array que satisface la condición indicada. El método no afecta al array original.

#### Ejemplo
Dada una lista de puntajes obtener el primero que sea mayor que 50.

```jsx
const scores = [10,5,0,40,30,10,90,70];
const firstHighScore = scores.find(score => score>50); //90
```

> Basta con que un valor cumpla la condición para que se interrumpa la iteración.  Este método es similar a `some()` sólo que este último retorna `true` cuando se cumple la condición para un elemento y `find()` retorna al elemento en sí.

### Ejemplo 2:
Dada una lista de cursos quiero obtener aquel que tiene un cierto nombre.
```jsx
const courses = [{id: 1, name: 'a'},{id:2, name:'b'}];
const myCourse = courses.find(c => c.name === 'a') //myCourse vale { id: 1, name: 'a' }
```
La búsqueda se interrumpe cuando el primer elemento satisface la condición. En caso de no encontrar ningun elemento que la satisfaga retorna `undefined`

### `findIndex()`
El método `findIndex()` es similar a `find()` pero entrega el índice de la posición dentro del array que cumple con la condición solicitada o `-1` si ningún elemento reúne las características indicadas.

```jsx
const courses = [{id: 1, name: 'a'},{id:2, name:'b'}];
const myCourse = courses.findIndex(c => c.name === 'a') //myCourse vale 0
```

### `sort()`
El método `sort()` nos permite ordenar elementos de una array, modificando el array original.

#### Ejemplo: 
Dado un array con puntajes `const scores=[10,50,20,5,35,70,45]` ordenar de menor a mayor.
Si hacemos `scores.sort();` ordena por el primer número, por lo que obtendríamos `10,20,35,45,5,50,70`

Para solucionar esto hacemos uso de `a` y `b` siendo estos elementos consecutivos:
```jsx
scores.sort((a,b) => a-b); //[5, 10, 20, 35, 45, 50, 70]
```
* Si `a > b` retorna un valor positivo por lo que invierte.
* si `a = b` reotrna `0` y no invierte.
* Si `a < b` retorna un valor negativo por lo que no invierte.

## Encadenar Métodos
Dijimos que `filter()` y `array()` son métodos no destructivos que entregan un nuevo array. Esto hace que sea posible encadenarlos:

> Por convención cuando encadenamos métodos sin pasar por una variable auxiliar, colocamos cada método en una línea aparte:
```js
const numbers =[1,-2,3,-4];
const items = numbers
				.filter(n => n>=0)
				.map(n => ({value: n}))
```
> Recordar que cuando quiero retornar un objeto utilizando función flecha y sin el *keyword* `return` debo colocarlo entre paréntesis.