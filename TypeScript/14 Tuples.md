## Tuples
Los tuples son similares a los arrays (utilizamos `[]` para definirlos y cuentan con métodos de arrays), pero la diferencia de que el tipo de datos en cada posición es fijo una vez inicializado. 

Si tenemos por ejemplo un array `let arr = ['ryu', 25, true];` Cualquier posición puede ser un `string`, `number` o `boolean` por lo que podremos poner: `arr[0] = false;` `arr[1] = 'yoshi';` o bien `arr = [30, false, 'yoshi'];`

En cambio al trabajar con tuples una vez definido el tipo de datos en cada posición luego no podremos cambiarlo. 
Supongamos que definimos un tuple de manera explícita:
```ts
let tup: [string, number, boolean] = ['ryu',25,true];
tup[0]='ken';
//tup[0]=false; //error
tup[1]=40;
//tup[1]='hello'; //error
tup[2]= false;
//tup[2]= 0 //error
```

Supongamos que tenemos `let student: [string,number]` esto significa que siempre tendremos que definir `student=['chun-li',1234];` es decir en primer lugar un `string` y luego un `number`