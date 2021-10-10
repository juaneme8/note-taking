# Componentes
Si bien hasta ahora con el *helper method* `getFieldProps()` nos ahorramos algunas líneas de código, debemos pasárselo a cada input de manera manual. Para ahorrarnos todavía más tiempo Formik proporciona una serie de componentes que usan React Context implícitamente y hacen que el código sea menos *verbose*.

Dentro de los componentes que proporciona **Formik**, trabajaremos en un comienzo con cuatro de ellos:

* `<Formik>`
* `<Form>`
* `<Field>`
* `<ErrorMessage>`

## Componente `<Formik>`
`<Formik>` es un reemplazo del hook `useFormik()`

1. En primer lugar importamos `import { Formik } from 'formik';` en lugar de `useFormik`

2. Eliminamos el código:
```jsx
const formik = useFormik({
	initialValues,
	onSubmit,
	validationSchema,
	// validate,
});
```
3. Envolvemos todo el `<form>` con `<Formik>`
```jsx
<Formik>
	<form>
	</form>
</Formik>
```

4. Le pasamos las `props` a `<Formik>` mediante las variables que usábamos con `useFormik()`
```jsx
<Formik initialValues={initialValues} onSubmit={onSubmit} validationSchema={validationSchema}>
	<form>
	</form>
</Formik>
```

`<Formik>` se comporta como un *context provider component* que proporciona las propiedades y *helper methods* para los otros tres componentes

Este componente no reduce la cantidad de líneas ni simplifica el código pero es necesario para los otros tres componentes que sí lo hacen.

## Componente `<Form>`

1. Importar `Form` de `'formik'` por lo que tendremos:
`import { Formik, Form } from 'formik';`

2. Reemplazar el elemento html `<form>` por el componente `<Form>`
Pasamos de tener
```jsx
<form onSubmit={formik.handleSubmit}>
	...
</form>
```
A tener 
```jsx
<Form onSubmit={formik.handleSubmit}>
	...
</Form>
```

3. Eliminar `onSubmit={formik.handleSubmit}` podemos hacer esto dado que el componente Form es un wrapper del elemento html `form` que vincula el método `onSubmit` que le pasamos a `Formik` con el **submit event** del formulario.

## Componente `<Field>`
1. Importar el componente Field por lo que nos queda:
`import { Formik, Form, Field } from 'formik';`

3. Reemplazar todos los `input` por `Field`

Pasamos de tener:
```jsx
<input type='text' id='name' name='name' {...formik.getFieldProps('name')} />
```
A tener:
```jsx
<Field type='text' id='name' name='name' {...formik.getFieldProps('name')} />
```

3. Eliminar `{...formik.getFieldProps('name')}` ya que se trata de un patrón que repetimos en todos los campos y Formik nos ofrece una abstracción.

Por lo que nos queda:
```jsx
<Field type='text' id='name' name='name' />
```

El componente `Field` se encarga de:
* Vincular los input al componente `Formik`
* Vincula el atributo `name`con el estado de formik.
* Por default renderiza un elemento `input`


## Componente `<ErrorMessage>`
Hasta el momento mostramos mensajes de error de la siguiente forma:
```jsx
{formik.touched.name && formik.errors.name ? <div className='error'>{formik.errors.name}</div> : null}
```

Como nuevamente se trata de un patrón que repetimos en todos los campos Formik nos ofrece una solución al respecto.

1. Importar el componente por lo que nos queda: `import { Formik, Form, Field, ErrorMessage } from 'formik';`

2. Reemplazar el bloque anterior por el componente, por lo que pasamos de tener:
```jsx
{formik.touched.name && formik.errors.name ? <div className='error'>{formik.errors.name}</div> : null}
```
A tener:
```jsx
<ErrorMessage/>
```

3. Pasarle una `prop` `name` que es igual al atributo `name` del campo en cuestión.
```jsx
<ErrorMessage name='name'/>
```
## Refactoring Completo
Con el agregado de `Formik`, `Form`, `Field`, `ErrorMessage` terminamos el refactoring y logramos reducir la cantidad de líneas y mejoramos la facilidad de lectura del código.
Si probamos ahora la aplicación con el refactoring completo veremos que funciona todo igual, excepto por el estilo del error que no lo respecta.

Si tomamos como ejemplo un `form-control` pasamos de tener:
```jsx
<div className='form-control'>
	<label htmlFor='name'>Name</label>
	<input 
		type='text'
		id='name'
		name='name'
		onChange={formik.handleChange}
		onBlur={formik.handleBlur}
		value={formik.values.name}
	/>
	{formik.touched.name && formik.errors.name ? (
		<div className='error'>{formik.errors.name}</div> 
	): null}
</div>
```
A tener
```jsx
<div className='form-control'>
	<label htmlFor='name'>Name</label>
	<Field type='text' id='name' name='name' />
	<ErrorMessage name='name' />
</div>
```