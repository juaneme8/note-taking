# Reusable Formik Controls
La idea es crear una serie de controles que podamos utilizar en formularios de registro, login, etc.


`npx create-react-app controls` 
`npm i formik yup`

El primer paso será crear un contenedor de Formik que usaremos para testear los distintos campos. Para ello creamos la carpeta `components` en `src` y el componente `FormikContainer`

```jsx
import React from 'react';
import { Formik, Form } from 'formik';
import * as Yup from 'yup';

function FormikContainer() {
	const initialValues = {};
	const validationSchema = Yup.object({});
	const onSubmit = values => console.log('Form Data', values);
	return (
		<Formik initialValues={initialValues} validationSchema={validationSchema} onSubmit={onSubmit}>
			{formik => (
				<Form>
					<button type='submit'>Submit</button>
				</Form>
			)}
		</Formik>
	);
}

export default FormikContainer;

```

Luego en `App.js`
```jsx
mport './App.css';
import FormikContainer from './components/FormikContainer';

function App() {
	return (
		<div className='App'>
			<FormikContainer />
		</div>
	);
}

export default App;

```

y por último en `App.css`
```css
.App {
	display: flex;
	justify-content: center;
}

label {
	font-weight: bold;
	display: flex;
	margin-bottom: 5px;
}

input[type='text'],
input[type='email'],
textarea {
	display: block;
	width: 400px;
	padding: 6px 12px;
	font-size: 14px;
	line-height: 1.42857143;
	color: #555;
	background-color: #fff;
	background-image: none;
	border: 1px solid #ccc;
	border-radius: 4px;
	/* margin-bottom: 20px; */
}

.form-control {
	margin-bottom: 20px;
}

.error {
	color: red;
}

```

Creamos el archivo `FormikControl.js` encargado de decidir qué campo debe renderizarse de acuerdo al valor recibido en lvia prop `control`s.
```jsx
import React from 'react';

function FormikControl(props) {
	const { control, ...rest } = props;
	switch (control) {
		case 'input':
		case 'textarea':
		case 'select':
		case 'radio':
		case 'checkbox':
		case 'date':
		default:
			return null;
	}
	return <div></div>;
}

export default FormikControl;

```

# Input FormikControl
Tomando como ejemplo el UI de un Input `email` podemos distinguir 3 elementos:
* `<label>`
* `<Field/>`
* `<ErrorMessage/>`

Necesitaremos las siguientes props:
* `control='input'`
* `label='Email'`
* `name='email'`
* `type='email'` (podría ser `text`, `password`)

1. Creamos un componente especifico para ese tipo de campo `Input.js`
2. Escribimos el código en `FormikControl`
3. Escribimos el código en `FormikContainer` para testear el código que escribamos.

1. Creamos un componente `Input.js`

> Notar que decidimos utilizar el atributo `id` con el mismo valor que `name` pero podríamos pasar una prop aparte en caso de así desearlo. 


```jsx
import React from 'react';
import { Field, ErrorMessage } from 'formik';
import TextError from './TextError';
function Input(props) {
	const { label, name, ...rest } = props;
	return (
		<div className='form-control'>
			<label htmlFor={name}>{label}</label>
			<Field id={name} name={name} {...rest} />
			<ErrorMessage name={name} component={TextError} />
		</div>
	);
}

export default Input;

```

Creamos un componente `TextError.js` para lograr que el texto de error aparezca en rojo.
```jsx
import React from 'react';

function TextError(props) {
	return <div className='error'>{props.children}</div>;
}

export default TextError;
```

2. En `FormikControl`
```jsx
import React from 'react';

function FormikControl(props) {
	const { control, ...rest } = props;
	switch (control) {
		case 'input':
			return <Input {...rest} />;
		case 'textarea':
		case 'select':
		case 'radio':
		case 'checkbox':
		case 'date':
		default:
			return null;
	}
	return <div></div>;
}

export default FormikControl;

```

En `FormikContainer` completamos `initialValues` y `validationSchema` e invocamos a `FormikControl` con las props necesarias.
```jsx
import React from 'react';
import { Formik, Form } from 'formik';
import * as Yup from 'yup';
import FormikControl from './FormikControl';

function FormikContainer() {
	const initialValues = { email: '' };
	const validationSchema = Yup.object({ 
	email: Yup.string().email('Formato Inválido').required('Requerido')
	 });
	const onSubmit = values => console.log('Form Data', values);
	return (
		<Formik initialValues={initialValues} validationSchema={validationSchema} onSubmit={onSubmit}>
			{formik => (
				<Form>
					<FormikControl control='input' type='email' label='Email' name='email' />
					<button type='submit'>Submit</button>
				</Form>
			)}
		</Formik>
	);
}

export default FormikContainer;

```

