# Resetear Datos de Formulario

## Reset con botón
Si queremos resear el formulario **a los valores iniciales** de cada campo (no es que se vuelve un string vacío), simplemente debemos agregar un `button` con el `type='reset'`
```jsx
<button type='reset'>Reset</button>
```

## Reset luego de Form Submission
Si queremos resear el formulario **a los valores iniciales** de cada campo  cuando se realiza el envío dl formulario, en el método `onSubmit` utilizamos las `onSubmitProps` :

```jsx
const onSubmit = (values, onSubmitProps) => {
	console.log('Form Data', values);
	console.log('Submit Props', onSubmitProps);

	//Esperar a la respuesta de la API
	//(...)

	onSubmitProps.setSubmitting(false);
	onSubmitProps.resetForm();
};

```