# Introducción
> Basado en Code with Mosh - JavaScript for Beginners

## Características JavaScript:
* Es uno de los lenguajes más utilizados actualmente.
* Lenguaje ligero e interpretado.
* Lenguaje de **tipado débil** (una variable puede almacenar un string primero y luego un entero) y **dinámico** (al asignar un string es de tipo string y al almacenar un número será de tipo número) (más info a continuación).
* En realidad el lenguaje se llama ECMAscript y JavaScript sería como un dialecto.
* Elegido por Netflix, Wallmart, PayPal, etc.
* Lenguaje ligero e interpretado.
* Lenguaje de script.
* Lenguaje utilizado también sin navegador (Node.js -> Server Side).
* Es un lenguaje **multiparadigma**, ya que sSoporta distintos estilos de programación (declarativa o funcional, orientada a objetos, imperativa).
* Usado por Front-end developers, Back-end developers y Full-Stack developers (más info a continuación). 
* Los comandos ingresados en la consola son analizados por el **intérprete** que posee el navegador. (La consola se puede abrir con `F12` o `CTRL+SHIFT+J`). 

## Terminología
* `jQuery` es una librería de JavaScript que permite lograr un código más legible y simplifica el manejo del DOM, reduciendo así la curva de aprendizaje de JavaScript pero a expensas de un mayor tiempo de carga del sitio. 
* `Bootstrap` framwork de JavaScript, HTML y CSS.
* `React` librería para el desarrollo de interfaces de usuario.
* `NodeJS` entorno de ejecución para código JavaScript del lado del servidor.

## Editores Online
* https://codepen.io/
* https://codesandbox.io/
* https://jsfiddle.net/Es un **lenguaje dinámico** (más info a continuación)

### Front-end vs Back-end
Front-end representa la **capa de presentación** que interactúa con los  usuaurios mientras que el back-end es la **capa de acceso a datos** que procesa la entrada recibida por el Front-end.

### ECMAScript
ECMAScript es la especificación del lenguaje de programación publicada por ECMA International. Son los encargados de definir los standards del lenguaje. En 2015 salió la versión ECMAScript 6 (ES6).

### Script
El agregado del script podemos realizarlo entre las etiquetas `<head>` y `</head>` o bien entre `<body>` y `</body>`. Sin embargo, como buena práctica se recomienda agregarlo antes de la etiqueta de cierre de `</body>`. De esta manera primero renderiza los elementos y luego parsea el JavaScript mejorando así la experiencia de usuario ya que no verá la pantalla en blanco y no intentaremos referenciar elementos que todavía no fueron cargados.

### Web APIs
Las Web APIs nos permiten extender la funcionalidad del navegador, simplificar funciones complejas y proporcionar una sintaxis simple para trabajar con código complejo. 
* **EventTarget** nos permite recibir eventos y crear listeners para ellos.
* **HTMLInputElement** nos permite contar con métodos para manipular los elementos `<input>`.
* **DOM**: Nos permite manipular el HTML como por ejemplo con `document.getElementById`

### Lenguaje Dinámico
JavaScript es un lenguaje dinámico, entendiéndose como tal a aquellos que permiten que una vez definida una variable, esta pueda cambiar su tipo de manera dinámica. Es decir si hago `let name= 'Mosh'` el valor de `console.log(typeof name)` será `"string"` mientras que si luego hago `name=9` el valor devuelto será `"number"`. El tipo será determinado en tiempo de ejecución (*at runtime*). 

Otra característica de JavaScript relacionada con el tipado débil y dinámico es la **inferencia**:
* `"hola"+2`; inferirá que lo que quiero es concatenar dos strings mostrando como resultado `hola2`.

* `2+"2"-1;` el resultado será `21` ya que primero concatena formando `"22"` y luego le resta `1`y  asume que queríamos hacer una resta de números por eso lo convierte en `22 - 1`.

* `2 == "2"` el resultado será `true` ya que inferirá que debe convertir al `2` en un string.

> Antiguamente se agregaba `"use strict"` al comienzo del código para lograr que JavaScript sea más restrictivo y arroje más excepciones. Por ejemplo si utilizamos una variable sin desclararla. Esto hoy por hoy viene por defecto si usamos ES6 Modules.

JavaScript es un lenguaje débilmente tipado o *loosely typed language*

