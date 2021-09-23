# let vs var vs const
* Las variables declaradas con **var** tienen como scope la función.
* Las variables declaradas con **let** tienen como scope el bloque.
* Las variables declaradas con **const** tienen como scope el bloque.

# `var`
Con una única variable:
```js
var i = "global"
function foo(){
	i="local";
	console.log(i); //local
}
foo(); 
console.log(i); //local
```

Posibles problemas: dos variables con el mismo identificador pero en distintos ámbitos.
```js
var i = "global"
function foo(){
	var i="local";
	console.log(i); //local 
}
foo(); //local
console.log(i); //global
```

Intento de acceso desde fuera de la función:
```js
function foo(){
	var i="local";
}
console.log(i); //reference error: i is not defined
```

# `var` y hoisting:
La declaración usando `var` es levantada hasta el inicio del ámbito de la aplicación, pero la asignación permanece donde se realice.

Trabajando con `var`
```js
console.log(i); //undefined
var i = 1; 
```

Trabajando con `const`
```js
console.log(x) //reference error: x is not defined
const x = 1;
```
Trabajando con `const` y `let` el hoisting no es posible y obtendremos siempre *reference error*.

# `const` y `let`
Cuando utilizamos `const` y `let` se reduce el scope de la variable al bloque donde fue definida, entendiendo como tal a lo que queda entre los dos corchetes.

Si la varible es declarada con `const` o `let` en el ámbito global o de una función pertenecerá al ámbito global como ocurría con `var`.

Las variables declaradas con `const` no pueden ser reasignadas pero si es posible mutarlas si almacenan un array u objeto cambiando el valor de sus elementos.

