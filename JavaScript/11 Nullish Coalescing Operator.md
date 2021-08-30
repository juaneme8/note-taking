# Nullish Coalescing
> Basado en https://www.youtube.com/watch?v=v2tJ3nzXh8I&ab_channel=WebDevSimplified
> https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Nullish_Coalescing_Operator
>
> El operador **nullish coalescing** es un operador lógico que retorna el operando de la derecha si el de la izquierda es `null` o `undefined`.
> A diferencia del operador lógico OR `||` que retorna el operando de la derecha si el de la izquierda es `falsy` (es decir no solamente cuando este es `null` o `undefined`). Esto podría ocasionar un comportamiento inesperado al trabajar con `0` o `''`

```js
const foo = null ?? 'default string';
console.log(foo);
// expected output: "default string"

const baz = 0 ?? 42;
console.log(baz);
// expected output: 0
```

## Asignación con logical nullish
El operador **nullish coalescing**
```js
const a = { duration: 50 };

a.duration ??= 10;
console.log(a.duration);
// expected output: 50

a.speed ??= 25;
console.log(a.speed);
// expected output: 25

```

## Ejemplo 1
Esto mismo llevado a un ejemplo interesante donde queremos agregar propiedades a un objeto sólo si no las tiene definidas:

```js
function config(options) {
  options.duration ??= 100;
  options.speed ??= 25;
  return options;
}

config({ duration: 125 }); // { duration: 125, speed: 25 }
config({}); // { duration: 100, speed: 25 }
```

## Ejemplo 2
Una situación similar donde se puede ver la solución que `??` nos brinda frente al uso de `||`:

```js
function calculatePrice(price, taxes, description) {
  taxes = taxes || 0.05;
  description = description || "Default item";
  const total = price * (1 + taxes);
  console.log(`${description} with Tax: ${total}`);
}

calculatePrice(100, 0.07, "My Item"); 
//My Item with Tax: 107

calculatePrice(100, 0.07, "My other Item"); 
//My other Item with Tax: 107

calculatePrice(100, undefined, undefined); //Default item with Tax: 105

calculatePrice(100, 0, ""); 
//(MAL) Default item with Tax: 105 
```

Notar que cuando invocamos la función con un valor válido de `taxes` (0 sin impuesto) y `description` también válido de valor igual a un string vacío `""` estos al ser `falsy` son reemplazados por los valores default.
En cambio usando **nullish coalescing** vemos que solo usará el valor de la derecha si el de la izquierda es `null` o `undefined`
```js
function calculatePrice(price, taxes, description) {
  taxes = taxes ?? 0.05;
  description = description ?? "Default item";
  const total = price * (1 + taxes);
  console.log(`${description} with Tax: ${total}`);
}

calculatePrice(100, 0.07, "My Item"); //My Item with Tax: 107
calculatePrice(100, 0.07, "My other Item"); //My other Item with Tax: 107
calculatePrice(100, undefined, undefined); //(OK) Default item with Tax: 105
calculatePrice(100, 0, ""); // (OK) "" with Tax: 100 
```

```js

```

```js

```

```js

```

```js

```

```js

```