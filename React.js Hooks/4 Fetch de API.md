# Fetch Data de API endpoint
Si bien utilizaremos `useState` y `useEffect`, en el futuro esto podrá ser llevado a cabo por `Suspense` que se trata de una nueva característica de React. 

# Utilizando Fetch
En `useEffect()` invocamos una función `fetchTours` que se encuentra fuera.

```jsx
const [loading, setLoading] = useState(true);
const [tours, setTours] = useState([]);

const fetchTours = async () => {
	try{
		const res = await fetch(url);
		const tours = await res.json();
		setLoading(false);
		setTours(tours);
	}
	catch(error){
		setLoading(false);
		console.error(error)
	}
}

useEffect(()=>{
	fetchTours();
},[]);
```

Tanto en caso de éxito como ante un error colocamos `setLoading(false)` de modo que no quede colgado.

# Utilizando Axios
`npm install axios`

Utilizaremos como fake REST API a https://jsonplaceholder.typicode.com/

## `GET` de todos los posts
Trabajaremos con el endpoint `https://jsonplaceholder.typicode.com/posts`
Notar que el segundo parámetro de `useEffect` es un array vacío `[]`

```jsx
import React, { useState, useEffect } from 'react';
import axios from 'axios';

function DataFetching() {
	const [posts, setPosts] = useState([]);
	useEffect(() => {
		axios
			.get('https://jsonplaceholder.typicode.com/posts')
			.then((response) => {
				console.log(response);
				setPosts(response.data);
			})

			.catch((error) => console.log(error));
	}, []);

	const postList = posts.map((post) => <li key={post.id}>{post.title}</li>);

	return <ul>{postList}</ul>;
}

export default DataFetching;

```

## `GET` de post por id
Para obtener un post de acuerdo al id utilizaremos el endpoint por ejemplo para el `id=1`
`https://jsonplaceholder.typicode.com/posts/1` 
Agregamos un `input` de texto (será un componente controlado) de manera tal que podamos ingresar ahí el valor del `id`

Notar que en el estado tenemos como valor inicial un objeto vacío en lugar de un array vacío:
`const [post, setPost] = useState({});`

En `useEffect` como lista de dependencias debemos colocar `[id]` ya que queremos que el efecto vuelva a ejecutarse cuando cambie `id`

```jsx
import React, { useState, useEffect } from 'react';
import axios from 'axios';

function DataFetching() {
	const [post, setPost] = useState({});
	const [id, setid] = useState(1);
	useEffect(() => {
		axios
			.get(`https://jsonplaceholder.typicode.com/posts/${id}`)
			.then((response) => {
				console.log(response);
				setPost(response.data);
			})

			.catch((error) => console.log(error));
	}, [id]);

	return (
		<div>
			<input type="text" value={id} onChange={(e) => setid(e.target.value)} />
			<div>{post.title}</div>
		</div>
	);
}

export default DataFetching;

```
Hasta el momento cada vez que escribimos un caracter se ejecuta el efecto lo que produce un nueva solicitud de datos. 
**NOTA:** Por el momento le restamos importancia al hecho de que cuando luego de ingresar un valor en el campo de texto borramos todo, obtenemos los 100 posts ya que el endpoint deja de ser el de obtener un post específico y se convierte en el de obtener 100. 

A continuación implementaremos lo mismo pero con un botón que recién al presionarlo efectuará la solicitud. 
**NOTA:** Típicamente esto lo resolveríamos con un botón y un `clickHandler` donde haríamos el get request, pero en este caso queremos ver cómo hacerlo con el hook de efecto.


Creamos el estado `const [idFromButtonClick, setidFromButtonClick] = useState(1);` y al presionar el botón lo seteamos con el valor del campo de texto. Además la lista de dependencias del efecto ahora es `[idFromButtonClick]`.

```jsx
import React, { useState, useEffect } from 'react';
import axios from 'axios';

function DataFetching() {
	const [post, setPost] = useState({});
	const [id, setid] = useState(1);
	const [idFromButtonClick, setidFromButtonClick] = useState(1);

	const handleClick = () => {
		setidFromButtonClick(id);
	};

	useEffect(() => {
		axios
			.get(`https://jsonplaceholder.typicode.com/posts/${idFromButtonClick}`)
			.then((response) => {
				console.log(response);
				setPost(response.data);
			})

			.catch((error) => console.log(error));
	}, [idFromButtonClick]);

	return (
		<div>
			<input type="text" value={id} onChange={(e) => setid(e.target.value)} />
			<button type="button" onClick={handleClick}>
				Obtener Post
			</button>
			<div>{post.title}</div>
		</div>
	);
}

export default DataFetching;

```