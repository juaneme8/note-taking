# `AppBar`
En caso de querer implementar este componente en varias páginas leer la unidad sobre Layout Component.
Si queremos tener una barra superior debemos utilizar el componente `AppBar` y a su vez el componente `Toolbar` en su interior.
```jsx
<AppBar>
	<Toolbar>
	</Toolbar>
</AppBar>
```
Esta por default será del color `primary`.

Si queremos agregarle un icono de una flecha hacia atrás:
```jsx
<>
	<AppBar
		position="fixed"
		className={classes.appBar}
		elevation={0}
		color="primary"
	>
		<Toolbar>
			<Typography>Mario</Typography>
			<Avatar className={classes.avatar} src="/mario-av.png" />
		</Toolbar>
	</AppBar>
</>
```

## Establecer ancho desde fin `Drawer`
Si tenemos además un componente `Drawer` vamos a querer que la barra comience cuando termina dicho menu lateral. Eso podemos realizarlo asignándole una clase y dándole el siguiente ancho:
```jsx
 appbar:{
	 width: `calc(100% -  ${drawerWidth}px)`
	 marginLeft: drawerWidth,
 } 
```

## Establecer espaciado de contenido principal
Normalmente tanto `AppBar` como `Drawer` serán componentes que formarán parte de todas las páginas, es por eso que como vimos en la unidad sobre **Drawer** estos estarán en un componente `Layout` y recibirán el contenido principal como `props.children`. Sin embargo, la parte superior de ese contenido quedará pisado por el `AppBar`, para ello podemos colocar un `<div>` al cual le asignamos usando mixins las clases de `AppBar`

```jsx
<div className={classes.page}>
	<div className={classes.toolbar}></div>
	{ children }
</div>
```

```jsx
useStyles = makeStyles(theme => ({
	toolbar:theme.mixins.toolbar,

	page: {
		background: '#f9f9f9',
		width: '100%',
		padding: theme.spacing(3),
	},
}));

```