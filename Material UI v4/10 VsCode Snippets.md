# VScode Snippets
> Basado en https://www.youtube.com/watch?v=zH84acobsQ4&ab_channel=AnthonySistilli

Cuando utilizamos Material UI existen porciones de código que tenemos que ingresar en repetidas ocasiones, es por eso que sería interesante contar con snippets de código que simplifiquen esa tarea tediosa. Una opción es recurrir a una [extensión](https://marketplace.visualstudio.com/items?itemName=vscodeshift.material-ui-snippets) que incluya todos los componentes y la otra es crear nuestros propios snippets. 

Supongamos que queremos crear un snippet para cuando utilizamos `makeStyles` en este código:
```jsx
import { makeStyles } from '@material-ui/styles';
const useStyles = makeStyles({});

const classes = useStyles(props);
```
> Tener en cuenta que la última línea debemos cortarla y pegarla dentro de la función.

Para ello con `ctrl+shift+p` abrimos **VScode Palette** y luego ingresamos **Preferences: Configure user snippets** luego podremos elegir entre local snippets (si sólo los vamos a utilizar en el proyecto actual) o global snippets (si queremos que sirvan para todos los proyectos), luego le damos un nombre por ejemplo `makeStylesSnippet` y colocamos el siguiente código:
```jsx
{
  "makeStyles": {
    "prefix": "makeStyles",
    "body": [
      "import { makeStyles } from '@material-ui/styles';",
      "",
      "const useStyles = makeStyles({});",
      "",
      "  const classes = useStyles(props);"
    ],
    "description": ""
  }
}
```

`prefix` será la palabra que deberá ingresar el usuario para acceder al snippet y `body` es el snippet en sí.

Si queremos un snippet un poco más complejo que trabaje con el nombre del archivo podemos crear otro snippet que llamamos `BasicMUIComponent` que nos permitirá crear un componente funcional que use Material UI de manera rápida
```jsx
{
  "MUI": {
    "prefix": "mui",
    "body": [
      "import React from 'react';",
      "import { Typography, makeStyles } from '@material-ui/core';",
      "",
      "const useStyles = makeStyles({});",
      "",
      "const ${TM_FILENAME_BASE} = () => {",
      "  return (",
      "    <>",
      "      <Typography> ${TM_FILENAME_BASE} </Typography>",
      "    </>",
      "  );",
      "};",
      "",
      "export default ${TM_FILENAME_BASE};",
      ""
    ],
    "description": "Material UI Component"
  }
}
```
`${TM_FILENAME_BASE}` es el nombre del archivo en el que utilicemos el snippet.