> Basado en el [video](https://youtu.be/taPz40VmyzQ) de Sonny Sangha

## Tecnologías

:large_blue_diamond: **React Native**

La idea es a partir de una única codebase contar con una aplicación web, Android y iOS

:large_blue_diamond: **Expo**

https://docs.expo.dev/

Cuando desarrollamos una app para Android o iOS podemos utilizar RN de manera pura o usando una herramienta como Expo que actúa como wrapper alrededor de la app de RN.

:large_blue_diamond:**Redux**

:large_blue_diamond:**TailwindCSS**

Utilizaremos el paquete [tailwindcss-react-native](https://www.npmjs.com/package/tailwindcss-react-native)

:large_blue_diamond:**React Navigation**

https://reactnavigation.org/

:large_blue_diamond:**Sanity**

https://www.sanity.io/

CMS (content management system) que aporta todos los datos de la aplicación.



## Primeros pasos

#### Instalación Expo

Lo primero que haremos es instalar Expo yluego crear un proyecto para iniciar, como vemos en el Getting Started debemos ejecutar:

```
npm install --global expo-cli 
npx create-expo-app md-deliveroo-clone
cd md-deliveroo-clone
npm start
```

> Nos aparecerá un QR en pantalla, si descargamos la app de Expo y lo scaneamos, estando conectados a la misma red podremos ver los cambios en la medida que desarrollemos.



Usaremos `View` de manera similar a un div.



#### Instalación TailwindCSS

Luego seguiremos el resto de los pasos indicados en https://tailwindcss-react-native.vercel.app/installation