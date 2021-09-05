# Components
os componentes describen una parte de la UI, son independientes  eutilizables.

Toda aplicación tiene al menos un component que recibe el nombre root component* y este podrá tener *child components*. Es importante usar componentes anidados en lugar de tener todo la UI en un único componente ya que así lograremos una modularidad que nos permitirá tanto reutilizar componentes como que cada uno tenga su propio estado y métodos.

Tienen la apariencia de templates de html pero se trata de `jsx` (JavaScript XML). También pueden contener estado y funcionalidades en JavaScript.

Los componentes pueden tener las extensión `.js` o `.jsx`

Los componentes pueden ser de dos tipos:
* **Componentes Funcionales**: son funciones JavaScript que retornan `jsx`.
* **Componentes de Clase**: son clases que extienden a la clase `Component`, pueden tener un estado y deben incluir un método `render()` que retorna .

## Componentes Funcionales
Los componentes funcionales son funciones JavaScript que pueden recibir propiedades en un objeto llamados `props` y retornan `` que describe el UI.

Limpiamos el `App.js` de modo que sólo retorne el div más externo `<div className="App"></div>`

 Hasta el momento en `App.js` tenemos: 
```jsx
const App = () => {
	return (
		<div className="App">
			Hola Mundo
		</div>
	)
}
export default App;
```

Si queremos mostrar más de una vez el mismo mensaje de bienvenida, lo que haremos en lugar de duplicar el código será crear un componente reutilizable. La forma más simple de crear un componente es hacerlo en el mismo archivo `App.js` al cual llamamos `Mensaje`. 

Notar que usamos **PascalCase** para el nombre de los componentes. Si los definimos comenzando con minúsculas obtendremos un warning pues los considerará una etiqueta.

```jsx
const Mensaje = () =>{
	return <h1>Hola Mundo!</h1>
}
const App = () => {
	return (
		<div className="App">
			<Mensaje/>
			<Mensaje/>
			<Mensaje/>
		</div>
	)
}
export default App;
```
> Tener en cuenta que el componente debe estar fuera del componente `App`.

Otra forma es hacerlo en un archivo aparte. Para ello en en `src` creamos una carpeta `components` y en ella `Greet.js`. 

A la hora de definir un componente podemos utilizar Function Declarations o Function Expressions. Utilizando **Function Declarations**:
```jsx
import React from 'react';

function Greet (){
	return <h1>Hello World!</h1>;
};

export default Greet;
```
Otra opción es utilizar **Function Expressions**, en este caso se utilizan *arrow functions*. Por simplicidad podemos utilizar el snippet provisto por la extensión **ES7 React/Redux/GraphQL/React-Native snippets** `rafce` (*react arrow function component with export*),`rfce` (*react funcional  with an export)*.

> NOTA 2021: En las versiones actuales no es necesario colocar `import React from 'react';` ya que lo han incorporado ese import automáticamente en Babel. Esto era necesario porque luego teníamos por ejemplo `React.createElement()` y teníamos que tener React importado.En `src` creamos una carpeta `components` y en ella `Greet.js`. Notar que usamos **PascalCase** para el nombre de los omponentes.
> Por convención se utilizan *arrow functions* para definirlos, podemos utilizar el snippet provisto por la extensión **ES7 React/Redux/GraphQL/React-Native snippets** `rafce` (*react arrow function component with export*),`rfce` (*react functional component with an export)*
```jsx
import React from 'react';

const Greet = () => {
	return <h1>Hello Worlds!</h1>;
};

export default Greet;
```
Luego en `App.js` importamos el componente. 

En este caso como lo único que hacemos es retornar jsx, al estar utilizando funciones flecha podríamos omitir el keyword `return`:

```jsx 
import React from 'react';

const Greet = () => <h1>Hello World!</h1>;
};

export default Greet;
```

