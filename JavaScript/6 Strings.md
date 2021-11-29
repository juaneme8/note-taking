# String

## Creación
A la hora de crear un string podemos hacerlo de dos formas distintas:
La primera forma es utilizando un **String Primitive**
`const message = 'Hello'`
Si bien el string es un tipo primitivo, si escribimos `message.` veremos que tiene métodos y propiedades asociados. Lo que está sucediendo es que el motor de JavaScript wrapea el string primitive con un string object. Gracias a esto podemos a acceder a propiedades `message.length` o métodos como `message.toUppercase()`.

La segunda forma es utilizando el **String Object**, se trata de un *built-in* object:
`const another = new String('Hello')`.

Se trata de dos tipos de strings y podemos chequear que con `typeof(message)` obtendremos `"string"` y con `typeof(another)` obtenemos `"object"`.

## Inmutabilidad
**Los tipos primitivos son inmutables**

Cuando creamos un string este hereda una serie de métodos uno de los cuales es `toUpperCase()`.
El método `toUpperCase()` no cambia el valor del string sino que devuelve un nuevo string.

```javascript
const message = 'Hello'
message.toUpperCase();
console.log(message); //"Hello"
```

## Template Literals
Los template literals son **string literals** que nos permiten almacenar dentro expresiones.

* Admite expresiones multilínea
* Si queremos agregar comillas no hace falta usar caracteres de escape.
* Podemos usarlos para obtener el valor de una variable `${variable}`, si queremos obtener una expresión `${2+2}` o mostrar el valor devuelto por una función `${callToFunction()}`


Podemos usarlos para crear templates html:
```javascript
const html = `
	<h2>${title}</h2>
	<p>${author}</p>
`
```



## `toUpperCase()`

```javascript
message.toUpperCase() //HELLO
```



## `toLowerCase()`

```javascript
message.toLowerCase() //hello
```



## `startsWith()`

```javascript
message.startsWith('H') //true
message.startsWith('h') //false
```



## `endsWith()`

```javascript
message.endsWith('o') //true
```



## `indexOf()`

El método `indexOf()` retorna la primera ocurrencia del valor especificado.  **Este método no modifica el string original.**
```javascript
const message = 'This is my first message';
const result = message.indexOf('s'); //3
```



## `lastIndexOf()`

El método `lastIndexOf()` retorna la última ocurrencia del valor especificado.  **Este método no modifica el string original.**
```javascript
const message = 'This is my first message';
const result = message.lastIndexOf('s'); //20
```



## `includes()`

El método `includes()` retorna un booleano si el string en cuestión incluye la cadena especificada. **Este método no modifica el string original.**
```javascript
const message = 'This is my first message';
const result = message.includes('my'); //true
const result = message.includes('yours'); //false
```



## `slice()`

El método `slice()` nos permite extraer una sección de un string y retornar un nuevo string sin afectar el original. Le pasamos como argumentos la posición inicial y la posición final (que no será incluida).  **Este método no modifica el string original.**

Es posible utilizar como inicio o como fin un valor negativo siendo -1 el último caracter.

```javascript
const message = 'This is my first message';
const result = message.slice(2,5); //'is '
const result0 = message.slice(5,2); //''
const result1 = message.slice(0); //'This is my first message';
const result2 = message.slice(0,1); //'T';
const result3 = message.slice(-1); //'e';
const result4 = message.slice(-9,-5); //'t me';
const result5 = message.slice(1,-1); //'his is my first messag';
```



## `substring()`

El método `substr()` es prácticamente igual a `slice()` 
```javascript
const message = 'This is my first message';
const result = message.substring(2,5); //'is '
const result1 = message.substring(0); //'This is my first message';
const result2 = message.substring(0,1); //'T';
const result3 = message.substring(-1); //
const result4 = message.substring(-9,-5); //
const result5 = message.substring(1,-1); //
```



### `substring()` vs `slice()`

Existen dos diferencias entre `substring` y `slice`:

* La primera es que si el `indexStart` > `indexEnd` `slice` retorna un string vacío mienras que `substring` los swapea.
* Si uno o ambos argumentos son negativos o `NaN` `substring` los trata como si fueran cero. `slice` también trata los `NaN` como si fueran `0` pero con los negativos cuenta desde atrás como vimos anteriormente.



### `substring()` vs `substr()`

No debemos confundir a `substring()` con `substr()`  que es un método deprecado que no se recomienda seguir utilizando cuyo comportamiento es muy similar al de `substring` pero que como segundo argumento recibía la cantidad de caracteres a retornar.



