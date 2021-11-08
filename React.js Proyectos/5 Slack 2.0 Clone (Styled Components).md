# Slack 2.0 Clone
> Basado en el video de Sonny Sangha en [youtube](https://www.youtube.com/watch?v=QiTq5WrWoJw&ab_channel=SonnySangha)
> Los archivos de este proyecto están en `md-slack-clone-yt`

# Firebase Config
Ver LinkedIn Clone

# App Config
Ver LinkedIn Clone
`npx create-react-app slack-clone-yt --template redux`

## Cleanup de la App
Eliminamos `App.test.js`, `logo.svg` y `setupTests.js`. 

Luego en `App.js` eliminamos el `header` de modo de dejar sólo el `div` y como es habitual cambiamos `className="App"` a "app" aunque no vamos a usar BEM sino styled components.

En `App.css` borrar todo el contenido.

En `index.css`  

```css
* {
	margin: 0;
}
body {
	--slack-color: #3f0f40;
	/* overflow: hidden; */
	...
}
```
También eliminamos los imports de `import logo from './logo.svg';`
y `import { Counter } from './features/counter/Counter';`

 En `/features/counter` tenemos el archivo `counterSlice.js`, lo colocamos en `/features` así lo aprovechamos y borramos la carpeta `/counter`. Luego en `store.js` corregimos la ruta del import de `counterSlice`

Luego presionamos `control+J` para abrir una nueva terminal y ejecutamos `yarn start`.  Si no lo tenemos instalado `npm install -g npm`

# React Router
Vamos a utilizar React Router y la forma más fácil de hacerlo es ir a la [documentación oficial](https://reactrouter.com/web/guides/quick-start) y ver los ejemplos:
`yarn add react-router-dom`

# Material UI
`yarn add @material-ui/core`
`yarn add @material-ui/icons`

# Styled Components
Los styled components se caracterizan porque **la definición de los estilos se lleva a cabo en componentes en lugar de trabajar sobre el CSS**.
Una de las ventajas de usar styled components es que no sucederá como con css donde fácilmente puede haber  superposición de estilos.

## Instalación
Primero debemos instalarlo `yarn add styled-components`

> Por estar utilizando yarn debemos agregar en `package.json` la siguiente entrada:
```json
{
	{...},
	"resolutions": {
	    "styled-components": "^5"
	}
}
```

### Extensión `vscode-styled-components`
Podemos instalar la extensión `vscode-styled-components` para obtener syntax highlighting e intelliSense para styled-components

Creamos la carpeta `components` y en ella un archivo llamado `Header` que importamos en `app.js`. 

En primer lugar usamos el snippet `rfce` para convertirlo en un componente funcional y además queremos tener un contenedor llamado `HeaderContainer` con un cierto estilo que se lo daremos usando styled components. Este elemento será un `div` por lo que ponemos `const HeaderContainer=styled.div``;`
```jsx
import React from 'react';
import styled from 'styled-components';
function Header() {
	return <HeaderContainer>Header</HeaderContainer>;
}

export default Header;

const HeaderContainer = styled.div`
	background-color: green;
`;
```

Volviendo a la maqueta el header consta de tres componentes `HeaderLeft`, `HeaderSearch` y `HeaderRight`.

Como en `HeaderLeft` queremos mostrar el componente de Material UI `<Avatar />` lo que hacemos es crear un componente `HeaderAvatar` que anidaremos dentro de `HeaderLeft` `const HeaderAvatar = styled(Avatar)``;`


```jsx
function Header() {
	return (
		<HeaderContainer>
			<HeaderLeft>
				<HeaderAvatar />
				<AccessTime />
			</HeaderLeft>
		</HeaderContainer>
	);
}

export default Header;
```

Queremos que `HeaderLeft` ocupe un 30% de la pantalla, por lo que hacemos de `HeaderContainer` una caja flexible y de `HeaderLeft` le damos `flex:0.3`. Luego para acceder al ícono de Material UI trabajamos con `> .MuiSvgIcon-root` y como queremos que se ubique sobre la derecha ponemos `margin-left: auto;` y después lo despegamos del borde con `margin-right: 30px;`.
```jsx
const HeaderContainer = styled.div`
	display: flex;
`;
const HeaderLeft = styled.div`
	flex: 0.3;
	display: flex;
	align-items: center;
	margin-left: 20px;

	> .MuiSvgIcon-root {
		margin-left: auto;
		margin-right: 30px;
	}
`;
```
## Estilos en un archivo aparte
Colocamos todos los estilos en un archivo separado llamado `Header.styles.js` :
```jsx
import styled from 'styled-components';
import { Avatar } from '@material-ui/core';

export const HeaderContainer = styled.div`
	display: flex;
	/* position: fixed; */
	/* width: 100%; */
	/* align-items: center; */
	/* justify-content: space-space-between; */
	padding: 10px 0;
	background-color: var(--slack-color);
	color: white;
`;
	/*...*/
	/*...*/
	/*...*/
```

Luego lo importamos en `Header`
```jsx
import { HeaderContainer, HeaderLeft, HeaderAvatar, HeaderSearch, HeaderRight } from './Header.styles';

```

