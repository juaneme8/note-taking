# Asignar Estilos
Si tenemos un componente cuyos estilos queremos modificar, la forma habitual de hacerlo sería con una hoja de estilos global `index.css` y luego a la etiqueta deseada le asignamos la clase que hayamos creado por ejemplo: `<div className='side-menu'></div>`

En Material UI existe otra forma de hacerlo y es con la función `makeStyles()` a la cual le pasamos las clases deseadas representadas mediante objetos JavaScript y nos retorna un hook `useStyles` que nos permitirá acceder a estos estilos dentro del componente.

En primer lugar fuera del componente ponemos:
```jsx
import {makeStles} from '@material-ui/core'

useStyles = makeStyles({
	btn:{
		fontSize: 60,
		backgroundColor: 'violet',
	},
});
```

> Notar que como se trata de una función y no de un componente la única forma de importarla es con destructuring: `import {makeStles} from '@material-ui/core'` es decir que no existe tal cosa como `import makeStyles from '@material-ui/core/makeStyles'`

Luego dentro del componente utilizamos la clase:
```jsx
	const classes = useStyles();
	<Button className={classes.btn}> Soy un botón </Button>
```

Si hacemos `console.log(classes)` veremos un objeto `{btn: "makeStyles-btn-1"}` si luego inspeccionamos el botón veremos que le ha asignado una clase con ese nombre.

Si quisiéramos asignarle un estilo para cuando se pase el mouse por encima al botón, podríamos hacerlo de este modo:

```jsx
import {makeStles} from '@material-ui/core'

useStyles = makeStyles({
	btn:{
		fontSize: 60,
		backgroundColor: 'violet',
		"&:hover":{
			backgroundColor: 'blue',
		}
	},
});
```

### con `withStyles`

```jsx
import React from "react";
import { withStyles } from "@material-ui/core";

const style = {
  sideMenu: {
    display: "flex",
    flexDirection: "column",
    position: "absolute",
    left: "0px",
    width: "320px",
    height: "100%",
    backgroundColor: "#253053"
  }
};

const SideMenu2 = (props) => {
  const {classes} = props;
  return <div className={classes.sideMenu}>Hello</div>;
};

export default withStyles(styles)(SideMenu2);

```

Notar lo siguiente:
* Nuevamente llamamos al objeto `style`
* La función del componente es una *arrow function*.
* `withStyles(style)` retorna una funcion a la cual le pasamos como argumento el componente `sideMenu`.
* Accedemos mediante las props. Si hacemos un `console.log(props)` veremos que se trata de un objeto con la propiedad `classes` que a su vez es otro objeto `{sideMenu: "SideMenu2-sideMenu-1"}`

## JSS
Otra opción es usar JSS podemos encontrar la documentación oficial en cssinjs.org. Nos permite utilizar JavaScript para definir estilos. Estos podrán ser dinámicos respecto a parámetros del `theme` y reutilizables en los distintos componentes.
Para utilizarlo será necesario contar con algunos plugins del estilo `jss-plugin-...` que veremos que con la aplicación de **create-react-app** ya están instalados por default.

Creamos la constante `styles` y definimos un objeto con las clases a utilizar siendo estas también objetos con los nombres de cada propiedad de css como **key** (camelCase) y los valores como **value** (strings).
```jsx
const styles={
	sideMenu:{
		display: "flex",
		flexDirection: "column",
		position: "absolute",
		left: "0px",
		width: "320px",
		height: "100%",
		backgroundColor: "#253053"
	}
}
```

## 3. Estilos en Línea
Cuando tenemos pocos estilos que aplicar podemos hacerlo en línea con el atributo `style` y utilizando sintaxis JSS por ejemplo: `<div style={{backgroundColor:'red'}}>`

# Component Customization
Todos los componentes de Material UI se convierten en elementos HTML.
En la documentación https://material-ui.com/customization/components/

En las dev tools podremos ver que estos elementos html tienen clases aplicadas de la forma: `Mui[component name]-[style rule name]-[UUID]`, por ejemplo `MuiAppBar-root` representa los estilos aplicados al elemento root por lo que al verlo sabremos que se trata del elemento externo de ese componente. Decimos esto debido a que algunos componentes pueden tener más de un elemento html.

Si en las dev tools hacemos click en `<style>` veremos que los estilos de las clases que comienzan con Mui son inyectados al comienzo y a lo último se inyectan los custom styles a través de clases como `.makesStyles-root-3`

## 1. con atributo `className`
Utilizando la técnica vista anteriormente:

1. Importamos `makeStyles` que nos permitirá cambiar los estilos default de Material UI
```jsx
import { makeStyles} from '@material-ui/core/styles';
```
2. Creamos las clases deseadas, una forma de hacerlo es:
```jsx
const useStyles = makeStyles({
	root: {
		display: 'flex',
	},
	(...)
});
```
Pero si queremos trabajar con propiedades del `theme` de Material UI como por ejemplo establecer un margen en función del espaciado de material UI, supongamos `theme.spacing(1,2)` para dar un margen de 8px en X y 16 en Y:
```jsx
const useStyles = makeStyles(theme => ({
	root: {
		display: 'flex',
	},
	container:{
		margin: theme.spacing(1,2);
	}
	(...)
}));
```

También podemos utilizar *string templates* y poner por ejemplo
```jsx
padding: `0px ${theme.spacing(1)px}`
```

Notar que en este caso es necesario poner la unidad `px` mientras que cuando es un único valor no hace falta. por ejemplo `marginRight: theme.spacing(1)`

*Cuando hacemos `const useStyles = makeStyles(theme => ({ })` notar que como `makeStyles` **retorna** un objeto lo colocamos entre paréntesis.

