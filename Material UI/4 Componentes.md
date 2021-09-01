# `Typography`
Podemos importarlo tanto con `import Typography from '@material-ui/core/Typography` o bien con `import {Typography} from '@material-ui` lo cual es menos performante.

```jsx
<Typography>
	texto
</Typography>
```
Si vamos a las DevTools veremos que por default nos muestra una una etiqueta `p`, si por cuestiones semánticas queremos que sea un `h1` podemos hacerlo utilizando la prop `variant`. El valor de `variant` podremos cambiarlo entre `h1`, `h2`, `subtitle1`, `body1`, etc.
```jsx
<Typography variant='h1'>
	texto
</Typography>
```

Si queremos tener los estilos de la tipografía `h1` de Material UI pero mostrando el tag `h2`:
```jsx
<Typography variant='h1' component='h2'>
	texto
</Typography>
```

Si queremos tener un `div` con los estilos de un `h2` podemos hacerlo con:
```jsx
<Typography variant='h2' component='div'>
	texto
</Typography>
```

* Con `color` podemos pasarle los theme colors, por ejemplo `color="primary"`, `color="textSecondary"`

* Con `align` podemos alinear el texto, por ejemplo: `align="center"`

* Con  `noWrap` podremos establecer un truncamiento cuando tenemos un texto largo (mostrando la primera línea y terminando con `...`) y que el texto no cambie a más de una línea
```jsx
<Typography noWrap>
	Lorem ipsum dolor sit amet, consectetur adipiscing elit. Proin eu urna vel eros commodo maximus. Mauris at ipsum non turpis sodales maximus sit amet quis ligula. Integer facilisis volutpat nunc sed lacinia. Morbi eget odio nisi. Praesent convallis varius lectus quis lobortis. Mauris feugiat orci vel mauris blandit bibendum.
</Typography>
```

* Con `gutterBottom` establecemos si queremos que tenga un margen inferior.

# `Button`
En este componente al igual que en `Typography` tendremos la prop `variant` (siendo `contained` el valor más común) y `color` para el color de fondo. Si elegimos un color de fondo oscuro automáticamente nos pondrá la letra en blanco.

Si lo usamos en un formulario podremos especificarle el `type="submit"`

# `ButtonGroup`
Se trata de un contenedor para un grupo de botones serán estilados como un continuado sin espacio y podremos especificar en este componente el estilo de tidos ellos por ejemplo `color`, `variant`, etc.

# `Container`
`Container` es un componente contenedor que aplica `margin` y `padding` de manera automática. Además con el es posible definir el máximo ancho que tendrá ese contenedor :
```jsx
	<Container max-width='xs'>
		(...)
	</Container>
```

# `CSSBaseline`
Con el componente `<CssBaseline />` obtenemos un punto de partida para comenzar a desarrollar, por ejemplo gracias a este componente establecemos `font-size: 0.875rem;`, junto con `box-sizing: border-box`, eliminamos los márgenes/paddings que tenemos por default, etc. 

Es conveniente agregarlo en el `root component` que en nuestro caso es `App.js` 

```jsx
import React from 'react';
import CssBaseline from '@material-ui/core/CssBaseline';

function App() {
  return (
    <>
      <CssBaseline />
      {/* The rest of your application */}
    </>
  );
}
```

# Material Icons
Podremos instalarlos con `npm install @material-ui/icons` y ver la lista completa de íconos en https://material-ui.com/components/material-icons/

* Por default serán de color negro, podremos cambiar el color con  `color="primary"`. También contamos con los colores `action`, `error` y `disabled`

* Por default son de tamaño mediano, podremos cambiar el tamaño a chicos o grandes con `fontSize="small"` o `fontSize="large"`

Es posible aplicar íconos a botones utilizando `startIcon={<SendIcon>}` o `endIcon={<KeyboardArrowRightIcon}`

# `Box`
Se trata de un contenedor.
Si queremos que `Box` sea renderizado como un `<nav>` (cosa que podremos constatar con las DevTools) debemos utilizar la prop `component` es decir:  `<Box component='nav'/></box>` el valor por defecto es `'div'`

Es posible trabajar con ella como una caja flexible `display="flex" ` https://material-ui.com/system/flexbox/

# `Avatar`
El componente `<Avatar/>` nos permite incorporar imágenes con los estilos típicos de un avatar.