## Primeros pasos
Para comenzar a utilizar JavaScript podemos utilizar las DevTools e ingresar el código en al consola o bien crear un proyecto utilizando Visual Studio Code.
Creamos una carpeta con el nombre del proyecto y dentro de ella un archivo `index.html` y otro `script.js`

### Script
Existen distintas formas de trabajar con código JavaScript una de ellas es con un **script en línea** el código directamente entre los tags de apertura y cierre `<script></script>.`
Otra forma es con un **archivo externo**. En nuestro caso como hemos creado un archivo `script.js` es allí dondeJavaScript es un lenguaje débilmente tipado o *loosely typed language*

## Objetos
Cuando tenemos variables relacionadas las colocaremos el código y para eso referenciamos el archivo externo de esta forma:  `<script src="script.js></script>`. 
Una tercera forma es trabajando con **módulos** `<script type='module' src='script.js'></script>`. Esta es la forma más moderna y nos permitirá trabajar con **EcmaScript Modules**.

El agregado del script podemos realizarlo entre las etiquetas `<head>` y `</head>` o bien entre `<body>` y `</body>`. Sin embargo, como buena práctica se recomienda agregarlo antes de la etiqueta de cierre de `</body>`. De esta manera primero renderiza los elementos y luego parsea el JavaScript mejorando así la experiencia de usuario ya que no verá la pantalla en blanco y no intentaremos referenciar elementos que todavía no fueron cargados.

Sin embargo si queremos asegurarnos que el código JavaScript sea ejecutado cuando el DOM esté completamente cargado:

```jsx
document.addEventListener("DOMContentLoaded",()=>{
	//Código JavaScript que se ejecuta cuando el DOM está completamente cargado.
});
```

# Variables
Existen distintos tipos de variables: `var`, `let` y `const`. Siempre que sea posilbe debemos utilizar `const` y en aquellos casos donde tengamos que reasignar utilizar `let`.

* Se utiliza camelCase para definirlas
* Pueden empezar con `$` o  con `_` pero no con números ni `-`.
* Como buena práctica definir cada variable en una línea nueva y no varias juntas.

## `var`
* Admite redeclaración
* Admite reasignación
* Alcance global

## `let`
* No admite redeclaración
* Admite reasignación
* Alcance nivel bloque (*block scope*)

## `const`
* No admite redeclaración
* No admite reasignación (pero sí admite redefinición de componentes internos)
* Alcance nivel bloque (*block scope*)

 > En caso de utilizar herramientas como Babel lo convertirán finalmente a `var` para asegurarse que el código sea soportado por cualquier navegador.


# Tipos de Datos
En JavaScript una variable puede almacenar dos tipos de valores *primitive types* y *reference types*

## Tipos Primitivos
Los tipos primitivos almacenan el valor dentro de la variable y tenemos 6 tipos de primitivos. Los tipos primitivos son inmutables.
* boolean
* number
* string
* symbol
* undefined
* null

```jsx
let x = 10;
let y = x;
x++;
console.log(x); //11
console.log(y); //10
```


Cuando hacemos `let y = x;` estamos copiando el valor de la variable `x` en una variable `y`.

### Objetos
Cuando tenemos variables relacionadas las colocamos como propiedades de un objeto. En ella agrupamos pares key/value.
```jsx
const person = {
	name: 'Mosh',
	age: 30,
}
```

#como propiedades de un objeto. 
```jsx
const person = {
	name: 'Mosh',
	age: 30,
}
```

A pesar de que sea `const` podremos cambiar el valor de sus propiedades, lo que no podremos hacer es reasignarlo a un nuevo objeto.

### Acceso a propiedades de objeto:
* dot notation: `person.name = 'Juan'`
* bracket notation `person['name'] = 'Juan'`

Si bien *dot notation* es más conciso y debería ser nuestra opción por default, *bracket notation* posibilita el acceso a través de variables las cuales pueden ser definidas en tiempo de ejecución.

```jsx
let selection = 'name';
person[selection]='Juan';
```

### Arrays
#### Arrays con largo dinámico:
De la misma manera que el tipo de dato de una variable puede variar dinámicamente también puede hacerlo el largo de un array.
```jsx
let selectedColors = ['red','blue'];
selectedColors[2] = 'green';
```
No necesariamente deben ser todos los elementos del mismo tipo.

Un array es un objeto lo cual podemos verificar con `typeof selectedColors` y obtenemos `"object"`.

