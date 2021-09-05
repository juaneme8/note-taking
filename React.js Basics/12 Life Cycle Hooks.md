# Lifecycle Methods
Los componentes estos pasan por distintas fases en su ciclo de vida. React proporciona métodos para ejecutar acciones en cada una de ellas.

Veremos los *lifecycle methods* de un **componente de clase** (los cuales no existen en los **componentes funcionales** donde sí contamos con el `useEffect` hook que tiene bastante relación con ellos).

La clasificación que estudiaremos corresponde a React 16.4 en adelante.

 Podemos clasificar a los métodos dentro de cuatro fases:
## Mounting: (4)
La fase de montaje se desarrolla cuando una instancia de un componente está siendo creado e insertado o montado en el DOM. 
Está formado por los cuatro métodos cuyo orden es el siguiente:

* `constructor`: Se trata de una función especial que es llamada cuando un nuevo componente es creado. Es ideal para **inicializar** `state` y para hacer el **binding** de los event handlers como vimos anteriormente. Debemos tener en presente dos cosas: la primera es llamar a `super(props)` para llamar al constructor de la clase base y así tener acceso a `this.props` y la segunda es que es el único lugar en el que puedo setear `this.state` de manera directa en todos los otras situaciones debo usar `this.setState()` **No debemos realizar nada que produzca efectos secundarios** (http requests)

* `static getDerivedStateFromProps(props,state)` la documentación de react lo clasifica como un método rara vez usado. Se utiliza cuando el `state` del componente depende de los cambios en `props` en el tiempo. Por ejemplo en el caso que tenemos un componente cuyo `state` inicial depende de las `props` que le pasamos a ese componente. En ese caso podríamos usar este método para setear el `state`. Como se trata de un método estático no tiene acceso al `this` keyword por lo que no podremos hacer `this.setState` y tendremos que retornar un objeto que represente el nuevo `state` o null. **En este método tampoco debemos realizar nada que produzca efectos secundarios** (http requests)
Este método se dispara tanto cuando se renderiza por primera vez un componente como en a fase de actualización.

* `render()` es el único método **requerido** en los componentes de clase. Básicamente en él leemos `this.props` y `this.state` y retornamos `jsx` que describe la UI. Lo que **no debemos hacer** en este método es cambiar el `state`, interactuar con el DOM o hacer **AJAX calls**. Como en el método render tenemos el `jsx` que contiene a los componentes hijos luego del método render se ejecutan los life cycle methods de dichos componentes hijos.

* `componentDidMount()` será llamado sólo una vez en todo el ciclo de vida del componente y es llamado inmediatamente después de que el componente y todos sus componentes hijos fueron renderizados en el DOM. Es el **lugar ideal para para producir efectos secundarios** como ser interactuar con el DOM o realizar llamadas AJAX para obtener datos.

> https://reactjs.org/docs/react-component.html#componentdidmount
> You **may call  `setState()`  immediately** in `componentDidMount()`. It will trigger an extra rendering, but it will happen before the browser updates the screen. This guarantees that even though the `render()` will be called twice in this case, the user won’t see the intermediate state. Use this pattern with caution because it often causes performance issues. In most cases, you should be able to assign the initial state in the `constructor()` instead. It can, however, be necessary for cases like modals and tooltips when you need to measure a DOM node before rendering something that depends on its size or position.

Supongamos que tenemos un componente llamado `LifecycleA` que tiene como hijo a `LifecycleB` queremos conocer el orden de ejecución por lo que colocamos `console.log` en cada método.

```jsx
import React, { Component } from 'react';
import LifecycleB from './LifecycleB';

class LifecycleA extends Component {
	constructor(props) {
		super(props);

		this.state = {
			name: 'Juan',
		};
		console.log('LifecycleA Constructor');
	}

	static getDerivedStateFromProps(props, state) {
		console.log('LifecycleA getDerivedStateFromProps');
		return null;
	}

	componentDidMount() {
		console.log('Lifecycle ComponentDidMount');
	}

	render() {
		console.log('LifecycleA Render');
		return (
			<div>
				<h1>LifeCycleA</h1>
				<LifecycleB />
			</div>
		);
	}
}

export default LifecycleA;

```
El componente `LifecycleB`
```jsx
import React, { Component } from 'react';

class LifecycleB extends Component {
	constructor(props) {
		super(props);

		this.state = {
			name: 'Juan',
		};
		console.log('LifecycleB Constructor');
	}

	static getDerivedStateFromProps(props, state) {
		console.log('LifecycleB getDerivedStateFromProps');
		return null;
	}

	componentDidMount() {
		console.log('Lifecycle ComponentDidMount');
	}

	render() {
		console.log('LifecycleB Render');
		return (
			<div>
				<h1>LifeCycleB</h1>
			</div>
		);
	}
}

export default LifecycleB;

```

