# Layout
En ocasiones queremos que todos nuestras páginas tengan ciertas características como puede ser un `AppBar` y un `Drawer`. En esos casos podemos crear un archivo en `components` llamado `Layout.js` con un componente funcional que wrapee a los componentes propios de cada página:

En `App.js` tendremos:
```jsx
function App() {
  return (
    <ThemeProvider theme={theme}>
      <Router>
        <Layout>
          <Switch>
            <Route exact path="/">
              <Notes />
            </Route>
            <Route path="/create">
              <Create />
            </Route>
          </Switch>
        </Layout>
      </Router>
    </ThemeProvider>
  );
}
```

```jsx
import React from 'react'

export default function Layout({ children }) {
  return (
    <div>
      <div>app bar</div>
      <div>side drawer</div>

      <div>
        { children }
      </div>
    </div>
  )
}
```

Notar que hacemos uso de `{children}` para mostrar el contenido propiamente dicho de cada página.

# Implementación con Next.js
En caso de trabajar con Nextjs la implementación es similar sólo que en `_app.js` debemos wrapear a `<Component/>` con ese componente:
```jsx
function MyApp({ Component, pageProps }){
	return (
		<Layout>
			<Component {...pageProps} />
		</Layout>
	)
}
```



```jsx

```