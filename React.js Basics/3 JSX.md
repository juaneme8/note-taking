# JSX
JSX significa **JavaScript XML** lo cual es una extensión de JavaScript que nos permite escribir código similar al XML (Extensible Markup Language) para describir componentes. Si bien no es obligatorio, nos permite obtener un código más simple y elegante. Luego el JSX es transpilado a JavaScript (tarea que se lleva a cabo con Babel) puro de modo de ser entendido por los navegadores. Esto podemos comprobarlo entrando a babeljs.io y copiando el código de un componente.

Si bien funciona tanto utilizando la extensión `.js` como `.jsx` no es recomendable utilizar la extensión `.jsx` (lo mismo para `tsx` si utilizamos TypeScript)

Para entender su importancia crearemos el mismo componente utilizando `jsx` y sin utilizarlo. 
Utilizando JSX tenemos al componente `Greet` utilizado anteriormente.
```jsx
import React from 'react';

const Greet = () => {
	return <h1>Hello World!</h1>;
};

export default Greet;
```

Si quisiéramos escribir el mismo componente sin usar `jsx` lo hacemos haciendo uso de `React.createElement()` que como mínimo acepta tres parámetros. El primer parámetro es un string con el *html tag* que queremos renderizar, el segundo con propiedades opcionales (como ser id, clase y si no queremos ninguna ponemos `null`) y el tercero con los hijos del elemento.

Si quisiéremos crear un `div` con un nodo de texto:  `React.createElement('div', null,'texto')` 
Si quisiéramos crear un `div` con dos nodos de texto:
`React.createElement('div', null,'texto1','texto2')` 

Pero como lo que queremos es mostrar un texto formateado con un `<h1>` lo que hacemos es como contenido del `div` colocar otro elemento.
`React.createElement('div', null, React.createElement('h1', null, 'texto'));`

Si quisiéramos hacer lo mismo pero asignándole una clase y un id al div, le pasaríamos un objeto como segundo parámetro.
`React.createElement('div', {id: 'top', className:'container'}, React.createElement('h1', null, 'texto'));`

**Babel** es el encargado de transpilar el `jsx` y convertirlo en código `React.createElement()`

## Conclusión JSX
Resumiendo JSX es *syntactic sugar* para llamar a `React.createElement()` es por eso que tenemos que importar `import React from 'react'` cuando usamos `jsx`.

## Diferencias JSX y HTML
En vez de `clio `aame`
En vez de `for` usamos `htmlFor`
En los nombres de atributos en vez de `onclick` usamos la convención *camelCase* por lo que será `onClick`

# Contenido Dinámico
El propósito de React es trabajar con contenido dinámico, es por esto que dentro del `jsx` del componente podremos elponer codigo JavaScript que queremos evaluar entre llaves, por ejemplo en el componente `App`

```jsx
const App = () => {
	const mensaje = 'Hola';
	const a = 2;
	const b = 3;
	return (
		<div className="App">
			{mensaje}
			{a+b}
			{+new Date()}
		</div>
	)
}
export default App;
```

La expresión `+new Date()` nos permite convertir el objeto en un timestamp si intentáramos mostrar en pantalla `new Date()` directamente nos tiraría un error.:
`<p>{Math.random()}</p>`