# Textarea FormikControl
La finalidad es lograr tener un componente Textarea FormikControl reutilizable. 
En un textarea contamos con tres el elementos `<label>`, `<Field />` que será el encargado de renderizar un elemento HTML `textarea` y `<ErrorMessage/>`. Necesitaremos las siguientes props:
* `control='textarea'`
* `label='Description'`
* `name='description'`

Creamos un componente `Textarea.js`
```jsx
import React from 'react';
import { Field, ErrorMessage } from 'formik';
import TextError from './TextError';
function Textarea() {
	const { label, name, ...rest } = props;
	return (
		<div className='form-control'>
			<label htmlFor={name}>{label}</label>
			<Field as='textarea' id={name} name={name} {...rest} />
			<ErrorMessage name={name} component={TextError} />
		</div>
	);
}

export default Textarea;
```

Actualizamos `FormikControl`
```jsx
import React from 'react';
import Input from './Input';

function FormikControl(props) {
	const { control, ...rest } = props;
	switch (control) {
		case 'input':
			return <Input {...rest} />;
		case 'textarea':
			return <Textarea {...rest} />;
		case 'select':
		case 'radio':
		case 'checkbox':
		case 'date':
		default:
			return null;
	}
	return <div></div>;
}

export default FormikControl;

```
Por último en `FormikContainer` actualizamos los `initialValues`, `validationSchema` e incorporamos al componente `FormikControl`
```jsx
import React from 'react';
import { Formik, Form } from 'formik';
import * as Yup from 'yup';
import FormikControl from './FormikControl';

function FormikContainer() {
	const initialValues = { email: '', description: '' };
	const validationSchema = Yup.object({
		email: Yup.string().email('Formato Inválido').required('Requerido'),
		description: Yup.string().required('Requerido'),
	});
	const onSubmit = values => console.log('Form Data', values);
	return (
		<Formik initialValues={initialValues} validationSchema={validationSchema} onSubmit={onSubmit}>
			{formik => (
				<Form>
					<FormikControl control='input' type='email' label='Email' name='email' />
					<FormikControl control='textarea' label='Description' name='description' />

					<button type='submit'>Submit</button>
				</Form>
			)}
		</Formik>
	);
}

export default FormikContainer;
```

# Select FormikControl
En cuando a UI un Select FormikControl tendrá los siguientes elementos: `<label>`, `<Field/>` que debe renderizar un elemento html `<select>` y `<ErrorMessage/>`.
Al hacer click sobre el `Field` obtendremos la lista de opciones a seleccionar.
Necesitaremos las siguientes props:
* `control='select'`
* `label='Select a topic'`
* `name='selectOption'`
* `options=[{key, value}]`

1. Creamos un componente `Select.js`
```jsx
import React from 'react';
import { Field, ErrorMessage } from 'formik';
import TextError from './TextError';
function Select(props) {
	const { label, name, options, ...rest } = props;
	return (
		<div>
			<div className='form-control'>
				<label htmlFor={name}>{label}</label>
				<Field as='select' id={name} name={name} {...rest}>
					{options.map(option => {
						return (
							<option key={option.value} value={option.value}>
								{option.key}
							</option>
						);
					})}
				</Field>
				<ErrorMessage name={name} component={TextError} />
			</div>
		</div>
	);
}

export default Select;

```

2. Agregamos un caso al switch en `FormikControl`
```jsx
import React from 'react';
import Input from './Input';
import Select from './Select';
import Textarea from './Textarea';

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
		case 'checkbox':
		case 'date':
		default:
			return null;
	}
	return <div></div>;
}

export default FormikControl;
```

3. En `FormikContainer`
Definimos una constante `dropdownOptions` que contendrá un array de objetos con los valores del select (la propiedad `key` (otros autores lo llaman label) corresponde al texto y `value` lógicamente a un valor)
```jsx
import React from 'react';
import { Formik, Form } from 'formik';
import * as Yup from 'yup';
import FormikControl from './FormikControl';

function FormikContainer() {
	const dropdownOptions = [
		{ key: 'Select an option', value: '' },
		{ key: 'Option 1', value: 'option1' },
		{ key: 'Option 2', value: 'option2' },
		{ key: 'Option 3', value: 'option3' },
	];
	const initialValues = { email: '', description: '', selectOption: '' };
	const validationSchema = Yup.object({
		email: Yup.string().email('Formato Inválido').required('Requerido'),
		description: Yup.string().required('Requerido'),
		selectOption: Yup.string().required('Requerido'),
	});
	const onSubmit = values => console.log('Form Data', values);
	return (
		<Formik initialValues={initialValues} validationSchema={validationSchema} onSubmit={onSubmit}>
			{formik => (
				<Form>
					<FormikControl control='input' type='email' label='Email' name='email' />
					<FormikControl control='textarea' label='Description' name='description' />
					<FormikControl control='select' label='Select a Topic' name='selectOption' options={dropdownOptions} />

					<button type='submit'>Submit</button>
				</Form>
			)}
		</Formik>
	);
}

export default FormikContainer;
```

