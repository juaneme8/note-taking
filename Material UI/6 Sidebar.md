# Drawer
En caso de querer implementar este componente en varias páginas leer la unidad sobre Layout Component.

En la [documentación](https://material-ui.com/components/drawers/) nos encontramos con distintos tipos de Drawers, en este ejemplo buscamos desarrollar uno permanente partiendo sólo del componente `Drawer` y colocamos dentro un `div` y un `Typography`.

Establecemos `drawerWidth` como una constante ya que lo usaremos en más de una oportunidad ya que para definir el ancho debemos especificarlo tanto sobre la clase de `Drawer` como así también sobrescribiendo la clase `paper` que este tiene dentro.

```jsx
import React from 'react'
import { makeStyles } from '@material-ui/core'
import Drawer from '@material-ui/core/Drawer'
import Typography from '@material-ui/core/Typography'

const drawerWidth = 240

const useStyles = makeStyles({
  root: {
    display: 'flex',
  },
  drawer: {
    width: drawerWidth,
  },
  drawerPaper: {
    width: drawerWidth,
  },
})

export default function Layout({ children }) {
  const classes = useStyles()

  return (
    <div className={classes.root}>
      {/* app bar */}

      {/* side drawer */}
      <Drawer
        className={classes.drawer}
        variant="permanent"
        classes={{ paper: classes.drawerPaper }}
        anchor="left"
      >
        <div>
          <Typography variant="h5" className={classes.title}>
            Ninja Notes
          </Typography>
        </div>

        {/* links/list section */}
        
      </Drawer>

      {/* main content */}
      <div>
        { children }
      </div>
    </div>
  )
}
```

Si no tuviesemos la clase `root` el contenido (representado en este ejemplo por `{children}`) nos quedaría abajo del `Drawer`.

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

Otra forma de trabajar con `ListItemText` es poniendo directamente `<ListItemText primary="BACK"/>`

### Programáticamente y usando `useHistory()`
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

==Verificar si es necesario==  `history.push(`${url}`)` ya que en otros ejemplos lo vi con la forma `history.push(item.url)`

Estamos usando `history.push()` para redireccionar lo cual es gracias a `const history = useHistory()` 

En `ListItem` usamos la prop `button` para lograr un comportamiento de texto e ícono como si se tratara de un botón.

En `ListItemText` usamos atributo `primary` para colocar el texto a mostrar. 

Siendo `sidebarLinks`un array de objetos como el siguiente:

```jsx
const sidebarLinks = [
	{
		id: 1,
		url: '/',
		title: 'Dashboard',
		icon: <DashboardIcon color="secondary"/>,
	},
	(...)
];
```
Notar que el `id` podríamos omitirlo usando como `key={title}` ya que sabemos que es *unique*.


### `useLocation()`
Si queremos darle un estilo específico a los links dependiendo de si nos encontramos en esa página, podemos hacerlo haciendo uso de otro hook de **react-router-dom** llamado `useLocation()` para saber en qué página nos encontramos. Es decir primero definimos `const location = useLocation();` y luego en `location.pathname` tenemos la ubicación.

Por último le asignamos a `ListItem` condicionalmente la clase `active` 
```jsx
<
ListItem
	button
	key={id}
	onClick={() => history.push(`${url}`)}
	className={location.pathname == url ? classes.active : null}
/>
```

# Comentarios Implementación con con Next.js
Visitar la [documentación](https://nextjs.org/docs/api-reference/next/link) y [este artículo](https://www.manuelkruisz.com/blog/posts/next-js-links-and-material-ui) sobre el uso de ListElement de Material UI y Link de Next.js.

Debemos importar `Link` con `import Link from 'next/link'` y luego lo usamos para envolver a `ListItem`, como `href` le pasamos la url y además el atributo `passHref` esto lo hacemos para forzar el pasaje de `href` al elemento `ListItem`. Además especificamos que `ListItem` es `component="a"` de manera explícita pues de lo contrario tendremos un `div` con atributo `href`.

```jsx
<List>
	{sidebarLinks.map(({ id, title, url, icon }) => (
		<Link key={id} href={url} passHref>
			<ListItem button component='a'>
				<Tooltip title={title}>
					<ListItemIcon>{icon}</ListItemIcon>
				</Tooltip>
				<ListItemText primary={title} />
			</ListItem>
		</Link>
	))}
</List>
```

# Diferencias entre `router.push()`, `Link` y `a`
`router.push('/push')`  behaves similar to window.location. It does not create a tag, which means if you are concern with SEO, your links will not be detected.
`<Link>`  creates a link which means your links will be detected when crawlers scrape your site. You navigate within javacscript without reloading the page, providing the behavior of a Single Page App.
`<a>`  tag without  `<Link>`  creates a link which reloads your website entirely to the url. (standard behavior).```jsx

```

```jsx

```

```jsx

```