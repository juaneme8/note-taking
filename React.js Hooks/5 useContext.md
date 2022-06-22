# useContext
> Agregados del [video](https://youtu.be/t9WmZFnE6Hg) de PedroTech

Para entender la motivación detras del  `useContext` y cómo solían hacerse las cosas antes de la aparición de los hooks, recomendamos **volver a leer el capítulo de Context de React Basics**.

Al igual que cuando estudiamos `Context API` planteamos una situación en la cual tenemos el *root component* `App` que es padre del `ComponentC` este lo es de `ComponentE` y este último de `ComponentF`. El objetivo es pasar la prop `username` desde `App` y lograr leerla en `ComponentF`. Si bien el planteo es idéntico, la solución abordada tiene algunas diferencias por lo que lo volvemos a hacer.

1. El primer paso es crear el contexto lo cual hacemos en `App` con `const UserContext = React.createContext();`

2. El segundo paso es envolver a los componentes hijos con el Provider y pasarle el valor al contexto.
```jsx
<UserContext.Provider value="Vishwas">
	<ComponentC />
</UserContext.Provider>
```

3. Consumir el valor del contexto en `ComponentF` para ello primero en `App` debemos exportar el contexto (*named export*) `export const UserContext = React.createContext();` e importarlo en `ComponentF` 
Luego consumirlo y hacer uso del **patrón render props** para obtener el valor pasado por el contexto
```jsx
return (
	<div>
		<UserContext.Consumer>
			{(user) => {
				return <div>Usuario de Contexto: {user}</div>;
			}}
		</UserContext.Consumer>
	</div>
);
```

`App`
```jsx
import React from 'react';
import ComponentC from './components/ComponentC';

export const UserContext = React.createContext();

function App() {
	return (
		<div className="App">
			<UserContext.Provider value="Vishwas">
				<ComponentC />
			</UserContext.Provider>
		</div>
	);
}

export default App;

```

`ComponentC`
```jsx
import React from 'react';
import ComponentE from './ComponentE';

function ComponentC() {
	return (
		<div>
			<ComponentE />
		</div>
	);
}

export default ComponentC;

```

`ComponentE`
```jsx
import React from 'react';
import ComponentF from './ComponentF';

function ComponentE() {
	return (
		<div>
			<ComponentF />
		</div>
	);
}

export default ComponentE;

```

`ComponentF`
```jsx
import React from 'react';
import { UserContext } from '../App';

function ComponentF() {
	return (
		<div>
			<UserContext.Consumer>
				{(user) => {
					return <div>Usuario {user}</div>;
				}}
			</UserContext.Consumer>
		</div>
	);
}

export default ComponentF;

```
Como podemos ver el modo de consumir los datos es algo complejo. Si bien para un contexto único esto será aceptable, cuando tenemos múltiples valores:

`App` 
```jsx
import React from 'react';
import ComponentC from './components/ComponentC';

export const UserContext = React.createContext();
export const ChannelContext = React.createContext();

function App() {
	return (
		<div className="App">
			<UserContext.Provider value={'Vishwas'}>
				<ChannelContext.Provider value={'Codevolution'}>
					<ComponentC />
				</ChannelContext.Provider>
			</UserContext.Provider>
		</div>
	);
}

export default App;

```

`ComponenentF` 
```jsx
import React from 'react';
import { UserContext, ChannelContext } from '../App';

function ComponentF() {
	return (
		<div>
			<UserContext.Consumer>
				{user => {
					return (
						<ChannelContext.Consumer>
							{channel => {
								return (
									<div>
										Usuario {user} y Canal {channel}
									</div>
								);
							}}
						</ChannelContext.Consumer>
					);
				}}
			</UserContext.Consumer>
		</div>
	);
}

export default ComponentF;

```
Podemos ver que eta implementación no es de fácil lectura debido a los anidamientos que es necesario realizar. El `useContext` hook nos permite consumir valores del contexto de manera más simple.

Para la implementación usando hooks veremos que el primer y segundo paso son iguales y sólo cambia el tercero, es decir el consumo del valor del contexto. Trabajaremos en este caso con `ComponentE`

1. Importamos `useContext` `import React, { useContext } from 'react';`
2. Importamos los contextos `import { UserContext, ChannelContext } from '../App';`
3. 
```jsx
const user = useContext(UserContext);
const channel = useContext(ChannelContext);
```

Por lo tanto `ComponentE` nos queda:
```jsx
import React, { useContext } from 'react';
import ComponentF from './ComponentF';
import { UserContext, ChannelContext } from '../App';

function ComponentE() {
	const user = useContext(UserContext);
	const channel = useContext(ChannelContext);
	return (
		<div>
			Usuario {user} y Canal {channel}
			<ComponentF />
		</div>
	);
}

export default ComponentE;

```
De esta manera podemos ver que el uso de hooks simplifica mucho el proceso de consumo del contexto.

La ventajas del uso de Context API son:

* Evitamos el prop drilling, es decir tener que pasarle a todos los hijos las props que necesitamos en uno de ellos probablemente  muy abajo en el árbol de componentes.
* Evitamos re-renderizados innecesarios de componentes intermedios a raíz de cambios en alguna variable que pasan hacia componentes hijos.



Como aspectos negativos:

* Si cambia un valor del contexto se renderizan todos los componentes que utilizan dicho contexto aunque no utilicen el valor que cambió.

  



## Buenas Prácticas

Se aconseja crear un componente para cada contexto por ejemplo si queremos almacenar datos del usuario podemos crear un componente `UserContextProvider` en un archivo `UserContext.js`. 

En este componente crearemos el contexto `UserContex` y trabajamos con las props `children`.

```jsx
import React, { createContext, useState } from "react";

export const UserContext = createContext(null);

export const UserContextProvider = ({ children }) => {
  const [userInfo, setUserInfo] = useState(null);
  const [isAuth, setIsAuth] = useState(false);

  const login = () => {
    fetch("/login").then((res) => {
      setIsAuth(true);
      setUserInfo(res.user);
    });
  };

  const logout = () => {
    fetch("/logout").then((res) => {
      setIsAuth(false);
      setUserInfo(null);
    });
  };

  const value = {
    userInfo,
    setUserInfo,
    isAuth,
    setIsAuth,
    login,
    logout,
  };

  return (
    <UserContext.Provider value={value}> {children} </UserContext.Provider>
  );
};

export default UserContext;
```

