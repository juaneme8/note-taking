# Material UI v5

> Basado en la [playlist](https://www.youtube.com/playlist?list=PLC3y8-rFHvwh-K9mDlrrcDywl7CeVL2rO) de Codevolution

## Introducción

Material UI se trata de una librería de componentes de UI que nos permite lograr interfaces de usuario de manera rápida.

Estas notas son basadas en la versión 5 de Material UI, para la versión 4 ver las notas anteriores.

Se trata de una implementación de la especificación de Material Design de Google. 

| Librería | Implementación   |
| -------- | ---------------- |
| React    | Material UI      |
| Angular  | Angular Material |
| Vue      | Vuetify          |



## Estructura Base

Utilizaremos Create React App con una plantilla de TypeScript.

```
npx create-react-app react-mui-demo --template typescript
```

```
cd react-mui-demo
npm start
```

Luego realizar la limpieza inicial en `src` y crear dentro la carpeta `components`.



## Instalación MUI

En la documentación nos encontramos con [getting starter](https://mui.com/material-ui/getting-started/installation/)

```
npm install @mui/material @emotion/react @emotion/styled
```



## Instalación Iconos

```tsx
npm i @mui/icons-material
```



## Typography

```tsx
<Typography variant="h1">
    ...
</Typography>
```



* Las variantes de tipografía de h1 a h6 mantienen su elemento semántico, es decir que si inspeccionamos el HTML veremos que efectivamente son h1, h2, etc.

* Las variantes `subtitle1` y `subtitle2` corresponden a un elemento h6.

* Las variantes `body1` (valor por defecto de `variant`) y `body2` corresponden a un elemento `p`



Como vimos cada variante hace referencia a un elemento semántico en particular, sin embargo esto podrá ser cambiado utilizando la prop `component` de modo tal que si tenemos `<Typography variant="h1" component="div">` corresponderá a un div y no a un h1.



`gutterBottom` es una prop que aportará un margen inferior (por defecto es `false`). Será interesante usarlo a la hora de colocar títulos o secciones.



Los valores por defecto de `fontSize` y `fontWeigth` pueden ser consultados en la documentación en el apartado [default theme](https://mui.com/material-ui/customization/default-theme/#main-content) que podrá ser personalizado.



## Components

### `Button`

La prop `variant` acepta los valores `text` (default), `contained` o `outlined`.

Serán representados como elementos HTML de tipo button y si usamos `href` se convertirán en links y el elemento por debajo será un anchor.

Tener presente que `display` por defecto es `flex` por lo que si queremos personalizar los tamaños tendremos que poner `display="block"`

### `Stack`

Nos permite el espaciado de elementos. 

```tsx
<Stack direction="row" spacing={2}>
  <Item>Item 1</Item>
  <Item>Item 2</Item>
  <Item>Item 3</Item>
</Stack>
```

Por defecto la `direction` es `column`.

### Input / Form controls

### Layout

### Navigation

### Data display and feedback

### MUI Lab

## Customization

:rotating_light:  HASTA VIDEO 4 - MIN 9