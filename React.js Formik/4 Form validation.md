# Form Validation
En cuanto a las reglas de validación para el formulario con el que estamos trabajando establecemos que todos los campos (`name`, `email`, `channel`) son **required** y que `email` debe tener un formato de email válido.

Agregamos una nueva propiedad al objeto que le pasamos a `useFormik` llamada `validate`. Al igual que en el caso anterior se trata de una función que recibe automáticamente al objeto de valores como argumento.
Esta función debe retornar un objeto **errors** que para cada error tenga una propiedad con un string indicando cuál es el error detectado.
Esta función será ejecutada cada vez que cambiemos un valor en el input.

Para verificar que el email tenga un formato válido utilizo la siguiente `RegEx`:
`/^[A-Z0-9._%+-]+@[A-Z0-9.-]+\.[A-Z]{2,4}$/i`

```jsx
const formik = useFormik({
	initialValues: {
		name: '',
		email: '',
		channel: '',
	},
	onSubmit: values => {
		console.log('Form Data', values);
	},
	validate: values => {
		let errors = {};
		if (!values.name) {
			errors.name = 'Requerido';
		}
		if (!values.email) {
			errors.email = 'Requerido';
		} else if (!/^[A-Z0-9._%+-]+@[A-Z0-9.-]+\.[A-Z]{2,4}$/i.test(values.email)) {
			errors.email = 'Formato inválido';
		}
		if (!values.channel) {
			errors.channel = 'Requerido';
		}

		return errors;
	},
});
```

Para mejorar la facilidad de lectura hacemos un *refactoring*

```jsx
	const initialValues = {
		name: '',
		email: '',
		channel: '',
	};

	const onSubmit = values => {
		console.log('Form Data', values);
	};

	const validate = values => {
		let errors = {};
		if (!values.name) {
			errors.name = 'Requerido';
		}
		if (!values.email) {
			errors.email = 'Requerido';
		} else if (!/^[A-Z0-9._%+-]+@[A-Z0-9.-]+\.[A-Z]{2,4}$/i.test(values.email)) {
			errors.email = 'Formato inválido';
		}
		if (!values.channel) {
			errors.channel = 'Requerido';
		}

		return errors;
	};

function YouTubeForm(){

	const formik = useFormik({
		initialValues,
		onSubmit,
		validate,
	});

(...)
}
```

## Mostrando Errores
Así como cuando hicimos `console.log(formik.values)` vimos los valores de los campos actualizados, si hacemos `console.log(formik.errors)` podremos ver los errores de validación. 
Cuando la página carga por primera vez es un objeto vacío y en la medida que vamos llenando uno de los campos el objeto errores deja de estar vacío y pasa a mostrar los errores encontrados.
Por lo tanto procesamos `formik.errors` mediante jsx usando el *ternary operator* para mostrar cada uno de ellos sólo en caso de que exista:
```jsx
{formik.errors.name ? <div className='error'>{formik.errors.name}</div> : null}
```
Con la clase `.error` ponemos la letra del mensaje de error en color rojo.

A su vez por motivos de estilo envolvemos a `label`, `input` y la línea anterior en un `<div className="form-control>`

```jsx
import React from 'react';
import { useFormik } from 'formik';

const initialValues = {
		name: '',
		email: '',
		channel: '',
	};

	const onSubmit = values => {
		console.log('Form Data', values);
	};

	const validate = values => {
		let errors = {};
		if (!values.name) {
			errors.name = 'Requerido';
		}
		if (!values.email) {
			errors.email = 'Requerido';
		} else if (!/^[A-Z0-9._%+-]+@[A-Z0-9.-]+\.[A-Z]{2,4}$/i.test(values.email)) {
			errors.email = 'Formato inválido';
		}
		if (!values.channel) {
			errors.channel = 'Requerido';
		}

		return errors;
	};


function YoutubeForm() {
	
	const formik = useFormik({
		initialValues,
		onSubmit,
		validate,
	});

	return (
		<div>
			<form onSubmit={formik.handleSubmit}>
				<div className='form-control'>
					<label htmlFor='name'>Name</label>
					<input type='text' id='name' name='name' onChange={formik.handleChange} value={formik.values.name} />
					{formik.errors.name ? <div className='error'>{formik.errors.name}</div> : null}
				</div>
				<div className='form-control'>
					<label htmlFor='email'>Email</label>
					<input type='email' id='email' name='email' onChange={formik.handleChange} value={formik.values.email} />
					{formik.errors.email ? <div className='error'>{formik.errors.email}</div> : null}
				</div>
				<div className='form-control'>
					<label htmlFor='channel'>Channel</label>
					<input type='text' id='channel' name='channel' onChange={formik.handleChange} value={formik.values.channel} />
					{formik.errors.channel ? <div className='error'>{formik.errors.channel}</div> : null}
				</div>
				<button type='submit'>Submit</button>
			</form>
		</div>
	);
}

export default YoutubeForm;

```
## Error en Campos Visitados

