# `useCallback`
En primer lugar veremos algunos conceptos de  **optimización de performance** para luego entender la motivación detrás de `useCallback`.
Contamos con `ParentComponent` tiene dos variables de estado (`age` y `salary`)  y métodos para incrementalas (`incrementAge()` y `incrementSalary`). Luego le pasa todo esto a componentes hijos:

```jsx
<Title />
<Count text="Age" count={age} />
<Button handleClick={incrementAge}>Inc. Age</Button>
<Count text="Salary" count={salary} />
<Button handleClick={incrementSalary}>Inc. Salary</Button>
```

`ParentComponent`
```jsx
import React, { useState } from 'react';
import Title from './Title';
import Count from './Count';
import Button from './Button';

function ParentComponent() {
	const [age, setAge] = useState(25);
	const [salary, setSalary] = useState(5000);
	const incrementAge = () => {
		setAge(age + 1);
	};
	const incrementSalary = () => {
		setSalary(salary + 1000);
	};
	return (
		<div>
			<Title />
			<Count text="Age" count={age} />
			<Button handleClick={incrementAge}>Increment Age</Button>
			<Count text="Salary" count={salary} />
			<Button handleClick={incrementSalary}>Ingrement Salary</Button>
		</div>
	);
}

export default ParentComponent;

```

`Title`
```jsx
import React from 'react';

function Title() {
	console.log('Rendering Title');
	return <h2>useCallback Hook</h2>;
}

export default Title;

```

`Count`
```jsx
import React from 'react';

function Count({ text, count }) {
	console.log(`Rendering ${text}`);
	return (
		<div>
			{text} - {count}
		</div>
	);
}

export default Count;

```

`Button`
```jsx
import React from 'react';

function Button({ handleClick, children }) {
	console.log(`Rendering Button - ${children}`);
	return (
		<div>
			<button onClick={handleClick}>{children}</button>
		</div>
	);
}

export default Button;

```

Al ejecutar el programa primero veremos los 5 `console.log()` correspondientes a cada componente. Sin embargo, si hacemos click en el botón `Increment Age` todos los componentes vuelven a renderizarse. Si bien esto no es un problema en este caso, en situaciones con muchos componentes o con lógica compleja puede ocasionar inconvenientes de performance. Tendríamos que restringir los re-renderizados sólo a aquellos componentes que lo necesitan.

```bash
Rendering Title
Count.js:4 Rendering Age
Button.js:4 Rendering Button - Increment Age
Count.js:4 Rendering Salary
Button.js:4 Rendering Button - Ingrement Salary
```

Al presionar el botón `Increment Age` sólo deberían re-renderizarse ese `Button` y el `Counter` de `Age`. 

En primer lugar debemos utilizar el HOC `React.memo()` para evitar que el componente sea re-renderizado si sus props o state no cambiaron. Al exportar lo agregamos en `Title`, `Count` y `Button`
Por ejemplo en `Title` tendremos:
```jsx
export default React.memo(Title);
```

Al hacer click ahora sobre `Increment Age` veremos que no se re-renderizan los 5 componentes sino que sólo lo hacen:
```bash
Rendering Age
Rendering Button - Increment Age
Rendering Button - Ingrement Salary
```
Al haber hecho click sobre `Increment Age` accedemos al método:
```jsx
const incrementAge = () => {
	setAge(age + 1);
};
```
Como podemos ver es dependiente de `age` por lo que cuando cambia `age` ambos componentes (`Count` y `Button`) deben volver a renderizarse.

La pregunta sería entonces porqué el botón de `Increment Salary` vuelve a renderizarse a pesar de haber presionado el de Age. Esto sucede debido a que cada vez que el componente padre se vuelve a renderizar, se crea una nueva función `incrementSalary()` (con una nueva referencia) por lo que cambian las `props` que recibe el componente `Button` de salario, y es por eso que `React.memo()` no prviene el re-renderizado.
Debemos indicarle a React que no hay necesidad de crear una nueva función `incrementSalary()` cuando incrementamos la edad y esto lo hacemos con el hook `useCallback`.


## Introducción a `useCallback`
`useCallback` es un hook que retorna una versión memorizada de una función callback que sólo cambia si cambia una de sus dependencias. En nuestro caso el `useCallback` va a cachear la función `incrementSalary` y retornarla si `salary` no cambió, mientras que si cambió retornará una nueva función. 
Es útil cuando le pasamos *callbacks* a componentes hijos optimizados (con `Rect.memo`), que confían en la igualdad de referencia para prevenir renderizados ineficientes.
Utilizamos entonces `useCallback` que acepta una arrow function como primer parámetro y una array de dependencias como segundo:
```jsx
import React, { useState, useCallback } from 'react';
import Title from './Title';
import Count from './Count';
import Button from './Button';

function ParentComponent() {
	const [age, setAge] = useState(25);
	const [salary, setSalary] = useState(5000);

	const incrementAge = useCallback(() => {
		setAge(age + 1);
	}, [age]);

	const incrementSalary = useCallback(() => {
		setSalary(salary + 1000);
	}, [salary]);

	console.log('Rendering Parent');
	return (
		<div>
			<Title />
			<Count text="Age" count={age} />
			<Button handleClick={incrementAge}>Increment Age</Button>
			<Count text="Salary" count={salary} />
			<Button handleClick={incrementSalary}>Ingrement Salary</Button>
		</div>
	);
}

export default ParentComponent;

```
Lo aconsejable es utilizar `useCallback` sólo cuando lo necesitamos y no todo el tiempo ya que de acuerdo al articulo https://kentcdodds.com/blog/usememo-and-usecallback toda optimización viene con un costo pero no siempre viene con un beneficio que justifique dicho costo.