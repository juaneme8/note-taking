# Disable Submit Button
En ocasiones por requerimientos de la aplicación o para mejorar la UX puede que queramos deshabiltar el botón de Submit bajo ciertas condiciones:

* Cuando el formulario tiene un **estadoestado del formulario es inválido**.
* Cuando el envío del formulario tiene **envío(*form submission*) está en proceso**.

## Deshabilitar botón por estado inválido
Analizando las props `formik` obtenidas al implementar el patrón render props en el componente `Formik` veremos que cuenta con una propiedad `isValid`. Se trata de una propiedad de solo lectura que se pondrá en true si el objeto `errors` está vacío. Es por esto que al iniciar la aplicación `isValid: true`. En la medida que hacemos click en un campo y luego fuera veremos que pasa a ser `false`.
Podemos usar esta propiedad para deshabilitar el formulario `disabled={!formik.isValid}` pero esto implicaría que cuando iniciemos la aplicación dado que el objeto erros está vacío, nos aparezca disponible el botón cuando claramente los datos no son válidos.

```jsx
<button type='submit' disabled={formik.isValid}>
	Submit
</button>
```

Si tenemos un **formulario con pocos campos y validaciones simples** podemos solucionar esto con la prop de `Formik` `validateOnMount` (como es una *boolean prop* no hace falta especificar que es igual a `true`.

```jsx
	return (
		<>
			<Formik initialValues={initialValues} onSubmit={onSubmit} validationSchema={validationSchema} validateOnMount>
				{formik => {
					console.log('Formik Props', formik);
					return (
						<Form>
							<div className='form-control'>
								(...)
							</div>
				
							<button type='submit' disabled={!formik.isValid}>
								Submit
							</button>
						</Form>
					);
				}}
			</Formik>
		</>
	);
}
```

Como consecuencia de esto formik ejecutará las validaciones contra cada campo y completará el objeto `errors` y de esa manera `isValid` pasará a ser `false` con lo cual se deshabilitará el botón.

Decimos que este método es apropiado para un formulario con pocos campos y validaciones simples porque no tiene mucho sentido ejecutar todas las validaciones antes de que el usuario haya ingresado un caracter.

Otra solución más apropiada para casos donde tenemos un **formulario con muchos campos o una validación compleja** es usar la propiedad `dirty` que indica si al menos un campo del formulario cambió desde que fue inicializado. Al inicializar `dirty: false` y si cambiamos apenas un valor pasará a ser `true` (si sólo hacemos click en un campo y luego click fuera no cambiará).

```jsx
<button type='submit' disabled={!(formik.dirty && formik.isValid)}>
	Submit
</button>
```

Ocultamos el botón de submit si el usuario no ingresó ningún dato o si los datos ingresados son inválidos. `!(formik.dirty && formik.isValid)` es lo mismo que `(!formik.dirty || !formik.isValid)`

Sin embargo esta implementación también tiene una desventaja en el hecho de que si los valores iniciales a validar son correctos, el botón estará deshabilitado ya que `dirty: false` **Solo debemos usar este método si asumimos que siempre el estado del formulario será inválido.**

Tener en cuenta que si tenemos en el campo `name: 'Juan'` si borro la `n` `dirty: true` pero si vuelvo a agregar una `n` volverá a ser `false`

## Deshabilitar botón por submit en proceso
Queremos deshabilitar el formulario una vez que el usuario hizo click en el botón de submit. En el caso de un registro de usuario cuando hace click luego de completar sus datos, haremos una llamada a una API para guardar dichos datos durante ese tiempo queremos tener deshabilitado el formulario hasta qu esa operación se complete.

Utilizaremos la propiedad `isSubmitting` por lo que tendremos que fijarnos si es `true` y en ese caso deshabilitar el botón.

```jsx
<button type='submit' disabled={formik.isSubmitting}>
	Submit
</button>
```

Si hacemos click en el botón Submit sin completar el formulario, analizando los logs veremos que `isSubmitting` pasa de `true` a `false` . Si en cambio completamos el formulario de modo tal que no haya errores veremos que se queda en `true` y esto ocasionará que sigamos con el botón deshabilitado. Tenemos que pasarlo a `false` manualmente en el método `onSubmit`
Hasta el momento tenemos:
```jsx
const onSubmit = values => {
	console.log('Form Data', values);
};
```
Pero `onSubmit()` recibe un segundo argumento `onSubmitProps` que al loguearlo en pantalla veremos que tiene un método `setSubmitting()`
```jsx
const onSubmit = (values, onSubmitProps) => {
	console.log('Form Data', values);
	console.log('Submit Props', onSubmitProps);

	//Esperar a la respuesta de la API
	//(...)

	onSubmitProps.setSubmitting(false);
};
```
De esta manera luego de enviar el formulario el botón volverá a estar disponible.

Si unimos las dos condiciones:

```jsx
<button type='submit' disabled={!formik.isValid || formik.isSubmitting}>
	Submit
</button>
```