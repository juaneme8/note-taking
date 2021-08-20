# RegEx
> Basado en:
> * [The Net Ninja - RegEx](https://www.youtube.com/playlist?list=PL4cUxeGkcC9g6m_6Sld9Q4jzqdqHd2HiD)
> * Code with Mosh - The Complete Node.js Course (Video 86)
> * [FalconMasters - Guía Fácil y Completa de Expresiones Regulares desde Cero](https://www.youtube.com/watch?v=wfogZfIS03U)
> * [freeCodeCamp.org - Regular Expressions (Regex) in JavaScript](https://www.youtube.com/watch?v=909NfO1St0A&t=1s)

## Introducción:
Las **expresiones regulares** (regular expressions) son fórmulas a través de las cuales vamos configuramos un patrón de búsqueda en texto y de esta manera podremos *validar información*. 

## Características:
* Nos permiten no tener que hacer funciones de validación manualmente.
* Existen muchas formas de escribir una misma expresión regular.
* Son universales y sirven para cualquier lenguaje.
* Se trata de un string que comienza y termina con `/`


## Primera coincidencia
`/juanocho/`

## Múltiples coincidencias
Utilizando el flag global `g`
`/juanocho/g`

## Coincidencia al principio
`/^juanocho/`
Si tenemos un documento multilínea y queremos que `^` busque al comienzo de cada línea (y `$` al final de cada línea) debemos agregar la bandera `\m`

## Coincidencia al final
`/juanocho$/`

## Coincidencia al principio y al final
`/^juanocho$/`

Esta combinación es muy utilizada a la hora de validar formularios ya que así nos aseguramos que la totalidad del texto cumple la condición y no solo una parte.

## Case insensitive
`/juanocho/i`
Habrá match independientemente de las mayúsculas o minússculas (juanocho, JuanOcho, JUANOCHO, etc).

## Conjunto de caracteres
 `/[ab.?/]juanocho/`
Habrá match para `ajuanocho`, `bjuanocho`,`.juanocho`, `?juanocho`, `/juanocho` 

1) Notar que en este caso no hace falta escapar a los caracteres especiales.

2) Notar que el contenido dentro de los corchetes representa **un único caracter**, por lo que si yo ingreso `abjuanocho` el match estará dado por `bjuanocho`, a menos que ponga `/[ab]+juanocho/` con el + represento "1 o +" apariciones de esos caracteres, por lo que matchearán `abjuanocho` , `bajuanocho` 

## Conjunto de caracteres excluídos
`/[^xyz]juanocho/` Si quiero que el primer caracter no sea ninguno de los indicados.

## Rango de caracteres
Si quiero cualquier letra en minúscula puedo ponerlo `/[abcd....z]/` o bien especificando su rango `/[a-z]/`
`/[a-z]juanocho/` acepto cualquier letra en minúscula antes de "juanocho"
`/[a-zA-z]juanocho/` acepto cualquier letra antes de "juanocho"
`/[0-9]juanocho/` acepto cualquier número antes de "juanocho"
`/[a-zA-z0-9]juanocho/`  acepto cualquier letra o número antes de "juanocho"

## Repetición caracteres:
Si quiero obtener un match cuando haya 5 números seguidos, una forma de expresarlo es la siguiente:
`/[0-9][0-9][0-9][0-9][0-9]/`

Otra forma:
`/[0-9]{5}/`

Si quiero que haya como mínimo 5 números:
`/[0-9]{5,}/`

Si quiero que haya como mínimo 5 y como máximo 8:
`/[0-9]{5,8}/`
Notar que si la secuencia es `123456789` el match será `12345678` y si tuvieramos el flag `/g` y la secuencia es `1234567890123456789` tendríamos un match de `12345678` y otro de `90123456`.

Si quiero tener **1 o más** números:
`/[0-9]+/`
`/\d+/`

Si quiero tener **0 o más** números:
`/[0-9]*/`
`/\d*/`

Si quiero tener **0 o más** caracteres antes y después de una cadena: `/.*juanocho.*/`

## Caracter de alternación
Nos permite matchear una regEx individual de un conjunto de regEx, por ejemplo:
`p|t` matchea con `p` o con `t`
`p|tyre` matchea con `p` o con `tyre`
`/(p|t)yre/` matchea con `pyre` o `type` 
`cat|dog|mouse|fish` matchea con `cat`, `dog`, `mouse` y `fish`.

## Grupos
Los establecemos con `( )` y nos permitirán encerrar porciones de la expresión regular para luego extraer en el lenguaje de programación las coincidencias de una en una.
Al grupo también le podemos agregar cuantificadores, si tenemos por ejemplo que detectar un teléfono de la siguiente forma `449 45 67` podríamos poner: `\d{3}(\s\d{2}){2}`

## Metacaracteres
* `\d` dígito es lo mismo que poner `[0-9]`
* `\D` no dígito
* `\w` word (letras, números, _ ):
> Para matchear todas las palabras: `\w+`
> Para matchear palabras que contengan la cadena work: `\b\w*work\w*\b`
* `\W` todo lo que no sea word
* `\s` caracter en blanco (espacio, tabulación o nueva línea)
* `\S` todo lo que no sea caracter en blanco
* `\n` new line
* `\t` caracter de tab
* `\b` al comienzo o final de una palabra: 
>`\bocho` con `ocho` habrá match pero con `juanocho` no. 
>`ocho\b` con `ocho` habrá match y con `juanocho` también. 
* `\B` no al comienzo o final de una palabra
* `{N}` cantidad exacta de caracteres.
* `{N,M}` mínimo y máximo de caracteres.

## Páginas de Prueba
Al trabajar con expresiones regulares es conveniente hacerlo con estas páginas
* [https://regex101.com/](https://regex101.com/)
* [https://regexr.com/](https://regexr.com/)

==Regla Mnemotécnica: el string matchea==

## Caracteres especiales
* `.` cualquier caracter (excepto nueva línea)
* `*` 0 o + caracteres
* `+` 1 o + caracteres
* `?` 0 o 1 caracter
* `\` caracter de escape
* `[]` set de caracteres
* `[^]` símbolo de negación en un set de caracteres

Si queremos buscar alguno de estos caracteres tendremos que colocar el caracter de escape: `/\*/` a menos que estemos dentro de un set. 
Si queremos buscar todas las palabras que tengan un asterisco al inicio y otro al final por ejemplo: `*pattern*` podremos hacer `\*\w+\*` veremos que `**` no matchea y `*q*` sí.

## Ejemplos de validaciones
* Validar nombre de usuario con 5 a 12 caracteres alfanuméricos.
`/^[a-zA-Z0-9]{5,12}$/` si tuviéramos en vez de un único nombre,  una lista de nombres de usuario agregaríamos los flags `g` para encontrar más de una coincidencia y `m` para que el `^` y `$` tomaran como comienzo y fin el principio y el fin respectivamente **de cada línea**.

* Validar passwords que a su vez puede contener `letras`, `números`, `@`, `-` , `_` y debe tener entre 8 y 20 caracteres.
`^[a-zA-Z0-9@_-]{8,20}$` notar que el guion medio lo ponemos a lo ultimo sino lo toma como un rango. Una forma más corta `^[\w@-]{8,20}$`

* Validar teléfono de 11 dígitos `/^\d{11}$/`

* Buscar teléfonos con el siguiente formato:
`449 123 45 67` 
`/\d{3} \d{3} \d{2} \d{3}/` colocando los espacios de manera visual o bien utilizando `\s` para ellos: `/\d{3}\s\d{3}\s\d{2}\s\d{3}/`

* Buscar teléfonos con los siguientes formato:
`449 123 45 67` o `448-234-56-78`

**método 1:** utilizando el `.` para indicar cualquier caracter:
`/\d{3}.\d{3}.\d{2}.\d{3}/` 

**método 2**: utilizando el pipe `(espacio | guión)` para indicar que me sirven ambas cosas: 
`\d{3}( |-)\d{3}( |-)\d{2}( |-)\d{2}`

**método 3** utilizando conjuntos de caracteres [espacio guión]:
`\d{3}[ -]\d{3}[ -]\d{2}[ -]\d{2}`
`\d{3}[\s-]\d{3}[\s-]\d{2}[\s-]\d{2}`

* Buscar los nombre y apellidos en un archivo con el siguiente formato:
```
Carlos Arturo	
449 123 45 67
carlos_@hotmail.com
www.carlos.com
```
`/^\w+\s\w+$/gm`
`^` debido a que queremos que empiece con un caracter de palabra `\w`de 1 o mas caracteres `+`, queremos que haya un espacio `\s` y luego queremos que termine `$` con otra palabra `\w` de uno o mas caracteres `+`.
Es importante contar con las banderas `\gm` ya que con global logramos encontrar más de un nombre y con multiline logramos que al poner `^` busque al comienzo de cada línea y no sólo al comienzo del texto.

* Buscar los nombres y apellidos pero suponiendo que uno de ellos tiene una tabulación luego del apellido:
```
Carlos Arturo TABULACION
449 123 45 67
carlos_@hotmail.com
www.carlos.com
```
`/^\w+\s\w+\s?$/gm` con `\s?` indicamos que puede haber 0 o 1 caracter de espacio luego del apellido. También podríamos hacerlo con `/^\w+\s\w+\t?$/gm` 

* Buscar los nombres y apellidos pero suponiendo que uno de ellos tiene sólo nombre:
```
Carlos
449 123 45 67
carlos_@hotmail.com
www.carlos.com
```
`/^\w+\s?\w+\s?$/gm`

* Buscar los correos electrónicos:
```
Carlos
449 123 45 67
carlos_@hotmail.com
www.carlos.com
```

`\w+@\w+\.[a-zA-Z]` no lo hacemos con `\w+@\w+\.\w` dado que eso admitiría correos electrónicos con guión bajo por ejemplo: `juan@hotmail.__`

* Buscar los correos electrónicos sabiendo que estos pueden incluir puntos, guiones bajos, o guiones en la parte del nombre de usuario y también guiones en la parte del dominio:

 ```
Juan
449 123 45 67
car.los_@hotmail-arg.com
www.carlos.com
 ```

`[a-zA-Z0-9_.-]+@[a-zA-Z0-9_.-]+\.[a-zA-Z]+`

Si bien con esta expresión matchearíamos correos terminados en .com.ar por ejemplo, puede que queramos almacenar en una variable cada una de las partes del correo (nombre@host.com.ar) por lo que en ese caso tendríamos que usar grouping. `/^([\w.-]+)@([\w.-]+)\.([a-zA-Z]+)([.a-zA-Z]+)?$/gm`


* Buscar las direcciones de páginas web
`^(https?:\/\/)?(www.)?\w+\.\w+\/?$` 
Queremos capturar los siguientes tipos:
`https://regexr.com/`
 `http://regexr.com/`
 `regexr.com`

## Testear expresiones en JavaScript:
* Para testear la validez de una expresión regular podemos usar el método `test()` que entregará entre `true` o `false` de acuerdo al resultado del match.
```js
const  string  =  'mi nombre es Juan, mi usuario JuanOcho';
const  regex  =  /\w*juan\w*/gi;
const match = regex.test(string) //true
```
==Regla Mnemotécnica: testeo la regEx==

* El método `match()` se usa para obtener todas las ocurrencias de una _expresión regular_ dentro de una cadena, en caso de que no haya ninguna devolverá `null`.
 El método `search()` devuelve el índice de la primera ocurrencia o -1 en caso de que no haya ninguna.
```js
const  string  =  'mi nombre es Juan, mi usuario JuanOcho';
const  regex  =  /\w*juan\w*/gi;
const match = string.match(regex); //[ 'Juan', 'JuanOcho' ]
const position = string.search(regex); //13
```
## Validación formularios JavaScript
Cuando trabajamos con formularios con múltiples validaciones podemos almacenar todas las regEx en un objeto donde cada una de ellas es una propiedad.
```js
const patterns = {
	campo1: /pattern/,
	campo2: /pattern2//
}
```

En Vainilla JS para validar cada uno de sus campos debo agregar un EventListener a cada input.

```js
const inputs = document.querySelectorAll('input');

inputs.forEach(input => {
	input.addEventListener('keyup',(e) => {
		//Obtengo el nombre de cada input
		console.log(e.target.attributes.name.value);
		//Como el nombre de la propiedad en patterns coincide
		//con el atributo name del input:
		validate(e.target, patterns[e.target.attributes.name.value]);
	});
});

function validate(field,regex){
	if(regex.test(field.value))
		field.className = 'valid';
	else
		field.className = 'invalid';
}

```

## Extraer Datos en JavaScript:
Para lograr esto como ya adelantamos haremos uso del *grouping* `( )` dentro de la expresión regular y del método `exec()` en JavaScript.

Supongamos que tenemos en un string `nombreArchivo.extension` y queremos almacenar en una variable el nombre y en otra la extensión. Conformamos la expresión regular `/(\w+)\.(\w+)/` con dos grupos y al ejecutar `const  match  = regex.exec(string)` almacenará el `match[0]` la cadena que pasa la prueba (en este caso todo el nombre del archivo) y en  `match[1]` y `match[2]` las subcadenas determinadas por esos grupos.

==Regla Mnemotécnica: ejecuto la regEx==
```js
const  string  =  'file1.mp3';
const  regex  =  /(\w+)\.(\w+)/;
const  match  =  regex.exec(string);
  
if (match) {
	const  nombre  =  match[1];
	const  extension  =  match[2];
	console.log(nombre); //file1
	console.log(extension); //mp3
} else {
	console.log('formato incorrecto');
}
```
Supongamos ahora que queremos hacer lo mismo pero tenemos una lista de archivos.
Utilizamos la misma expresión pero en este caso debemos agregar la bandera global `g` ya que queremos buscar varias coincidencias.
Tendremos que repetir esto para cada matcheo, es decir para cada archivo por lo cual no ejecutamos `regex.exec(string)` una vez sino tantas como coincidencias haya.

```js
const  string  =  'file1.mp3 file2.mp3 index.html illustration.ai';
const  regex  =  /(\w+)\.(\w+)/g;

let  match  =  regex.exec(string);
while (match) {
	const  nombre  =  match[1];
	const  extension  =  match[2];
	console.log(match[1]); //es el nombre
	console.log(match[2]); //es la extension
	match  =  regex.exec(string);
}
```