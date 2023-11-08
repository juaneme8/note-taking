# Next.js con Material UI
Para trabajar con Next.js y Material UI como indica la [documentación oficial](https://material-ui.com/guides/server-rendering/) debemos basarnos en el siguiente [repositorio](https://github.com/mui-org/material-ui/tree/master/examples/nextjs).

El primer paso es agregar en el directorio `pages` los archivos `_app.js` y `_document.js`. En estos archivos tenemos una línea `import theme from '../src/theme';` es por eso que debemos crear en la raíz el directorio `src` y dentro de el un archivo `theme.js`.

```js
import { createMuiTheme } from '@material-ui/core/styles';

const theme = createMuiTheme({});

export default theme;
```