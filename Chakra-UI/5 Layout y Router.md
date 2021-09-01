# Layout
Cuando queremos que todas las páginas tengan determinados elementos en su maqueta como ser un un header o un sidebar podemos definir un componente `Layout`. Para ello creamos un archivo `layout.tsx` en el que definimos los estilos deseados. Para limitar el ancho de la pantalla usamos un `Container`, luego una `Image`, `Heading`, `Text` y `Divider`.

Este componente recibe como `children` el contenido específico de cada página a mostrar.
```jsx
import React from 'react';

import { Box, Container, Divider, Heading, Image, Text, VStack } from '@chakra-ui/react';
const Layout: React.FC = ({ children }) => {
  return (
    <Box padding={4}>
      <Container backgroundColor="white" borderRadius="sm" boxShadow="md" maxWidth="container.xl" padding={4}>
        <VStack mb={4}>
          <Image borderRadius="9999" src="//placehold.it/128x128" />
          <Heading>Titulo</Heading>
          <Text>Subtítulo</Text>
        </VStack>
        <Divider marginY={6} />
        {children}
      </Container>
    </Box>
  );
};
export default Layout;
```


Si estamos usando Next.js en `_app.tsx` tendremos que envolver a `<Component {...pageProps} />` con el componente `Layout`

```jsx
import { ChakraProvider } from '@chakra-ui/react';
import { AppProps } from 'next/app';

import Layout from '../components/layout';
import theme from '../theme';
const App: React.FC<AppProps> = ({ Component, pageProps }) => {
  return (
    <ChakraProvider theme={theme}>
      <Layout>
        <Component {...pageProps} />
      </Layout>
    </ChakraProvider>
  );
};

export default App;

```


# React Router  
Si estamos usando Vite.js como no estamos trabajando con Next.js que realiza un *filesystem routing* podemos trabajar con React Router: `npm install react-router-dom`  

React Router DOM no tiene incluidos los tipos dentro de la librería por lo que debemos instalar también:  
`npm install @types/react-router-dom`  

En `main.tsx` agregamos `<BrowserRouter>`  
```jsx  
import React from 'react';  
  
import { ChakraProvider, theme } from '@chakra-ui/react';  
import ReactDOM from 'react-dom';  
import { BrowserRouter } from 'react-router-dom';  
  
import App from './app';  
  
ReactDOM.render(  
  <React.StrictMode>  
    <BrowserRouter>  
      <ChakraProvider theme={theme}>  
        <App />  
      </ChakraProvider>  
    </BrowserRouter>  
  </React.StrictMode>,  
  document.getElementById('root')  
);  
```
Luego en `app.tsx` ponemos las rutas como `children` de `Layout`

```jsx
import React from 'react';

import { Redirect, Route, Switch } from 'react-router-dom';

import Layout from './layout';
import AboutScreen from './screens/About';
import FeedScreen from './screens/Feed';

const App: React.FC = () => {
  return (
    <>
      <Layout>
        <Switch>
          <Route exact component={AboutScreen} path="/about" />
          <Route exact component={FeedScreen} path="/" />
          <Redirect to="/" />
        </Switch>
      </Layout>
    </>
  );
};

export default App;

```

Mientras que `Layout` nos queda:
```jsx
import React from 'react';

import { Container, Stack, Text } from '@chakra-ui/react';

const Layout: React.FC = ({ children }) => {
  return (
    <Container maxWidth="container.lg">
      <Stack direction="row">
        <Stack>
          {/* Sidebar */}
          <Text fontWeight="bold" textColor="twitter.500">
            Logo Twitter
          </Text>
          <Text>Enlace 1</Text>
          <Text>Enlace 2</Text>
          <Text>Enlace 3</Text>
          <Text>Enlace 4</Text>
        </Stack>
        {/* Rutas */}
        {children}
      </Stack>
    </Container>
  );
};

export default Layout;
```