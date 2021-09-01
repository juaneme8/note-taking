# JSON Server
Una tarea habitual de los desarrolladores frontend consiste en simular un backend con una API REST que nos devuelva datos en formato JSON a nuestra aplicación.
Si bien podríamos crear el server backend usando Node, Express y MongoDB esto nos insumirá bastante tiempo y con JSON Server podemos crear un *local server* con una *dummy REST API* de manera mucho más rápida. Básicamente se trata de un archivo `.json` al cual accedemos mediante endpoints para obtener (GET) y agregar (POST) información.

## Instalación

```bash
npm i -g json-server 
```

En el directorio raíz (al mismo nivel de `package.json`,`node_modules`, `public`, `src`, etc) creamos una carpeta `data` y en ella un archivo `db.json`

Luego en el archivo creamos un objeto en formato JSON con aquellos datos que queremos obtener mediante endpoints.
 Notar el uso de comillas dobles tanto para las propiedades como para los valores que son strings y que el último elemento del array no debe tener una `,` después de el.

 Las *top level properties* como en este caso `notes` son considerados *resources* y tendrán endpoints asociados para obtener y agregar datos.
```json
{
	"notes": [
		{
		"title":
		"details":
		"category":
		"id":1
		},
		
	]
}
```

Si quisiéramos tener otro recurso `polls` podríamos agregar otra propiedad de nivel superior:
```json
{
	"notes": [
		{
		"title":
		"details":
		"category":
		"id":1
		},
		{...},
		{...}
		
	],
	"polls":[
		{...},
		{...},
		{...}
	]
}
```

En un ejemplo completo:
```json
{
  "notes": [
    {
      "title": "Yoshi's birthday bash",
      "details": "Lorem Ipsum is simply dummy text of the printing and typesetting industry. Lorem Ipsum has been the industry's standard dummy text ever since the 1500s, when an unknown printer took a galley of type and scrambled it to make a type specimen book.",
      "category": "reminders",
      "id": 1
    },
    {
      "title": "Complete my ninja training",
      "details": "Lorem Ipsum is simply dummy text of the printing and typesetting industry. Lorem Ipsum has been the industry's standard dummy text ever since the 1500s, when an unknown printer took.",
      "category": "work",
      "id": 2
    },
    {
      "title": "Order a pizza!",
      "details": "Lorem Ipsum is simply dummy text of the printing and typesetting industry. Lorem Ipsum has been the industry's standard dummy text ever since the 1500s, when an unknown printer took a galley of type and scrambled it to make a type specimen book.\nLorem Ipsum is simply dummy text of the printing and typesetting industry. Lorem Ipsum has been the industry's standard dummy text ever since the 1500s, when an unknown printer took a galley of type and scrambled it to make a type specimen book.",
      "category": "todos",
      "id": 3
    }
  ]
}
```

JSON Server verá la propiedad de nivel superior `notes` como un recurso y nos proporciona de endpoints para interactuar con ella.

Para ello ejecutamos `json-server --watch data/db.json`. Si ya tenemos ocupado el puerto 3000 como suele suceder con las aplicaciones de react debemos especificarle un nuevo puerto y lo hacemos del siguiente modo: `json-server --watch data/db.json --port 8000` y luego podemos acceder a los datos haciendo un GET a `http://localhost:8000/notes`.

Para hacerlo programáticamente usando `useEffect` y `fetch()`

```jsx
import React, { useEffect, useState } from 'react'

export default function Notes() {
  const [notes, setNotes] = useState([]);

  useEffect(() => {
    fetch('http://localhost:8000/notes')
      .then(res => res.json())
      .then(data => setNotes(data))
  }, [])

  return (
    <div>
      {notes.map(note => (
        <p key={note.id}>{ note.title }</p>
      ))}
    </div>
  )
}
```

Si queremos implementar un POST por ejemplo cuando el usuario ingresa datos en un formulario y presiona el *submit button*. Notar el uso de `JSON.stringify({})` para convertir los objetos JavaScript a strings JSON.
```jsx
const history = useHistory();
const handleSubmit = (e) => {
	e.preventDefault()
	fetch('http://localhost:8000/notes', {
		method: 'POST',
		headers: {"Content-type": "application/json"},
		body: JSON.stringify({ title, details, category })
	})
	.then(() => history.push('/'));
}
```

Vemos que el *unique id* será asignado de manera automática y que para redireccionar una vez enviado el formulario hacemos uso del hook `useHistory()` que importamos de la siguiente forma `import  {  useHistory  }  from  'react-router-dom'`