## Reference Types
Los *reference types* son objetos y estos no se almacenan en variables sino en memoria y es la dirección del objeto lo que va almacenado en variables. Los objetos son mutables.

* array
* object literal
* function

```jsx
let x = {value: 10}
let y = x;
x.value = 20;
console.log(x); // {value: 20}
console.log(y); // {value: 20}
```

Cuando hacemos `let y = x;` estamos copiando la dirección en memoria donde está almacenado el objeto.

> **Los tipos de referencia son mutables** y esto podemos verlo en el siguiente ejemplo:
```jsx
const list = [];
list.push(1);
console.log(list); //[1]
```
Cuando creamos un array este hereda una serie de métodos y uno de ellos es `push()` y nos permite agregar un nuevo elemento al array.
Si quisiéramos no mutar el array sino trabajar con una nueva copia podríamos utilizar el método `concat()` que nos devuelve un nuevo array.
```jsx
const arr = []
const anotherArr = arr.concat(2);
console.log(arr); //[]
console.log(anotherArr); //[2]
```

> Estamos trabajando con `const` y si bien las variables no pueden ser  reasignadas, sí son mutables.


## Value vs Reference con Variables Globales
Supongamos que tenemos una variable global con un cierto valor que queremos incrementar dentro de una función y luego mostrar el nuevo valor fuera de ella.

### Value Types
```jsx
let number = 10;

function increse(number){
	number++; //distinta variable
}

increase(number); //pasaje por valor
console.log(number); //10
```

### Reference Types
```jsx
let obj = {value: 10}

function increse(obj){
	obj.value++; //mismo objeto
}

increse(obj); //pasaje por referencia
console.log(obj.value); //11
```

# Funciones 
Existen distintas formas de definir una función. Una de ellas es utilizando **Function Declarations** la sintaxis básica y la otra es utilizando **Function Expressions** que es la sintaxis de función anónima (que permite la reducción convirtiéndolas en funciones flecha). 
La única diferencia entre utilizar una u otra está en lo que se conoce como **Hoisting**. En caso de utilizar **function declarations** podremos utilizarlas arriba de todo del código, mientras que si las definimos con **function expressions** sólo podremos utilizarla luego de crearla.


## Function Declaration
```jsx
function restar(a,b){
	return a-b;
}

console.log(restar(5,8));
```

## Function Expression
Se conoce como **Function Expression** cuando a una constante se le asigna una función.

### Funciones Flecha
Si tenemos una función que no recibe ningún parámetros:
```jsx
const foo = () => {}
```

Si tenemos una función que recibe un parámetro:
```jsx
const foo = item => {}
```

Si tenemos una función que recibe un parámetro y retorna ese valor duplicado:
```jsx
const foo = item => item*2
```

A pesar de que sea `const` podremos cambiar el valor de sus propiedades, lo que no podremos hacer es reasignarlo a un nuevo objeto.

## Parámetros y Argumentos
Un **argumento** es el valor que se le pasa a un **parámetro**.

### Parámetros
```jsx
function sum(a,b){
	return a+b;
}

console.log(sum(5,8));a+b);
}
```
En este caso `a` y `b` son los parámetros.
En cambio 5 y 8 son los argumentos.

También podríamos haber utilizado variables para invocar la función, por ejemplo:

```jsx
function sum(a,b){
	return a+b;
}

const op1 = 5;
const op2 = 8;
console.log(sum(op1,op2));
```
Notar que no hace falta que las variables que utilicemos como argumentos se llamen igual que los parámetros.
### Argumentos
```jsx
sum(5,8);
```

## Operadores Aritméticos
Además de `+`, `-`, `/`, `*`, `%` (resto) contamos con:

```jsx
let x = 2;
let y = 3;
console.log(x**y); // 2^3

x= 10;
console.log(++x); //11
console.log(x); //11


x= 10;
console.log(x++); //10 lo muestra y luego lo incrementa.
console.log(x); //11
```

## Operadores Comparación
```jsx
const name = 'shaun';
name == 'shaun' //true
name == 'Shaun' //false (como tengo una capitalización diferente no es verdadero)
name > 'Crystal' //true (S es mayor que C alfabéticamente)
name > 'Shaun' // true (lowerCase > upperCase en JavaScript)
```

##  `in` operator
El operador `in` nos permite chequear la existencia de una propiedad en un objeto.

