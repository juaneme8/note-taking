# Componente `FieldArray`
El componente `FieldArray` está relacionado con los **campos de formularios dinámicos** y nos ayuda a realizar manipulaciones en arrays o listas. 

Si bien anteriormente manejamos el estado de un campo como array y un componente `Field` para cada uno de sus índices, típicamente cuando tenemos que recolectar múltiples números de teléfono o múltiples direcciones sólo renderizamos un único campo y les damos la opción de agregar más números si lo desean. Luego cada uno de estos números forman parte de un array en el estado del formulario.

Supongamos que queremos ahora preguntar nuevamente los números de teléfono, peor utilizando `FieldArray`

1. Importamos `FieldArray` 
`import { Formik, Form, Field, ErrorMessage, FieldArray } from 'formik';`

2. Agregamos una nueva propiedad al objeto `initialValues`
```jsx
function YoutubeForm() {
	const initialValues = {
		name: '',
		email: '',
		channel: '',
		comments: '',
		address: '',
		social: {
			facebook: '',
			twitter: '',
		},
		phoneNumbers: ['', ''],
		phNumbers: [''],
	};
```
Notar que tenemos una array con un único elemento ya que comenzaremos preguntando por un teléfono.

3.1 Agregamos el JSX comenzando por un `div` con la clase `form-control` como es habitual y un `label`.  Además agregamos el componente `FieldArray` y especificamos el atributo `name`.
```jsx
<div className='form-control'>
	<label htmlFor='secondaryPh'>Lista de Teléfonos</label>
	<FieldArray name='phNumbers'>
	
	</FieldArray>
</div>
```

3.2 Utilizamos el **render props pattern** de manera similar a lo hecho para el componente `Field`

```jsx
<div className='form-control'>
	<label htmlFor='secondaryPh'>Lista de Teléfonos</label>
	<FieldArray name='phNumbers'>
		{fieldArrayProps => {
			console.log('fieldArrayProps:', fieldArrayProps);
			return <div>Field Array</div>;
		}}
	</FieldArray>
</div>
```
Notar que retornamos un `div` sino obtendríamos eror.

Analizando `fieldArrayProps` podremos destacar los métodos `push` (lo usaremos para agregar un número de teléfono), `pop`, `remove` (lo usaremos para eliminar uno), `unshift` que se asemejan a los de operaciones comunes de arrays. Por otra parte cuenta con la propiedad `forms` que es un objeto que tiene una propiedad `values` con un array llamado en este caso `phNumbers` con los números agregados. 

Realizamos destructuring para contar con `push`, `remove` y `form` primero y luego destructuring sucesivas veces hasta obtener  el array con los datos:

Solo queremos mostrar el botón de eliminar si el `index>0` es decir si tenemos más de un elemento. Sino sería posible eliminar incluso el primero.

```jsx
<FieldArray name='phNumbers'>
	{fieldArrayProps => {
		const { push, remove, form } = fieldArrayProps;
		const { values } = form;
		const { phNumbers } = values;

		// console.log('fieldArrayProps:', fieldArrayProps);
		return (
			<div>
				{phNumbers.map((phNumbers, index) => (
					<div key={index}>
						<Field type='text' name={`phNumbers[${index}]`} />

						{index > 0 && (
							<button type='button' onClick={() => remove(index)}>
								-
							</button>
						)}
						<button type='button' onClick={() => push('')}>
							+
						</button>
					</div>
				))}
			</div>
		);
	}}
</FieldArray>
```

Otra opción es la de tener un único botón para agregar campos:


