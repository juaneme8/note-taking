# Portals
Los portales nos permiten renderizar hijos en un nodo del árbol del DOM que existe por fuera de la jerarquía del componente padre (árbol de React) 
Hasta ahora trabajamos con un único elemento del DOM en el html sobre el cual **montamos la aplicación react.**
Si vamos a `/public/index.html` veremos `<div id="root"></div>` y en `/src/index.js` especificamos que queremos montar el componente `App` en el elemento `root` de la siguiente forma `ReactDOM.render(<App />, document.getElementById('root'));`
Es por eso que al ejecutar la aplicación y observar en las en las DevTools el árbol del DOM veremos que todos los componentes están dentro del root element:
```html
<div id="root">
	<div class="app">
	...
	</div>
</div>
```
Los portales nos permiten renderizar un componente en un nodo distinto del nodo padre más cercano
1. El primer paso es en `/public/index.html` agregar un nuevo nodo, esto lo hacemos de la siguiente forma:
```html
<div id="root"></div>
<div id="portal-root"></div>
```

2. Creamos un nuevo componente funcional `PortalDemo`
3. Incorporamos este elemento como es habitual en el return statement del componente `App`
4. En `PortalDemo` En lugar de retornar sólo el JSX retornamos `return ReactDOM.createPortal()` que toma dos parámetros el primero es el JSX y el segundo es el elemento es el nodo del DOM sobre el cual queremos montar el componente.
```jsx
import React from 'react';
import ReactDOM from 'react-dom';
function PortalDemo() {
	return ReactDOM.createPortal(
		<div>
			<h1>Portal Demo</h1>
		</div>,
		document.getElementById('portal-root')
	);
}

export default PortalDemo;

```
Ahora al inspeccionar los elementos veremos que el componente está dentro del nodo con `id="portal-root"`
Si bien el componente es hijo de `App` que está montado sobre el nodo `root` logramos ubicarlo en otro nodo

Uno de los casos de uso de los portales es cuando tenemos que lidiar con estilos de CSS del componente padre cuando el hijo es un `modal`, `pop-up`, `tooltip`. Por ejemplo si en el padre tenemos especificado un `max-width` este se aplicaría también en el hijo y logramos evitarlo colocandolo en otro nodo.

## Propagación de Eventos en Portales
En cuanto a la propagación de eventos o event bubbling en portales es que aunque pueden estar en cualquier nodo del DOM tree se comportan como cualquier hijo en los otros aspectos (lo cual incluye propagación de eventos). Esto significa que un evento disparado desde dentro de un portal se propagará a los ancestros en el **árbol de React** aun cuando esté en otro nodo del **árbol del DOM**. 