# Chakra UI
> [# Chakra UI + Next JS Quickstart](https://youtu.be/lhOvI9s5gQY)
> https://www.freecodecamp.org/news/how-to-use-chakra-ui-with-next-js-and-react/
> Directo de Twitch de Goncy sobre UI Twitter (3 partes)
> Directo de Twitch de Goncy sobre Introducción a Chakra UI.
> Directo de Twitch de Goncy sobre UI ML https://www.twitch.tv/goncypozzo/video/1013551948

# Características
Chakra UI se define como una **librería de UI simple, modular y accesible**.
* Está basado en la especificación de [Styled-Systems](https://styled-system.com/) donde explica cómo deberíamos crear un theme de manera eficiente.
* Cuenta con theme default que puede ser personalizado fácil.
* Para estilos y personalización utiliza Emotion *under the hood* creando clases de style que se aplican en run time.
* Soporta **dark mode** de manera nativa.
* Nos permite realizar sitios **responsive** sin la necesidad de escribir media queries.
* Es accesible ya que sigue los lineamientos de WAI-ARIA y esto podemos notarlo fácilmente ya que las páginas pueden ser navegadas utilizando la tecla TAB.

## Ventajas de Chakra
En librerías como **Material-UI** cuando queremos modificar un componente que ellos nos proporcionan, debemos crear una carpeta `components`, importamos el componente y extendemos alguna propiedad que queremos modificar. Luego lo exportamos y utilizamos ese componente modificado de acuerdo a nuestras necesidades. Esto puede volverse tedioso si tenemos que hacerlo para muchos componentes.  

Con Chakra podemos editar el archivo `theme` no solo para personalizar colores, tipografías, espaciados, sino también para modificar alguna propiedad de un componente y así extender sus funcionalidades.
Al instalarlo importamos `Emotion` que es muy similar a styled-components en cuando a uso, por lo que podríamos crear componentes que no estén en Chakra.

## Chakra-UI vs Tailwind
La primera diferencia que podemos mencionar es que Tailwind usa CSS mientras que Chakra-UI utiliza CSS-in-JS. Otra diferencia es que con Chakra contamos con gran cantidad de componentes de base con los cuales comenzar. 

# Instalación 
## Con Vite
En primer lugar ejecutamos `npm init @vitejs/app` luego indicamos el nombre del proyecto, que utilizaremos React y TypeScript. A continuación navegamos a esa carpeta que nos creo y ejecutamos `npm install`

## Con Next.js
`npx create-next-app`

# Instalación Dependencias
En primer lugar instamos las dependencias:
`npm i @chakra-ui/react @emotion/react@^11 @emotion/styled@^11 framer-motion@^4`



# Primeros pasos con Chakra-UI 

## Con Vite

* Si estamos utilizando Vite debemos editar `app.tsx` de modo que nos queda
```jsx
import React from 'react'
import ReactDOM from 'react-dom'
import App from './App'
import {ChakraProvider} from '@chakra-ui/react'

ReactDOM.render(
  <React.StrictMode>
    <ChakraProvider>
    <App />
    </ChakraProvider>
  </React.StrictMode>,
  document.getElementById('root')
)
```



## Con Next.js

Si estamos utilizando Next.js de manera similar debemos editar `_app.js` retornando el `ChakraProvider` para que todos los componentes accedan a los estilos y configuraciones default.

```jsx
import Layout from '../components/Layout';

function MyApp({ Component, pageProps }) {
  return (
    <ChakraProvider>
      <Layout>
        <Component {...pageProps} />
      </Layout>
    </ChakraProvider>
  );
}
export default MyApp;
```