Una forma de usarlo es especiifcando una imagen: `<Avatar src="/avatar.png" alt='Juan Ocho' />`
Siendo `avatar.png` un recurso ubicado en `public `

Otra forma de usarlo es pasándole una letra: `<Avatar>J</Avatar>`


# `Badge`
`Badge` se trata de un componente que genera una insignia en el vértice superior derecho de el o los componentes hijos.


# `Divider`
El componente `<Divider />` nos permite agregar una línea divisora.

# `Paper`
El componente `<Paper />` se trata de un contenedor con apariencia de papel y una cierta elevación.

# `Grid`
Material UI nos ofrece un sistema de 12 columnas basado en flex-box.

El componente `Grid` nos permite organizar el contenido, tenemos que tener un `<Grid container>` y luego tantos `<Grid item>` como necesitemos. Con `spacing` determinamos la separación entre esos elementos, en este caso `spacing={2}` será de 16px.
Si tenemos 
```jsx
<Grid container spacing={2} justify='center'>
	<Grid item>
		<Paper>1</Paper>
	</Grid>
	<Grid item>
		<Paper>2</Paper>
	</Grid>
	<Grid item>
		<Paper>3</PaperBox`
Se trata de un contenedor.
Si queremos que `Box` sea renderizado como un `<nav>` (cosa que podremos constatar con las DevTools) debemos utilizar la prop `component` es decir:  `<Box component='nav'/></box>` el valor por defecto es `'div'`

Es posible trabajar con ella como una caja flexible `display="flex" ` https://material-ui.com/system/flexbox/

# `AppBar`
Si queremos tener una barra superior debemos utilizar el componente `AppBar` y a su vez el componente `Toolbar` en su interior.
```jsx
<AppBar>
	<Toolbar>
	</Toolbar>
</AppBar>
```
Esta por default será del color `primary`.

Si queremos agregarle un icono de una flecha hacia atras:
```jsx
<>
	<AppBar>
		<Toolbar>
			<IconButton>
				<ArrowBack />
			</IconButton>
		</Toolbar>
	</AppBar>
</>
```

Si queremos darle color a la barra podremos hacerlo con `style` en `AppBar` (también le damos color al icono y al texto)

```jsx
<>
	<CssBaseline />
	<AppBar position='static' style={{ backgroundColor: '#222' }}>
		<Toolbar>
			<IconButton>
				<ArrowBack style={{ color: 'tomato' }} />
			</IconButton>
			<Typography variant='h6' style={{ color: 'tan' }}>
				Portfolio
			</Typography>
		</Toolbar>
	</AppBar>
</>
```

# CSSBaseline
Con el componente `<CssBaseline />` obtenemos un punto de partida para comenzar a desarrollar, por ejemplo gracias a este componente establecemos `font-size: 0.875rem;`, junto con `box-sizing: border-box`, eliminamos los márgenes/paddings que tenemos por default, etc. 

Es conveniente agregarlo en el `root component` que en nuestro caso es `App.js` 

```jsx
import React from 'react';
import CssBaseline from '@material-ui/core/CssBaseline';

function App() {
  return (
    <>
      <CssBaseline />
      {/* The rest of your application */}
    </>
  );
}
```

# Avatar
El componente `<Avatar/>` nos permite incorporar imágenes con los estilos tipicos de un avatar.
```jsx
<Avatar src={avatar} alt='Juan Ocho' />
```

# Badge
`Badge` se trata de un componente que genera una insignia en el vértice superior derecho de el o los componentes hijos.

# `List`, `ListItem`, `ListItemIcon` y `ListItemText`
El componente `List` se trata de una lista de elementos `ListItem` apilados uno arriba del otro, si queremos que tenga un icono usaremos también el componente `ListItmeIcon` colocando dentro el componente ícono como en este caso `<ArrowBack />`, mientras que para el texto usamos `ListItemText`

```jsx
<List>
	<ListItem>
		<ListItemIcon>
			<ArrowBack />
		</ListItemIcon>
		<ListItemText>
			BACK
		</ListItemText>
	</ListItem>
</List>
```

Para implementar esto programáticamente a partir de `sidebarLinks`
```jsx
<List>
	{/* Make list items from data array */}
	{sidebarLinks.map(({ id, title, url, icon }) => (
		<ListItem button key={id} onClick={() => history.push(`${url}`)}>
			<Tooltip title={title}>
				<ListItemIcon>{icon}</ListItemIcon>
			</Tooltip>
			<ListItemText primary={title} />
		</ListItem>
	))}
</List>
```
En `ListItem` usamos el atributo `button` para lograr un comportamiento de texto e ícono como si se tratara de un botón.

