# -----(NO FUNCIONÓ) 
# Pre-commit hook

Si queremos asegurarnos de que el código que se sube al repositorio cumple con las reglas establecidas, podemos configurar un [hook de git](https://git-scm.com/docs/githooks) para automatizar el proceso.

Es conveniente realizar el linteo del código antes de realizar commits. De esta manera nos aseguramos que no haya errores en el repositorio y reforzamos el _code style_.

> Si bien existen muchas estrategias una de ellas es instalar `npx mrm lint-staged` que está basado en Husky (es una herramienta que facilita la configuración de hooks de git) y gracias a el antes hacer un commit se ejecutará Prettier y ESLint y si falla no se commiteará el código.

> Como alternativa este [POST](https://medium.com/capua-dev/integrando-prettier-con-eslint-961d1d8b716c) explica una usando npm

https://medium.com/dubizzletechblog/setting-up-prettier-and-eslint-for-js-and-react-apps-bbc779d29062

# -----