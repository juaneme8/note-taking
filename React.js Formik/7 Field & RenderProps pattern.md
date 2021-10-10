# `<Field/>` en Profundidad
El componente `Field` dijimos que por default renderiza un elemento html `input` y lo vincula con Formik teniendo así `handleChange`, `hanldleBlur` y `value`.

## Pasaje de props adicionales:
1. El componente `Field` además pasará las `props` adicionales que le pasemos al elemento html, esto lo podemos ver tomando por ejemplo el `id='name'`
```jsx
<Field type='text' id='name' name='name' />
```
Si en las dev tools miramos el elemento `input` veremos que tiene `id='name'`.

De manera similar si le pasamos un `placeholder='Nombre de usuario'` veremos que el elemento `input` también lo recibe.

## Renderizado de un elemento diferente
Dijimos que el componente `Field` tiene la habilidad para renderizar por default un `input` pero también puede renderizar otros elementos.

Supongamos que queremos incorporar un campo llamado `comments` y que sea del tipo `text-area`

Para ello agregamos a `initialValues` una propiedad con el string vacío:

```jsx
const initialValues = {
	name: '',
	email: '',
	channel: '',
	comments: ''
};
```
Luego agregamos el componente utilizando la prop `as='textarea'`.

La prop `as` puede aceptar como valor `input` (valor default por eso anteriormente no lo pusimos), `textarea`, `select` o un componente *custom* de React.

De manera alternativa en lugar de la prop `as` podríamos usar la prop `component` con idéntico resultado.

Con `as` (opción recomendada)
```jsx
<div className='form-control'>
	<label htmlFor='comments'>Comments</label>
	<Field as='textarea' id='comments' name='comments' />
	<ErrorMessage name='comments' />
</div>
```

El `id` en el `<Field>` es necesario para que al hacer click en el `<label>` nos seleccione el input.

Con `component` (antiguamente fue *deprecated* y luego *undeprecated*)
```jsx
<div className='form-control'>
	<label htmlFor='comments'>Comments</label>
	<Field component='textarea' id='comments' name='comments' />
	<ErrorMessage name='comments' />
</div>
```

### Render Props Pattern
Veremos la implementación del componente `Field` usando el *render props pattern*.

*Render Props Pattern* se trata de un patrón avanzado que nos da un mayor control sobre el renderizado y comportamiento del campo del formulario.

A modo de ejemplo agregamos un nuevo input de `address` al formulario cosa que podríamos agregar de la misma manera que hicimos con `name`, `email`, `channel` pero queremos hacerlo mediante render props.
Primero lo agregamos a `initialValues`
```jsx
const initialValues = {
		name: '',
		email: '',
		channel: '',
		comments: '',
		address: '',
	};
```

Luego en el jsx si lo hiciéramos como hasta ahora:
```jsx
<div className='form-control'>
	<label htmlFor='comments'>Comments</label>
	<Field name='address' />
</div>
```

> NOTA: Si no le pasamos la prop `type='text'` (que Field le pasará a `input`, el css no será el deseado porque no aplicará la regla `input[type='text']{...}`

Usando render props tenemos una función como *children* del componente.
```jsx
<Field type='text' name='address'>
	{props => {
		console.log('Render props:', props);
		return <input type='text' id='address' />;
	}}
</Field>
```
Este elemento `input` no está vinculado a Formik.

Gracias al `console.log(props)` veremos que `props` es un objeto con propiedades `field`, `form` y `meta`. 
 1. `field` a su vez es un objeto que tiene dentro propiedades `name`, `onBlur`, `onChange` y `value` es decir todo lo que Formik requiere para manejar el estado.
 2. `form` es básicamente la constante `formik` que el hook `useFormik` nos retornaba (podemos ver los objetos usados `values`, `errors`, `touched` junto con los *helper methods* también usados `handleSubmit`, `handleChange`, `handleBlur`.
 3. `meta` nos otorga la información para renderizar los mensajes de error, es decir si un campo fue visitado con `touched`, si tiene error con `error` y su valor con `value` .

Hacemos destructuring de `props` y para vincular el `input` con Formik hacemos el *spread* de `field` 

A pesar de que todavía no tenemos reglas de validación veremos cómo mostrar un error haciendo uso de `meta`
```jsx
<Field type='text' name='address'>
	{props => {
		const { field, form, meta } = props;
		console.log('Render props:', props);
		return (
			<div>
				<input type='text' id='address' {...field} />
				{meta.touched && meta.error ? <div>{meta.error}</div> : null}
			</div>
		);
	}}
</Field>
```
Si bien esto parece exagerado cuando queremos utilizar *custom components* en los formularios y queremos que estén vinculados a Formik este patrón nos será útil.