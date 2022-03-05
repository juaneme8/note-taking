# Debugging
Normalmente cuando tenemos algún problema en el backend comenzamos a hacer `console.log()` para saber qué es lo que está pasando. Sin embargo, como Next.js es una aplicación de Node si le pasamos el flag `--inspect` a ese proceso de Node.js podremos empezar en debug mode.

Para ello modificando`package.json` donde tenemos el script de `dev` en vez de `"dev": "next,"` poniendo `"dev": "NODE_OPTIONS='--inspect' next,"` luego ejecutamos `yarn dev`.

Si en chrome escribimos `about: inspect` y luego hacemos click en Open dedicated DevTools for Node podremos accede a la consola para la aplicación Node.