# Cargar Datos en Formularios
En ocasiones vamos a querer cargar datos guardados en formularios. Esto es particularmente útil cuando tenemos muchos campos y queremos darle la posibilidad al usuario de guardar el progreso y continuar en otro momento.

1. Normalmente estos datos provendrán de una base de datos a través de una API pero en este caso utilizaremos como *mocking data* un objeto `savedValues`. Debe tener la misma estructura que `initialValues`.

```jsx
const savedValues = {
	name: 'Juan',
	email: 'j@example.com',
	channel: 'JNM8',
	comments: 'Welcome',
	address: '123 Street',
	social: {
		facebook: '',
		twitter: '',
	},
	phoneNumbers: ['', ''],
	phNumbers: [''],
};
```

2. Agregamos un botón que nos permita cargar los campos en el formulario (simboliza el fetch a una API):
```jsx
<button type='button'>Load Saved Data</button>
```

3. Creamos una variable de estado con valor inicial `null`
 `const [formValues, setFormValues] = useState(null)`

4. Cuando el usuario hace click en el botón  
```jsx
<button type='button' onClick={() => {()=> setFormValues(savedValues)}}>Load Saved Data</button>

```

5. Cambiar el valor de la prop `initialValues` del componente `Formik` agregando `initialValues={formValues || initialValues}` de modo tal que si `formValues` es distinto de null ese sea el valor inicial.

```jsx
<Formik initialValues={formValues || initialValues} onSubmit={onSubmit} validationSchema={validationSchema}>
```

7. Agregamos la prop `enableReinitialize` también al componente `Formik` que nos permite que el valor cambie sus valores iniciales una vez que fue cargado.
```jsx
<Formik initialValues={formValues || initialValues} onSubmit={onSubmit} validationSchema={validationSchema} enableReinitialize>
```

En este ejemplo planteamos un escenario en el que el formulario ya se cargó y ahí mediante un botón el cliente carga datos guardados

Otra situación en la que podríamos usar esto es cuando tenemos que cargar los datos para el formulario de una API, por ejemplo los valores de un  *select dropdown* y queremos mostrar un cartel *Loading* y recién cuando la respuesta modificar el estado y mostrar el componente `Formik`.