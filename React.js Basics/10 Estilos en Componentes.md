# Estilo en Componentes
Existen distintas opcioens para darle estilo a componentes:
* **CSS Stylesheets**
* **Inline styling**
* **CSS Modules**
* **CSS in JS Libraries** (no será cubierto pero recomienda **styled components**)

## CSS Stylesheets
Creamos un componente funcional llamado `Stylesheet` y también dentro de la carpeta `/components` creamos un archivo `myStyles.css` donde colocamos los estilos deseados. A continuación en el componente importamos esta hoja de estilos y aplicamos la clase con el atributo `className`

```jsx
import React from 'react';
import './myStyles.css';
function Stylesheet() {
	return (
		<div>
			<h1 className="primary">Stylesheet</h1>
		</div>
	);
}

export default Stylesheet;

```
La ventaja de este método es que agrega automáticamente los vendor prefixes, si escribimos.
```css
transition: all 1s;
```
A la salida obtendremos:
```css
-webkit-transition: all 1s;
-o-transition: all 1s;
transition: all 1s;
```

También podríamos aplicar una clase condicionalmente de acuerdo a `props` o `state`. Por ejemplo en `App.js` podríamos pasarle una `prop` `<Stylesheet primary={false} />`

```jsx
import React from 'react';
import './myStyles.css';
function Stylesheet(props) {
	const className = props.primary ? 'primary' : '';
	return (
		<div>
			<h1 className={className}>Stylesheet</h1>
		</div>
	);
}

export default Stylesheet;

```

Si queremos especificar múltiples clases la forma más simple es usando *template literals* por ejemplo:
```jsx
 `<h1 className={`${className} font-xl`}>Stylesheets</h1>`
```
Otra opción es usar la library https://www.npmjs.com/package/classnames la cual nos permitirá una mayor claridad.

Los estilos aplicados de este modo afectan tanto a componentes hijos como al componente padre

## Inline Styling
Creamos un componente funcional `Inline.js`. En React los estilos en línea no son especificados como un string sino como un objeto **propiedad** sin guiones y en camelCase y el **valor** como string.
```jsx
import React from 'react';

function Inline() {
	const heading = {
		marginTop: '100px',
		fontSize: '2em',
		color: 'red',
	};
	return (
		<div>
			<h1 style={heading}>Inline</h1>
		</div>
	);
}

export default Inline;
```

## CSS Modules
CSS Modules está disponible con `react-scripts`  versión 2 o superior (chequear en `package.json` la versión con la que estamos trabajando).

Utilizamos como *file naming convention*  de modo tal que terminen en `.module.css` por ejemplo `appStyles.module.css`

Supongamos que en `src` creamos una *CSS module stylesheet* `appStyles.modules.css` y también una *regular stylesheet* llamada `appStyles.css`

Luego importamos ambas clases a `App.js` y lo hacemos de la siguiente forma

```jsx
import './appStyles.css';
import styles from './appStyles.module.css';

```
Finalmente podremos utilizarlas así:
```jsx
<h1 className="error">Error</h1>
<h1 className={styles.success}>Success</h1>
```

Dentro de `App` podremos usar ambas hojas de estilo como veremos a continuación:

La ventaja de usar *CSS Module stylesheets* es que los estilos tienen un *scope local* por default, lo cual hace que no sean aplicados a componentes hijos evitando así posibles conflictos por uso accidental. Si queremos utilizarlos en el componente hijo al estar referenciando a la variable `styles` nos dirá que no sabe qué es.
En cambio si usamos *regular stylesheets* los estilos se aplicarán por default en los componentes hijos y también en el padre.