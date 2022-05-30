# Masonry CSS
En ocasiones cuando trabajemos con Cards puede que queramos implementar un layout de Masonry CSS, si bien Material UI no tiene esta característica podemos recurrir a un paquete llamado [react-masonry-css](https://www.npmjs.com/package/react-masonry-css), para instalarlo: `npm i react-masonry-css`.

Luego para usarlo de acuerdo a lo indicado en la documentación vemos que el funcionamiento se basa en un componente llamado `Masonry`:
```jsx
import Masonry from  'react-masonry-css'

<Masonry
	 breakpointCols={3}
	 className="my-masonry-grid"
	 columnClassName="my-masonry-grid_column">
	 {/* array of JSX items */}
</Masonry>
```
Como vemos el componente utiliza una serie de clases y nos proporciona del CSS. La forma más rápida es pegarlo dentro de los estilos globales de `index.css`, pero otra opción sería adaptar los nombre de las propiedades y sus valores para trabajar con `makeStyles` como hasta ahora.

Si bien en el ejemplo anterior con `breakpointCols={3}` establecimos una cantidad fija de columnas, también es posible establecerla de manera responsiva de acuerdo al tamaño de la pantalla:
```jsx
const  breakpointColumnsObj  =  {
	default:  4,
	1100:  3,
	700:  2,
	500:  1
};
```