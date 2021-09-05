# Context API
**Context** provee una forma de pasar datos a través del árbol de componentes sin tener que pasar *props* manualmente en cada nivel.

En una aplicación típica de React, los datos se pasan de padre a hijo a través de *props*, pero esto puede ser complicado para ciertos tipos de *props* (por ejemplo, usuario autenticado, preferencias de idioma, el tema de la interfaz si estamos en dark mode) que son necesarios para muchos componentes dentro de una aplicación. Context proporciona una forma de compartir valores como estos entre componentes sin tener que pasar explícitamente una *prop* a través de cada nivel del árbol.

Consideramos un ejemplo en el cual `App`, tiene de hijo a `ComponentC`, este a `ComponentE` y este último a  `ComponentF`. El objetivo es pasar `username` desde `App` y leer ese valor en `ComponentF` usando Context. Si bien podríamos hacerlo pasando `props` esto implicaría que reciban esa información componentes intermedios que no la necesitan.

1. Crear el contexto para ello creamos el archivo `userContext.js`

```jsx
import React from 'react';

const UserContext = React.createContext();

const UserProvider = UserContext.Provider;
const UserConsumer = UserContext.Consumer;

export { UserProvider, UserConsumer };

```

2. Proporcionar un valor al contexto. Esto lo haremos con el componente `UserProvider`, el lugar donde lo hacemos es importante ya que sólo los componentes descendientes podrán consumirlo.
En `App` *wrapeamos* a `ComponentC` con `UserProvider` y le pasamos el atributo `value` con el valor que queremos recibir en `ComponentF`
```jsx
import React, { Component } from 'react';
import ComponentC from './components/ComponentC';
import { UserProvider } from './components/userContext';

class App extends Component {
	render() {
		return (
			<UserProvider value='Juan'>
				<ComponentC />
			</UserProvider>
		);
	}
}

export default App;
```

3. Consumir el valor del contexto en los componentes que lo necesitamos. En `Component F` importamos `<UserConsumer>` y entre los tags de apertura y cierre colocamos una función flecha que recibe el valor del user context como parámetro y retorna un elemento React (no olvidar colocar `return`)
```jsx
import React, { Component } from 'react';
import { UserConsumer } from './userContext';
class ComponentF extends Component {
	render() {
		return (
			<UserConsumer>
				{(username) => {
					return <div>Mi nombre es {username}</div>;
				}}
			</UserConsumer>
		);
	}
}

export default ComponentF;

```

## Valor Default del Contexto
Es posible definir un valor default del contexto de modo tal que si no hay provider sea el valor que obtenemos en el consumer. El valor default lo pasamos al crear el contexto como argumento de `React.createContext()`

`userContext`
```jsx
import React from 'react';

const UserContext = React.createContext('Codevolution');

const UserProvider = UserContext.Provider;
const UserConsumer = UserContext.Consumer;

export { UserProvider, UserConsumer };

```
Luego en `App` comentamos al componente provider
```jsx
import React, { Component } from 'react';
import ComponentC from './components/ComponentC';
import { UserProvider } from './components/userContext';

class App extends Component {
	render() {
		return (
			// <UserProvider value="Juan">
			<ComponentC />
			// </UserProvider>
		);
	}
}

export default App;

```

## Context Type Property
Una alternativa al uso del Consumer component para consumir el valor del contexto es usando la propiedad `contextType`  de la clase.

Supongamos ahora que también queremos mostrar el valor del contexto en `ComponentE` y esto queremos hacerlo aplicando el nuevo método.

1. En primer lugar en `userContext` debemos exportar `UserContext`.
```jsx
import React from 'react';

const UserContext = React.createContext('Codevolution');

const UserProvider = UserContext.Provider;
const UserConsumer = UserContext.Consumer;

export { UserProvider, UserConsumer };
export default UserContext;

```
2. En `ComponentE` asignamos este contexto `UserContext` a la propiedad `contextType` de la clase y luego accedemos al contexto con `this.context`
```jsx
import React, { Component } from 'react';
import ComponentF from './ComponentF';
import UserContext from './userContext';

class ComponentE extends Component {
	render() {
		return (
			<div>
				Component E context {this.context}
				<ComponentF />
			</div>
		);
	}
}

ComponentE.contextType = UserContext;

export default ComponentE;

```

Por último si la aplicación soporta sintaxis de *public class fields* podemos reemplazar `ComponentE.contextType = UserContext;` dentro de la clase por `static contextType = UserContext`

La implementación mediante `contextType` tiene dos **limitaciones**:
La primera es que sólo funciona con componentes de clase.
La segunda es que sólo podemos suscribirnos a un único contexto usando `contextType`. En ocasiones tendremos que leer más de un contexto.