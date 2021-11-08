# Authentication
> Basado en [video midudev](https://youtu.be/UlYGGCNFcWo?list=PLV8x_i1fqBw1VR86y4C72xMGJ8ifjBwJ6) con algunos cambios en el modo en que se importa `firebase`.
> Basado en el [video](https://youtu.be/SYnu6CLKD70)

Para agregar autenticación a nuestros proyectos debemos ir a [firebase.google.com](firebase.google.com), luego **Go to console** y por último hacer click en el proyecto al que queramos agregarle esa característica.

En la barra de la izquierda hacemos click en **Build** y luego en **Authentication**.

Una vez en esa sección hacemos click en **Get started** y en la pestaña **Sign-in method** nos dará a elegir las distintas opciones de autenticación.

> Por más que hayamos definido varios proveedores por default sólo será posible tener una cuenta para cada correo electrónico.

## Habilitar Autenticación con GitHub
Activamos la llave **Enable** y nos pedirá el Client ID y Client secret.

Para obtener estos datos vamos en GitHub a **Settings**, luego **Developer Settings** y por último **OAuth Apps**. 

A continuación hacemos click en **Register a new application**.

Nos preguntará el nombre de la aplicación, la url (que probablemente no tengamos todavía, ponemos cualquiera) y una descripción y por último el **Authorization callback URL** donde pegamos la información que nos entrega Firebase. En ese momento nos entregará el Client ID y Client secret que ingresamos en Firebase logrando así habilitar la autenticación mediante GitHub.

## Habilitar Autenticación con Email/Password
Activamos la llave **Enable** (y nos aseguramos que la de abajo esté deshabilitada, que sería permitir un ingreso sin contraseña) y hacemos click en **Save**.

## Agregar Authentication al Proyecto
En `components` creamos la carpeta `auth` y dentro un archivo `FirebaseAuth.js`
En ese archivo vamos a llamar a `initFirebase()` cosa que hacíamos provisoriamente en `index.js` pero puede que el usuario no ingrese a través de esa ruta y nos quedaríamos sin inicializarlo.












```jsx
export const loginWithGitHub = () => {
  const githubProvider = new firebase.auth.GithubAuthProvider();

  return firebase
    .auth()
    .signInWithPopup(githubProvider)
    .then((user) => {
       const { displayName,email, photoURL } = user;

      return ({
	      avatar: photoURL,
	      username: displayName,
	      email
      })
    });
};

```

En este `client.js` exportamos una función para hacer login con GitHub esta luego la pasaremos en el `onClick` del botón que usemos para loguearnos.

> Frecuentemente nos toparemos con un mensaje de error ** FirebaseError: Firebase: Firebase App named '[DEFAULT]' already exists** para evitarlo debemos agregar `!firebase.apps.length && firebase.initializeApp(firebaseConfig);` con lo que sólo inicializará la aplicación si no hay ninguna otra. 

## Cambio de Estado
Independientemente de la variable de estado `user` queremos tener una forma de saber si el usuario está autenticado o no, sino bastará con refrescar para que user sea `null` y nos considere no autenticados.

Es posible saber en qué momento el usuario cambia de estado entre autenticado y no autenticado con un método llamado `onAuthStateChanged`. Hasta ahora en `loginWithGithub` usamos `firebase.auth().signInWithPopup(githubProvider)` y obteníamos el usuario. Ahora gracias a este método esto no será necesario, ya que accederemos a `onAuthStateChanged` y ahí mapearemos los datos obtenidos para lograr sólo un objeto con `avatar`, `url` y `username`.

Luego `onAuthStateChanged` nos queda de esta forma:

```jsx
export const onAuthStateChanged = (onChange) => {
  return firebase
    .auth()
    .onAuthStateChanged(user => {
      const { displayName,email, photoURL } = user;

      const normalizedUser = {
      avatar: photoURL,
      username: displayName,
      email
  }
      onChange(normalizedUser);
    });
}
```

Para utilizar `onAuthStateChanged` en `index.js` creamos un nuevo efecto con `useEffect`:

```jsx
useEffect(()=> {
	onAuthStateChanged(user => setUser(user)
},[]);
```

O lo que es lo mismo cambiando `	onAuthStateChanged(user => setUser(user)` por `	onAuthStateChanged(setUser)`  dado que las funciones son de primera clase y por lo tanto se pueden utilizar como parámetros. Por lo tanto nos queda:

```jsx
useEffect(()=> {
	onAuthStateChanged(setUser)
},[]);
```
Con esto cuando montemos el componente nos fijaremos si tiene sesión autentificada y en ese caso la variable de estado dejará de ser null y pasará a ser un objeto con `username`, `avatar` y `email`. 

En `onAuthStateChanged` recibo `null` si el usuario no está logueado mientras que si está logueado recibo un objeto `user`. 

La variable de estado `user` tiene tres estados `undefined` (el valor inicial para cuando refrescamos y no sabemos si está logueado o no, por lo que queremos mostrar un placeholder), `null` (para cuando no está autenticado) y un valor distinto con los datos del usuario para cuando está autenticado.

> Una vez que iniciemos sesión quedarán datos guadados en IndexedDB