# Radio Group FormikControl
A diferencia de lo hecho en los otro tres casos, tendremos que usar el **render props pattern** para el componente `Field`.
En cuanto al UI veremos tres elementos distintivos: 
* `<label>` 
* `<Field/>` es una lista de elementos html de tipo `input` y `label` que solo nos permite seleccionar uno a la vez.
* `<ErrorMessage/>`

Las `props` que necesitaremos serán:
* `control='radio'`
* `label='Pick one option'`
* `name='radioButton'`
* `options=[{key,value}]`

1. Creamos un componente `RadioButtons.js`
```jsx
import React from 'react';
import { Field, ErrorMessage } from 'Formik';
import TextError from './TextError';

function RadioButtons(props) {
	const { label, name, options, ...rest } = props;
	return (
		<div className='form-control'>
			<label>{label}</label>
			<Field name={name} {...rest}>
				{
				({ field }) => {
					return options.map(option => {
						return (
							<React.Fragment key={option.key}>
								<input type='radio' id={option.value} {...field} value={option.value} checked={field.value === option.value} />
								<label htmlFor={option.value}>{option.key}</label>
							</React.Fragment>
						);
					});
				}}
			</Field>
			<ErrorMessage name={name} component={TextError} />
		</div>
	);
}

export default RadioButtons;

```
Aunque no lo hemos especificado, si inspeccionamos el código veremos que los `input` tienen el mismo `name`, esto lo logramos gracias a que le estamos pasando `{...field}` a dicho input (también hace el bind de `onBlur` y `onChange`)

2. En `FormikControl`
```jsx
import React from 'react';
import Input from './Input';
import RadioButtons from './RadioButtons';
import Select from './Select';
import Textarea from './Textarea';

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
		case 'date':
		default:
			return null;
	}
	return <div></div>;
}

export default FormikControl;

```