En `ListItemText` usamos atributo `primary` para colocar el texto a mostrar. 

Siendo `sidebarLinks`un array de objetos como el siguiente:

```jsx
const sidebarLinks = [
	{
		id: 1,
		url: '/',
		title: 'Dashboard',
		icon: <DashboardIcon />,
	},
	(...)
];
```

# Button
https://material-ui.com/api/button/
`color` podremos sobre escribir `primary` y `secondary` en el Material UI Theme

# Divider
El componente `<Divider />` nos permite agregar una linea divisora.

# Typography
El valor de `variant` podremos cambiarlo entre `h1`, `h2`, `subtitle1`, `body1`, etc.

```jsx
<Typography variant='h1'>
	texto
</Typography>
```

Si queremos tener un `div` con los estilos de un `h2` podemos hacerlo con:
```jsx
<Typography variant='h2' component='div'>
	texto
</Typography>
```

# Container
`Container` es un componente que nos permite definir el máximo ancho de ese contenedor 
```jsx
	<Container max-width='xs'>
		(...)
	</Container>
```

# Paper
Se trata de un contenedor con apariencia de papel `<Paper style={{ height: 75, width: 50}}/>`

# Grid
`Grid` nos permite organizar el contenido, tenemos que tener un `<Grid container>` y luego tantos `<Grid item>` como necesitemos.
Si tenemos 
```jsx
<Grid container spacing={2} justify='center'>
	<Grid item>
	(...)
	</Grid>
</Grid>
```
En este caso el  `Grid` hijo tomará todo el espacio necesario para mostrar sus elementos hijos.

Un ejemplo similar sería el siguiente.
```jsx
<Grid container spacing={2} justify='center'>
	<Grid item>
		<Paper style={{ height: 75, width: 50}}/>
	</Grid>
	<Grid item>
		<Paper style={{ height: 75, width: 50}}/>
	</Grid>
	<Grid item>
		<Paper style={{ height: 75, width: 50}}/>
	</Grid>
</Grid>
	
```
En este caso cada  `Grid` item tomará sólo el espacio necesario para mostrar sus elementos hijos, si en cambio queremos que tengan un ancho determinado una forma sería con: `<Paper style={{ height: 75, width: 50}}/>`

Sin embargo es posible especificarle la cantidad de columnas de ancho que queremos que tenga cada `item`. Además al ser un sistema responsivo podremos indicarle una cantidad de columnas dependiente del tamaño de pantalla.

Al componente `Grid` podemos pasarle las props:
* `direction` (que acepta los valores `row`, `row-reverse`, `column`, `column-reverse`) 
* `justify` (que acepta `flex-start`, `center`, `flex-end`, `space-between`, `space-around`, `space-evenly`) 
* `alignItems`(que acepta `flex-start`, `center`, `flex-end`, `stretch`, `baseline`)  

Un ejemplo típico será usar Grid items para cada elemento de un array que estemos mapeando:
```jsx
<Grid container>
	{notes.map(note => (
		<Grid item key={note.id} xs={12} md={6}>
			<Paper>{note.title}</Paper>
		</Grid>
	))}
</Grid>
```

## Responsive Grids
Los grids nos permiten organizar el contenido en  columnas cuyo ancho podrá ser variable de acuerdo al tamaño de la pantalla.

-   **xs,**  extra-small: 0px
-   **sm,**  small: 600px
-   **md,**  medium: 960px
-   **lg,**  large: 1280px
-   **xl,**  extra-large: 1920px
- 
Si ponemos `xs={12}` significa que para pantallas `xs` y superiores (es decir para todos los tamaños) queremos que ocupe 12 columnas. Mientras que si ponemos `xs={12} sm={6}` significa que queremos que ocupe 12 columnas para pantallas `xs` y `sm` y que de `md` en adelante ocupe 6
En cambio cuando queremos establecer la cantidad de cantidad de columnas que queremos que ocupe, debemos indicarlo mediante el pasaje de breakpoints. Esto es utilizado para el diseño responsivo.

Podremos establecer las props:
* `direction` (que acepta los valores `row`, `row-reverse`, `column`, `column-reverse`) 
* `justify` (que acepta `flex-start`, `center`, `flex-end`, `space-between`, `space-around`, `space-evenly`) 
* `alignItems`(que acepta `flex-start`, `center`, `flex-end`, `stretch`, `baseline`)  

