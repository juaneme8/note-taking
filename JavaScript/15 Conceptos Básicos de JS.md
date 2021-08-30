# JavaScript
>Compilado de:

>El código utilizado para este tema están en `/md-javascript`

# Introducción:
JavaScript es un lenguaje de programación interpretado, de tipado débil (no indicamos el tipo de variable al declararla), de scripting, multiplataforma y orientado a objetos.

## Script
Se coloca dentro de los tags `<script></script>`, normalmente lo colocamos antes del `</body>` para asegurarnos no intentar usar algo que aún no fue cargado, pero esto puede evitarse si usamos:
```js
document.addEventListener('DOMContentLoaded',()=>{
	//Código JavaScript que se ejecuta cuando el DOM está completamente cargado.
});
```

Otra forma de evitarlo es utilizando el atributo `defer` con el cual indicamos que el script se ejecute cuando la página haya terminado de parsearse. Además tiene la ventaja que el script no se comienza a descargar a lo último cosa que podría afectar las velocidades de descarga.
```html
<head>
	<script src="script.js" defer></scrip>
</head>
```

## `for...of`
La sentencia `for...of` ejecuta un bloque de código para cada elemento de un objeto iterable (`String`, `Array`, `Map`, `Set`, etc).

Por ejemplo iterando un `Array`
```js
let iterable = [10, 20, 30];

for (let value of iterable) {
  value += 1;
  console.log(value);
}
// 11
// 21
// 31
```

Si quisiéramos utilizarlo para obtener `keys` o `keys/values` de un objeto deberíamos trabajar con el constructor `Object()` y en particular con los métodos `Object.keys()` y `Object.entries()` respectivamente

```js
const person = {
	name: 'Juan',
	age: 33
}
```
Si quisiéramos utilizarlo de la siguiente forma:
```js
for (const i of person){
	console.log(i)
}
```
Obtendríamos: `Uncaught TypeError: person is not iterable`

Si queremos obtener las `keys` debemos usar `Object.keys(person)`
```js
for (const keys of Object.keys(person)){
	console.log(keys)
}
// name
// age
```
Mientras que si queremos obtener tanto `keys` como `values` debemos usar `Object.entries(person)`

```js
for (const entry of Object.entries(person)) {
	console.log(entry);
}
//(2) ["name", "Juan"]
//(2) ["age", 33]
```
# `console.log()`
## Múltiples Valores de Salida

```js
console.log(a,b)
```

# Funciones vs Métodos
Las funciones son porciones de código que afectúan una tarea específica al igual que los métodos, sólo que estos últimos estan asociadas a un objeto en particular o a un tipo de dato (por ejemplo en un string `name.toUppercase()`)
Los métodos se invoan con *dot notation*
```js
```

```js
```

```js
```