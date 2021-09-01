# Custom Themes
Material UI cuenta con un [Default Theme](https://material-ui.com/customization/default-theme/) que establece todas las características de los estilos, por ejemplo que la tipografía sea Roboto o que los colores primarios y secundarios sean tal o cual. 
Sin embargo es posible personalizar alguno de estos aspectos logrando que tomen precedencia esos valores por sobre los default. Para ello creamos un archivo `theme.js` donde pondremos los valores que queremos personalizar 

[Documentación](https://material-ui.com/customization/theming/#createmuitheme-options-args-theme)

```jsx
import { createMuiTheme } from '@material-ui/core/styles';
import purple from '@material-ui/core/colors/purple';
import green from '@material-ui/core/colors/green';

const theme = createMuiTheme({
	palette: {
		primary: {
			main: purple[500],
		},
		secondary: {
			main: green[500],
		},
		background:{
			default: #f4f5fd
		}
	},
});

export default theme;
```

> Con `background:{default: #f4f5fd}` cambiamos el color de fondo por default.

> Estamos usando los colores de Material UI, si quisiéramos usar colores hexadecimales bastaría con poner: `main: '#707070'`

> Notar que si quisiéramos que todos los colores secundarios (light, main y dark) fueran violetas podríamos poner directamente `secondary: purple` sin la necesidad de especificar uno a uno.

Tendremos que envolver con `<ThemeProvider>` a nuestra aplicación https://material-ui.com/styles/api/#themeprovider para que pueda acceder a estos estilos. Una opción es hacelro en `index.js`, mientras que otra sería hacerlo directamente en `App.js`

Primero importamos 
```jsx
import { ThemeProvider } from '@material-ui/core/styles';
import theme from './theme';

ReactDOM.render(
	<React.StrictMode>
		<ThemeProvider theme={theme}>
			<App />
		</ThemeProvider>
	</React.StrictMode>,
	document.getElementById('root')
);

```

Contamos con la siguiente herramienta para la elección de colores: https://material.io/resources/color

# Cambiar fuente default
En `index.css` importamos la fuente deseada por ejemplo Poppins

```css
@import url('https://fonts.googleapis.com/css2?family=Poppins:wght@100;400;600&display=swap');
```

Como en el default *theme* Roboto es la fuente por defecto debemos especificar la nueva tipografía en en `theme.js`:

```jsx
import { createMuiTheme } from '@material-ui/core/styles';

const theme = createMuiTheme({
	(...)

	typography: {
		fontFamily: `'Poppins', sans-serif`,
	},
});

export default theme;

```

# propiedad `overrides`
Con `overrides` es posible personalizar los estilos inyectados en DOM por Material UI.
Por ejemplo supongamos que queremos modificar el tamaño de fuente de todos los `Button`. Para ello vamos a la [documentación de la API](https://material-ui.com/api/button/) y donde dice **Component name** obtenemos `MuiButton`.
Luego si inyectamos un `<Button>` y lo examinamos con las DevTools, veremos que la clase que tiene asociado el tamaño de fuente es `.MuiButton-root-139` por lo que nos fijamos el **Rule Name** y obtenemos `root`

```jsx
const theme = createMuiTheme({
  overrides: {
    MuiButton: {
      root: {
        fontSize: '1rem',
      },
    },
  },
});
```

# propiedad `props`
Con `props` también podremos afectar características de los componentes de manera global, pero aquellas indicadas en la documentación con el título de **Props**. 
Por ejemplo supongamos que queremos que por default `IconButton` no tenga el ripple al hacer click en el botón.
En primer lugar obtenemos el **Component name** que en este caso es `MuiIconButton` y luego queremos afectar la prop `disableRipple`:

```jsx
const theme = createMuiTheme({
  props: {
	  MuiIconButton: {
		  disableRipple: true,
	  }
  }
  
  
});
```

# Múltiples Themes
Hoy en día muchas aplicaciones tienen múltiples themes y la selección de una u otra está dada por los gustos del usuario. Por lo que podríamos guardar estos themes en un array y cambiarlos de manera programática.