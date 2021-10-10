# Form Submission
1. En el `form` debemos especificar `onSubmit={formik.handleSubmit}`

2. Agregamos una nueva propiedad `onSubmit` en el objeto que le pasamos al hook `useFormik()`, se trata de un método que recibe automáticamente el estado del formulario como argumento y será ejecutado al hacer el submit.

```jsx
import React from 'react';
import { useFormik } from 'formik';
function YoutubeForm() {
	const formik = useFormik({
		initialValues: {
			name: '',
			email: '',
			channel: '',
		},
		onSubmit: values => {
			console.log('Form Data', values);
		},
	});

	return (
		<div>
			<form onSubmit={formik.handleSubmit}>
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

En este caso simplemente estamos imprimiendo el valor en pantalla pero en una aplicación real puede que hagamos un llamado a una API con los valores como *payload*.