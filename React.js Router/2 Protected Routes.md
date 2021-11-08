# Protected Routes
> Basado en el [https://www.youtube.com/watch?v=Y0-qdp-XBJg&ab_channel=freeCodeCamp.org](Video)

Supongamos una situación donde tenemos una ruta `/dashboard` en la cual se muestra el componente `Dashboard`y queremos que sólo sea visible si el usuario está autenticado.

Inicialmente tenemos en `App.js`

```jsx
import React from "react";
import "./styles.css";
import { Route, Switch } from "react-router-dom";
import { LandingPage } from "./LandingPage";
import { Dashboard } from "./Dashboard";

export default function App() {
  return (
    <div className="App">
      <h1>App Component</h1>

      <Switch>
        <Route path="/" exact component={LandingPage} />
        <Route path="/dashboard" component={Dashboard} />
      </Switch>
    </div>
  );
}

```
Si en este momento ingresamos  a `/dashboard` podremos acceder sin inconvenientes, al componente `Dashboard` mostrado a continuación:

```jsx
import React from 'react';

export const Dashboard = ()=>{
  return (
    <h1>Dashboard (Ruta Protegida)</h1>
  )
}
```

Creamos un archivo `auth.js` (notar que es en minúsculas por tratarse de una instancia de una clase) que nos ayudará con la autenticación del usuario:
```jsx
class Auth{
  constructor(){
    this.authenticated = false;
  }

  login(cb){
    this.authenticated = true;
    cb();
  }

  logout(cb){
    this.authenticated = false;
    cb();
  }

  isAuthenticated(){
    return this.authenticated;
  }
}

export default new Auth();
```

Luego en `LandingPage` colocamos un botón de **Login** que llama a `auth.login(cb)` pasándole como *callback* `props.history.push('/dashboard');` de modo tal que una vez autenticado lo redirija a la ruta protegida `/dashboard`

```jsx
import React from 'react';
import auth from './auth'

export const LandingPage =  (props)=>{
  return (
    <>
      <p>LandingPage Component</p>
      <button onClick={()=> auth.login(()=>{
        props.history.push('/dashboard');
      })}>Login</button>
    </>
  )
}
```

Sin embargo seguimos con el inconveniente de que si el usuario ingresa manualmente la dirección `/dashboard` podrá acceder a los datos sensibles. Para solucionar el primer pasó será crear un componente `ProtectedRoute` cuya tarea será retornar un `Route` con el componente que le hayamos pasado.

```jsx
import React from 'react';
import {Route} from 'react-router-dom'

export const ProtectedRoute = ({component: Component, ...rest})=>{
  return (
    <Route {...rest} render={
      (props)=>{
        return <Component {...props}/>
      }} 
    />
  );
}
```

Usando este nuevo componente en `App` pasamos a tener:
```jsx
import React from "react";
import "./styles.css";
import { Route, Switch } from "react-router-dom";
import { LandingPage } from "./LandingPage";
import { Dashboard } from "./Dashboard";
import { ProtectedRoute } from "./ProtectedRoute";

export default function App() {
  return (
    <div className="App">
      <h1>App Component</h1>

      <Switch>
        <Route path="/" exact component={LandingPage} />
        <ProtectedRoute path="/dashboard" component={Dashboard} />
      </Switch>
    </div>
  );
}

```

Sin embargo, no hemos solucionado el problema del ingreso manual a la ruta `/dashboard` es decir que en `ProtectedRoute` no queremos retornar siempre el `Route` sino solo si el usuario está logueado.

```jsx
import React from "react";
import { Redirect, Route } from "react-router-dom";
import auth from "./auth";

export const ProtectedRoute = ({ component: Component, ...rest }) => {
  return (
    <Route
      {...rest}
      render={(props) => {
        if (auth.isAuthenticated()) {
          return <Component {...props} />;
        } else {
          return <Redirect to="/" />;
        }
      }}
    />
  );
};

```

En el `Dashboard` agregamos un botón de **Logout** que llama a `auth.logout(cb)` pasándole como *callback* `props.history.push('/');` de modo tal que una vez autenticado lo redirija a la ruta protegida `/dashboard`. Notar que contamos `history` en las `props` 

```jsx
import React from "react";
import auth from "./auth";

export const Dashboard = (props) => {
  return (
    <>
      <h1>Dashboard</h1>
      <button
        onClick={() =>
          auth.logout(() => {
            props.history.push("/");
          })
        }
      >
        Logout
      </button>
    </>
  );
};

```

# Página No Encontrada
Implementamos una 404 Page

```jsx
 <Switch>
  <Route path="/" exact component={LandingPage} />
  <ProtectedRoute path="/dashboard" component={Dashboard} />
  <Route path='*' component={()=> '404 ERROR'}/>
</Switch>
```