# Redux Toolkit

* Basado en el [video](https://youtu.be/w2rAP7d6ndg) de Fazt Code.



## Getting Started

### Instalación Vite.js

:warning: Si bien en el material de estudio utilizan Create React App, vamos a utilizar Vite.js para hacer el scaffolding del proyecto por simplicidad.

```
npm create vite@latest
```



# Inicio Aplicación

La idea es crear una simple TODO List utilizando Redux Toolkit y 

Como podemos ver en la [documentación](https://redux-toolkit.js.org/introduction/getting-started) Redux Toolkit es el modo aconsejado actualmente de escribir lógica Redux. Fue desarrollado para solucionar algunos aspectos asociados a usar simplemente el core de Redux (configuración complicada, mucho boilerplate, muchos paquetes).

Nos vamos a guiar con el aparado [Quick Start](https://redux-toolkit.js.org/tutorials/quick-start) que nos indica qué debemos instalar y qué archivos debemos crear inicialmente.



## Instalación

```
npm install @reduxjs/toolkit react-redux
```



## Extensión Redux DevTools

Se recomienda instalar en Chrome la extensión [Redux DevTools](https://chrome.google.com/webstore/detail/redux-devtools/lmhkpmbekcpmknklioeibfkpmmfibljd?hl=en) que podremos lanzar con `Control+Shift+E`

## Creación de store

Creamos un archivo llamado `src/app/store.js` donde importamos `configureStore` de :sparkles: **Redux Toolkit** :sparkles: en el cual crearemos un store y lo exportamos.

```jsx
import { configureStore } from '@reduxjs/toolkit'

export const store = configureStore({
  reducer: {}
})
```



## Provide del store a React

Una vez que creamos el store lo disponibilizamos para React mediante el `<Provider>` de :sparkles: **React-Redux** :sparkles: en `src/main.js`

```jsx
import React from 'react'
import ReactDOM from 'react-dom/client'
import App from './App'

import {store} from './app/store'
import { Provider } from 'react-redux'

ReactDOM.createRoot(document.getElementById('root')).render(
  <React.StrictMode>
    <Provider store={store}>
      <App />
      </Provider>
  </React.StrictMode>,
)
```



## Creamos un State Slice

En el patrón redux es a través de los reducers que vamos a alterar el estado y en Redux RTK lo haremos mediante slices.

Como se trata de una aplicación de una lista de tareas creamos el siguiente archivo `src/features/task/tasksSlice.`

En primer lugar importamos `createSlice` de :sparkles: **Redux Toolkit** :sparkles: 

```jsx
import { createSlice } from '@reduxjs/toolkit'

export const tasksSlice = createSlice({
  name: 'tasks',
  initialState: [],
  reducers: {
    
  },
})

export default tasksSlice.reducer
```

Dentro del objeto `reducers` es dónde le diremos qué es lo que queremos hacer `addTask`, `deleteTask`, etc y luego exportaremos estas acciones.

Notar que `createSlice` nos devuelve un objeto y estamos exportando ese objeto y también como default `tasksSlice.reducer`.



## Agregar slice reducers al store

En `store.js` asociamos el slice reducer al store. Al definir un campo dentro del parámetro `reducer` le estamos indicando que debe usar ese slice reducer para todas las actualizaciones de ese estado.

```jsx
import { configureStore } from '@reduxjs/toolkit'
import tasksReducer from '../features/tasks/tasksSlice'

export const store = configureStore({
  reducer: {
    tasks: tasksReducer
  }
})
```



## Lectura del estado en componentes

Para lograr que los componentes de React interactúen con el store utilizamos los hooks de React-Redux. En primer lugar usaremos `useSelector` que nos permite leer datos del store.



En `App.js`  vamos a obtener el estado del store y a mostrarlo en pantalla:

```jsx
import { useSelector } from "react-redux"

function App() {
  const tasks = useSelector(state => state.tasks);
  console.log(tasks)
  return (
    <div>
      <h1>Redux Toolkit</h1>
    </div>
  )
}

export default App
```



## Store con valor inicial

Hasta ahora consideramos que el valor inicial del store sería un array vacío, ahora trabajaremos con un array de objetos.

```jsx
import { createSlice } from '@reduxjs/toolkit'

const initialState = [
  {
    id: 1,
    title: "Task 1", 
    description: "Task 1 description",
    completed: false
  }, 
  {
    id: 2,
    title: "Task 2", 
    description: "Task 2 description",
    completed: false
  }
]

export const tasksSlice = createSlice({
  name: 'task',
  initialState,
  reducers: {
    
  },
})

export default tasksSlice.reducer
```



## Nuevos componetes

Vamos a crear una carpeta `src/components` dentro de la cual tendremos dos componentes uno llamado `TaskList.jsx` y otro `TaskForm.jsx` y los importamos en `App.jsx` de la forma habitual.

## Listado de Tareas

Creamos un componente `src/components/TasksList.jsx` en el que por el momento sólo mostraremos los elementos del valor inicial:

```jsx
import React from 'react'
import {useSelector} from 'react-redux'

function TasksList() {
  const tasks = useSelector(state => state.tasks)

  return (
    <>
      {tasks.map(task => (
        <div key={task.id}>
          <p>{task.title}</p>
          <p>{task.description}</p>
        </div>
      ))}
    </>
  )
}

export default TasksList
```

Con las Redux DevTools podremos ver lo siguiente:

![image-20230123141320093](3 Redux Toolkit.assets/image-20230123141320093.png)



## Creación de Tareas

Creamos el componente `src/components/TaskForm.jsx` que contendrá un formulario simple con un un input de texto y un textarea en los cuales tendremos que ingresar el título y la descripción de la nueva tarea. Al presionar el botón Save por el momento se mostrará la variable de estado en pantalla. 

```jsx
import React, { useState } from 'react'

function TaskForm() {
  const [task, setTask] = useState({
    title: '',
    description:''
  })
  const handleChange = e => {
    setTask({...task, [e.target.name]:e.target.value})
  }

  const handleSubmit = (e) => {
    e.preventDefault();
    console.log(task)
  }

  return (
    <form onSubmit={handleSubmit}>
      <input type='text' name='title' placeholder='title' onChange={handleChange}/>
      <textarea name='description' placeholder='description' onChange={handleChange}/>
      <button>Save</button>
    </form>
  )
}

export default TaskForm

```

En las React DevTools podremos ver el valor que va tomando el estado en cada momento sin la necesidad de loguear en pantalla:

![image-20230123141751552](3 Redux Toolkit.assets/image-20230123141751552.png)



## Modificación Slice

Como en última instancia lo que queremos hacer es agregar este elemento al store, debemos modificar `tasksSlice`