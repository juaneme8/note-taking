# 404
Es posible contar con una página 404 creando en `pages` el archivo `404.js` y este componente se mostrará en pantalla cada vez que ingresemos a una url que no tenga una página asociada nos mostrará este componente:

```jsx
import React from 'react';
import Link from 'next/link';
const NotFound = () => {
  return (
    <div>
      <h1>Ooooops...</h1>
      <h2>Página No Encontrada</h2>
      <p>
        Ir al{' '}
        <Link href="/">
          <a>Inicio</a>
        </Link>
      </p>
    </div>
  );
};

export default NotFound;

```