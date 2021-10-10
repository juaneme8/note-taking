# `ErrorMessage` en profundidad
Si inspeccionamos el mensaje de error que nos aparece veremos que se trata de un nodo de texto sin ningún elemento html.
Para indicar que queremos que el mensaje de error esté envuelto por un elemento html debemos usar la `prop` `component`.

Tomamos como ejemplo el input de `name` que hasta el momento tenemos así:
```jsx
<ErrorMessage name='name' />
```
Si queremos que quede envuelto por un `div` podemos ponerle:
```jsx
<ErrorMessage name='name' component='div' />
```
Sin embargo podemos indicar cualquier elemento html e incluso un *custom component*

## Estilo del Mensaje
Hasta el momento el texto del error no se muestra en color rojo como queremos. Para solucionar esto creamos un componente que muestre su texto en color rojo y le pasamos ese componente a la prop `component`.

Creamos un componente `TextError`
```jsx
import React from 'react';

function TextError(props) {
	return <div className='error'>{props.children}</div>;
}

export default TextError;

```
Luego en `YoutubeForm2` importamos `TextError` y colocamos:
```jsx
<ErrorMessage name='name' component={TextError} />
```

El componente completo nos queda:

```jsx
import React from 'react';
import { Formik, Form, Field, ErrorMessage } from 'formik';
import * as Yup from 'yup';
import TextError from './TextError';
	const initialValues = {
		name: '',
		email: '',
		channel: '',
		comments: '',
		address: '',
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
						<ErrorMessage name='name' component={TextError} />
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

					<button type='submit'>Submit</button>
				</Form>
			</Formik>
		</>
	);
}

export default YoutubeForm;
```

# Render Props
## Método Alternativo
Una alternativa es usar el patrón *render props* para mostrar el JSX deseado que en este caso será un div con un la clase `error`.

Lo primero que hacemos es cambiar `ErrorMessage` de modo que tenga tag de apertura y cierre.

```jsx
<ErrorMessage name='name'>
	{
		(errorMsg) =>{
			return <div className='error'>{errorMsg}</div>
		}
	}
</ErrorMessage>
```

El componente completo nos queda:
```jsx
import React from 'react';
import { Formik, Form, Field, ErrorMessage } from 'formik';
import * as Yup from 'yup';

const initialValues = {
		name: '',
		email: '',
		channel: '',
		comments: '',
		address: '',
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

					<button type='submit'>Submit</button>
				</Form>
			</Formik>
		</>
	);
}

export default YoutubeForm;

```