3. En `FormikContainer` agregamos `radioOptions` (que es un array de objetos con *key/value pairs* donde la `key` es el label a mostrar y `value` el valor, actualizamos `initialValues` y `validationSchema`
```jsx
import React from 'react';
import { Formik, Form } from 'formik';
import * as Yup from 'yup';
import FormikControl from './FormikControl';

function FormikContainer() {
	const dropdownOptions = [
		{ key: 'Select an option', value: '' },
		{ key: 'Option 1', value: 'option1' },
		{ key: 'Option 2', value: 'option2' },
		{ key: 'Option 3', value: 'option3' },
	];
	const radioOptions = [
		{ key: 'Option 1', value: 'rOption1' },
		{ key: 'Option 2', value: 'rOption2' },
		{ key: 'Option 3', value: 'rOption3' },
	];
	const initialValues = { email: '', description: '', selectOption: '', radioOption: '' };
	const validationSchema = Yup.object({
		email: Yup.string().email('Formato Inválido').required('Requerido'),
		description: Yup.string().required('Requerido'),
		selectOption: Yup.string().required('Requerido'),
		radioOption: Yup.string().required('Requerido'),
	});
	const onSubmit = values => console.log('Form Data', values);
	return (
		<Formik initialValues={initialValues} validationSchema={validationSchema} onSubmit={onSubmit}>
			{formik => (
				<Form>
					<FormikControl control='input' type='email' label='Email' name='email' />
					<FormikControl control='textarea' label='Description' name='description' />
					<FormikControl control='select' label='Select a Topic' name='selectOption' options={dropdownOptions} />
					<FormikControl control='radio' label='Radio Topic' name='radioButton' options={radioOptions} />

					<button type='submit'>Submit</button>
				</Form>
			)}
		</Formik>
	);
}

export default FormikContainer;

```

# Checkbox Group FormikControl
Partimos del código del ejemplo anterior de los Radio Buttons.

En cuanto al UI contamos con tres elementos distintivos:
* `<label>`
* `<Field/>`
* `<ErrorMessage/>`

Las props que necesitaremos son:
* `control='checkbox'`
* `label='Pick Options'`
* `name='checkboxOption'`
* `options=[{key,value}]`

A diferencia del uso de radio buttons es posible seleccionar más de un elemento a la vez.

En este caso implementamos los tres pasos de siempre pero en el orden inverso.
1. Comenzando en `FormikContainer` notar que en `initialValues` tenemos un array vacío `checkboxOption: []` con la finalidad de poder elegir múltiples valores, a diferencia de lo que tuvimos anteriormente con`radioOption: ''` . En cuanto a la validación por lo tanto tendremos `checkboxOption: Yup.array().required('Requerido'),`

```jsx
import React from 'react';
import { Formik, Form } from 'formik';
import * as Yup from 'yup';
import FormikControl from './FormikControl';

function FormikContainer() {
	const dropdownOptions = [
		{ key: 'Select an option', value: '' },
		{ key: 'Option 1', value: 'option1' },
		{ key: 'Option 2', value: 'option2' },
		{ key: 'Option 3', value: 'option3' },
	];
	const radioOptions = [
		{ key: 'Option 1', value: 'rOption1' },
		{ key: 'Option 2', value: 'rOption2' },
		{ key: 'Option 3', value: 'rOption3' },
	];
	const checkboxOptions = [
		{ key: 'Option 1', value: 'cOption1' },
		{ key: 'Option 2', value: 'cOption2' },
		{ key: 'Option 3', value: 'cOption3' },
	];
	const initialValues = { email: '', description: '', selectOption: '', radioOption: '', checkboxOption: [] };
	const validationSchema = Yup.object({
		email: Yup.string().email('Formato Inválido').required('Requerido'),
		description: Yup.string().required('Requerido'),
		selectOption: Yup.string().required('Requerido'),
		radioOption: Yup.string().required('Requerido'),
		checkboxOption: Yup.array().required('Requerido'),
	});
	const onSubmit = values => console.log('Form Data', values);
	return (
		<Formik initialValues={initialValues} validationSchema={validationSchema} onSubmit={onSubmit}>
			{formik => (
				<Form>
					<FormikControl control='input' type='email' label='Email' name='email' />
					<FormikControl control='textarea' label='Description' name='description' />
					<FormikControl control='select' label='Select a Topic' name='selectOption' options={dropdownOptions} />
					<FormikControl control='radio' label='Radio Topic' name='radioOption' options={radioOptions} />
					<FormikControl control='checkbox' label='Checkbox Topics' name='checkboxOption' options={checkboxOptions} />

					<button type='submit'>Submit</button>
				</Form>
			)}
		</Formik>
	);
}

export default FormikContainer;

```

2. En `FormikControl`
```jsx
import React from 'react';
import Input from './Input';
import Textarea from './Textarea';
import Select from './Select';
import RadioButtons from './RadioButtons';
import Checkbox from './Checkbox';

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
			return <Checkbox {...rest} />;
		case 'date':
		default:
			return null;
	}
	return <div></div>;
}

export default FormikControl;

```

3. Creamos un componente `Checkbox.js` y debido a las grandes similitudes, partimos del codigo de `RadioButtons.js` donde cambiamos el nombre de la funcion, el `type='checkbox'` y la condición de `checked={field.value.includes(option.value)}`
```jsx
import React from 'react';
import { Field, ErrorMessage } from 'formik';
import TextError from './TextError';

function CheckboxGroup(props) {
	const { label, name, options, ...rest } = props;
	return (
		<div className='form-control'>
			<label>{label}</label>
			<Field name={name} {...rest}>
				{({ field }) => {
					console.log('Field', field);
					return options.map(option => {
						return (
							<React.Fragment key={option.key}>
								<input
									type='checkbox'
									id={option.value}
									{...field}
									value={option.value}
									checked={field.value.includes(option.value)}
								/>
								<label htmlFor={option.value}>{option.key}</label>
							</React.Fragment>
						);
					});
				}}
			</Field>
			<ErrorMessage name={name} component={TextError} />
		</div>
	);
}

export default CheckboxGroup;

```

Si completamos el formulario y presionamos **Submit** veremos que `checkboxOption` es un array que admite múltiples valores.

# Date Picker FormikControl
La implementación es similar a la de los radio buttons y checkboxes ya que usaremos *render props pattern* en el componente `<Field/>`. Veremos como usar una *datepicker library* con Formik.

Usaremos el paquete [react-datepicker](https://www.npmjs.com/package/react-datepicker)
```bash
npm i react-datepicker
```
En la documentación nos encontramos con este ejemplo básico (notar que importamos el componente y el CSS).
```jsx
import  React,  {  useState  }  from  "react";
import  DatePicker  from  "react-datepicker";
import  "react-datepicker/dist/react-datepicker.css";

const  Example  =  ()  =>  {
 const  [startDate,  setStartDate]  =  useState(new  Date());
 return  (
 <DatePicker selected={startDate} onChange={date  =>  setStartDate(date)}  />
 );
};/
```

1. Creamos un componente `DatePicker.js` Como al componente lo llamamos `DatePicker` al default export de la library lo llamamos `DateView`

`setFieldValue` nos permite setear programáticamente le valor de un campo.
```jsx
import React from 'react';
import DateView from 'react-datepicker';
import { Field, errorMessage, ErrorMessage } from 'formik';
import 'react-datepicker/dist/react-datepicker.css';

function DatePicker({ label, name, ...rest }) {
	return (
		<div className='form-control'>
			<label httpFor={name}>{label}</label>
			<Field name={name}>
				{({ form, field }) => {
					const { setFieldValue } = form;
					const { value } = field;
					return <DateView id={name} {...field} selected={value} onChange={value => setFieldValue(name, value)} />;
				}}
			</Field>
			<ErrorMessage name={name} component={TextError} />
		</div>
	);
}
export default DatePicker;

```

2. Actualizamos `FormikControl`
```jsx
import React from 'react';
import Input from './Input';
import Textarea from './Textarea';
import Select from './Select';
import RadioButtons from './RadioButtons';
import CheckboxGroup from './CheckboxGroup';
import DatePicker from './DatePicker';

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
		default:
			return null;
	}
	return <div></div>;
}

export default FormikControl;

```

3. Actualizamos `FormikContainer`
En la validación indicamos que admitimos valor `null`
`birthDate: Yup.date().required('Requerido').nullable(),`
```jsx
import React from 'react';
import { Formik, Form } from 'formik';
import * as Yup from 'yup';
import FormikControl from './FormikControl';

function FormikContainer() {
	const dropdownOptions = [
		{ key: 'Select an option', value: '' },
		{ key: 'Option 1', value: 'option1' },
		{ key: 'Option 2', value: 'option2' },
		{ key: 'Option 3', value: 'option3' },
	];
	const radioOptions = [
		{ key: 'Option 1', value: 'rOption1' },
		{ key: 'Option 2', value: 'rOption2' },
		{ key: 'Option 3', value: 'rOption3' },
	];
	const checkboxOptions = [
		{ key: 'Option 1', value: 'cOption1' },
		{ key: 'Option 2', value: 'cOption2' },
		{ key: 'Option 3', value: 'cOption3' },
	];
	const initialValues = { email: '', description: '', selectOption: '', radioOption: '', checkboxOption: [], birthDate: null };
	const validationSchema = Yup.object({
		email: Yup.string().email('Formato Inválido').required('Requerido'),
		description: Yup.string().required('Requerido'),
		selectOption: Yup.string().required('Requerido'),
		radioOption: Yup.string().required('Requerido'),
		checkboxOption: Yup.array().required('Requerido'),
		birthDate: Yup.date().required('Requerido').nullable(),
	});
	const onSubmit = values => console.log('Form Data', values);
	return (
		<Formik initialValues={initialValues} validationSchema={validationSchema} onSubmit={onSubmit}>
			{formik => (
				<Form>
					<FormikControl control='input' type='email' label='Email' name='email' />
					<FormikControl control='textarea' label='Description' name='description' />
					<FormikControl control='select' label='Select a Topic' name='selectOption' options={dropdownOptions} />
					<FormikControl control='radio' label='Radio Topic' name='radioOption' options={radioOptions} />
					<FormikControl control='checkbox' label='Checkbox Topics' name='checkboxOption' options={checkboxOptions} />
					<FormikControl control='date' label='Pick a Data' name='birthDate' options={checkboxOptions} />

					<button type='submit'>Submit</button>
				</Form>
			)}
		</Formik>
	);
}

export default FormikContainer;

```

 Al enviar los formularios muchas veces no enviaremos las fechas como *Date object* sino que tendremos que pasarlos por `JSON.stringify()`. Si queremos cargar datos guardados provistos por una API al parsearlos obtendremos un valor UTC String.

Para simular esto en `onSubmit()` hacemos `console.log('Saved Data', JSON.parse(JSON.stringify(values)))` obtendremos `"2020-07-06T18:30:00.000Z"` es decir un string del formato UTC. Si quisieramos reinicializar el formulario con estos datos obtendríamos error, debemos agregar un parse al `JSON.parse()` para convertir el string en un JavaScript Date.

