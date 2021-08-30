# String

## Creación
A la hora de crear un string podemos hacerlo de dos formas distintas:
La primera forma es utilizando un **String Primitive**
`const message = 'Hello'`
Si bien el string es un tipo primitivo, si escribimos `message.` veremos que tiene métodos y propiedades asociados. Lo que está sucediendo es que el motor de JavaScript wrapea el string primitive con un string object. Gracias a esto podemos a acceder a propiedades `message.length` o métodos como `message.toUppercase()`.

La segunda forma es utilizando el **String Object**, se trata de un *built-in* object:
`const another = new String('Hello')`.

Estos dos tipos de strings podemos chequearlo utilizando `typeof(message)` que obtendremos `"string"` y `typeof(another)` con el que obtenemos `"object"`.

## Inmutabilidad
 **Los tipos primitivos son inmutables**

Cuando creamos un string este hereda una serie de métodos uno de los cuales es `toUpperCase()`.
El método `toUpperCase()` no cambia el valor del string sino que devuelve un nuevo string.
```jsx
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
```jsx
const html = `
	<h2>${title}</h2>
	<p>${author}</p>
`
```

## `toUpperCase()`
```jsx
message.toUpperCase() //HELLO
```

## `toLowerCase()`
```jsx
message.toLowerCase() //hello
```

## `startsWith()`
```jsx
message.startsWith('H') //true
message.startsWith('h') //false
```

## `endsWith()`
```jsx
message.endsWith('o') //true
```

## `indexOf()`
El método `indexOf()` retorna la primera ocurrencia del valor especificado.  **Este método no modifica el string original.**
```jsx
const message = 'This is my first message';
const result = message.indexOf('s'); //3
```

## `lastIndexOf()`
El método `lastIndexOf()` retorna la última ocurrencia del valor especificado.  **Este método no modifica el string original.**
```jsx
const message = 'This is my first message';
const result = message.lastIndexOf('s'); //20
```

## `includes()`
El método `includes()` retorna un booleano si el string en cuestión incluye la cadena especificada. **Este método no modifica el string original.**
```jsx
const message = 'This is my first message';
const result = message.includes('my'); //true
const result = message.includes('yours'); //false
```

## `slice()`
El método `slice()` nos permite extraer una sección de un string y retornar un nuevo string sin afectar el original. Le pasamos como argumentos la posición inicial y la posición final (que no será incluida).  **Este método no modifica el string original.**
```jsx
const message = 'This is my first message';
const result = message.slice(2,5); //'is '
```

## `substr()`
El método `substr()` es similar a `slice()` pero le pasamos la posición inicial y la cantidad de caracteres. **Este método no modifica el string original.**
```jsx
const message = 'This is my first message';
const result = message.substr(2,5); //'is is'
```

## `replace()`
El método `replace()` busca el string que le pasamos (o RegEx) y retorna un nuevo string con los cambios que hayamos indicado como segundo argumento. En caso de utilizar un string nos permite cambiar sólo la **primera ocurrencia**. **Este método no modifica el string original.**

```jsx
const message = 'This is my first message';
const result = message.replace('m','w'); //"This is wy first message"
```

## `trim()`
El método `trim()` recorta los espacios de un string que existan al comienzo o al final del mismo (los espacios existentes en el medio no se modifican). **Este método no modifica el string original.**
```jsx
const name="    Juan Ocho    ";
name.trim(); //'Juan Ocho'
```

## `trimLeft()`
El método `trimLeft()` recorta los espacios de un string que existan al comienzo. **Este método no modifica el string original.**
```jsx
const name="    Juan Ocho    ";
name.trimLeft(); //'Juan Ocho    '
```

## `trimRight()`
El método `trimRight()` recorta los espacios de un string que existan al final. **Este método no modifica el string original.**
```jsx
const name="    Juan Ocho    ";
name.trimRight(); //'    Juan Ocho'
```

## `split()` - Conversión a Array
El método `split()` nos permite dividir un string en una lista de substrings organizados en un array y retorna este array. Esta división se realiza buscando el patrón que le pasemos como primer argumento.
```jsx
const msg = "Mensaje de Prueba 1";
const words = msg.split(' ');
```