# React Redux Library
Tenemos una **aplicación React** cuyo estado se mantiene de manera separada en el **Redux Store**. La aplicación se encuentra suscripta al Redux Store y no puede modificar de manera directa el estado. Si quiere cambiar el estado debe emitir o **dispatch an action**. Una vez emitida la acción el **reducer** la maneja y actualiza el estado. Como la aplicación se encuentra suscripta al Redux Store, apenas el valor se actualiza este es pasado a la aplicación.

`npx create-react-app react-redux-demo`

`npm redux react-redux`

Creamos una carpeta `components` y dentro de ella el componente `CakeContainer.js`

```jsx
import React from 'react';

function CakeContainer() {
	return (
		<div>
			<h2>Number of Cakes</h2>
			<button>Buy Cake</button>
		</div>
	);
}

export default CakeContainer;
```

# Folder Structure
Para las aplicaciones React Redux los distintos autores emplean distintas estructuras de carpetas. En `src` creamos una carpeta `redux` y luego crearemos carpetas por cada feature, por ejemplo una carpeta `cake`

```
├── src
│   ├── redux
│   	├── cake
			├── cakeActions.js
			├── cakeTypes.js
```

# Action Creators

En  `cakeActions` definimos el action creator:
```jsx
import { BUY_CAKE } from './cakeTypes';

export const buyCake = () => {
	return {
		type: BUY_CAKE,
	};
};

```

En tanto que la constante la definimos en `cakeTypes`
```jsx
export const BUY_CAKE = 'BUY_CAKE';
```

Creamos el reducer en el archivo `cakeReducer`
```jsx
import { BUY_CAKE } from './cakeTypes';

const initialState = {
	numOfCakes: 10,
};

const cakeReducer = (state = initialState, action) => {
	switch (action.type) {
		case BUY_CAKE:
			return {
				...state,
				numOfCakes: state.numOfCakes - 1,
			};
		default:
			return state;
	}
};

export default cakeReducer;

```

En la carpeta `redux` creamos `store.js`
```jsx
import { createStore } from 'redux';
import cakeReducer from './cake/cakeReducer';

const store = createStore(cakeReducer);

export default store;

```

Ahora debemos proporcionar el Redux Store a la aplicación React y eso lo hacemos con la React Redux Library.

En `App.js` incorporamos `import {Provider} from 'react-redux'`

Normalmente colocamos el provider en la parte superior para que de esta manera podemos utilizarlo en cualquier componente del árbol de componentes.
```jsx
import './App.css';
import CakeContainer from './components/CakeContainer';
import { Provider } from 'react-redux';
import store from './redux/store';

function App() {
	return (
		<Provider store={store}>
			<div className='App'>
				<CakeContainer />
			</div>
		</Provider>
	);
}

export default App;

```

Volviendo a `CakeContainer` que es donde mostraremos el número de tortas y donde haceremos el dispatch.

1. En primer lugar creamos `mapStateToProps` (el nombre empleado es una convención que se suele utilizar). Esta función recibe `state` y retorna un objeto.

```jsx
const mapStateToProps = state => {
	return {
		numOfCakes: state.numOfCakes,
	};
};
```

En la documentación de React Redux nos con el concepto de **selectors** se trata de un archivo aparte como `cakeReducer` y `cakeActions` que retornan información del estado proveniente de Redux Store. Si bien en nuestro caso esto es muy simple (`state.numOfCakes` ) en aplicaciones de magnitud puede que tengamos que hacer algunas transformaciones de datos, por lo que en lugar de tener que escribir varias líneas de código en `mapStateToProps()` lo haremos en un archivo aparte llamado `cakeSelectors`

2. Creamos la función `mapDispatchToProps` recibe `dispatch` y retorna un objeto.

En la carpeta redux creamos un archivo `index.js` desde el cual exportamos todos los action creators.
```jsx
export { buyCake } from './cake/cakeActions';
```

Luego lo importamos en `CakeContainer` `import { buyCake } from '../redux';`
```jsx
const mapDispatchToProps = dispatch => {
	return {
		buyCake: () => dispatch(buyCake()),
	};
};
```

3. Conectamos estas dos funciones
En la parte superior `import {connect} from 'react-redux'`
En la parte inferior pasamos de tener `export default CakeContainer;` a tener `export default connect(mapStateToProps,mapDispatchToProps)(CakeContainer);`
Ahora ambas funciones harán lo que su nombre especifica por lo que recibiremos via props `props.numOfCakes` y también `props.buyCake()` que asociaremos a `onClick` del botón.

Entonces `CakeContainer` nos queda:

```jsx
import React from 'react';
import { connect } from 'react-redux';

import { buyCake } from '../redux';
function CakeContainer(props) {
	return (
		<div>
			<h2>Number of Cakes {props.numOfCakes}</h2>
			<button onClick={props.buyCake}>Buy Cake</button>
		</div>
	);
}

const mapStateToProps = state => {
	return {
		numOfCakes: state.numOfCakes,
	};
};

const mapDispatchToProps = dispatch => {
	return {
		buyCake: () => dispatch(buyCake()),
	};
};

export default connect(mapStateToProps, mapDispatchToProps)(CakeContainer);

```

