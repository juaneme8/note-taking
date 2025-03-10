> Basado en el video de [Free Code Camp](https://youtu.be/2esQdKzRUCw)



## Inicialización Expo

Lo primero que hacemos es iniciar un proyecto de Expo en blanco con TypeScript de acuerdo a la [documentación](https://docs.expo.dev/router/introduction/) 

```
npx create-expo-app@latest -t
```

> Con `-t` indicamos que queremos usar un template y elegimos Blank (TypeScript) y luego le asignamos el nombre `md-rn`.

Luego podremos empezar la aplicación por ejemplo con `npm start --ios` para comenzar con el simulador de iOS.



## Instalación dependencias:

```
npx expo install expo-router react-native-safe-area-context react-native-screens expo-linking expo-constants expo-status-bar
```



### Setup entry point

```
{
  "main": "expo-router/entry"
}
```



###  Modify project configuration

Add a deep linking `scheme` in your app config in `App.json`

```
{
  "scheme": "your-app-scheme"
}
```