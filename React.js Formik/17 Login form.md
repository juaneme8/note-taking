# Login Form
Utilizaremos los Form Controls creados.

Crearemos un formulario de Login con Email (required y valid) y Password (required).

Creamos el componente `LoginForm.`

```jsx
import React from 'react';
import { Formik, Form } from 'formik';
import * as Yup from 'yup';
import FormikControl from './FormikControl';

function LoginForm() {
	const initialValues = {
		email: '',
		password: '',
	};
	const validationSchema = Yup.object({
		email: Yup.string().email('Formato Inválido').required('Requerido'),
		password: Yup.string().required('Requerido'),
	});

	const onSubmit = values => {
		console.log('Form Data', values);
	};

	return (
		<Formik initialValues={initialValues} validationSchema={validationSchema} onSubmit={onSubmit}>
			{formik => {
				return (
					<Form>
						<FormikControl control='input' type='email' label='Email' name='email' />
						<FormikControl control='input' type='password' label='Contraseña' name='password' />
						<button type='submit' disabled={!formik.isValid}>
							Submit
						</button>
					</Form>
				);
			}}
		</Formik>
	);
}

export default LoginForm;

```