```jsx
<FieldArray name='phNumbers'>
	{fieldArrayProps => {
		const { push, remove, form } = fieldArrayProps;
		const { values } = form;
		const { phNumbers } = values;

		// console.log('fieldArrayProps:', fieldArrayProps);
		return (
			<div>
				{phNumbers.map((phNumbers, index) => (
					<div key={index}>
						<Field name={`phNumbers[${index}]`} />

						{index > 0 && (
							<button type='button' onClick={() => remove(index)}>
								-
							</button>
						)}
					</div>
				))}
				<button type='button' onClick={() => push('')}>
					+
				</button>
			</div>
		);
	}}
</FieldArray>
```
El componente completo nos queda:
```jsx
import React from 'react';
import { Formik, Form, Field, ErrorMessage, FieldArray } from 'formik';
import * as Yup from 'yup';

const initialValues = {
		name: '',
		email: '',
		channel: '',
		comments: '',
		address: '',
		social: {
			facebook: '',
			twitter: '',
		},
		phoneNumbers: ['', ''],
		phNumbers: [''],
	};

	const onSubmit = values => {
		console.log('Form Data', values);
	};

	const validationSchema = Yup.object({
		name: Yup.string().required('Requerido'),
		email: Yup.string().email('Formato Inválido').required('Requerido'),
		channel: Yup.string().required('Requerido'),
	});

function YoutubeForm() {
	return (
		<>
			<Formik initialValues={initialValues} onSubmit={onSubmit} validationSchema={validationSchema}>
				<Form>
					<div className='form-control'>
						<label htmlFor='name'>Name</label>
						<Field type='text' id='name' name='name' placeholder='Nombre de usuario' />
						<ErrorMessage name='name'>
							{errorMsg => {
								return <div className='error'>{errorMsg}</div>;
							}}
						</ErrorMessage>
					</div>
					<div className='form-control'>
						<label htmlFor='email'>Email</label>
						<Field type='email' id='email' name='email' />
						<ErrorMessage name='email' />
					</div>
					<div className='form-control'>
						<label htmlFor='channel'>Channel</label>
						<Field type='text' id='channel' name='channel' />
						<ErrorMessage name='channel' />
					</div>
					<div className='form-control'>
						<label htmlFor='comments'>Comments</label>
						<Field as='textarea' id='comments' name='comments' />
						<ErrorMessage name='comments' />
					</div>
					<div className='form-control'>
						<label htmlFor='address'>Address</label>
						<Field type='text' name='address'>
							{props => {
								const { field, form, meta } = props;
								// console.log('Render props:', props);
								return (
									<div>
										<input type='text' id='address' {...field} />
										{meta.touched && meta.error ? <div>{meta.error}</div> : null}
									</div>
								);
							}}
						</Field>
					</div>
					<div className='form-control'>
						<label htmlFor='facebook'>Facebook Profile</label>
						<Field type='text' id='facebook' name='social.facebook' />
						<ErrorMessage name='facebook' />
					</div>
					<div className='form-control'>
						<label htmlFor='twitter'>Twitter Profile</label>
						<Field type='text' id='twitter' name='social.twitter' />
						<ErrorMessage name='twitter' />
					</div>
					<div className='form-control'>
						<label htmlFor='primaryPh'>Teléfono Primario</label>
						<Field type='text' id='primaryPh' name='phoneNumbers[0]' />
						<ErrorMessage name='primaryPh' />
					</div>
					<div className='form-control'>
						<label htmlFor='secondaryPh'>Teléfono Secundario</label>
						<Field type='text' id='secondaryPh' name='phoneNumbers[1]' />
						<ErrorMessage name='secondaryPh' />
					</div>

					<div className='form-control'>
						<label htmlFor='secondaryPh'>Lista de Teléfonos</label>
						<FieldArray name='phNumbers'>
							{fieldArrayProps => {
								const { push, remove, form } = fieldArrayProps;
								const { values } = form;
								const { phNumbers } = values;

								// console.log('fieldArrayProps:', fieldArrayProps);
								return (
									<div>
										{phNumbers.map((phNumbers, index) => (
											<div key={index}>
												<Field name={`phNumbers[${index}]`} />

												{index > 0 && (
													<button type='button' onClick={() => remove(index)}>
														-
													</button>
												)}
											</div>
										))}
										<button type='button' onClick={() => push('')}>
											+
										</button>
									</div>
								);
							}}
						</FieldArray>
					</div>

					<button type='submit'>Submit</button>
				</Form>
			</Formik>
		</>
	);
}

export default YoutubeForm;

```