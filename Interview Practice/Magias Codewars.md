# Conceptos Interesantes

A continuación se recopilan algunos conceptos interesantes que surgieron de resolver katas en Codewars.



#### **Moving Zeros To The End**

https://www.codewars.com/kata/52597aa56021e91c93000cb0

Write an algorithm that takes an array and moves all of the zeros to the end, preserving the order of the other elements.

```javascript
moveZeros([false,1,0,1,2,0,1,3,"a"]) // returns[false,1,1,2,1,3,"a",0,0]
```

* Un `filter` que obtenga todos los no ceros y otro `filter` que obtenga los ceros y después concatenar con `concat` ambos. Pensar que si tuviéramos una lista de muchos elementos someterlo a dos filtros que iteren sobre todos ellos en dos oportunidades seguramente no sería muy performante.
* Un `filter` que obtenga todos los no ceros y según la diferencia de `lenght` entre el original y el filtrado agrego ceros. Para eso podríamos crear un array y luego rellenarlo de ceros: `new Array(arr.lenght-noZerosArr.length).fill(0)`
* Un `reduce` que tenga como acumulador dos arrays uno con los no ceros y otro con los ceros y después los concateno.

```javascript
var moveZeros = function (arr) {
  const [nonZeros,zeros] = arr.reduce(([nonZeros,zeros],element)=>{
    if(element===0){
      return [nonZeros,zeros.concat(element)]
    }
    else{
      return [[...nonZeros,element],zeros];
    }
  },[[],[]])
  
  return nonZeros.concat(zeros);
}

moveZeros([false,1,0,1,2,0,1,3,"a"])
```

> Notar que  para los el array de los no ceros cuando agregamos el elemento actual no usamos `concat` sino que usamos el spread operator porque sino si por ejemplo recibíamos un `[]` fallaba el test ya que no concatenaba ese elemento.



#### Arrow function compacta

> Averiguar cómo se llama este concepto

Esto es útil cuando en la función ejecutada para cada uno de los elementos queremos retornar un elemento o en este caso un elemento 

```js
function countSheeps(arrayOfSheeps) {
  return arrayOfSheeps.filter(Boolean).length;
}
```

En este caso es lo mismo que si tuviéramos `arrayOfSheeps.filter(sheep => Boolean(sheep))`



#### Valor Retornado y Operador de incremento `++`

Si utilizamos postfix `x++` el valor retornado será previo al incremento

Si utilizamos prefix el valor retornado será posterior al incremento.



#### Recursividad

A la hora de pensar usar recursividad es conveniente que en la función recibamos dos parámetros, por ejemplo `function countSheep(array,count)` de modo tal que podríamos volver a llamar a `countSheep` con el array sin el elemento que acabamos de procesar y con el contador actualizado.



#### Crear array a partir de string

Si tenemos un string `"hola"` y queremos obtener un array con cada uno de los caracteres como elemento `["h","o","l","a"]` tenemos distintas formas de lograrlo:

```javascript
str.split("")
Array.from(str)
[...str]
Object.assign([], string);
```



## Coercion

> freecodecamp.org/news/js-type-coercion-explained-27ba3d9a2839
>
> https://www.oreilly.com/library/view/you-dont-know/9781491905159/ch04.html

Si restamos dos arrays JavaScript intentará *coerce* ambos lados de la ecuación para que sean números.

Esto aplica tanto si son números

```
[5]-[3] //2
```

Como si son string

```
["5"]-["3"] //2
```

o una mezcla de ambos

```
[5]-["3"] //2
```



Un uso interesante podría ser para restar lo devuelto por la substracción de la operación `match` (aunque este método devuelve un array con la forma de un objeto)

```
"5suy".match(/\d/)-"suy3".match(/\d/) //2
```

:warning: Amplicar con mas información.



## `BigInt`

Cuando tenemos que lidiar con enteros muy grandes podemos usar `BigInt`. Supongamos que los recibimos como string y queremos sumarlos

```
function sumStrings(a,b) { 
  return (BigInt(a) + BigInt(b)).toString();
}
```

