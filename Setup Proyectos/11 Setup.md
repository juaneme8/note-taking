Frontend - 15/03/2022

:gem: Next.js

:gem: TypeScript

Chakra UI



# Next.js con TypeScript

```
npx create-next-app pm-front --typescript
```



# CleanUp

* Eliminamos `Home.module.css` dentro de `styles`
*  `globals.css` lo ponemos en la raiz
* Eliminamos la carpeta `styles`

* Eliminamos carpeta `api`

* Eliminamos `vercel.svg` en `public`
* Modificamos `index.tsx` de modo que nos quede:

```
import type { NextPage } from 'next'

const Home: NextPage = () => {
  return (
    <main>
      Hola Mundo!
      </main>
  )
}

export default Home
```

* En `_app.tsx` cambiamos la importación de `globals.css` de acuerdo a los cambios:

```
import '../globals.css'
```

* En `_app.tsx` renombrar `MyApp` por `App`.





1. `npx eslint --init`

# ESLint

sugiere downgrade.

You can also run this command directly using 'npm init @eslint/config'.
√ How would you like to use ESLint? · style
√ What type of modules does your project use? · esm
√ Which framework does your project use? · react
√ Does your project use TypeScript? · No / Yes
√ Where does your code run? · browser
√ How would you like to define a style for your project? · guide
√ Which style guide do you want to follow? · standard
√ What format do you want your config file to be in? · JSON
Checking peerDependencies of eslint-config-standard@latest
√ The style guide "standard" requires eslint@^7.12.1. You are currently using eslint@8.11.0.
  Do you want to downgrade? · No / Yes
The config that you've selected requires the following dependencies:

eslint-plugin-react@latest @typescript-eslint/eslint-plugin@latest eslint-config-standard@latest eslint@^7.12.1 eslint-plugin-import@^2.22.1 eslint-plugin-node@^11.1.0 eslint-plugin-promise@^4.2.1 || ^5.0.0 @typescript-eslint/parser@latest
√ Would you like to install them now with npm? · No / Yes
Installing eslint-plugin-react@latest, @typescript-eslint/eslint-plugin@latest, eslint-config-standard@latest, eslint@^7.12.1, eslint-plugin-import@^2.22.1, eslint-plugin-node@^11.1.0, eslint-plugin-promise@^4.2.1 || ^5.0.0, @typescript-eslint/parser@latest

added 45 packages, removed 1 package, changed 9 packages, and audited 268 packages in 4s

71 packages are looking for funding
  run `npm fund` for details

found 0 vulnerabilities
Successfully created .eslintrc.json file in C:\Users\JuaNeMe\Documents\Code\pm-front



# Instalación Chakra-UI

Nos basamos en la [documentación para Next.js](https://chakra-ui.com/guides/getting-started/nextjs-guide) y copiamos el comando para npm.

También según la documentación en [Provider Setup](https://chakra-ui.com/guides/getting-started/nextjs-guide#provider-setup) modificamos el `pages/_app.tsx` wrapeando todo con `ChakraProvider`.