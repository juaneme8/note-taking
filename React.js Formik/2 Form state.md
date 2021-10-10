# Form State
1. En primer lugar a `useFormik()` le pasamos una propiedad llamada `initialValues`que se trata de un objeto con los valores iniciales de cada uno de los campos del formulario (las propiedades coinciden con el atributo `name` de cada uno de los `input`).

```jsx
const formik = useFormik({
	initialValues: {
		name: '',
		email: '',
		channel: '',
	}
});
```

2. A continuación agregamos `onChange` y `value` a los `input` de la siguiente forma:
```jsx
<label htmlFor='name'>Name</label>
<input type='text' id='name' name='name' onChange={formik.handleChange} value={formik.values.name} />
```

Si hacemos `console.log(formik.values)` antes del `return` veremos que se trata de un objeto que tiene como propiedades a cada uno de los campos del formulario con sus valores actualizados a cada momento. 
Es por eso que lo estoy luego vinculando al atributo **value** por tratarse de **componentes controlados** (normalmente lo asocio a **variables de estado** tanto en componentes de clase con `this.state.variable` o en componentes funcionales con la variable obtenida de usar `useState()`).

El código completo de `YoutubeForm` nos queda:

```jsx
import React from 'react';
import { useFormik } from 'formik';
function YoutubeForm() {
	const formik = useFormik({
		initialValues: {
			name: '',
			email: '',
			channel: '',
		}
	});
	console.log(formik.values);
	return (
		<div>
			<form>
				<label htmlFor='name'>Name</label>
				<input type='text' id='name' name='name' onChange={formik.handleChange} value={formik.values.name} />
				<label htmlFor='email'>Email</label>
				<input type='email' id='email' name='email' onChange={formik.handleChange} value={formik.values.email} />
				<label htmlFor='channel'>Channel</label>
				<input type='text' id='channel' name='channel' onChange={formik.handleChange} value={formik.values.channel} />
				<button type='submit'>Submit</button>
			</form>
		</div>
	);
	}

export default YoutubeForm;

```