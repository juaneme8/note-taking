## Type Aliases 

Con la palabra clave `type` podemos definir alias de tipo. Esto será útil por ejemplo si tenemos una función que recibe un string pero que sólo podrá ser de algunos valores en particular:

```tsx
type Operation = 'multiply' | 'add' | 'divide';
```

```tsx
type Operation = 'multiply' | 'add' | 'divide';

const calculator = (a: number, b: number, op : Operation) => {
  if (op === 'multiply') {
    return a * b;
  } else if (op === 'add') {
    return a + b;
  } else if (op === 'divide') {
    if (b === 0) return 'can\'t divide by 0!';
    return a / b;
  }
}
```

Tenemos dos cosas a mejorar:

* Cuando su código puede terminar en una situación en la que algo se divide por 0, es probable que algo haya salido terriblemente mal y **se debe generar un error** y manejarlo donde se llamó a la función

* Aunque hemos definido tipos para nuestros parámetros, **el JavaScript generado que se usa en tiempo de ejecución no contiene las verificaciones de tipos**. Entonces, si, por ejemplo, el valor del parámetro de *operation* proviene de una interfaz externa, no hay garantía definitiva de que sea uno de los valores permitidos. Por lo tanto, es mejor incluir el manejo de errores y estar preparado para lo inesperado. 

```tsx
type Operation = 'multiply' | 'add' | 'divide';

type Result = number;

const calculator = (a: number, b: number, op : Operation) : Result => {
  switch(op) {
    case 'multiply':
      return a * b;
    case 'divide':
      if( b === 0) throw new Error('Can\'t divide by 0!');
      return a / b;
    case 'add':
      return a + b;
    default:
      throw new Error('Operation is not multiply, add or divide!');
  }
}

try {
  console.log(calculator(1, 5 , 'divide'))
} catch (e) {
  console.log('Something went wrong, error message: ', e.message);
}
```



Otra utilidad de los alias de tipo puede ser en ocasiones al trabajar con funciones con parámetros tenemos especificaciones de tipo bastante largas o complejas que muchas veces tendremos que repetir para distintas funciones:

```ts
const logDetails = (uid: string | number, item: string) => {
	console.log(`${item} hasta uid of ${uid}`);
};

const greet = (user: { name: string; uid: string | number }) => {
	console.log(`${user.name} says hello`);
};

```
En esos casos es conveniente utilizar alias de tipo con lo cual nos queda mucho más compacto:
```ts
type StringOrNum = string | number;
type objWithName = { name: string; uid: StringOrNum };

const logDetails = (uid: StringOrNum, item: string) => {
	console.log(`${item} hasta uid of ${uid}`);
};

const greet = (user: objWithName) => {
	console.log(`${user.name} says hello`);
};

```