En la consola veremos lo siguiente:
1. LifecycleA Constructor
2. LifecycleA getDerivedStateFromProps
3. LifecycleA Render
4. LifecycleB Constructor
5. LifecycleB getDerivedStateFromProps
6. LifecycleB Render
7. LifecycleB ComponentDidMount
8. LifecycleA ComponentDidMount

Es decir que una vez que se ejecuta el método `render()` del componente padre **se ejecutan todos los métodos del hijo** y recién ahí el método `componentDidMount()` del padre.

Si al ejecutar estos ejemplos obtenemos dos entradas al constructor por ejemplo, se debe al uso de `<StrictMode>`.

## Updating: (5)
Cuando un componente está siendo re-renderizado como consecuencia de un cambio en sus `props` o `state`.
* `static getDerivedStateFromProps`
* `shouldComponentUpdate`
* `render`
* `getSnapshotBeforeUpdate`
* `componentDidUpdate`

 `static getDerivedStateFromProps(props, state)`: este método es llamado cada vez que un componente es re-renderizado. 
Rige todo lo mencionado anteriormente para este método en la fase de montaje.

* `shouldComponentUpdate(nextProps, nextState)`: Determina si el componente debe volver a renderizarse o no. El comportamiento default en los componentes de clase es re renderizar cada vez que las `props` o el `state` "cambian". Este método puede evitar ese comportamiento default retornando `false`. Lo que podemos hacer es comparar las `props` y `state` actuales con `nextProps` y `nextState` para determinar si debemos actualizar o no. Este método es para optimización en términos de performance. 
Debemos evitar causar efectos secundarios y llamar a `setState` en este método. Este método también es catalogado en la documentación de react como raramente utilizado. Se trata de una alternativa a **Pure Components**.

* `render` 
Rige todo lo mencionado anteriormente para este método en la fase de montaje.

* `getSnapshotBeforeUpdate(prevProps, prevState)`: es llamado justo antes de que los cambios del virtual DOM se reflejen en el DOM. **Obtenemos acceso de lectura del DOM antes de que se produzca el cambio** También es un método raramente utilizado. Este método debe retornar `null` o un valor. En caso de retornar un valor será pasado como tercer parámetro al siguiente método.
> Notar que recibe los mismos parámetros que `shouldComponentUpdate` luego de render **prev** en vez de **next**.

* `componentDidUpdate(prevProps, prevState, snapshot)`: este método será llamado una vez que el render termine en el ciclo de re-renderizado, es decir cuando el componente y todos sus hijos se hayan renderizado correctamente. Este método sólo será llamado una vez en cada ciclo de re-renderizado. Es posible causar **efectos secundarios** como ser realizar llamadas **AJAX** pero antes de hacerlas debemos comparar las `prevProps` con las nuevas `props` (comparar con componentDidMount que no recibe parámetros) y luego decidir si la hacemos. De lo contrario puede que estemos haciendo solicitudes no deseadas. `snapshot` es entregado por `getSnapshotBeforeUpdate()`

Nuevamente queremos determinar el **orden de ejecución** por lo que agregamos los métodos `shouldComponentUpdate`, `getSnapshotBeforeUpdate` y `componentDidUpdate`

Para disparar un "update lifecycle" debemos modificar `props` o `state`. Creamos en el componente `LifecycleA` un botón que al presionarlo realice un cambio en `state`

Obtenemos los valores de `console.log()` de la fase de montaje, luego limpiamos la consola y finalmente presionamos el botón para visualizar el orden de la fase de actualización donde obtenemos lo siguiente:

LifecycleA getDerivedStateFromProps
LifecycleA shouldComponentUpdate
LifecycleA Render
LifecycleB getDerivedStateFromProps
LifecycleB shouldComponentUpdate
LifecycleB Render
LifecycleB getSnapshotBeforeUpdate
LifecycleA getSnapshotBeforeUpdate
LifecycleB componentDidUpdate
LifecycleA componentDidUpdate

Una vez que el componente padre y el hijo han alcanzado el `render()`, hay un cambio respecto del montaje.

Los métodos más comunes son `render` y `componentDidUpdate`


## Unmounting: (1)
Cuando un componente está siendo eliminado del DOM.

* `componentWillUnmount`
 Este método es invocado inmediatamente antes de que un componente sea desmontado y destruido. Es posible realizar tareas de limpieza como ser cancelar *network requests*, eliminar *event handlers*, invalidar timers, etc. Lo que no debemos hacer es llamar a `setState()`.

## Error Handling: (2)
Cuando hay un error durante el **renderizado**, en un **lifecycle method** o en el **constructor de un componente hijo**. Estos métodos son llamados cuando hay un error durante el renderizado, en un método del lifecycle o en el construtor de cualquier componente hijo.

* `static getDerivedStateFromError(error)`
* `componentDidCatch(error, info)`