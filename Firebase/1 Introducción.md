# Introducción
Firebase se define como un *backend as a service* y nos permitirá realizar distintas tareas:  Authentication, Firestore Database, Realtime Database, Storage, Hosting, Cloud Functions, etc.
> Repositorio `md-firebase`



# Creación de Proyecto
Lo primero que tenemos que hacer es crear un proyecto, para eso debemos ir a https://firebase.google.com/ y seguir estos pasos:

1. Hacemos click en **Create a project**
2. Le asignamos el nombre deseado, aceptamos los términos y presionamos **Continue**
3. Habilitamos Google Analytics para este proyecto.
4. Donde dice **Analytics location** puse Argentina (dice que debe ser la ubicación de la organización). Luego destilé **use the default settings for sharing Google Analytics data** (y todos los sub-items) y aceptamos los términos.
5. Luego nos dirá que el proyecto está listo y presionamos **Continue**.


# Obtener Objeto de Configuración
En primer lugar vamos a **Project Overview** (en la barra lateral)
Hacemos click en el botón de Web `</>` 
Registramos la aplicación poniéndole un nombre `proyecto-web` e indicamos que no queremos Firebase Hosting en caso de que tengamos en mente deployar en Vercel.
Luego en **Project Overview** hacemos click en el engranaje y luego en **Project settings**
En el apartado **Your apps** veremos una parte que dice **SDK setup and configuration** hacer click en **Config** y copiamos el objeto de configuración `const firebaseConfig = {...}`.

> Como este objeto de configuración `firebaseConfig` lo vamos a enviar con JavaScript este podrá ser visto por cualquier persona, por lo que como seguridad debemos indicar en Firebase desde qué dominios se puede utilizar la aplicación.

## Instalación Firebase
Instalar firebase `npm install firebase`

## Variables de Entorno
Anteriormente obtuvimos un objeto llamado  `firebaseConfig` en lugar de colocar las credenciales del cliente como un string vamos a utilizar variables de entorno, para ello creamos el archivo `.env.local` y dentro ponemos:
```jsx
NEXT_PUBLIC_FIREBASE_API_KEY=
NEXT_PUBLIC_FIREBASE_AUTH_DOMAIN=
NEXT_PUBLIC_FIREBASE_PROJECT_ID=
NEXT_PUBLIC_FIREBASE_STORAGE_BUCKET=
NEXT_PUBLIC_FIREBASE_MESSAGING_SENDER_ID=
NEXT_PUBLIC_FIREBASE_APP_ID=
NEXT_PUBLIC_FIREBASE_MEASUREMENT_ID=
```
Con los secretos que obtuvimos del objeto de configuración.

Luego accederemos a estos valores con `process.env.NEXT_PUBLIC_FIREBASE_...`

> En caso de tener problemas con la creación de archivos (los cuales se solucionan si usamos el string hardcodeado sin variables de entorno) reiniciar al servidor.

## Initialización Firebase
Crear carpeta `firebase` y dentro de ella un archivo `initFirebase.js`

> En el video de **midudev** importaba `import * as firebase from 'firebase'`pero debemos poner `import firebase from 'firebase/app'` y luego importar los módulos que utilizaremos, por ejemplo si queremos tener autenticación debemos poner `import '@firebase/auth';`, si vamos a trabajar con Firestore Database `import '@firebase/firestore';`, etc.

```jsx
import firebase from 'firebase/app';
import '@firebase/auth';
import '@firebase/firestore';

// For Firebase JS SDK v7.20.0 and later, measurementId is optional
const firebaseConfig = {
  apiKey: process.env.NEXT_PUBLIC_FIREBASE_API_KEY,
  authDomain: process.env.NEXT_PUBLIC_FIREBASE_AUTH_DOMAIN,
  projectId: process.env.NEXT_PUBLIC_FIREBASE_PROJECT_ID,
  storageBucket: process.env.NEXT_PUBLIC_FIREBASE_STORAGE_BUCKET,
  messagingSenderId: process.env.NEXT_PUBLIC_FIREBASE_MESSAGING_SENDER_ID,
  appId: process.env.NEXT_PUBLIC_FIREBASE_APP_ID,
  measurementId: process.env.NEXT_PUBLIC_FIREBASE_MEASUREMENT_ID,
};

export const initFirebase = () => {
  if(!firebase.apps.length){
    firebase.initializeApp(firebaseConfig);
    console.log('Firebase was successfully initialized');
  } 
}

```

Luego en `index.js` llamamos a `initFirebase` y veremos en la consola que se inició exitosamente. **Esto es provisorio** ya que luego cuando tengamos Authentication lo haremos en `components/auth/FirebaseAuth.js` porque los usuarios puede que no ingresen directamente a index y en ese caso no tendríamos la inicialización realizada.

```jsx
...
import {initFirebase} from '../firebase/initFirebase'

initFirebase();

export default function Home() {
  return (
    <>
      ...
    </>
  )
}

```