# `useMemo`
Este hook también tiene relación con la **optimización de performance**.
Trabajaremos con un componente `Counter` que tiene dos contadores manejados mediante variables de estado. La idea es contar con dos botones que muestren el valor del contador y al presionarlos que incrementen la cuenta. Además cercano al primer botón debemos mostrar una leyenda que indique si el número de dicho contador es par o impar.

```jsx
import React, { useState } from 'react';

function Counter() {
	const [counterOne, setCounterOne] = useState(0);
	const [counterTwo, setCounterTwo] = useState(0);

	const incrementOne = () => {
		setCounterOne(counterOne + 1);
	};
	const incrementTwo = () => {
		setCounterTwo(counterTwo + 1);
	};

	const isEven = () => {
		return counterOne % 2 === 0;
	};

	return (
		<div>
			<div>
				<button onClick={incrementOne}>Count One - {counterOne}</button>
				{isEven() ? 'PAR' : 'IMPAR'}
			</div>
			<div>
				<button onClick={incrementTwo}>CounttTwo - {counterTwo}</button>
			</div>
		</div>
	);
}

export default Counter;

```

En este caso `isEven` no tiene una lógica demandante en términos de performance, pero en ocasiones tendremos que obtener una lista de muchos items, realizar un mapeo del array y un ordenamiento lo cual lleva bastante tiempo de ejecución. Para simular esa lentitud agregamos un `while (i < 2000000000) i++;`
Al presionar el botón 1 veremos que pasa uno o dos segundos hasta que se actualiza el valor del contador y la leyenda PAR/IMPAR. Lo mismo ocurre cuando presionamos el contador 2 incluso cuando el cambio en el contador 2 no afecta al valor obtenido por `isEven()`. Esto se debe a que como cambia el estado el componente vuelve a renderizarse y al hacerlo se vuelve a llamar a `isEven()`. Necesitamos decirle a React que no recalcule algunos valores cuando no es necesario, **en especial aquellos que demandan mucho tiempo en computarse**.
Necesitamos decirle a React que no recalcule si el contador 1 es par o impar cuando estamos cambiando valores del contador 2, esto lo hacemos con el hook `useMemo`.

A `useMemo()` le pasamos como primer parámetro una arrow function que retorne el valor que queremos cachear y como segundo un array de dependencias y sólo debe recalcular el valor si una de sus dependencias cambió.


```jsx
import React, { useState, useMemo } from 'react';

function Counter() {
	const [counterOne, setCounterOne] = useState(0);
	const [counterTwo, setCounterTwo] = useState(0);

	const incrementOne = () => {
		setCounterOne(counterOne + 1);
	};
	const incrementTwo = () => {
		setCounterTwo(counterTwo + 1);
	};

	const isEven = useMemo(() => {
		let i = 0;
		while (i < 2000000000) i++;
		return counterOne % 2 === 0;
	}, [counterOne]);

	return (
		<div>
			<div>
				<button onClick={incrementOne}>Count One - {counterOne}</button>
				{isEven ? 'PAR' : 'IMPAR'}
			</div>
			<div>
				<button onClick={incrementTwo}>Count Two - {counterTwo}</button>
			</div>
		</div>
	);
}

export default Counter;

```

Notar que donde tenemos `isEven` antes teníamos `isEven()` porque era una llamada a función ahora es un valor.

Ahora cuando hagamos click en el botón del contador 1 el delay estará presente pero no lo veremos cuando hagamos click en el contador 2.

## Diferencias `useCallback` y `useMemo`
Si bien tanto `useCallback` como `useMemo` son hooks utilizados para lograr optimización de performance, `useCallback` cachea la instancia de la función provista y `useMemo` invoca la función provista y cachea el valor devuelto por la función.