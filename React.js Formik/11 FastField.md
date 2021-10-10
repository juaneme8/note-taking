# Componente `FastField`
Se trata de un componente con propósitos de **optimización de performance** se recomienda si el formulario tiene más de 30 campos o si tiene campos con una lógica de validación muy compleja.
La documentación advierte que debe ser usado con atención: https://formik.org/docs/api/fastfield

Veremos las diferencias entre `FastField` y `Field`, 
```jsx
<div className='form-control'>
	<label htmlFor='address'>Address</label>
	<Field type='text' name='address'>
		{props => {
			console.log('Field Rendered');
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

Veremos que ante un cambio en el un campo, lo cual ocasiona un cambio en el estado del formulario hace que todos los componentes vuelvan a renderizarse. Si reemplazamos `Field` por `FastField` veremos que este sólo se vuelve a renderizar si cambiamos el valor de ese campo (en este caso `address`). 
`FastField` es una versión optimizada de `Field` que internamente implementa el método del ciclo de vida `shouldComponentUpdate`