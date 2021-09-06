# useImperativeHandle
> Basado en el video de [midudev](https://youtu.be/a7_S6ZeydeU)

`useImperativeHandle` nos permite personalizar el valor de `ref` que será expuesto al padre en la mayoría de los casos debemos evitar su uso pero en ocasiones puede que no tengamos otra opción. 

Un posible uso sería para ejecutar desde el padre un método del hijo.

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

En el `Parent` creamos una referencia con `const childRef = useRef(null)` 

```jsx
import { useRef } from "react";
import Child from "./Child";

const Parent = () => {
  const childRef = useRef(null);

  return (
    <div>
      <h1>Parent</h1>
      <Child ref={childRef} />
    </div>
  );
};

export default Parent;

```
En ese momento nos aparecerá el mensaje: *Warning: Function components cannot be given refs. Attempts to access this ref will fail. Did you mean to use React.forwardRef()?* Por lo tanto esto nos da la pauta de que debemos incorporar `forwardRef()` en `Child` y este método será el encargado cuando le llega una prop con el valor `ref` de llevarlo hacia arriba.

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