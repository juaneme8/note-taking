# useImperativeHandle
> Basado en el video de [midudev](https://youtu.be/a7_S6ZeydeU)

`useImperativeHandle` como su nombre lo indica nos permite realizar trabajos imperativos. 

Un ejemplo de estos trabajos imperativos sería si queremos ejecutar desde el componente padre un método del hijo. Nos permitirá personalizar el valor de `ref` que será expuesto al padre. 

> La mayoría de las veces debemos evitar su uso y buscar otra solución pero en ocasiones puede que no tengamos otra opción (por ejemplo si estamos trabajando con librerías de terceros).
>
> React normalmente trabaja de modo declarativo, en el modo imperativo en cambio vamos a forzar, es decir llamaremos al método directamente sin decirle qué es lo que tiene que hacer.



En primer lugar creamos un componente `Parent` y uno `Child` lo más simples posibles.

En `Parent`
```jsx
import Child from "./Child";

const Parent = () => {
   return (
    <div>
      <h1>Parent</h1>
      <Child ref={childRef} />
    </div>
  );
};

export default Parent;
```

En `Child`
```jsx
const Child = () => {
  return <h1>Child</h1>;
};
export default Child;
```

En el `Parent` creamos una referencia con `const childRef = useRef(null)` . Recordemos que `useRef()` nos permite guardar en un objeto una referencia que no va a cambiar entre renderizados.

Normalmente lo usaremos asociado a elementos del DOM:

```jsx
const elementRef= useRef();

console.log(elementRef);

return (
	<h1 ref={elementRef}>Titulo</h1>
)
```

`ref` es una prop especial que guarda el valor del elemento del DOM en la referencia de React `elementRef`.

Veremos en la consola que `elementRef` primero vale `undefined` dado que todavía el heading no se ha renderizado y luego vale `h1` es decir el DOM element. Una vez con este dato podríamos usar para obtener su ancho con `clientWidth` o bien agregar un listener a este evento (no es la forma correcta de hacerlo pero puede que por algún motivo nos veamos obligados a hacerlo).



En nuestro caso la referencia no será de un elemento del DOM sino de un componente y si implementamos lo mismo que antes:

```jsx
import { useRef } from "react";
import Child from "./Child";

const Parent = () => {
  const childRef = useRef(null);

  console.log(childRef);
    
  return (
    <div>
      <h1>Parent</h1>
      <Child ref={childRef} />
    </div>
  );
};

export default Parent;

```
Veremos que en la consola nos aparecerá el mensaje: *Warning: Function components cannot be given refs. Attempts to access this ref will fail. Did you mean to use React.forwardRef()?* Por lo tanto esto nos da la pauta de que debemos incorporar `forwardRef()` en `Child` y este método será el encargado cuando le llega una prop con el valor `ref` de llevarlo hacia arriba.

Su utilización tendrá la siguiente forma:
```jsx
React.forwardRef(props, ref) => {

})
```

El componente `Child` nos quedará de esta forma:
```jsx
import { forwardRef, useImperativeHandle, useState } from "react";

const Child = forwardRef((props, ref) => {
  const [visible, setVisible] = useState(false);

  const toggleVisibility = () => setVisible(!visible);

  useImperativeHandle(ref, () => {
    return {
      toggleVisibility
    };
  });
  return (
    <div>
      <h1 ref={ref}>Child</h1>
      {visible ? "true" : "false"}
    </div>
  );
});

export default Child;

```
 Resumiento la operatoria es la siguiente creamos la referencia en el padre con `useRef()` y se la pasamos al hijo mediante `props`. En el hijo utilizando `forwardRef()` para asegurarnos que lleve el valor hacia arriba. Luego con `useImperativeHandle` le pasamos como primer argumento la `ref` y como segundo una arrow function que retorna un objeto con los métodos que queremos que sea posible utilizar desde el padre.