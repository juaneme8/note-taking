## Reducing Boilerplate
```jsx
<input
	type='text'
	id='channel'
	name='channel'
	onChange={formik.handleChange}
	onBlur={formik.handleBlur}
	value={formik.values.name}
/>
```

El código utilizado hasta ahora es muy explícito acerca de lo que Formik está haciendo tenenemos:   `onChange`  ->  `handleChange`,  `onBlur`  ->  `handleBlur`, etc. Sin embargo para ahorrarnos tiempo `useFormik()`  retorna un *helper method* llamado  `formik.getFieldProps()`  para lograr un *wire up* de los inputs más rápido. Pasándole información sobre el campo obtenemos el grupo de  `onChange`,  `onBlur`,  `value`,  `checked`  para ese campo. Luego podremos hacer el *spread* en un input, select, o textarea.

```jsx
<input 
	type='text' 
	id='name' 
	name='name' 
	{...formik.getFieldProps('name')} 
/>
```

Creamos un nuevo componente `YoutubeForm2.js` que iniciamos como una copia idéntica de `YoutubeForm` e iremos simplificando:

```jsx
import React from 'react';
import { useFormik } from 'formik';
import * as Yup from 'yup';

const initialValues = {
		name: '',
		email: '',
		channel: '',
	};

	const onSubmit = values => {
		console.log('Form Data', values);
	};

	// const validate = values => {
	// 	let errors = {};
	// 	if (!values.name) {
	// 		errors.name = 'Requerido';
	// 	}
	// 	if (!values.email) {
	// 		errors.email = 'Requerido';
	// 	} else if (!/^[A-Z0-9._%+-]+@[A-Z0-9.-]+\.[A-Z]{2,4}$/i.test(values.email)) {
	// 		errors.email = 'Formato inválido';
	// 	}
	// 	if (!values.channel) {
	// 		errors.channel = 'Requerido';
	// 	}

	// 	return errors;
	// };

	const validationSchema = Yup.object({
		name: Yup.string().required('Requerido'),
		email: Yup.string().email('Formato Inválido').required('Requerido'),
		channel: Yup.string().required('Requerido'),
	});
	
function YoutubeForm() {
	
	const formik = useFormik({
		initialValues,
		onSubmit,
		validationSchema,
		// validate,
	});

	// console.log('Visited Fields', formik.touched);

	return (
		<div>
			<h1>¡Bienvenidos a YoutubeForm2!</h1>
			<form onSubmit={formik.handleSubmit}>
				<div className='form-control'>
					<label htmlFor='name'>Name</label>
					<input type='text' id='name' name='name' {...formik.getFieldProps('name')} />
					{formik.touched.name && formik.errors.name ? <div className='error'>{formik.errors.name}</div> : null}
				</div>
				<div className='form-control'>
					<label htmlFor='email'>Email</label>
					<input type='email' id='email' name='email' {...formik.getFieldProps('email')} />
					{formik.touched.email && formik.errors.email ? <div className='error'>{formik.errors.email}</div> : null}
				</div>
				<div className='form-control'>
					<label htmlFor='channel'>Channel</label>
					<input type='text' id='channel' name='channel' {...formik.getFieldProps('channel')} />
					{formik.touched.channel && formik.errors.channel ? <div className='error'>{formik.errors.channel}</div> : null}
				</div>
				<button type='submit'>Submit</button>
			</form>
		</div>
	);
}

export default YoutubeForm;

```