3. Creamos la constante `classes = useStyles();` dentro del componente funcional

4. Aplicamos las clases al elemento deseado.
```jsx
return (
	<div className={classes.root}>
	(...)
	</div>
);	
```

Si queremos tener estilos dependientes del valor de las props pasarle `props` a `useStyles()` es decir `const classes = useStyles(props)` y luego en la parte superior:
```js
const useStyles = makeStyles({
	root: {
		color: props => props.color
	}
});
```

Un caso interesante de uso sería si recibimos `props.note` como un objeto que tiene una propiedad categoríay segun ella queremos poner un color. En ese caso suponiendo que hayamos hecho destructuring y tengamos a un objeto `note` como una variable independiente podríamos haber hecho `const classes = useStyles(note)` (aunque tengamos varias props sólo le pasamos aquella que nos interesa para los estilos) y luego:
```js
const useStyles = makeStyles({
	avatar: {
		backgroundColor: (note) => {
			if(note.category == 'work'){
				return yellow[700];
			}
			if(note.category == 'money'){
				return green[500]
			}
			(...)
		}
	}
});
```

### Pseudo Selectores 
A la hora de utilizar pseudo selectores debemos utilizar & para referenciar a la regla padre. 
Por ejemplo si queremos agregar estilos al pasar el mouse por encima:
```jsx
const useStyles = makeStyles({
  container: {
    backgroundColor: "yellow",
    color: "#FFF",
    
    "&:hover": {
      backgroundColor: "blue"
    }
    
  }
});
```

### Selectores Anidados

```jsx
const useStyles = makeStyles({
  container: {
    backgroundColor: "yellow",
    color: "#FFF",
    
    "&.button": {
      backgroundColor: "red"
    }
    
  }
});
```
La última parte será compilada como:
```css
container-0{
	background-color: red;
	color: #FFF;
}
container-0 .button{
	background-color: red;
}
```
Estamos referenciando a una clase **GLOBAL**.

### $ruleName
En ocasiones veremos también `$ruleName` para referenciar a una regla en la misma hoja de estilos. 

Mas información en https://cssinjs.org/jss-plugin-nested/?v=v10.5.0

```jsx
const useStyles = makeStyles({
  container: {
  
    // Reference the local rule "button".
    '& $button': {
      padding: '10px'
    },
 
  },
  button: {
    color: 'grey'
  }
});
```

En cambio en este caso estamos referenciando a una clase **LOCAL** y una vez compilado será: 
```css
.button-1 {
  color: grey;
}
.container-0 .button-1 {
  padding: 10px;
}
```

### Clases Mui
Si con las dev tools vemos que luego de aplicar una clase custom anidados en ese elemento tenemos elementos con clases de `Mui` podemos aplicar lo aprendido recientemnete para modificarlos.

Supongamos que estamos trabajando sobre una clase `searchInput` que debtro tiene un ícono al cual queremos agregarle un margen. Con las dev tools vamos a ver algo así:

```html
<div class="... makeStyles-searchInput-4 ... >
	<svg class="MuiSvgIcon-root" ... >
</div>
```

```jsx
const useStyles = makeStyles({
  searchInput: {
	  (...)


	"& .MuiSvgIcon-root"
		marginLeft: 8px;
    },
});

```

## 2. con atributo `classes`
La diferencia entre usar `className` y `classes` es que `className` aplicará los estilos en el componente, mientras que `classes` lo hará a los elementos de adentro pudiendo ser mucho más específicos.
Es util cuando un componente de Material UI genera más de un elemento html

Por ejemplo si tenemos:

```jsx
 <IconButton>
   <NotificationsNoneIcon />
 </IconButton>
```

```html
<button class="MuiButtonBase-root MuiIconButton-root" tabindex="0" type="button">
	<span class="MuiIconButton-label">
		<svg ...
	</span>
	<span ...
</button>
```

Veremos que tenemos `MuiIconButton-root` y `MuiIconButton-label` si queremos cambiarle el color si hiciéramos:

```jsx
const useStyles = makeStyles({
  btn: {
    backgroundColor: 'red'
  },

});
```

Y luego lo aplicáramos en `<IconButton>`

```jsx
 <IconButton className={classes.btn}>
   <NotificationsNoneIcon />
 </IconButton>
```

Veríamos que el color del ícono no cambió sino que cambió el del contenedor, debemos trabajar sobre la etiqueta que tiene asignada la clase `.MuiIconButton-label`

Una opción sería cambiarlo anidando selectores y haciendo uso del atributo **className**.
```jsx
const useStyles = makeStyles({

  btn: {
    "& .MuiIconButton-label": {
      backgroundColor: "red"
    }
  }
});
```

Sin cambiar nada de 
```jsx
 <IconButton className={classes.btn}>
   <NotificationsNoneIcon />
 </IconButton>
```

Sin embargo en esta sección estamos aprendiendo a cambiar los estilos haciendo uso del atributo `classes`.

Si nos fijamos en la documentación de IconButton API https://material-ui.com/api/icon-button/#iconbutton-api veremos los **nombres de las reglas y la clase css aplicada**.

En particular nos interesa:
>label
>.MuiIconButton-label
>Styles applied to the children container element.

Para poner de manifiesto que se trata de dos elementos distintos les daremos un color distintivo:
```jsx
const useStyles = makeStyles({
  btnRoot: {
    backgroundColor: "blue"
  },
  btnLabel: {
    backgroundColor: "red"
  }
});
```

Luego usando el atributo `classes`
```jsx
 <IconButton classes={{root:classes.btnRoot, label: classes.btnLabel}}>
   <NotificationsNoneIcon/>
 </IconButton>
```