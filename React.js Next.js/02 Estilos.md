# Estilos en Next.js
Como podremos ver en la [documentación](https://nextjs.org/docs/basic-features/built-in-css-support) es posible utilizar Next.js distintas técnicas para estilar nuestra aplicación.

## Hoja de Estilos Global

Si queremos importar una hoja de estilos global se recomienda hacerlo en `pages/_app.js` y del siguiente modo `import '../styles.css'` notar que Next.js extiende el concepto de `import` más allá de JavaScript. Estos estilos aplicarán a todas las páginas y componentes de nuestra aplicación, es por eso que para evitar conflictos se recomienda sólo importarlo en `pages/_app.js`.

> Recordemos que `_app.js` que es el root component y los distintos *page componentes* son renderizados allí:

En **desarrollo** nos permite que los estilos sean *hot reloaded* en la medida que los editamos mientras que en **producción** se los concatenará en un *minified* `.css`

## CSS Modules
Next.js soporta CSS Modules usando `[name].module.css` como convención de nombres. Esta tecnología se encarga de tener un scope local creando nombres de clases únicos, esto significa que podemos usar el mismo nombre de clase en distintos archivos sin preocuparnos por colisiones. Si a estos elementos los visualizamos en las DevTools veríamos que al nombre de la clase le agrega un **hash** de modo que sea único. 

Tener en cuenta que debemos usar selectores de clase y no selectores de elementos.

```jsx
import styles from './Button.module.css'
```

En producción son concatenados en *many minified and code-split* `.css` asegurándonos que la cantidad de CSS cargada para pintar la aplicación sea mínima.

> Por otra parte si queremos reutilizar un estilo en más de un componente podemos utilizar la hoja de estilos `global.css`.



## CSS-in-JS

Es posible utilizar cualquier solución de CSS-in-JS.

### Inline Styles
```jsx
function HiThere() {
  return <p style={{ color: 'red' }}>hi there</p>
}

export default HiThere
```

## Styled JSX
Styled JSX es una librería de CSS-in-JS que viene *out of the box* con Next.js que permite escribir CSS encapsulado y con alcance a nivel componente, lo que escribamos en uno de ellos no afectará a los restantes. 
Debemos utilizar el elemento `<style>` con el atributo `jsx` en el root del componente

> Podemos utilizar también la extensión **vscode-styled-jsx** para lograr el highlighting del código.

```jsx
// pages/index.js
function Home() {
  return (
    <div className="container">
      <h1>Hello Next.js</h1>
      <p>Let's explore different ways to style Next.js apps</p>
      <style jsx>{`
        .container {
          margin: 50px;
        }
        p {
          color: blue;
        }
      `}</style>
    </div>
  )
}

export default Home
```
Notar que a pesar de que estamos usando selectores genéricos si en otros componentes tenemos elementos `p` o `.container` estos no se verán afectados, no habrá colisiones ya que Styled JSX utiliza nombres de clases únicos.

Si queremos utilizar estilos globales con Styled JSX debemos agregar el atributo global `<style jsx global>{``}</style>` y esto lo hacemos en `_app.js` o en un componente `Layout` que incorporamos en `./pages/_app.js`.

> Tener en cuenta que si tuviéramos estilos globales en un componente que luego es desmontado estos dejarían de estar disponibles, es por eso que debemos ponerlos en `_app.js`.

```jsx
const App= ({ Component, pageProps }) => {
	return (
			<>
				<Component {...pageProps} />
				<style jsx global>{`
					html,
					body{
						(...)
					}
				´}</style>
			</>
			
	);
};
export default App;
```



## `Layout`

Cuando queremos que un componente se muestre en todas las páginas lo aconsejable es crear un componente `Layout`que wrapee a todas las páginas, esto lo hacemos en `App` dentro de `pages/_app.js`.

```jsx
// pages/_app.js
import React from 'react'
import App, { Container } from 'next/app'
import Layout from '../components/Layout'

class MyApp extends App {
  render() {
    const { Component, pageProps } = this.props

    return (
      <Container>
        <Layout>
          <Component {...pageProps} />
        </Layout>
      </Container>
    )
  }
}

