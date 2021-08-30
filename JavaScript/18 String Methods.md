# Métodos y Propiedades de String

```js
const m = 'This is my first message';
```

```
console.log(m.length); //24
console.log(m[0]); //T
```

## Encontrar un String en un String
```js
console.log(m.startsWith('This')); //true
console.log(m.endsWith('e')); //true

console.log(m.indexOf('i')); //2
console.log(m.indexOf('x')); //-1
console.log(m.indexOf('i', 3)); //5

console.log(m.search('i')); //2
console.log(m.search('x')); //-1

console.log(m.replace('first', 'second')); //This is my second message
console.log(m.replace('i', 'x')); //Thxs is my first message

console.log(m.toLowerCase()); //this is my first message
console.log(m.toUpperCase()); //THIS IS MY FIRST MESSAGE

console.log(m.search('T'));

```

Notar que `search()` y `indexOf()` no son iguales dado que`indexOf()` adminte un segundo argumento donde establecemos el punto de partida de la búsqueda y `search()` admite regular expressions






```js
```