```jsx
const circle = {
	radius: 1,
	draw(){
		console.log('Drawing');
	}
}

if('radius' in circle){
	console.log('yes');
}
```

## Equality Operators
Strict equality vs loose equality:
```jsx
'1'===1
//false (strict equality: deben coincidir tipo y valor)

'1'==1; 
//true (loose equality: basta con que coincida el valor)

true===1
//false

true==1
//true
```

> Notar que no ponemos `console.log()` por simplicidad y además si ingresáramos esos comandos en las Devtools nos aparecería el output sin la necesidad de hacer dicho log.

## Conversión Explícita e Implícita
```jsx
let score = '100';
console.log(score+1); //Obtendremos '1001' debido a la que realiza una concatenación.
score = Number(score);
console.log(score+1); //101
let result = 'Hello';
console.log(Number(result)); //NaN
```
Por un lado tenemos una **conversión implícita** que es la que ocurre cuando tenemos `'100'+1` y automáticamente el número `1` se convierte en un string.

Por otra parte tenemos la **conversión explícita** que tiene lugar cuando hacemos `Number('100')` y lo convierte a un número.

Es posible realizar distintos tipos de conversiones explícitas:
```jsx
console.log(String(50)); //'50'
console.log(Boolean(50)); //true
console.log(Boolean(0)); //false
```

## Assignment Operators
```jsx
let x=10;
x+=2; 	//x pasa a ser 12
x-=2; 	//x pasa a ser 10
x*=2 	//x pasa a ser 20
x/=2 	//x pasa a ser 10
```

## Ternary Operator
Nos permite resolver un condicional en una línea `condicion ? trueValue : falseValue`
```jsx
let pints = 100;
let type = points > 10 ? 'gold' : 'silver';
```

Esto mismo lo podemos implementar sin pasar por esa variable `type`:

```jsx
return (points > 10) ? 'gold' : 'silver';
```

## Logical Operators
```jsx
true && false 				//false
true || false 				//true
false || 'Juan'				//'Juan'
false || 1					//1
undefined || 'red' 			//'red'
```

En situaciones donde tenemos más de un operando vemos que el logical OR retorna el primer valor truthy de izquierda a derecha (lo cual se conoce como *short circuiting*)
```jsx
false || 'Juan' || 'Paco'	//'Juan' 
```

Un **ejemplo de implementación** sería si tenemos un `userColor` que puede o no estar definido y un `defaultColor`, en ese caso haríamos lo siguiente:
```jsx
const color = userColor || defaultColor;
```
El valor será `userColor` a menos que este sea `undefined` y en ese caso usará `defaultColor`.

## Bitwise Operator
El **bitwise operator** como su nombre lo indica, nos permite realizar operaciones a nivel bit.
```jsx
console.log(1|2); //3
//0000 0001
//0000 0010
//0000 0011

console.log(1&2); //0
//0000 0001
//0000 0010
//0000 0000
```

Un **ejemplo de implementación** sería si queremos establecer permisos de escritura, lectura y ejecución:
```jsx
const readPermission = 4; //0000 0100
const writePermission = 2; //0000 0010
const executePermission = 1; //0000 0001

0000 0100 lectura (4)
0000 0110 lectura y escritura (4+2)
0000 0111 lectura, escritura y ejecución. (4+2+1)

//Para asignar permisos de lectura, escritura y ejecución usamos Logical OR.
let myPermission = readPermission | writePermission | executePermission;

//Para saber si tiene un permiso utilizamos Logital AND
myPermisssion & readPermission
//Si nos da true es que lo tiene.
```

## Falsy vs Truthy
En JavaScript existen los valores **falsy** (no necesariamente se trata de un `false` booleano) y estos pueden ser estos 6:

* `undefined`
* `null`
* `0`
* `false`
* `''` (empty string)
* `NaN`

Por el contrario todo aquello que no es **falsy** es **truthy**


# `null` y `undefined`
Si queremos indicar explícitamente la falta de valor utilizamos `null` mientras que `undefined` es asignado por el navegador cuando una variable no fue definida.

Si definimos una variable y no le damos valor tendrá `undefined`
```jsx
const age;
console.log(`La edad es ${age}`),age, age+3;
//La edad es undefined 
//undefined
//NaN
```

Si queremos indicar explícitamente que algo no tiene valor le asignamos `null`
```jsx
const gender = null;
console.log(`El sexo es ${gender}`, gender, gender+3);
//El sexo es null
//null
//3
```

