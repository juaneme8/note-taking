# Recursividad

> Basado en video [Recursion in Programming](https://youtu.be/IJDJ0kBx2LM) de freecodecamp. 

Un método recursivo es aquel que se llama a si mismo hasta alcanzar una condición que detiene la recursividad.

La recursividad consiste en tomar un problema grande y partirlo en un montón de subproblemas más pequeños.

Como primera analogía podríamos tomar el caso de una persona haciendo fila en un cajero que desea saber cuanta gente tiene adelante. Le preguntará a la persona de adelante y esta a la de adelante hasta llegar al último que dirá que es el número 1. A partir de esto todos sabrán qué número son y el primero obtendrá su resultado.

En términos de código llamaremos a la función recursivamente cambiando el valor con el cual la llamamos hasta alcanzar la condición de finalización.

```js
function getMyPositionInLine(person){
	if(person.nextInLine===null){
		return 1;
	}
	return 1+getMyPosition(person.nextPersonInLine)
}
```



# Ventajas Recursividad

* Código elegante con pocas líneas de código. 

* Desaparece la necesidad de loops complejos y estructuras auxiliares de datos (disponemos del stack).
* *Memoization* para acelerar llamadas redundantes.
* Funciona muy bien con objetos JSON, árboles y grafos de modo que podamos enfocarnos en una unidad de la estructura de datos a la vez.

# Desventajas Recursividad

* Aumento de la carga de CPU y lentitud en el código comparado con una iteración mediante un loop.
* Posible apareción de errores de *out of memory* o excepciones de *stack overflow*. 
* Código innecesariamente complejo si la situación particular no amerita el uso de recursividad.



# Call Stack

 Podemos entender el call stack como una abstracción del sistema operativo con la cual almacenamos las invocaciones a los métodos dentro de nuestro programa. De esta manera sabremos a qué direcciones de memoria debemos retornar datos y almacenar datos locales como los parámetros que le pasamos a cada método.   

```js
function A(){
	return "Hello " + B();
}
function B(){
	return "my " + C();
}
function C(){
	return "friends";
}
```

En este caso almacenaremos en lo que se conoce como *call frames* primero `"Hello + B()`, luego `my + C()` y por último `"Friends"`. Finalmente iremos popeando de ese stack primero `"Friends"` luego `"my"+"friends"` y por último `"Hello" + "my friends"`.



Puede darse una situación en la cual tengamos una recursividad que nunca termine y eso va a ocasionar la excepción conocida como *stack overflow*. Esto sucede cuando excedemos la memoria pre alocada que nuestro programa tiene.

```
function A(){
	return A();
}
```

Es por esto que es importante establecer lo que se conoce como *base case* que evita que la recursividad continúe eternamente.



## String Reversal

Suponemos que tenemos un string como entrada y queremos entregar a la salida el string  invertido.



A la hora de plantear estos problemas debemos preguntarnos dos cosas:

* ¿Cuál es el base case? Es decir cuándo puedo dejar de continuar con el algoritmo. 

  Para analizar esta pregunta debemos pensar cuál sería la menor entrada que le podemos insertar a este algoritmo. En este caso podríamos pensarlo como una letra (cuyo reverso es el mismo caracter por lo que lo retornamos ) o un string vacío (en cuyo caso retornaríamos un string vacío). 

* ¿Cuál es la mínima cantidad de trabajo que debo hacer en cada iteración? En este caso cual es la unidad más pequeña que puedo invertir en cada iteración.

  En este caso la cantidad mínima de trabajo consiste en modificar un único caracter.



En primer lugar suponemos que detenemos la iteración cuando nos queda un caracter y lo retornamos

```js
function reverse(str){
  if(str.length===1)
    return str;
  return reverse(str.slice(1))+str.charAt(0)
}

reverse('juaneme8') //'8emenauj'
```

>  Notar que trabajar con el índice `0` y colocar el carácter de la iteración al final es más sencillo que hacerlo con el `length - 1` y colocarlo al principio como vemos a continuación.
>
> ```js
> function reverse(str){
>   if(str.length===1)
>     return str;
>   return str[str.length-1]+reverse(str.slice(0,str.length-1))
> }
> ```



También podríamos detener la iteración cuando `str` es un string vacío:

```js
function reverse(str){
  if(str==="")
    return str;
  return reverse(str.slice(1))+str.charAt(0)
}

reverse('Juaneme8')
```



Si ejecutamos `reverse("Hello")` el call stack tendremos abajo de todo `reverse("ello")+"H"`, luego `reverse("llo")+"e"`, luego `reverse("lo")+"l"`, `reverse("o")+"l"` y por último `reverse("")+"o"`. Esta última llamada retornará `""` de modo tal que luego retornaremos `"o"`, `"ol"`, `oll`, `olle` y `olleH` en último lugar.



## Palíndromos

Los palíndromos son aquellas palabras o frases que se leen igual en un sentido que en el otro. Lo que para los números se llama capicúa.

El mecanismo para verificar si un string un palíndromo consiste en mirar los caracteres del principio y del final y fijarnos si coinciden. En caso de hacerlo continuamos haciéndolo comparando los caracteres hacia el centro y continuamos esta comparación hasta llegar al mismo carácter en el caso de palabras de una cantidad de letras impares.

Si tuviéramos que desarrollar una función `isPalindrome` que retornara `true` cuando la palabra con la cual la llamamos es palíndromo, detendríamos la recursividad cuando el carácter sea de una o menos letras o cuando la palabra de muestras de que no es palíndromo.



```js
function isPalindrome(str){

  if(str.length<=1)
    return true

  
  if(str.charAt(0)===str.charAt(str.length-1)){
    return isPalindrome(str.slice(1,str.length-1))
  }
  return false
}

isPalindrome('aba')
```

> Notar la importancia del `return` antes de llamar recursivamente a `isPalindrome` de modo tal que cuando este retorne `true` ese valor sea retornado propagándose a través de todos los llamados que haya tenido dicha función.



En el ejemplo de `isPalindrome('abcxcba')` en el call stack abajo de todo tendremos: `isPalindrome('bcxcb')`, luego `isPalindrome('cxc')`, luego `isPalindrome('x')` que retornará `true` y se propagará como valor retornado de todas las llamadas.



## Conversión a Binario

Como sabemos la conversión a binario se efectúa realizando sucesivas divisiones por cero hasta que el resultado de la división sea menor a 2.

```js
function toBinary(num){
  if(num<2)
    return num;
  
  return toBinary(Math.floor(num/2))+(num%2).toString()
}

toBinary(17) //10001
```



Otra forma de hacerlo que nos evita la conversión a string es recibiendo un string `bin` que inicialmente será un string vacío.

```js
function toBinary(dec, bin){
  if(dec===0)
    return bin;
  
  bin = bin + (dec%2)
  
  return toBinary(Math.floor(dec/2),bin)
}

toBinary(17,"")
```

> En este último ejemplo vemos que una vez alcanzado el caso base el resultado se propagará por todos los stack frames y todos retornarán lo mismo.



## Suma de Números Naturales

Suponemos que tenemos el número `10` como entrada y queremos entregar una salida que sea `1+2+3+4+5+6+7+8+9+10``=55`



```
function sumOfNaturals(num){
  if(num<=1){
    return num
  }
  
  return num+sumOfNaturals(num-1);
}

sumOfNaturals(10)
```

> Notar que no pregunto por `1` sino por `<=1` ya que si me pasan `sumOfNaturals(0)` crashearía la aplicación ya que obtendría un **RangeError: Maximum call stack size exceeded**.



## Búsqueda Binaria

Completado hasta el minuto 38.
