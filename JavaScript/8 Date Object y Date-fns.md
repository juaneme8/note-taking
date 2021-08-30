# Date Object

## Creación de una fecha

```jsx
const now = new Date();
console.log(now) // 1.  Thu May 20 2021 19:51:24 GMT-0300 (hora estándar de Argentina)
```
Cuando escribimos  `new Date()` en Visual Studio Code veremos que admite distintos valores como argumentos:

*  `string` por ejemplo `new Date("February 1 2019 07:30:59");` o `new Date('Thu May 20 2021 19:51:24 GMT-0300 (hora estándar de Argentina)');`

*  `number` (mseg desde 1970 que obtenemos con `getTime()`) `new Date(1622813227890)`, 

* `year`, `month` (de 0 a 11), `date?`, `hours?`, `minutes?`, `seconds?`, `ms?`

> Los parámetros que pusimos con `?` son opcionales.

```jsx
now //Thu May 20 2021 19:51:24 GMT-0300 (hora estándar de Argentina)
now.toDateString() //Thu May 20 2021
now.toTimeString() //19:51:24 GMT-0300 (hora estándar de Argentina)
now.toISOString() //'2021-05-20T22:51:24.000Z'

now.toLocaleString() //20/5/2021 19:51:24
now.toLocaleDateString() //20/5/2021
now.toLocaleTimeString() //19:51:24

now.getFullYear() //2021
now.getMonth() //4 (0 based -> Mayo)
now.getDate() //20 (día del mes)
now.getDay() //4 (0 es domingo, 4 es Jueves)

now.setFullYear(2022);
...

now.getHours() //19
now.getMinutes() //51
now.getSeconds() //24
now.getMilliseconds()

now.getTime() //mseg desde el 01/01/1970
```

## Diferencia entre fechas
A continuación mostramos una forma de obtener la diferencia entre dos fechas sin usar ninguna librería externa. Más adelante veremos que con el uso de `date-fns` esto se simplifica notablemente.
```jsx
const now = new Date();
const before = new Date("August 2 2019 03:28:00");

const diff = now.getTime() - before.getTime();

const mins = Math.round(diff / 1000 / 60);
const hours = Math.round(mins / 60);
const days = Math.round(hours / 24);
console.log(days);
```


# `date-fns`
Es una librería de JavaScript que nos permite manipular fechas de manera simple. Tiene la ventaja respecto de alternativas similares (como ser **Moment.js**) de que trabaja con el objeto `Date` nativo y lo manipula mediante funciones. En esta librería en vez de incorporar todas las funciones es posible sólo incorporar solo aquellas que vamos a utilizar.

A la hora de importarla utilizamos ECMAScript Modules.

> A la hora de probar los snippets de código se utilizó **RunJS** gracias a lo que veremos el output sin la necesidad de hacer `console.log()`

```jsx
import  { isToday }  from  'date-fns'

const now = new Date();
isToday(now); //true
```

## Formateo
```jsx
import { format} from 'date-fns'

const now = new Date();
format(now, 'yyyy'); //'2021'
format(now, 'd MMMM yyyy'); //'4 June 2021'
```

## Comparación de fechas
Si queremos saber el tiempo transcurrido desde una fecha dada.

```jsx
import { formatDistance } from 'date-fns'

const before = new Date('August 2 2019 02:26:00');
const now = new Date();
formatDistance(before,now) //'almost 2 years'
formatDistance(before,now, {addSuffix: true}) //'almost 2 years ago'
formatDistance(now,before, {addSuffix: true}) //'in almost 2 years'
```
> Tener en cuenta que el código fue ejecutado en Junio 2021.


## Fechas Relativas
Si tenemos una determinada fecha y queremos sumarle días, meses o años podemos utilizar `addDays`, `addMonths` o `addYears` respectivamente. En caso de querer restar podemos usar `subDays`, `subMonths` o `subYears`
```jsx
import { addDays, subDays } from "date-fns";

const now = new Date();
subDays(now, 1); //2021-06-03T11:01:56.254Z
addDays(now, 1); //2021-06-05T11:01:56.255Z
```

Si queremos sumar un valor más complejo podemos utilizar la función `add` que como segundo parámetro recibe un objeto con las características de la suma que queremos realizar:

```jsx
import { add } from "date-fns";

const date = new Date(2014, 8, 1, 10, 19, 50);
console.log(date);
//=> Mon Sep 01 2014 10:19:50 GMT-0300 (hora estándar de Argentina)

// Add the following duration to 1 September 2014, 10:19:50
const result = add(date, {
	years: 2,
	months: 9,
	weeks: 1,
	days: 7,
	hours: 5,
	minutes: 9,
	seconds: 30
});
console.log(result);
//=> Thu Jun 15 2017 15:29:20
//Thu Jun 15 2017 15:29:20 GMT-0300 (hora estándar de Argentina)
```

## I18n
I18n es la abreviatura que se suele usar para referirse a la **internationalization**, el nombre proviene de la cantidad de letras entre la i y la n.
```jsx
import  { formatRelative, subDays }  from  'date-fns'
import  { es}  from  'date-fns/locale'

formatRelative(subDays(new  Date(),  3),  new  Date())  //=> 'last Tuesday at 8:31 AM'
formatRelative(subDays(new  Date(),  3),  new  Date(),  { locale: es })  //=> 'el martes pasado a las 08:31'
```

# Moment.js
Moment.js se define como una librería para parsear, validar, manipular y mostrar fechas en JavaScript. **Los mismo autores recomiendan utilizar alternativas  más modernas a la hora de comenzar un nuevo proyecto.**