## `replace()`

El método `replace()` busca el string  (o RegEx) que le pasamos como primer argumento y lo reemplaza por el string o la función que le pasemos como segundo y retorna un nuevo string. En caso de utilizar un string nos permite cambiar sólo la **primera ocurrencia** mientras que si usamos una regex que termine con `/../g` modificará todas las ocurrencias. **Este método no modifica el string original.**

```javascript
const message = 'This is my first message';
const result = message.replace('m','w'); //"This is wy first message"
const result1 = message.replace(/m/,'w'); //"This is wy first message"
const result2 = message.replace(/m/g,'w'); //"This is wy first wessage"
```



## `trim()`

El método `trim()` recorta los espacios de un string que existan al comienzo o al final del mismo (los espacios existentes en el medio no se modifican). **Este método no modifica el string original.**
```javascript
const name="    Juan Ocho    ";
name.trim(); //'Juan Ocho'
```



## `trimLeft()`

El método `trimLeft()` recorta los espacios de un string que existan al comienzo. **Este método no modifica el string original.**
```javascript
const name="    Juan Ocho    ";
name.trimLeft(); //'Juan Ocho    '
```



## `trimRight()`

El método `trimRight()` recorta los espacios de un string que existan al final. **Este método no modifica el string original.**
```javascript
const name="    Juan Ocho    ";
name.trimRight(); //'    Juan Ocho'
```



## `split()` - Conversión a Array

El método `split()` nos permite dividir un string en una lista de substrings organizados en un array y retorna este array. Esta división se realiza buscando el patrón que le pasemos como primer argumento.
```javascript
const msg = "Mensaje de Prueba 1";
const words = msg.split(' ');
```



## `concat()`

El método `concat()` nos permite concatenear los argumentos al string con el cual lo llamemos.

```javascript
const message = 'first';
const words = message.concat('-','second',' third') //'first-second third'
```

Si los argumentos que utilicemos no son de tipo string serán convertidos a string antes de concatenar:

```javascript
"".concat({})    // [object Object]
"".concat([])    // ""
"".concat(null)  // "null"
"".concat(true)  // "true"
"".concat(4, 5)  // "45"
```



## `padStart()`

El método `padStart()` nos permite rellenar al inicio un string con otro string hasta alcanzar la longitud dada.

```javascript
const str1 = '5';
const str2 = str1.padStart(3,'0'); // 005

const fullNumber = '2034399002125581';
const last4Digits = fullNumber.slice(-4); //5581
const maskedNumber = last4Digits.padStart(fullNumber.length, '*'); //'************5581'
```



## `padEnd()`

El método `padEnd()` es similar a `padStart()` pero nos permite rellenar al final de string con otro string hasta alcanzar la longitud dada.

```javascript
const str1 = '5';
const str2 = str1.padEnd(3,'0'); // '500'

const fullNumber = '2034399002125581';
const last4Digits = fullNumber.slice(-4); //'5581'
const maskedNumber = last4Digits.padEnd(fullNumber.length, '*'); //'5581************'
```



## `charAt()`

El método `charAt()` devuelve un nuevo string con el caracter UTF-16 del índice especificado de una cadena. 

```
const message = 'hello world';
const first = message.charAt(0); //'h'
```

> Si bien esto es equivalente a hacer `message[0]` antiguamente la *bracket notation* era una mala idea por no ser compatible en IE7 y también porque al usarlo uno podría estar tentado a hacer `message[0] = 'H'` lo cual no funcionaría pero no obtendríamos ningún warning.



## `charCodeAt()`

El método `charCodeAt()` devuelve un entero entre 0 y 65535 representando el código UTF-16 del índice de un string.

```
const message = 'hello world';
const first = message.charCodeAt(0); //104
```



## `match()`

El método `match()` nos permite conocer si un string matchea con una RegEx y capturar en un array las coincidencias encontradas o `null` en caso de que no haya ninguna.

```javascript
const paragraph = 'The quick brown fox jumps over the lazy dog. It barked.';
const regex = /[A-Z]/g;
const regex1 = /[X-Z]/g;

const found = paragraph.match(regex); //Array ["T", "I"]
const found1 = paragraph.match(regex1); //null
```



## `matchAll()`

El comando `matchAll()` retorna un *iterator* de todos los elementos con los que matchea un string con una RegEx incluyendo *capturing groups*. La respuesta es un iterador con los matches y cada match es un array.

La RegEx debe tener un flag global sino obtendremos type error.



> COMPLETAR CON EJEMPLOS DE `matchAll()`
