# Course Enrollment Form
El formulario de suscripción a cursos consta de los siguientes **elementos**:
* email
* bio
* course (que es un dropdown)
* skills (checkboxes HTMl, CSS, JavaScript) 
* fecha del curso (datepicker)

En cuanto a la **validación** tenemos los siguiente requisitos:
* email es requerido y debe estar en el formato correcto
* bio es requerido
* course es requerido
* skills es requerido
* fecha es requerido

```jsx
import React from 'react';
import { Formik, Form } from 'formik';
import * as Yup from 'yup';
import FormikControl from './FormikControl';

function EnrollmentForm() {
	const dropdownOptions = [
		{ key: 'Select your Course', value: '' },
		{ key: 'React', value: 'react' },
		{ key: 'Angular', value: 'angular' },
		{ key: 'Vue', value: 'vue' },
	];
	const checkboxOptions = [
		{
			key: 'HTML',
			value: 'html',
		},
		{
			key: 'CSS',
			value: 'css',
		},
		{
			key: 'JavaScript',
			value: 'javascript',
		},
	];
	const initialValues = {
		email: '',
		bio: '',
		course: '',
		skills: [],
		courseDate: null,
	};
	const validationSchema = Yup.object({
		email: Yup.string().email('Formato Incorrecto').required('Requerido'),
		bio: Yup.string().required('Requerido'),
		course: Yup.string().required('Requerido'),
		skills: Yup.string().required('Requerido'),
		courseDate: Yup.date().required('Requerido').nullable(),
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
						<FormikControl control='textarea' label='Bio' name='bio' />
						<FormikControl control='select' label='Course' name='course' options={dropdownOptions} />
						<FormikControl control='checkbox' label='Your Skillset' name='skills' options={checkboxOptions} />
						<FormikControl control='date' label='Course Date' name='courseDate' />
						<button type='submit' disabled={!formik.isValid}>
							Submit
						</button>
					</Form>
				);
			}}
		</Formik>
	);
}

export default EnrollmentForm;

```