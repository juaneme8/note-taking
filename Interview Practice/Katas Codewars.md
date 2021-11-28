# **Moving Zeros To The End**

https://www.codewars.com/kata/52597aa56021e91c93000cb0

Write an algorithm that takes an array and moves all of the zeros to the end, preserving the order of the other elements.

```javascript
moveZeros([false,1,0,1,2,0,1,3,"a"]) // returns[false,1,1,2,1,3,"a",0,0]
```

* Un `filter` que obtenga todos los no ceros y otro `filter` que obtenga los ceros y después concatenar con `concat` ambos. Pensar que si tuviéramos una lista de muchos elementos someterlo a dos filtros que iteren sobre todos ellos en dos oportunidades seguramente no sería muy performante.
* Un `filter` que obtenga todos los no ceros y segun la direncia de `lenght` entre el original y el filtrado agrego ceros. Para eso podríamos crear un array y luego rellenarlo de ceros: `new Array(arr.lenght-noZerosArr.length).fill(0)`
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