## Responsive Grids
Los grids nos permiten organizar el contenido en  columnas cuyo ancho podrá ser variable de acuerdo al tamaño de la pantalla.
```jsx
<Grid container spacing={2} justify='center'>
	<Grid item xs={12} sm={6}>
		<Paper style={{ height: 75 }}>Texto1</Paper>
	</Grid>
	<Grid item xs={12} sm={6}>
		<Paper style={{ height: 75 }}>Texto2</Paper>
	</Grid>
	<Grid item xs={12} sm={6}>
		<Paper style={{ height: 75 }}>Texto3</Paper>
	</Grid>
</Grid>
```
>Notar que si tuviera cuatro items con `xs={4}` los primero tres irían en una fila y el restante abajo.

## Auto-Layout
Dijimos que si poníamos `<Grid item>` este ocuparía el espacio necesario para sus hijos, por lo que si ponemos:

```jsx
<Grid item>
	(...)
</Grid>
<Grid item sm>
	(...)
</Grid>
<Grid item>
	(...)
</Grid>
```

El primero y el último ocuparán el espacio que necesitan para sus hijos y el del medio ocupará el resto del espacio. 
Si tuviéramos dos veces el elemento del medio se repartirían en dos el espacio libre.

# `CircularProgress`
El componente `CircularProgress` nos permite mostrar la animación de "CARGANDO"

# InputBase
`InputBase` trata de un componente que podemos utilizar para personalizar inputs. 

Si queremos que tenga un ícono al comienzo podemos utilizar la prop `startAdornment` (`endAdornment` si lo queremos al final) [API](https://material-ui.com/api/input-base/)

# Cards
Si queremos trabajar con un card personalizado podemos tomar como base lo mostrado en la [documentación](https://material-ui.com/components/cards/) y eliminar aquellas partes que no nos interesen.
```jsx
<Card elevation={1}>
   <CardHeader
     action={
       <IconButton onClick={() => handleDelete(note.id)}>
         <DeleteOutlined />
       </IconButton>
     }
     title={note.title}
     subheader={note.category}
   />
   <CardContent>
     <Typography variant="body2" color="textSecondary">
       { note.details }
     </Typography>
   </CardContent>
 </Card>
```
También es posible tener un `Avatar` en la parte de `CardHeader` asignándoselo con la prop `avatar`

```jsx
 avatar={
	<Avatar aria-label="recipe" className={classes.avatar}>
		 R
	</Avatar>
}
```

Si bien esto no tiene que ver con Material UI como dato de color notar que en prop `action` en lugar de trabajar con `MoreVertIcon` se agregó el ícono `DeleteOutilined` y al presionarlo se llama a una función `handleDelete(note.id)`. Esta función es recibida via props y la implementación en el padre es la siguiente:


```jsx
const handleDelete = async (id) => {
  await fetch('http://localhost:8000/notes/' + id, {
    method: 'DELETE'
  })
  const newNotes = notes.filter(note => note.id != id)
  setNotes(newNotes)
}
```

Es decir que por un lado lo borramos del json administrado por **JSON Server** y por otro modificamos la variable de estado de modo que la UI también se actualice. Otra opción hubiera sido hacer a `useEffect` dependiente de una variable de estado `refresh` que cuando esta sea `true` deba realizar un nuevo fetch y resetearla luego de dicho fetch, pero esto sería más engorroso y sin ninguna ventaja aparente.eramos dos veces el elemento del medio se repartirían en dos el espacio libre.

# Loading
El componente `CircularProgress` nos permite mostrar la animación de "CARGANDO"

# Material Icons
Podemos todos los íconos en https://material-ui.com/components/material-icons/

Por default serán de color negro, podremos cambiar el color con  color="primary".
Por default son de tamaño mediano, podremos cambiar el tamaño a chicos o grandes con `fontSize="small"` o `fontSize="alrge"`
Por ejemplo podremos trabajar con `<HomeIcon/>`

# InputBase
`InputBase` trata de un componente que podemos utilizar para personalizar inputs. 

Si queremos que tenga un ícono al comienzo podemos utilizar la prop `startAdornment` (`endAdornment` si lo queremos al final) [API](https://material-ui.com/api/input-base/)

```jsx

```

```jsx

```

```jsx

```

```jsx

```