export default MyApp
```

Siendo `Layout` el siguiente componente

```jsx
// components/Layout.js
function Layout(props) {
  return (
    <div className="page-layout">
		<Navbar/>
		{props.children}
		<Footer/>
    </div>
  )
}

export default Layout
```

> Como alternativa podríamos crear este componente `Layout` pero que en vez de usarlo en `_app` lo usemos en todas las páginas y le pasemos una prop `title` y que esta modifique el componente `Header` que nos entrega Next.js.

##  `AppLayout`

Otra opción muy similar es crear un componente `AppLayout` **que importamos en cada página** (allí tendremos por ejemplo los estilos globales, un `<main>` dentro del cual mostramos los `children`). 

> Tener presente que si AppLayout está en un componente y luego este se demonta y vamos a otro componente podríamos llegar a ver flashes al volver a montarse.
>
> 

A continuación mostramos `AppLayout` y a su vez una técnica para usar Styled JSX y a su vez importar los estilos de otros archivos:

```jsx
import styles, {globalStyles} from './styles'

export default function AppLayout ({children}) {
  return (
    <>
      <div>
        <main>
          {children}
        </main>
      </div>
      <style jsx>{styles}</style>
      <style jsx global>{globalStyles}</style>
    </>
  )
}
```

Gracias a la línea `import styles, {globalStyles} from './styles'` vamos a importar los estilos que están en un archivo `styles.js`. 

> Prestar atención a que estamos importando `import css from 'styled-jsx/css'` para poder utilizar `css` y `css.global`

```jsx
import css from 'styled-jsx/css'

export const globalStyles = css.global`
  html,
  body {
    padding: 0;
    margin: 0;
    font-family: ${fonts.base}
  }

  * {
    box-sizing: border-box;
  }
`

export default css`
  div {
    display: grid;
    height: 100vh;
    place-items: center;
  }
`
```

> El valor que le damos a `font-family` proviene de `${fonts.base}` siendo `fonts` un objeto que importamos de un archivo `theme.js` donde tenemos:

```jsx
export const fonts = {
  base: 'system-ui, -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, Ubuntu, "Helvetica Neue", sans-serif'
}
```
> La idea es trabajar con la fuente del sistema operativo con el que visite la página el usuario tanto para asegurarnos que no tenga que descargarla como así también que sea similar al entorno en el cual está trabajando.

### Afectar estilos de un componente hijo
Supongamos que tenemos un componente que tiene un `button` con Styled JSX podemos darle estilos de manera simple, pero si a su vez sabemos que en los children habrá un `svg` puede que estemos tentados a hacer `button svg` pero como los estilos tienen el scope del componente esto eso sólo funcionaría si el `svg` estuviera dentro de este componente. Lo que debemos hacer es indicarle que el `svg` está en un scope global, cosa que hacemos con: `button > global(svg)` y así sí funcionará.

```jsx
<button>
	{children}
</button>
<style jsx>
{`
	button{
		(...)
	}
	button > global(svg){
	
	}
`}
</style>
```

## Colores con Opacidad en Hexadecimal
Cuando queremos darle opacidad a un color no necesariamente tenemos que trabajar con `rgba(a,b,c,0.5)` sino que podemos utilizar también hexadecimal, colocando en 8 en vez de 6 números y si son `ff` corresponde a opacidad 1 o sólido y `00` a opacidad 0 o transparente.

En `styles` creamos un archivo`utils.js` y dentro de el una función que se encarga de recibir un color y una opacidad (entre 0 y 1) y convertir esa opacidad a hexa y concatenar ambos valores.

```jsx
export const addOpacityToColor = (color, opacity) => {
  const opacityHex = Math.round(opacity * 255).toString(16)
  return `${color}${opacityHex}`
}
```

Luego para utilizarlo en el Styled JSX:

```jsx
<style jsx>
        {`
          h1 {
            color: ${addOpacityToColor('#FF0000',0.1)};
          }
        `}
</style>
```


## Styled Componentes
Es necesario agregar en `_document.js` lógica para que syled-components incluya en el `<head>` los estilos al trabajar con SSR (similar a lo que hay que hacer para trabajar con Material UI).
