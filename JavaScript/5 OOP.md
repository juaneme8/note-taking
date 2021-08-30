# OOP (Object Oriented Programming)

## Object Literals
En ocasiones podremos utilizar objetos con propiedades y métodos como vemos a continuación. Esto se conoce como *object literal*.

```jsx
const circle = {
	radius: 10,
	position: {
		x: 1,
		y: 1
	},
	draw: function(){
		...
	}
}
```

Incluso a partir de ES6 incorporamos una sintaxis más corta para definir los métodos:
```jsx
const circle = {
	radius: 10,
	position: {
		x: 1,
		y: 1
	},
	draw(){
		...
	}
}
```

Sin embargo, si queremos tener más de un círculo tendríamos que copiar y pegar el código anterior tantas veces como objetos necesitemos. Esto no representaría un problema si no tuviéramos lógica, pero como tenemos el método `draw()` este nos quedaría repetido en cada uno de ellos, haciendo difícil el mantenimiento.

Esta problemática tiene dos posibles soluciones:

## Factory Functions
Los Factory Functions son funciones que retornan un objeto.
```jsx
function createCircle(radius){
	return {
		radius,
		draw(){
			console.log('Drawing...');
		}
	}
}
```
> Notar que desde ES6 utilizamos *short object notation* que nos permite poner `radius` y no tener que poner `radius: radius`

> Cuando retornamos un objeto usando arrow functions sin incluir `return` debemos agregar paréntesis al valor retornado:

```jsx
() => ({ name: 'Juan' })
```

Es equivalente a:

```jsx
() => {
 return { name : 'Juan' }
}
```

Luego con `circle1 = createCircle(1);` obtenemos los distintos círculos y luego con `circle1.draw()` invocamos a sus métodos.

Gracias a esto a los métodos los tenemos definidos en un sólo lugar y será más sencillo solucionar bugs y agregar nuevas funcionalidades.

## Constructor Functions
```jsx
function Circle (radius){
	this.radius = radius;
	this.draw = function (){
		return `hello` 
	}
}
```

Luego utilizamos
```jsx
const circle = new Circle(1);
```
De esta manera primero creamos un objeto vacío con el `new` *keyword*, luego hacemos que `this` apunte a ese nuevo objeto y luego retornamos el objeto de la función constructora.

Luego para acceder a la propiedad `circle.radius` y para llamar al método `circle.draw()`

## Comparativa
**Object Literals** los usamos para objetos sin lógica.
**Factory Functions** se escriben en camelCase y retornan un objeto.
**Constructor Functions** se escriben en PascalCase y debemos utilizar `new` y `this` *keywords*.

## Naturaleza Dinámica de los Objectos
La naturaleza dinámica de los objetos (sin importar si los creamos como object literals, con factory functions o constructor functions) hace que en todo momento podamos agregar/quitar propiedades o métodos.

Supongamos que tenemos un objeto `const circle={radius: 1}`

Para agregar una propiedad `circle.color='yellow'` y para agregar un método `circle.draw = function() {...}`

Para quitar una propiedad `delete circle.color` o `delete circle.draw`


## Constructor Property
Todos los objetos tienen una propiedad con el nombre `constructor` que referencia a la función que fue usada para crearlos.

Si tenemos `let x = {...}` JavaScript traduce esto como `let x = new Object();` y `Object()` es una **built-in constructor function** y lo podemos verificar al invocar `x.constructor` que obtenemos en la consola del Chrome `ƒ Object() { [native code] }`

Esto que vimos al usar **Object Literals** sucede también al usar **Factory Functions**. 

En cambio al usar **Constructor Functions** supongamos que tenemos:
```jsx
function Circle(radius){
	this.radius = radius;
	this.draw = function(){
		console.log('Drawing');
		
	}
}
```
Si luego instanciamos con ellos un objeto `const c1 = new Circle(1)` vemos que si hacemos  `c1.constructor`  obtendremos la función en sí:
```jsx
ƒ Circle (radius){
	this.radius = radius;
	this.draw = function (){
		return `hello` 
	}
}
```

Decimos que **Functions Are Objects** ya que podemos acceder a propiedades de `Circle` por ejemplo:
* `Circle.name` obtenemos `"Circle"`
* `Circle.length` obtenemos el número de parámetros es decir `1`

Mientras que con `Circle.constructor` referenciamos a la función que creo el objeto y obtenemos `ƒ Function() { [native code] }` siendo `Function()` una **built-in constructor function** usada por JavaScript para crear una función.

Cuando declaramos una función internamente es representada de este modo:
```jsx
const Circle = new Function ('radius', 
	`
		this.radius = radius;
		this.draw = function(){
			console.log('Drawing');
		}
	`
);
```

> Con el *backtick* podemos ingresar código en múltiples líneas.

### `call`
`Circle.call({},1);` es lo mismo que hacer `const another = new Circle(1)`.

Notar que le pasamos un objeto vacío y de esta manera `this` apuntará a ese objeto. 

Si tenemos múltiples argumentos los pasamos `Circle.call({},1,2,3);`

### `apply`
`Circle.call({},[1]);` es lo mismo que hacer `const another = new Circle(1)`.

Si tenemos múltiples argumentos los pasamos `Circle.call({},[1,2,3]);` Este método es igual a `call()` pero en lugar de pasarle los argumentos de manera explícita se los pasamos mediante un array, lo cual puede ser útil si ya lo tenemos armado.

## Built-in Constructors
En JavaScript tenemos los siguientes built-in constructors:
* `new Object()` pero normalmente usamos **object literals**
* `new String()` pero normalmente usamos **string literals**
* `new Boolean()` pero normalmente usamos **boolean literals**
* `new Number()` pero normalmente usamos **boolean numbers**

# Clases
## Creación de clases
```jsx
class shoppingList {
	constructor(items, nr){
		this.items = items;
		this.nr = nr;
	}
	sayList(){
		console.log(this.items);
	}
}
```
A la hora de instanciar un objeto `const myList = new shoppingList(['item','item2','item3],3);`

## Herencia
Si quiero crear una clase que herede todas las propiedades y métodos de `shoppingList`
```jsx
class Product extends shoppingList {
	constructor (items, nr, ammount, price){
		super(items, nr);
		this.ammount = ammount;
		this.price = price;
	}
}
```
A la hora de instanciar un objeto `const product = new Product(['item','item2','item3],3,2,20);`