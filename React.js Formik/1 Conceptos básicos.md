# Formik
> Página Oficial: [https://formik.org/](https://formik.org/)
> Basado en la [playlist](https://www.youtube.com/watch?v=a94FOvaBomQ&list=PLC3y8-rFHvwiPmFbtzEWjESkqBVDbdgGu&ab_channel=Codevolution) de Codevolution - [Repositorio](https://github.com/gopinav/React-Formik-Tutorials)
> El código estará en el repositorio `md-formik`

Los formularios son vitales para las aplicaciones de negocios, es por eso que debemos crear una experiencia que guíe al usuario durante el proceso. 

Formik es una biblioteca que nos permite lidiar con formularios en React y React Native. Esto incluye manejar los datos del formulario, validarlos, entregar *feedback* visual con mensajes de error y encargarnos del *submit* del formulario. Si bien esto podríamos hacerlo con *plain React*, formik nos permite hacerlo de manera simple, escalable y performante.

En primer lugar `npx create-react-app demo` luego hacemos la limpieza en `/src/App.js` de modo que quede  vacío `<div className="App"></div>` y creamos la carpeta `/src/components`
Creamos el componente `YoutubeForm` que cuenta con tres `label` + `input` (name, email, channel) y un Submit `button`.

Recordar en la parte inferior elegir JavaScript React para poder utilizar emmet.

Una vez en el archivo creamos un *functional component* con `rfce` recordar tener instalada la extensión **ES7 React/Redux/GraphQL/React-Native snippets**

```jsx
<form>
	<label htmlFor="name">Name</label>
	<input type="text" id="name" name="name" />

	<label htmlFor="email">Email</label>
	<input type="email" id="email" name="email" />

	<label htmlFor="channel">Channel</label>
	<input type="text" id="channel" name="channel" />

	<button>Submit</button>
</form>
```
En `App.css` limpiar todo el contenido y aplicar los estilos deseados.

Luego en `App.js` importar el componente y utilizarlo:
```jsx
<div className="App">
	<YoutubeForm />
</div>
```

# Instalación:
`npm i formik`

En un comienzo utilizaremos el hook `useFormik`:
`const formik = useFormik({})`

Le pasamos un objeto y nos retorna un objeto llamado `formik` con el cual podremos realizar: 
1. Manejo del estado del formulario
2. Manejo de submit
3. Validación y mensajes de error
Basado en la [playlist](https://www.youtube.com/watch?v=a94FOvaBomQ&list=PLC3y8-rFHvwiPmFbtzEWjESkqBVDbdgGu&ab_channel=Codevolution) de Codevolution 
> El código estará en la carpeta `md-formik`

Los formularios son vitales para las aplicaciones de negocios, es por eso que debemos crear una experiencia que guíe al usuario de manera eficiente durante el proceso. 

Formik es una biblioteca que nos permite lidiar con formularios en React y React Native. Esto incluye manejar los datos del formulario, validarlos, entregar *feedback* visual con mensajes de error y encargarnos del *submit* del formulario. Si bien esto podríamos hacerlo con *plain* React, formik nos permite hacerlo de manera simple, escalable y performante.

En primer lugar `npx create-react-app demo` luego hacemos la limpieza en `/src/App.js` y creamos la carpeta `/src/components`
Creamos el componente `YoutubeForm` que cuenta con tres `input` (name, email, channel) y un Submit `button`.

## Instalación formik:
`yarn add formik`

Utilizamos el hook `useFormik`: `const formik = useFormik({})`
Le pasamos un objeto y nos retorna un objeto llamado `formik` con el cual podremos realizar: 
1. Manejo del estado del formulario
2. Manejo de submit
3. Validación y mensajes de error

# Manejo del estado del form
Por estado entendemos a los valores de cada uno de los inputs.

Las propiedades de `initialValues` coinciden con el atributo `name` de cada uno de los `input`

En la medida que escribamos en cada uno de estos campos el valor de la propiedad se actualiza.