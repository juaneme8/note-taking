# Validación Avanzada
A continuación estudiaremos cuándo sucede la validación. Sabemos que cuando se realiza la validación el objeto `formik.errors` se completa con los errores. Utilizaremos eso para saber cuándo sucede. 
Necesitamos un campo que implemente **render props pattern** con el cual accederemos a `formik.errors`

Tomamos por ejemplo el campo `address` y utilizando el **render props pattern** imprimimos el valor de `form.errors` de modo de ver cuándo se actualiza.

```jsx
<div className='form-control'>
	<label htmlFor='address'>Address</label>
	<Field type='text' name='address'>
		{props => {
			// console.log('Field Rendered');
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
```
Al iniciar la página veremos que el objeto de errores está vacío lo que implica que la validación aún no se ejecutó.

Veremos que la validación se efectúa en tres escenarios 1) ante un **change event** en alguno de los campos, 2) luego de un **blur event** (*blur out* en este caso) cuando sacamos el cursor de uno de ellos y 3) ante un **submit event**.

Si el usuario presiona el botón submit no debemos preocuparnos por que se ejecute el **onSubmit** handler ya que no lo hará a menos que se cumpla la validación.

De acuerdo a la complejidad de nuestro formulario o por el requerimiento que tengamos puede que queramos que no se ejecute la validación de manera automática.  Si no queremos que esto suceda contamos con dos props que le pasamos al componente `Formik` para evitar que esto suceda en los dos primeros escenarios. `validateOnChange={false}` y `validateOnBlur={false}`

```jsx
return (
<>
<Formik
initialValues={initialValues}
onSubmit={onSubmit}
validationSchema={validationSchema}
validateOnChange={false}
validateOnBlur={false}
>
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
		(...)
	</Formik>
</Form>
```

También existe la posibilidad de poner `validateOnSubmit={false}` si no queremos que la validación se ejecute ni siquiera cuando presionamos el botón de enviar.

# Validación a nivel Campo
Según lo visto anteriormente tanto usando `useFormik` como usando el componente `Formik` tenemos dos formas de especificar las reglas de validación de todo el formulario. Tomando el caso de `Formik` una es con la prop `validate` y la otra con Yup y el objeto schema usando la prop `validationSchema`. 
Sin embargo también es posible especificar las reglas de validación a nivel componente.

Si queremos agregar la validación de **required** al campo `comments` como veníamos haciendo hasta ahora:
```jsx
const validationSchema = Yup.object({
	(...)
	comments: Yup.string().required('Requerido'),
});
```

En cambio realizar una validación a nivel componente es muy similar a cuando usamos la función de validación personalizada:

```jsx
const validateComments = value => {
	let error;
	if (!value) {
		error = 'Requerido';
	}
	return error;
};
```

Luego en el JSX
```jsx
<div className='form-control'>
	<label htmlFor='comments'>Comments!</label>
	<Field as='textarea' id='comments' name='comments' validate={validateComments} />
	<ErrorMessage name='comments' component={TextError} />
</div>
```

Tanto ante un **change event** como ante un **blur event** disparará la función que completa el objeto `errors`.
La prop `validate` también está disponible en el componente `FastField`

Una posible aplicación de esta validación a nivel campo en lugar de la validación a nivel formulario podría ser si queremos renderizar los campos basándonos en un JSON que obtenemos de una API. Una vez con el JSON iteramos sobre los distintos objetos para armar la función de validación en cada iteración y asignarla a la prop `validate` de `Field` o `FastField`.

# Validación Manual
Es posible disparar manualmente tanto la validación a nivel formulario como la validación a nivel campo (complementariamente veremos que también es posible visitar los campos manualmente). Contamos con dos **helper methods** que para accederlos debemos usar el *render props pattern* en el formulario completo. Recordemos que consiste en tener una función como *children* que recibe algunas props y retorna JSX. En este caso implementamos lo mismo en el componente `Formik` con una función como *children* que recibirá props que llamaremos `formik` y retornamos el componente `Form`

```jsx
<Formik initialValues={initialValues} onSubmit={onSubmit} validationSchema={validationSchema}>
	{formik => {
		console.log('Formik Props', formik);
		return (
			<Form>
				(...)
			</Form>
		);
	}}
</Formik>
```

Estas props `formik` nos permitirán controlar todo lo relacionado con nuestro formulario. En el `console.log(formik)` veremos que nos resulta familiar (lo obtuvimos cuando usamos `useFormik()` y en el patrón render props de `Field` y `FieldArray` obteníamos `form`) con propiedades y métodos como `errors`, `touched`, `handleChange`, `handleBlur`, `handleSubmit`. También veremos los métodos `validateField()` y `validateForm()`

Vemos que tenemos las mismas props a nivel formulario que a nivel campo. Lo ideal es usar el objeto de nivel campo cuando tenemos que lidiar con ese campo en particular y el objeto a nivel formulario cuando tenemos que lidiar con algo de todo el formulario. Sin embargo contrariamente a esto usaremos las props de nivel formulario para controlar funcionalidades tanto a nivel formulario como campo.

> NOTA: La validación a nivel campo sería más adecuado hacerla desde las props obtenidas en dicho campo.

Agregamos dos botones:

```jsx
<button type='button'>Validate Comments</button>
<button type='button'>Validate All</button>
```

Le agregamos `onClick`
```jsx
<button type='button' onClick={() => formik.validateField('comments')}>
	Validate Comments
</button>
<button type='button' onClick={() => formik.validateForm()}>
	Validate All
</button>
```
Si hacemos click en **Validate Comments** con el formulario vacío veremos que no aparece el mensaje de error en pantalla. Sin embargo gracias al `console.log()` podremos notar que `errors`  se actualiza correctamente indicando el error de validación `{comments: 'Required'}` pero `touched` permanece vacío y como nuestra condición para mostrar un error es que este exista y que el campo haya sido visitado, es lógico lo que está sucediendo.

Lo mismo ocurre si presionamos **Validate All**

Esto podemos solucionarlo usando otros dos *helper methods* `setFieldTouched()` y `setTouched()` agregamos dos botones más encargados de esto:

```jsx
<button type='button' onClick={() => formik.setFieldTouched('comments')}>
	setFieldTouched
</button>
<button
	type='button'
	onClick={() =>
		formik.setTouched({
			name: true,
			email: true,
			channel: true,
			comments: true,
		})
	}
>
	setTouched
</button>
```

Si tuviéramos por ejemplo `name: false` no aparecería el mensaje de error de dicho campo ya que no lo incluiría el objeto `touched`.

Un caso en el que podríamos querer disparar la validación manual a nivel campo podría ser si queremos chequear si un nombre de usuario existe en la base de datos. En ese caso podríamos agregar un botón que realizaría esta validación

Lo visto anteriormente en situaciones donde tengamos `validateOnChange={false}` y `validateOnBlur={false}` nos daría un control total de las validaciones del formulario.