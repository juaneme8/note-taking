# Zod

> :link: Basado en el [video de Fazt](https://youtu.be/bUzGfrjg66M).

Zod es una librería de validación de datos escrita en TS.

En el [repositorio](https://github.com/colinhacks/zod) nos encontramos con la documentación y presentación de los métodos disponibles.



## Características

* Es muy liviana en producción (comparada con yup y joi por ejemplo)

* No tiene dependencias.

* Puede ser utilizada tanto en FE como en BE.

* Se basa en paradigmas de programación funcional ya que   trabaja con métodos inmutables (a partir de una entrada retornan una salida nueva)



## Creación de Proyecto

```bash
mkdir md-zod
cd md-zod
npm init -y
npm i typescript
```



## Instalación de Dependencias

### `ts-node-dev`

En lugar de **instalar** TypeScript, **iniciarlo** con `npx tsc --init` o mediante un script `tsc` que correríamos con `npm run build -- --init` y luego **generar el código de producción** con `npm run build` trabajaremos directamente con ts-node-dev.

Como consecuencia de esto estará utilizando TypeScript del visual studio Code por lo que al hacer hover puede que los tipos de datos observados no sean los esperados.

```bash
npm i ts-node-dev -D
```



Podemos crear un script o ejecutar el comando directamente desde la terminal.

* Creando un script:

```json
"dev": ts-node-dev --respawn src/index.ts
```



* Comando directo:

```bash
npx ts-node-dev --respawn src/index.ts
```



> EInicialmente en `src/index.ts` tenemos sólamente un `console.log` y luego de mostrarlo se terminaría (en otros proyectos esto no fue necesario ya que por ejemplo en los servidores Express el programa permanece activo todo el tiempo esperando conexiones entrantes). Queremos que permanezca vigilando este archivo por lo tanto agregamos la opción `--respawn`



## `zod`

```
npm i zod
```

Al tratarse de una librería que está escrita enteramente en TypeScript no necesitamos instalar types.



Lo primero que haremos será definir un schema y lo utilizaremos para comparar si lo que recibimos cumple con los lineamientos establecidos en él.



## Primeros Pasos



En `index.ts` crearemos una validación simple de un string.



### Validación de String

```tsx
import {z} from 'zod'

const nameSchema = z.string();

const name = 100;

nameSchema.parse(name)
```



Como la validación falla, zod se encarga de hace run `throw ` de un error que podremos ver en pantalla y nos permitirá tener una idea de lo que ha sucedido.

```json
ZodError: [
  {
    "code": "invalid_type",
    "expected": "string",
    "received": "number",
    "path": [],
    "message": "Expected string, received number"
  }
]
```



El **paradigma funcional** de procesar una entrada y entregar otra salida, se pone de manifiesto ya que el valor devuelto por `parse` es el mismo que tenemos en `name`, es decir que si con `name="Juan"` hacemos `const result = nameSchema.parse(name)` y veremos que `result` ahora es igual a "Juan". 



### Validación de Primitivos 

* `z.string()`

* `z.number()`

* `z.boolean()`

  

### Validación de Tipos Vacíos

* `z.undefined()`
* `z.null()`



## Validación de Objetos

* `z.object({})`

```tsx
import { z } from 'zod'

const UserSchema = z.object({
	email: z.string().email(),
	fullname: z.string(),
	phone: z.number()
})

const result = UserSchema.parse({
	email: 'j@gmail.com',
    fullname: 'Juan Ocho',
    phone: 1234567890,
    extra: 'http://...'
})
console.log(result)
```

En la pantalla veremos `{ email: 'j@gmail.com', fullname: 'Juan Ocho', phone: 1234567890 }` esto significa que si le pasamos alguna propiedad adicional que no está en el schema en el resultado no la veremos.

> En la documentación los schemas de objetos comienzan con mayúscula y no así los de primitivos.



## Inferencia de tipos de datos

Es una característica de zod que nos permite no tener código duplicado.

Normalmente al trabajar con objetos tendríamos algo así:

```tsx
import { z } from 'zod'

const UserSchema = z.object({
	email: z.string().email(),
	fullname: z.string(),
	phone: z.number()
})

type UserType = z.infer<typeof UserSchema>;

const userInput: UserType = {
  email: 'j@gmail.com',
  fullname: 'Juan Ocho',
  phone: 1234567890,
}

const result = UserSchema.parse(userInput)
console.log(result)
```



Como podemos ver una parte a la hora de definir el tipo de `userInput` estamos siendo redundantes con lo definido en el schema. Es ahi donde podemos hacer uso de una característica de zod conocida como infernecia de tipos que nos permite extraer tipos de datos de los esquemas.



## Merge

En ocasiones vamos a querer combinar dos schemas para validar de manera conjunta un dato.

Por ejemplo a partir de `UserSchema` y `AddressSchema` vamos a querer generar un tercer schema llamado `CitizenSchema` con `const CitizenSchema = UserSchema.merge(AddressSchema)`

Luego creamos un tipo infieriendo los datos de ese schema `type CitizenType = z.infer<typeof CitizenSchema>;`

```tsx
import { string, z } from 'zod'

const UserSchema = z.object({
	email: z.string().email(),
	fullname: z.string(),
	phone: z.number()
})

const AddressSchema = z.object({
  street: z.string(),
  city: z.string()
})

const CitizenSchema = UserSchema.merge(AddressSchema)

type CitizenType = z.infer<typeof CitizenSchema>;

const userInput: CitizenType = {
  email: 'j@gmail.com',
  fullname: 'Juan Ocho',
  phone: 1234567890,
  street: 'Calle 1',
  city: 'Buenos Aires'
}

const result = CitizenSchema.parse(userInput)
console.log(result)
```



## Validación Segura

Hasta ahora hemos visto que con `parse` validamos un schema y este falla se genera un error que acabará con la ejecución del programa. Esto si bien solucionarse colocando la validación dentro de un `try` `catch`, en ocasiones puede que necesitemos hacer una validación segura y en ese caso podemos utilizar `safeParse`



Utilizando `parse` vemos como la ejecución del código se interrumpe y no llegamos a ver el mensaje de "Resto del código..."

```tsx
import {z} from 'zod';

const stringSchema = z.string();

const result = stringSchema.parse(30);
console.log(result);

console.log('Resto del código...');
```



Con `parse` utilizando un bloque `try` `catch` logramos ver dicho mensaje.

```tsx
import { z } from 'zod'

const stringSchema = z.string()

try {
  
  const result = stringSchema.parse(30)
  console.log(result)

}
catch (err) {
  if (err instanceof Error) {
    console.log(err.message);
  } else {
    console.log('Unexpected error', err);
  }
}

console.log('Resto del código...')
```



Con `safeParse` veremos que no se interrumpe la ejecución del programa. 

```tsx
import {z} from 'zod';

const stringSchema = z.string();

const result = stringSchema.safeParse(30);
console.log(result);

console.log('Resto del código...');
```



En este caso a la salida en`result` obtenemos datos relacionados con la validación.

En este caso donde esperaba un string y le pasamos un number:

```json
{
  success: false,
  error: ZodError: [
    {
      "code": "invalid_type",
      "expected": "string",
      "received": "number",
      "path": [],
      "message": "Expected string, received number"
    }
  ]    	
```

En un caso donde la validación sea correcta:

```
{ success: true, data: 'Juan' }
```



## Validación de Arreglos

## Array de Primitivos

En primer lugar consideramos arrays de literales, por ejemplo un array de números.

```tsx
import {z} from 'zod';

const numbersArraySchema = z.array(z.number())

type numbersArrayType = z.infer<typeof numbersArraySchema>

const result = numbersArraySchema.parse([1,2,3])
console.log(result)
```

El schema también lo podríamos haber definido encadenando métodos, podríamos haber puesto `z.string().array()`, **es importante el orden en que lo hacemos**. 



### Campos Opcionales

Si qusiéramos que el array fuera **opcional** podríamos haber puesto  `string().array().optional()` y al generar y posicionar el mouse encima deberíamos ver `string[] | undefined`. 



> Si en cambio vemos `string[]` puede que se deba a que estamos usando el compilador de Visual Studio Code y en ese caso tendremos que generar `tsconfig.json`  con `npx tsc --init` para usar el ubicado en `node_modules/.bin`.



```tsx
import {z} from 'zod';

const s1 = z.string().array().optional();

type s1Type = z.infer<typeof s1>

// s1.parse([1,2,3])
s1.parse(undefined)
s1.parse(['a','b','c'])
```

Ambos ejemplos pasarán la validación.



Mientras que si hubiesemos puesto `const s2 = z.string().optional().array();` consideraría que puede ser `(string | undefined)[]` por lo que aceptaría `s1.parse(['a','b',undefined])`.



## Arreglo de Objetos

En ocasiones tendremos arrays de otros tipos de datos:

```tsx
import {z} from 'zod';

const UserSchema = z.object({
  name: z.string(),
  age: z.number()
})

const UsersSchema = z.array(UserSchema)

```



Si luego tenemos un tipo de dato incorrecto, podremos rastrear de donde proviene el error mediante la propiedad `path` del error. Por ejemplo si tenemos



```tsx
import {z} from 'zod';

const UserSchema = z.object({
  name: z.string(),
  age: z.number()
})

const UsersSchema = z.array(UserSchema)

const result = UsersSchema.parse([
  { name: 'juan', age: 1},
  { name: 'paco', age: '1'},
])
```



El error mostrado en pantalla tendrá la siguiente forma:

```json
{                                                
  "code": "invalid_type",                        
  "expected": "number",                          
  "received": "string",                          
  "path": [                                      
    1,                                           
    "age"                                        
  ],                                             
  "message": "Expected number, received string"  
}                                                
```

Esto significa que tenemos un error en el elemento ubicado en la posición `1` y en particular en la propiedad `age` y en `message` vemos qué fue lo que causó el error.