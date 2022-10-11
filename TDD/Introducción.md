# Introducción

> Basado en el [video](https://youtu.be/_t9l2TwGioc) de midudev

El TDD (Test-Driven Development) es una técnica de desarrollo de software guíado por tests.

En el paradigma tradicional de desarrollo de aplicaciones nos encontramos con las etapas de Design - Code - Test que se repiten cíclicamente, es decir se diseña una funcionalidad, se la implementa y luego se la testea. En el paradigma TDD lo que haremos será **Design - Test - Code** es decir que primero escribiremos el test y luego el código.

La creación del código antes de hacer los tests tiene algunos potenciales inconvenientes. Por un lado puede que directamente terminemos no haciendo tests o que en caso de hacerlos no estemos probando los distintos *corner cases* de la aplicación y caigamos en el *happy path*.

El **flujo de trabajo** debe **comenzar creando UN test que falle** (rojo), **luego hacer la implementación del código de producción solo para que pase ESE test** (verde) y a partir de allí **realizar las refactorizaciones** del código sabiendo que estará testeada dicha funcionalidad.



## Ventajas del TDD

Utilizando TDD tendremos una **mayor fiabilidad** del código teniendo la certeza de que el código funcionará. Además tendremos un **mejor diseño** separando mejor los distintos casos a cubrir, tendremos un **mejor code coverage**, **depuración más rápida** del código (pues ante una falla probablemente habrá un test que esté fallando), al realizar tests tendremos realizada la **documentación**.

 Si bien al aplicar TDD comenzaremos más lento, debemos entender al desarrollo como algo que nunca termina por lo que será una inversión a futuro pues acelerará el agregado de nuevas funcionalidades.



# Configuración

## Testing Framework

Utilziaremos Vitest como testing framework (alternativa a Jest, Mocha, Chai, etc) que ofrece una buena experiencia de desarrollo.

```
mkdir aprende-tdd
cd aprende-tdd
npm init -y
npm install -D vitest
```



## Configuración ESLint

Utilizaremos el conjunto de reglas del ESLint de standard.

```
npm install -D standard
```



Luego en el `package.json` agregamos lo siguiente:

```
"eslintConfig":{
	"extends": "./node_modules/standard/eslintrc.json"
} 
```



## Creación de Scripts

En la [documentación](https://vitest.dev/) vemos los tests que debemos agregar en el `package.json`

```json
"scripts": {
    "test": "vitest",
    "coverage": "vitest run --coverage"
}
```

> Al ejecutar `npm run test` se ejecutarán los tests e implementará un watch por lo que estará a la espera de cambios.



## ECMAScript Modules

Para poder utilizar imports o lo que es lo mismo trabajar con ECMAScript Modules debemos modificar el `package.json` agregando lo siguiente:

```
"type": "module"
```

 Gracias a esto podremos hacer `import {describe} from 'vitest'`



# Creación de Tests

Creamos en la raíz una carpeta `test`.

## Test FizzBuzz

### Enunciado

Debemos escribir una **función** que al pasarle un número:

- Muestre "fizz" si es múltiplo de 3.
- Muestre "buzz" si es múltiplo de 5.
- Muestre "fizzbuzz" si es múltiplo de 3 y 5.
- Muestra el número si no es múltiplo de ninguno de los anteriores.



### Creación del test

Creamos en la carpeta `test` un archivo llamado `fizzbuzz.test.js`

Si ejecutamos ahora `npm run test` veremos que falla y nos aparece un mensaje **"No test suite found in file ...fizzbuzz.test.js"**.

```
import {describe} from 'vitest'

describe('fizzbuzz', () => {

})
```

Ahora cambiará el error y pasará a ser **"No test found in suite fizzbuzz"**.



* Como primer test debemos chequear que fizzbuzz sea una función y esto por mas evidente que sea nos posibilitará iniciar en rojo como hemos dicho anteriormente:

```javascript
import { describe, expect, it } from "vitest";

describe('fizzbuzz', () => {
  it('should be a function', () => {
    expect(typeof fizzbuzz).toBe('function')
  })
})
```

> Notar que `typeof` siempre **retorna un string**.



También podríamos haberlo hecho con `expect(fizzbuzz).toBeTypeOf('function')`

```
it('should be a function 2', () => {
    expect(fizzbuzz).toBeTypeOf('function')
  })
```



En este momento obtendremos **AssertionError  4| : expected 'undefined' to be 'function'**. Creamos esta función y vemos que el test pasa a verde.



```javascript
import { describe, expect, it } from 'vitest'

const fizzbuzz = (n) => {
}

describe('fizzbuzz', () => {
  it('should be a function', () => {
    expect(typeof fizzbuzz).toBe('function')
  })
})

```



* Debemos tirar un error si no se proporciona un número como parámetro.

  ```javascript
  import { describe, expect, it } from 'vitest'
  
  const fizzbuzz = (num) => {
    if (typeof num !== 'number') throw new Error('parameter must be a number')
  }
  
  describe('fizzbuzz', () => {
    it('should throw if no number is provided', () => {
      expect(() => fizzbuzz()).toThrow()
    })
    it('should throw specific message if no number is provided', () => {
      expect(() => fizzbuzz()).toThrow(/number/)
    })
  })s
  
  ```
  
  > :rotating_light: Notar que usamos `() => fizzbuzz()` y no `fizzbuzz()` porque queremos ver el throw.
  
  

  > Notar que hicimos dos tests uno que pasaría en la medida que tiremos un error y otro que pasaría en la medida que tiremos un error y su mensaje incluya la regex /number/

 

* Debemos tirar un error si se proporciona un `NaN` como parámetro. Tener en cuenta que a como tenemos la función en este momento como`typeof NaN` es `number` la función no tiraría error pues consideraría que el tipo recibido es correcto.

```javascript
const fizzbuzz = (num) => {
  if (typeof num !== 'number') throw new Error('parameter must be a number')
  if (Number.isNaN(num)) throw new Error('parameter can not be NaN')
}

describe('fizzbuzz', () => {
  it('should throw specific message if NaN is provided', () => {
    expect(() => fizzbuzz(NaN)).toThrow(/NaN/)
  })
})
```



* Debemos retornar el número recibido si este no es múltiplo de 3 ni de 5. 

  Lo primero que podríamos hacer es esperar un 1 si le pasamos un 1. Ese test fallaría y luego si retornamos un 1 hardcodeado pasaría. Luego esperamos un 2 si le pasamos un 2 y esto nos lleva a pensar que tenemos que retornar `num` siempre que no sea un número múltiplo.

```javascript
const fizzbuzz = (num) => {
  if (typeof num !== 'number') throw new Error('parameter must be a number')
  if (Number.isNaN(num)) throw new Error('parameter can not be NaN')
  return num
}

  // it('should return 1 if number provided is 1', () => {
  //   expect(fizzbuzz(1)).toBe(1)
  // })

  // it('should return 2 if number provided is 2', () => {
  //   expect(fizzbuzz(2)).toBe(2)
  // })

  it('should return number if number its not multiple of 3 nor 5', () => {
    expect(fizzbuzz(1)).toBe(1)
    expect(fizzbuzz(2)).toBe(2)
    expect(fizzbuzz(4)).toBe(4)
  })
})
```



:rotating_light: **Apagar Tests**

Es aconsejable borrar o comentar los tests redundantes (por ejemplo no tiene sentido seguir chequeando que sea una función cuando lo estamos verificando en los sucesivos tests).



* Si el número es múltiplo de 3 debemos retornar 'fizz'. 

De manera similar a lo anterior el primer test que hacemos es que si recibimos 3 retornemos 'fizz'. Nuevamente hacemos una solución particular `if (number === 3) return 'fizz'` y luego 

```javascript
import { describe, expect, it } from 'vitest'

const fizzbuzz = (num) => {
  if (typeof num !== 'number') throw new Error('parameter must be a number')
  if (Number.isNaN(num)) throw new Error('parameter can not be NaN')
  if (num % 3 === 0) return 'fizz'
  return num
}

describe('fizzbuzz', () => {
  it('should return "fizz" if number provided is 3', () => {
    expect(fizzbuzz(3)).toBe('fizz')
  })
  it('should return "fizz" if number provided is multiple of 3', () => {
    expect(fizzbuzz(6)).toBe('fizz')
    expect(fizzbuzz(9)).toBe('fizz')
    expect(fizzbuzz(12)).toBe('fizz')
  })
})

```



* Si el número es múltiplo de 5 debemos retornar 'buzz'.

```javascript
import { describe, expect, it } from 'vitest'

const fizzbuzz = (num) => {
  if (typeof num !== 'number') throw new Error('parameter must be a number')
  if (Number.isNaN(num)) throw new Error('parameter can not be NaN')
  if (num % 3 === 0) return 'fizz'
  if (num % 5 === 0) return 'buzz'
  return num
}

describe('fizzbuzz', () => {
  it('should return "buzz" if number provided is 5', () => {
    expect(fizzbuzz(5)).toBe('buzz')
  })
  it('should return "buzz" if number provided is multiple of 5', () => {
    expect(fizzbuzz(5)).toBe('buzz')
  })
})

```



* Si el número es múltiplo de 3 y de 5 debe retornar "fizzbuzz"

```javascript
const fizzbuzz = (num) => {
  if (typeof num !== 'number') throw new Error('parameter must be a number')
  if (Number.isNaN(num)) throw new Error('parameter can not be NaN')
  if (num % 15 === 0) return 'fizzbuzz'
  if (num % 3 === 0) return 'fizz'
  if (num % 5 === 0) return 'buzz'
  return num
}

describe('fizzbuzz', () => {
  // it('should return "fizzbuzz" if number provided is 15', () => {
  //   expect(fizzbuzz(15)).toBe('fizzbuzz')
  // })
  it('should return "fizzbuzz" if number provided is multiple of 3 and 5', () => {
    expect(fizzbuzz(15)).toBe('fizzbuzz')
    expect(fizzbuzz(45)).toBe('fizzbuzz')
  })
})
```



## Refactor

Una de las características principales del TDD y el uso de tests en general es que nos permite realizar refactorizaciones con la certeza de que notaremos si algo deja de funcionar.

En este caso como la implementación  con ifs "no escala", sería difícil si tenemos que agregar más casos. Por ejemplo si nos piden que en caso de recibir un 7 enviemos un "woof". 

```javascript
export const fizzbuzz = (num) => {
  if (typeof num !== 'number') throw new Error('parameter must be a number')
  if (Number.isNaN(num)) throw new Error('parameter can not be NaN')

  const values = {
    fizz: 3,
    buzz: 5
  }

  let output = ''
  Object
    .entries(values)
    .forEach(([key, value]) => {
      if (num % value === 0) output += key
    })

  return output === '' ? num : output
}

```

> :thinking: Recordar que dentro de un `forEach` no podríamos poner un `return` como sí haríamos en un for normal.
>
> E e caso de "fizzbuzz" estamos concatenando strings en output cuando la entrada sea 15 primero agregamos fizz y luego buzz.



## Extracción a Archivo

Creamos una carpeta `src` y dentro de ella un archivo `fizzbuzz` donde pegamos la función desarrollada y luego la importamos en el test.



# Test AdventJS 23 - canReconfigure

Realizaremos ahora el TDD para la [kata de AdventJS](https://adventjs.dev/challenges/23)



## Enunciado

Estamos en la fábrica de Santa Claus 🎅 creando regalos.

La configuración de las máquinas es un **string**. Podemos reconfigurarla para que haga otro regalo y, para ello, podemos cambiar cada carácter por otro.

Pero **tiene limitaciones **: al reemplazar el carácter se debe mantener el orden, no se puede asignar al mismo carácter a dos letras distintas (pero sí a si mismo) y la longitud del string debe ser el mismo.

Necesitamos **una función que nos diga si podemos reconfigurar una máquina para que de un regalo pueda pasar a fabricar otro según las reglas** mencionadas. Lo mejor es que veamos un ejemplo:

```js
const from = 'BAL'
const to   = 'LIB'
const canReconfigure(from, to) // true
/* la transformación sería así:
B -> L
A -> I
L -> B
*/

const from = 'CON'
const to   = 'JUU'
const canReconfigure(from, to) // false
/* no se puede hacer la transformación:
C -> J
O -> U
N -> FALLO
*/

const from = 'XBOX'
const to   = 'XXBO'
const canReconfigure(from, to) // false
/* no se puede hacer la transformación:
X -> X
B -> X (FALLO, no mantiene el orden de transformación y la B no puede asignarse a la X que ya se asignó a otra) 
O -> B
X -> O (FALLO, la X no puede asignarse a la O que ya se asignó a la X)
*/

const from = 'XBOX'
const to   = 'XOBX'
const canReconfigure(from, to) // true

const from = 'MMM'
const to   = 'MID'
cons canReconfigure(from, to) // false
/* no se puede hacer la transformación:
M -> M (BIEN, asigna el mismo carácter a si mismo)
M -> I (FALLO, asigna el mismo carácter a dos letras distintas)
M -> D (FALLO, asigna el mismo carácter a dos letras distintas)
*/

const from = 'AA'
const to   = 'MID'
cons canReconfigure(from, to) // false -> no tiene la misma longitud
```



## Creación del test

Creamos un archivo en test llamado `can-reconfigure.test.js`

* Nos fijamos que `canReconfigure` sea una función.
* Nos fijamos que tire error si no recibe un primer parámetro
* Nos fijamos que tire error si recibe un primer parámetro que no es un `string`. Con esto el test anterior se vuelve redundante ya que si no recibe nada tendremos `undefined` cuyo typeof es `undefined` y no `string`.
* Nos fijamos que retorne `false` si tienen un largo distinto los strings `from` y `to`.
* Nos fijamos que retorne `false` **si tienen un número distinto de caracteres únicos** ambos strings.



```java
import { describe, expect, it } from 'vitest'

const canReconfigure = (from, to) => {
  if (from === undefined) throw new Error('from parameter is missing')
  if (typeof from !== 'string') throw new Error('from parameter must be a string')

  if (to === undefined) throw new Error('to parameter is missing')
  if (typeof to !== 'string') throw new Error('to parameter must be a string')
  if (from.length !== to.length) return false

  if (new Set(from.split('')).size !== new Set(to.split('')).size) return false

  return true
}

describe('can-configure', () => {
  it('should be a function', () => {
    expect(canReconfigure).toBeTypeOf('function')
  })
  it('should throw if first parameter is missing', () => {
    expect(() => canReconfigure()).toThrow()
  })
  it('should throw if first parameter is not a string', () => {
    expect(() => canReconfigure(2)).toThrow()
  })
  it('should throw if second parameter is missing', () => {
    expect(() => canReconfigure('a')).toThrow()
  })
  it('should throw if second parameter is not a string', () => {
    expect(() => canReconfigure('a', 2)).toThrow()
  })
  it('should return a boolean', () => {
    expect(canReconfigure('a', 'b')).toBeTypeOf('boolean')
  })
  it('should return false if from and to have different lengths', () => {
    expect(canReconfigure('a', 'bb')).toBe(false)
  })
  it('should return false if from and to have different number of unique chars', () => {
    expect(canReconfigure('abc', 'ddd')).toBe(false)
  })
})

```

