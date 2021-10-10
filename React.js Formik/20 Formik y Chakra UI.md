# Integrar Chakra UI con Formik
Utilizaremos **Chakra UI**, pero el procedimiento es similar si queremos usar por ejemplo **Material UI**.

## Instalación Chakra UI
De acuerdo a lo indicado en https://chakra-ui.com/docs/getting-started tenemos que instalar:

 `npm i @chakra-ui/react @emotion/react @emotion/styled framer-motion`

En `index.js` importar `import { ChakraProvider } from '@chakra-ui/react';` y luego colocar `<ChakraProvider>`

```jsx
ReactDOM.render(
	<React.StrictMode>
		<ChakraProvider>
			<App />
		</ChakraProvider>
	</React.StrictMode>,
	document.getElementById('root')
);
```

# Chakra Input Component
Se trata de un Input FormControl que usa componentes de Chakra para el estilo.

De manera similar a lo hecho anteriormente dividiremos todo en tres pasos.

1. Creamos el componente `ChakraInput.js`

```jsx
import React from 'react';
import { Field } from 'formik';
import { Input, FormControl, FormLabel, FormErrorMessage } from '@chakra-ui/react';

function ChakraInput(props) {
	const { label, name, ...rest } = props;
	return (
		<Field name={name}>
			{({ field, form }) => {
                return <FormControl isInvalid={form.erros[name] && form.touched[name]}>
                    <FormLabel html={name}>{label}</FormLabel>
                    <Input id={name} {...rest} {...field} />
                    <FormErrorMessage>{form.errors[name}</FormErrorMessage>
                </FormControl>;
			}}
		</Field>
	);
}

export default ChakraInput;

```

Podemos compararlo con el `Input` en particular con esta parte:

```jsx
<div className='form-control'>
	<label htmlFor={name}>{label}</label>
	<Field id={name} name={name} {...rest} />
	<ErrorMessage name={name} component={TextError} />
</div>
```

En `FormikControl` agregamos un nuevo switch:
```jsx
import React from 'react';
import Input from './Input';
import Textarea from './Textarea';
import Select from './Select';
import RadioButtons from './RadioButtons';
import CheckboxGroup from './CheckboxGroup';
import DatePicker from './DatePicker';
import ChakraInput from './ChakraInput';

function FormikControl(props) {
	const { control, ...rest } = props;
	switch (control) {
		case 'input':
			return <Input {...rest} />;
		case 'textarea':
			return <Textarea {...rest} />;
		case 'select':
			return <Select {...rest} />;
		case 'radio':
			return <RadioButtons {...rest} />;
		case 'checkbox':
			return <CheckboxGroup {...rest} />;
		case 'date':
			return <DatePicker {...rest} />;
		case 'chakrainput':
			return <ChakraInput {...rest} />;
		default:
			return null;
	}
	return <div></div>;
}

export default FormikControl;

```

En `LoginForm` cambiamos `control='input'` por `control='chakrainput'` 

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
						<FormikControl control='chakrainput' type='email' label='Email' name='email' />
						<FormikControl control='chakrainput' type='password' label='Contraseña' name='password' />
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

En `App.css` eliminamos los estilos para evitar conflictos.

