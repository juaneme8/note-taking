# Optional Chaining
> Basado en: https://javascript.info/optional-chaining

El encadenamiento opcional o **optional chaining** nos permite "acceder" de manera segura a propiedades de un objeto anidado incluso cuando una propiedad intermedia no exista. 

Por ejemplo suponiendo que tenemos un objeto `user` que almacena un objeto `address` dentro del cual uno de los campos será `street`. Suponiendo que el usuario no proporcionó `address` si intentamos acceder a `street` obtendremos un error: 

```jsx
let user = {} //usuario sin la propiedad address
console.log(user.address.street); //TypeError: Cannot read property 'street' of undefined
```
Si bien esto podríamos solucionarlo utilizando el *ternary operator*:
```jsx
let user = {} //usuario sin la propiedad address
console.log(user.address?user.address.street:undefined); //undefined
```

Esto no resulta muy elegante ya que tenemos `user.address` repetido. Utilizando **optional chaining**:
```jsx
let user = {} //usuario sin la propiedad address
console.log(user.address?.street); //undefined
```
Como podemos ver el encadenamiento opcional detiene la evaluación si el valor antes de `?.` es `undefined` o `null` y retorna `undefined`

Esto adquiere una mayor importancia si quisiéramos acceder a propiedades con un anidamiento más profundo, por ejemplo `user.address.street.name` si quisieramos usar el *ternary operator* tendríamos que hacer:

```js
let user = {} //usuario sin la propiedad address
console.log(user.address ? user.address.street ? user.address.street.name : null : null); //null
```
Lo cual es menos elegante aún e incluso difícil de leer. Como alternativa podríamos usar el operador `&&` que evalúa la existencia de cada componente y si uno no existe la evaluación se detiene.
```jsx
let user =  {};  // user has no address  
console.log( user.address && user.address.street && user.address.street.name );  // undefined 
```
Pero esto tampoco es ideal ya que tiene repeticiones de `user.address`

Utilizando **optional chaining**:
```jsx
let user =  {};  // user has no address  
console.log( user.address?.street.name );  // undefined 
```
Con esto le estamos diciendo que la propiedad opcional es `address` si quisiéramos indicar que `street` también es opcional deberíamos hacer:

```jsx
let user =  {
	address: {} // address no tiene street
};  
//console.log( user.address?.street.name );  // TypeError: Cannot read property 'name' of undefined

//console.log( user.address?.street?.name );  // undefined
```
No debemos abusar del uso de este recurso ya que si podría dificultar la detección de errores y complicar el debug del programa. Por ejemplo si sabemos que el objeto `user` debe existir pero `address` es opcional, no debemos poner `user?.address?.street` sino `user.address?.street` pues si por algun error `user` es `undefined` estaríamos silenciando el error.

i. Podemos usarlo con funciones
```jsx
let userAdmin =  {  
	admin(){  
		alert("I am admin"); 
	}  
};
let userGuest =  {};  

userAdmin.admin?.(); // I am admin  
userGuest.admin?.(); // nothing (no such method)_
```

ii. Podemos usarlo con `[]`
```jsx
let key =  "firstName";  
let user1 =  { firstName:  "John"  };  
let user2 =  null;  

alert( user1?.[key]  );  // John  
alert( user2?.[key]  );  // undefined
```

iii. Podemos usarlo con el operador `delete` para eliminar una propiedad de un objeto si esta existe:
```jsx
delete user?.name;  // delete user.name if user exists
```

Podemos usarlo para leer y eliminar de manera segura pero no para escribir
```jsx
let user =  null; 
user?.name =  "John";  // Error, doesn't work  
// because it evaluates to undefined = "John"
```