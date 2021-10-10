# Estado como Objeto Anidado
El concepto de *nested objects* está relacionado con el estado de los campos del formulario.

Analizando `initialValues` veremos que hay cinco campos del formulario y los tenemos registrados del mismo modo que esos campos:

```jsx
const initialValues = {
	name: '',
	email: '',
	channel: '',
	comments: '',
	address: '',
};
```
Si presionamos el botón de submit, como tenemos:
```jsx
const onSubmit = values => {
		console.log('Form Data', values);
	};
```
Veremos todos los campos de manera individual: `{name: "abc", email: "abc@asd.com", channel: "abc", comments: "abcdef", address: "abc 123"}`

En ocasiones queremos agrupar cierta cantidad de campos dentro de un objeto (por ejemplo si la **API** acepta los datos de cierta manera o si la **DB** la almacena de un modo en particular).

Supongamos que en nuestro formulario queremos recolectar también las redes sociales de Twitter y Facebook almacenándolas como *nested object*.
El primer paso es actualizar `initialValues`:
```jsx
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
	};
```
A continuación agregamos los campos en el JSX:
```jsx
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
```

Especificar el atributo `name` con *dot notation* notar que tenemos `name='social.facebook'`.

Cuando presionemos submit ahora veremos que`social` es un objeto con dos propiedades `facebook` y `twitter`.

# Estado como Array
Supongamos que tenemos que agregar al formulario el teléfono primario y secundario del usuario y queremos almacenarlos en un array.

En primer lugar agregamos una propiedad a `initialValues`

```jsx
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
		phoneNumbers: ['',''],
	};
```

A continuación agregamos el JSX

```jsx
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
```

Notar que el atributo `name` toma los valores `phoneNumbers[0]` y `phoneNumbers[1]`
Luego al hacer el submit veremos que la propiedad `phoneNumbers` es un array con los dos teléfonos especificados.
Será necesario en este caso escribir reglas de validación personalizadas.