# Personalizar Theme
Chakra está basado en los lineamientos de [System UI Theme Specification](https://system-ui.com/theme/)

Para personalizarlo creamos un archivo `theme.tsx` en la carpeta `src` si estamos utilizando **Vitejs** o **create-react-app** (o en la raíz si estamos trabajando con Next.js).

En un principio colocamos simplemente:
```jsx
import { extendTheme } from '@chakra-ui/react';

export default extendTheme({});
```
Más adelante colocaremos dentro del objeto que recibe `extendTheme` aquellas características que queremos modificar.

Luego para comenzar a usar este `theme` personalizado se lo tenemos que pasar al `ChakraProvider` y esto lo haremos en `main.tsx` (si estamos trabajando con CRA o Vite) o en `_app.tsx` (si estamos trabajando con Next.js)

Si estamos trabajando con CRA o Vitejs,  `main.tsx` nos quedará:
```jsx
import React from 'react';

import { ChakraProvider } from '@chakra-ui/react';
import ReactDOM from 'react-dom';

import App from './App';
import theme from './theme';

ReactDOM.render(
  <React.StrictMode>
    <ChakraProvider theme={theme}>
      <App />
    </ChakraProvider>
  </React.StrictMode>,
  document.getElementById('root')
);
```

En cambio si estamos trabajando con Next.js `_app.tsx` nos quedará:

```jsx
import { ChakraProvider } from '@chakra-ui/react';
import { AppProps } from 'next/app';

import theme from '../theme';
const App: React.FC<AppProps> = ({
  Component,
  pageProps,
}) => {
  return (
    <ChakraProvider theme={theme}>
      <Component {...pageProps} />
    </ChakraProvider>
  );
};

export default App;
```

## Color Personalizado
En este caso en `theme.tsx` además de `extendTheme` importamos `theme` que usaremos para obtener uno de los colores y asignarlo a nuestro color personalizado.

Supongamos que queremos que `primary` represente al color purple de modo tal que si en un futuro queremos cambiarlo a otro, no debemos cambiar en todos los lugares donde lo hayamos usado y bastará sólo con cambiarlo en el theme.

```jsx
import { extendTheme, theme } from '@chakra-ui/react';

export default extendTheme({
  colors: {
    primary: theme.colors.purple,
  },
});
```

> En vez de `primary` podemos usar `brand` que también suele ser un nombre de variable conveniente.

Luego como ya dijimos le pasamos al `<ChakraProvider>` este `theme` que exportamos.

Como consecuencia de esto podremos usar el color `primary` y mapeará con `purple` y si luego lo cambiamos todos los lugares donde hayamos usado `primary` se verán afectados.

> También podríamos usar `primary.500` y será equivalente a `purple.500` 



## Estilos Globales

Es posible agregar estilos globales trabajando con `theme.styles.global`. Este estilo puede ser un objeto o una función que recibe las props y retorna un objeto de estilos.

Vamos a ver dos ejemplos en los que usamos `global` como un objeto:

```jsx
import { extendTheme, theme } from '@chakra-ui/react';

export default extendTheme({
  styles: {
    global: {
      body: {
        backgroundColor: 'primary.50',
      },
    },
  },
});
```

Como segundo ejemplo, en ocasiones vamos a querer que los elementos `html` y `body` no tengan una altura dependiente del contenido, sino que ocupen todo el alto.

Esto es útil por ejemplo si tenemos una línea que ocupa de punta a punta la pantalla separando un sidebar del contenido (UI de Twitter por ejemplo). 

Para lograr esto, en `theme.js` debemos definir estilos globales:

```jsx
import { extendTheme, theme } from '@chakra-ui/react';

export default extendTheme({
  styles: {
    global: {
      'html, body, #root': {
        height: '100%',
      },
    },
  },
});

```

Luego por ejemplo suponiendo que es en`Layout.js` donde queremos tener el contenedor del 100% y ahí asignamos este `height` tanto al `Container` como al `Stack`.

```jsx
<Container height="100%" maxWidth="container.lg">
      <Stack
        direction="row"
        height="100%"
      >
        (...)
      </Stack>
    </Container>
```



Lo mismo podemos realizarlo utilizando una función que recibe las `props` y retorna un objeto de estilos.

```jsx
styles:  {
	global:  (props)  =>  ({
		'html, body, #root': {
	        height: '100%',
	     },
	}),
}
```

Esto tiene varias utilidades, una de ellas podría ser si queremos definir un color para *light mode* y otro para *dark mode* en ese caso llamamos a `mode(color1, color2)(props)`
Importamos a `import { mode } from '@chakra-ui/theme-tools';
`

```jsx
styles: {
    global: (props: any) => ({
      'html, body': { color: mode('#FF0000', undefined)(props)},
    }),
 },
```

Como consecuencia de esto en *light mode* veremos el texto en color rojo y en *dark mode* se usará el color que estaba por default ya que le pusimos *undefined* como valor.



## Modificar Componentes Chakra

> Más info en la [documentación](https://chakra-ui.com/docs/theming/component-style)

En ocasiones puede que queramos que nuestros componentes tengan alguna característica ligeramente distinta de los que vienen por default con Chakra-UI. 

Si queremos modificar un botón y utilizamos Material UI tendríamos que crear una carpeta `ui`, luego otra `controls` y allí un archivo `Button.js` donde , importamos el componente Button de la librería, modificamos la característica deseada hacemos el spread de las props y luego lo exportamos para usarlo en todos lados. Esto demanda la creación de más componentes, con el consecuente tiempo y trabajo, veremos que con Chakra podemos modificar directamente el *component style* desde el theme.

Podemos editar el archivo `theme.js` y suponiendo también que queremos modificar el componente `Button` para que tenga bordes completamente redonados y fuente mas pequeña podemos hacer lo siguiente:

```jsx
components: {
    Button: {
      baseStyle: {
        borderLeftRadius: 9999,
        borderRightRadius: 9999,
      },
      sizes: {
        lg: {
          fontSize: 'md',
        },
      },
    },
  },
```

Luego cada vez que agreguemos un botón tendrá las características que acabamos de definir:

```jsx
  <Button colorScheme="primary" fontWeight="bold" size="lg">

```

Otra utilidad la encontramos como consecuencia de que en *dark mode* Chakra-UI cambia algunos colores de los botones, por ejemplo si tenemos un color `twitter` veremos que en *dark mode* es mas clarito. En ese caso podemos asegurarnos tener los colores deseados podemos trabajar con `components.Button.variants.solid`

```jsx
components:{
	Button:{
		variants: {
			solid: (props) => ({
				backgroundColor: `${props.colorScheme}.500`,
				color: mode(undefined, "white")(props),
				fontWeight: "bold",
				_hover:{
					backgroundColor: `${props.colorScheme}.600`,
				}
			})
		}
	}
}
```

> En este caso estamos utilizando una función que recibe las props y retorna un objeto de estilos.

Con ``${props.colorScheme}.500`` nos aseguramos que si le pasamos `colorScheme="twitter"` tengamos un `twitter.500`



Otro ejemplo de personalización de componentes podría ser si tenemos `Container` y queremos especificar su `maxWidth` cambiando el valor en `px` que representa el `"container.md"` , podemos hacer un override de una manera similar a la que usamos para definir colores personalizados:
```jsx
import { extendTheme, theme } from '@chakra-ui/react';

export default extendTheme({
  sizes:{
	  container:{
		  md: '200px',
	  } 
  }
});
```



# Color Mode
Chakra incluye un modo de manejar el *color mode* en las aplicaciones. Chakra almacena el *color mode* en el localStorage y le agrega una clase al `body` para asegurarse que sea persistente.

Para obtener *dark mode* debemos hacer dos cosas:

## 1. Actualizar theme `config`:
Si queremos establecer que al iniciar tengamos el modo oscuro podemos hacerlo mediante la personalización del theme con la propiedad `config` como un objeto con la propiedad `initialColorMode: 'dark'` y `useSystemColorMode` en `false` ya que no queremos que cambie de acuerdo a las preferencias del sistema del usuario.

```jsx
import  { extendTheme }  from  "@chakra-ui/react"

const config =  {
 initialColorMode:  "light",
 useSystemColorMode:  false,
}

const theme =  extendTheme({ config })

export default theme
```
Luego debemos pasarle esta `theme` customizado al `ChakraProvider`.

## 2. Agregar `ColorModeScript`
El `ColorModeScript` debe ser agregado antes del `body` para que el localStorage esté sincronizado correctamente.

Ver la [documentación](https://chakra-ui.com/docs/features/color-mode#add-colormodescript) ya que difiere la implementación en Next.js, CRA, etc.

En Next.js debemos hacerlo en `_docuement.js`

```jsx
import { ColorModeScript } from "@chakra-ui/react" 
import NextDocument, { Html, Head, Main, NextScript } from "next/document" 
import theme from "./theme" 

export default class Document extends NextDocument { 
	render() {
		 return (
			  <Html lang="en"> 
			  <Head />
			  <body>
			  <ColorModeScript initialColorMode={theme.config.initialColorMode} /> 
			  <Main /> 
			  <NextScript /> 
			  </body> 
			  </Html> 
		) 
	} 
}
```

# Text Styles & Layer Styles
En la mayoría de los proyectos nos encontraremos repitiendo *text properties* como ser `fontSize`, `fontWeight` o *layer styles* como ser `backgroundColor`, `color`, `boxShadow`, etc. 

Las props `textStyle` y `layerStyle` nos permitirán ser consistentes reutilizando estas características. Bastará con pasarle esas propiedades a cualquier componente y adquirirá las propiedades deseadas.

## Layer Style
Los *layer styles* podremos definirlos en el `theme` bajo la propiedad `layerStyles` de modo de que sean reutilizables.
Supongamos que queremos tener un `layerStyle` llamado `base` y otro `selected` que luego se lo pasaremos a un componente `Box`.
```jsx
import  { extendTheme }  from  "@chakra-ui/react"

const theme =  extendTheme({
	layerStyles:  {
		base:  {
			bg:  "gray.50",
			border:  "2px solid",
			borderColor:  "gray.500",
		 },
		selected:  {
			bg:  "teal.500",
			color:  "teal.700",
			borderColor:  "orange.500",
		 },
	},
})

function  Example()  {
  return  <Box  layerStyle="selected">This is a box</Box>
}
```

También es posible definir el valor de `layerStyle` de acuerdo a una variable por ejemplo `isSelected`:

```jsx
const layerStyle = isSelected ?  "selected"  :  "base"
return  <Box  layerStyle={layerStyle}>This is a box</Box>
```

## Text Styles
De manera similar los *text styles* podremos definirlos en el `theme` bajo la propiedad `textStyles`.

```jsx

import { extendTheme } from "@chakra-ui/react"
const theme = extendTheme({
  textStyles: {
    h1: {
      // you can also use responsive styles
      fontSize: ["48px", "72px"],
      fontWeight: "bold",
      lineHeight: "110%",
      letterSpacing: "-2%",
    },
    h2: {
      fontSize: ["36px", "48px"],
      fontWeight: "semibold",
      lineHeight: "110%",
      letterSpacing: "-1%",
    },
  },
})
function Example() {
  return <Box textStyle="h1">This is a box</Box>
}
```