# `NaN`
Lo obtenemos cuando un cálculo no resulta un número.

Por ejemplo:
```jsx
5*'A'
5/'A'
Number('A')
undefined + 1
```

# Análisis con `typeof`
* `typeof` `null` = `"object"`
* `typeof` `array` = `"object"`
* `typeof` `undefined` =  `"undefined"`
* `typeof` `NaN` =  `"number"`

# Particularidades
```jsx
undefined + 1 //NaN
null + 1 //1
```

# Tipos de Datos
En JavaScript una variable peude almacenar dos tipos de valores *primitive types* y *reference types*
## Primitive/Value Types
Los *primitive types* almacenan el valor dentro de la variable y tenemos 6 tipos de primitivos:
* boolean
* number
* string
* symbol
* undefined
* null

```jsx
let x = 10;
let y = x;
x++;
console.log(x); //11
console.log(y); //10
```


Cuando hacemos `let y = x;` estamos copiando el valor de la variable `x` en una variable `y`.

## Reference Types
Los *reference types* son objetos y estos no se almacenan en variables sino en memoria y es la dirección del objeto lo que va almacenado en variables.

* array
* object literal
* function

```jsx
let x = {value: 10}
let y = x;
x.value = 20;
console.log(x); // {value: 20}
console.log(y); // {value: 20}
```

Cuando hacemos `let y = x;` estamos copiando la dirección en memoria donde está almacenado el objeto.

## Value vs Reference con Variables Globales
Supongamos que tenemos una variable global con un cierto valor que queremos incrementar dentro de una función y luego mostrar el nuevo valor fuera de ella.

## Value Types
```jsx
let number = 10;

function increse(number){
	number++; //distinta variable
}

increase(number); //pasaje por valor
console.log(number); //10
```

## Reference Types
```jsx
let obj = {value: 10}

function increse(obj){
	obj.value++; //mismo objeto
}

increse(obj); //pasaje por referencia
console.log(obj.value); //11
```

# Variables
Existen distintos tipos de variables: `var`, `let` y `const`.
* Se utiliza camelCase para definirlas
* Pueden empezar con `$` o  con `_` pero no con números ni `-`.
* Como buena práctica definir cada variable en una línea nueva y no varias juntas.

## `var`
* Admite redeclaración
* Admite reasignación
* Alcance global

## `let`
* No admite redeclaración
* Admite reasignación
* Alcance nivel bloque

## `const`
* No admite redeclaración
* No admite reasignación (pero sí admite redefinición de componentes internos)
* Alcance nivel bloque

# Funciones 
Existen distintas formas de definir una función. Una de ellas es usando la sintaxis básica y la otra usando la sintaxis de función anónima (que permite la reducción convirtiéndolas en funciones flecha). No hay ninguna diferencia entre usar una u otra forma.

## Funciones Flecha
Si tenemos una función que no recibe ningún parámetros:
```jsx
const foo = () => {}
```

Si tenemos una función que recibe un parámetro:
```jsx
const foo = item => {}
```

Si tenemos una función que recibe un parámetro y retorna ese valor duplicado:
```jsx
const foo = item => item*2
```

## Callback
Los callbacks son parte fundamental del código asincrónico o no bloqueante
Se trata de una función que podemos elegir el lugar o momento específico en que es ejecutada.
```jsx
function foo(item, callback){
	console.log(item);
	callback();
}
```

# Eventos
Existen distintos tipos de eventos asociados a funcionalidades por ejemplo eventos de click, eventos de teclado, etc.
```html
<button onClick="console.log('click')">Soy un botón</button>
```
Hemos colocado directamente  código JavaScript pero lo ideal es implementarlo en un archivo aparte `.js`:

```jsx
const btn = document.getElementByTagName('button');
btn.onClick = dos;
```

Como consecuencia de esto se ejecutaría la función `dos` al hacer click en el botón. En definitiva lo que estamos haciendo es registrar un callback que se ejecutará ante el click. Podemos simplificar este procedimiento haciendo uso de la web API Event Target.

## Con web API Event Target:
La Web API Event Target nos permite registrar eventos de `window`, `document` y elementos del DOM.

```jsx
const btn = document.getElementByTagName('button');
btn.addEventListener("click", dos);
```