Hasta ahora podemos ver que si bien inicialmente no nos aparece ningún error, ni bien completamos un campo nos mostrará error en todos los otros que aún ni siquiera visitó diciéndonos que son requeridos. Esto no es deseado pues atenta contra la UX.
Debemos saber si un campo fue visitado y sólo ahí mostrar el error asociado al mismo. Esto lo hacemos colocando en los input  `onBlur={formik.handleBlur}` y luego accedemos al objeto `formik.touched` (de la misma manera que accedimos a `formik.values` y `formik.errors`). Primero será un objeto vacío y luego tendrá una propiedad con el nombre del input en `true` al visitarlo. Por ejemplo si hacemos click sobre el input de `name` y luego click afuera, `formik.touched = {name: true}`

```jsx
import React from 'react';
import { useFormik } from 'formik';

const initialValues = {
		name: '',
		email: '',
		channel: '',
	};

	const onSubmit = values => {
		console.log('Form Data', values);
	};

	const validate = values => {
		let errors = {};
		if (!values.name) {
			errors.name = 'Requerido';
		}
		if (!values.email) {
			errors.email = 'Requerido';
		} else if (!/^[A-Z0-9._%+-]+@[A-Z0-9.-]+\.[A-Z]{2,4}$/i.test(values.email)) {
			errors.email = 'Formato inválido';
		}
		if (!values.channel) {
			errors.channel = 'Requerido';
		}

		return errors;
	};

function YoutubeForm() {
	const formik = useFormik({
		initialValues,
		onSubmit,
		validate,
	});

	// console.log('Visited Fields', formik.touched);

	return (
		<div>
			<form onSubmit={formik.handleSubmit}>
				<div className='form-control'>
					<label htmlFor='name'>Name</label>
					<input type='text' id='name' name='name' onChange={formik.handleChange} onBlur={formik.handleBlur} value={formik.values.name} />
					{formik.touched.name && formik.errors.name ? <div className='error'>{formik.errors.name}</div> : null}
				</div>
				<div className='form-control'>
					<label htmlFor='email'>Email</label>
					<input
						type='email'
						id='email'
						name='email'
						onChange={formik.handleChange}
						onBlur={formik.handleBlur}
						value={formik.values.email}
					/>
					{formik.touched.email && formik.errors.email ? <div className='error'>{formik.errors.email}</div> : null}
				</div>
				<div className='form-control'>
					<label htmlFor='channel'>Channel</label>
					<input
						type='text'
						id='channel'
						name='channel'
						onChange={formik.handleChange}
						onBlur={formik.handleBlur}
						value={formik.values.channel}
					/>
					{formik.touched.channel && formik.errors.channel ? <div className='error'>{formik.errors.channel}</div> : null}
				</div>
				<button type='submit'>Submit</button>
			</form>
		</div>
	);
}

export default YoutubeForm;

```

## Método Alternativo con `yup`
Existe otra forma más simple de escribir reglas de validación (sin tener que recurrir a RegEx para validar un email por ejemplo) y es usando la biblioteca`yup`
`npm i yup`

1. Importar yup con `import * as Yup from 'yup'`

2. Crear un Schema de validación con `const validationSchema = Yup.object({...});`

3. Pasarle el Schema a `useFormik()` en lugar de `validate`
```jsx
const validationSchema = Yup.object({
	name: Yup.string().required('Requerido'),
	email: Yup.string().email('Formato Inválido').required('Requerido'),
	channel: Yup.string().required('Requerido'),
});

const YoutubeForm(){
	const formik = useFormik({
		initialValues,
		onSubmit,
		validationSchema
		// validate,
	});

	(...)
}

```

Notar que ponemos `required('Requerido')` porque queremos que aparezca ese mensaje y no el mensaje por default que en el caso del campo `name` sería  `name is a required field`