> Como lo exportamos como default podríamos haberle puesto cualquier nombre al importarlo y hubiera funcionado de igual modo. Si en cambio hubiésemos hecho un *named export* `export const Greet = () => <h1> Hello World! </h1>` luego tendríamos que haberlo importado con el mismo nombre `import {Greet} from './components/Greet'`

En `App.js` a la hora de importar el componente le damos el nombre Greet pero como es el export default podríamos darle el nombre que quisiéramos y funcionaría de igual modo:
```jsx
import React from 'react';
import Greet from './components/Greet';

function App() {
	return (
		<div className="App">
			<Greet />
		</div>
	);
}

export default App;
```

## Componentes de Clase
Los **componentes de clase** son básicamente clases ES6 y al igual que los **componentes funcionales** pueden recibir como entrada `props` y deben tener un método `render()` en el cual pueden retornar `jsx`. Además pueden contar con un un estado interno privado llamado `state` que puede ser usarlo para describir la UI.
Crearemos un componente de clase`Welcome.js` que realice lo mismo que el componente funcional `Greet.js`

Para que una clase  se convierta en un componente debe extender a la clase `{Component}` de `'react'` y tener un método `render()` que retorne `null` o `jsx`.

Los componentes de clase podemos crearlos con el snippet `rce` (*react class component with an export*) o `rcc` (*react class component*)
El snippet para poner `import React from 'react';` es `imr` y para poner `import React, { Component } from 'react';` es `imrc`

```jsx
import React, { Component } from 'react';

class Welcome extends Component {
	render() {
		return <h1>Hello World!</h1>;
	}
}
export default Welcome;
```

En React siempre dDebemos retornar un **root element** como por ejemplo un `div` y luego podremos anidar todos los elementos que queramos. Más adelante veremos una alternativa utilizando `React.Fragment` o `<>` que nos permite envolver lo que queremos renderizar  sin crear elementos adicionales html.

El hecho de sólo poder devolver un **root element** tiene su explicación en el hecho de que sólo podemos retornar un `React.createEleement` y no varios.

Si queremos asignarle una clase a alguno de los elementos debemos usar la `className` en lugar de `class` ya que `class` es una palabra reservada en JavaScript.

El método `render()` entrega un React Element que no es más que un objeto JavaScript que mapea con un *DOM element* (el cual no es real sino una representación en memoria). Al realizar un cambio en un componente obtenemos un nuevo React Element y React lo compara a el y a sus hijos con el previo y actualiza el DOM real.

## Comparación Componentes de Clase y Funcionales
Debemos optar por usar componentes funcionales siempre que sea posible. Una de las razones de esto es para evitar tener que lidiar con `this` *keyword*. 

Tiempo atrás a los componentes funcionales se los llamaba *stateless functional components* pero en la actualidad gracias a la aparición de los **hooks** en 2018 es posible tener *state* sin escribir una clase.

Los componentes de clase proporcionan *life cycle hooks* y en las nuevas versiones de React los componentes funcionales también cuentan con algo similar.

Antes de los hooks a los **componentes de clase** se los llamaba contenedores ya que gracias a los métodos del ciclo de vida eran capaces de **obtener datos** de una base de datos, almacenarlos en el **state** y pasarlos como **props** a **componentes funcionales** cuya función primaria era **presentar los datos** al usuario.

# Primera Aplicación
```jsx
import React from 'react';
import ReactDOM from 'react-dom';

const element = <h1>Hello World<h1>; //Babel transpilará esto conviertiéndolo en un call a React.createElement

ReactDOM.render(element, document.getElementByI('root'));
```

En una aplicación real en vez de renderizar un simple elemento, renderizaremos al *root component* y a sus hijos.

En esos casos en `index.js` tendremos:
```jsx
ReactDOM.render(<App />,document.getElementById('root'));
```

**ReactDOM** es una librería que nos permite instalar las dependencias necesarias para renderizar React en el navegador. Como primer parámetro le decimos lo que tiene que renderizar y como segundo parámetro dónde debe renderizarlo.

Mientras que en `public/index.html` tendremos `<div id="root"></div>`