### Objeto Evento (`e`)
El objeto Evento `e` nos permite obtener una serie de propiedades y métodos para realizar una  manipulación más avanzada.
```jsx
const btn = document.getElementByTagName('button');
btn.addEventListener("click", function(e){console.log(e)});
```

### Propagación de Eventos
La propagación de eventos o *event bubbling* consiste en se propagan desde un elemento hijo hacia los padres revisando si tienen un evento de ese tipo registrado y en caso afirmativo ejecuta el callback.

En ocasiones la propagación de eventos será muy beneficiosa, por ejemplo si creamos dinámicamente elementos (por ende no podríamos utilizar `getElementById` ni similares pues obtendríamos `null`) y colocamos un listener a nivel documento. Luego gracias al objeto evento `e.target.tagName` sabremos de qué elemento se trata.

### `e.preventDefault()`
`e.preventDefault()` nos permite cancelar el evento si este es cancelable por lo que la acción default no sucederá.

Esto podrá ser útil al trabajar con el evento submit cuando enviamos un formulario presionando el botón `<button type="submit">Enviar</button>`. En ese caso para evitar el comportamiento default que ocasiona un refresco de la pantalla debemos hacer `e.preventDefault()`.

Otro ejemplo donde podríamos utilizarlo sería si hacemos click en un link y no queremos dirigirnos a la URL indicada mediante el `href` (debemos colocar `e.preventDefault()` en el callback)

Por último otro ejemplo podría ser si quermeos que al hacer click sobre un checkbox este no se tilde y destilde sino que ignore nuestro click:
```js
document.getElementById("myCheckbox").addEventListener("click", function(event){  
	event.preventDefault()  
});
```

### Evento Redimensionar Pantalla
```jsx
window.addEventListener("resize",function(){

});
```

# Formularios
## Validación Elementos
Si bien la validación de los elementos que tengamos dentro de un `form` es controlada automáticamente al presionar el botón Submit, también es posible chequear manualmente la validación de los elementos que tengamos sueltos.

Utilizando la web API **HTMLInputElement** obtendremos métodos y propiedades para manipular elementos de tipo `input`, mientras que con **HTMLSelectInputElement** lo propio para elementos de tipo `select`.

El método `checkValidity()` retorna un booleando dependiendo de si cumple o no los requisitos de validación (especificados en el html, por ejemplo `required`) el campo en cuestión.

En caso de que no se cumpla la condición y queramos mostrar un mensaje personalizado, podremos configurarlo con el método `setCustomValidity("mensaje")`

## Limitaciones Validación
Debemos tener presente las limitaciones de esta validación ya que bastaría con editar el HTML en las DevTools quitando el `required` y la validación aún sin completar dicho campo sería exitosa. Esto hace que sea necesario realizar una validación personalizada, pero colocar los elementos HTML de validación no está de más.

> Para obtener el valor de un campo debemos utilizar la propiedad `value`

## Ingresos de Parámetros
```jsx
const nombre = prompt('Ingrese su Nombre');
```

## `encodeURIComponent`
La función `encodeURIComponent()` nos permite convertir caracteres especiales en caracteres que no sean peligrosos y que un usuario podría colocar dentro del `script` sin que esto represente un riesgo de ataque XSS.

```jsx
const str = "<script>";
const str2 = encodeURIComponent(str); //"%3Cscript%3E"
```

## Obtener código ASCII
```jsx
const name = "ASD"
name[0].charCodeAt(); //65
```

# Local Development Server
Con el objetivo de previsualizar nuestro código debemos implementar un *local development server* de modo tal que nuestra PC actúe como un servidor y no sea necesario subir el código a un webserver.

Utilizando la extensión de Visual Studio Code llamada **Live Server** Cuando ingresamos a `http://localhost:5500` efectuamos una serie de *requests* y  obtendremos como *response* los recursos solicitados para ver la página html, css, js, imágenes, etc.

# Garbage Collector
En C o C++ (lenguajes de bajo nivel) al crear un objeto debemos destinar memoria (*allocate memory*) y luego al finalizar tendremos que liberarla (*deallocate memory*). En JavaScript en cambio podemos crear un objeto y al inicializarlo la memoria es automáticamente designada, lo utilizamos y cuando terminamos de usarlo no es necesario liberar la memoria. El motor de JavaScript cuenta con un *garbage collector* que es el encargado de encontrar las variables ya no usadas y liberar automáticamente la memoria que estaba destinada a ellas. 