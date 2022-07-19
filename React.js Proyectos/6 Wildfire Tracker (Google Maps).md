# Wildfire Tracker
Traversy Media - Wilfire Tracker [LINK](https://www.youtube.com/watch?v=ontX4zfVqK8&list=WL&index=3&t=824s&ab_channel=TraversyMedia)
> Los archivos están en la carpeta `location-tracker`
>
## API
https://api.nasa.gov/
EONET - https://eonet.sci.gsfc.nasa.gov/docs/v2.1
https://eonet.sci.gsfc.nasa.gov/api/v2.1/events

## Google Maps
Utilizaremos el paquete Google-map-react
https://www.npmjs.com/package/google-map-react

https://console.cloud.google.com/
1.API y servicios
Habilitar API y Servicios: Maps JavaScript API
 --> Credenciales
2. Crear credenciales --> Clave de API 
3. Restringir clave: 
4. URL de referencia 
http://localhost:3000/


## Iconos
https://iconify.design/

## Aplicación
`npx create-react-app wildfire-tracker`
Limpieza eliminando `setupTexts.js`, `App.css`, `logo.svg` y `App.test.js` (notar que `reportWebVitals.js` no es eliminado). En `App.js`

`npm i google-map-react @iconify/react @iconify/icons-mdi`

## Google API Key en CRA
Es posible consumir variables de entorno al utilizars `create-react-app` las cuales deberán comenzar con `REACT_APP_` y tendremos que colocarlas en un archivo `.env` en la raíz.
Luego en ese archivo colocamos por ejemplo: `REACT_APP_MAP_KEY = "..."` luego en la aplicación para utilizarla `key: process.env.REACT_APP_MAP_KEY`. Por último debemos tener presente que debemos reiniciar la aplicación para que nos tome las variables de entorno ingresadas.

No olvidar agregar al archivo `.gitignore` al archivo `.env`

Debemos tener presente que no existe forma segura de almacenar secretos en una aplicación cliente corriendo en un navegador. Aun en caso de utilizar variables de entorno el contenido podrá ser descubierto. Las variables son embebidas durante el *build time* por lo que estarán en el bundle estático de HTML/CSS/JS producido por CRA.



The canonical way to use a third party API key is for your client side app to send a request to your backend API. Your backend API then formats the request as per the third-party API, adds the key and makes the call to the third-party API. Once it receives the response, it can either unpack it and translate it into domain objects which your front-end app would understand or send the raw response back to the front-end app. In this way, the API key stays at the backend and is never sent to the client-side.



# defaultProps
En determinadas ocasiones queremos que aunque el padre no le pase al hijo cierta prop esta no valga `undefined` sino que adquiera un valor default, en ese caso podremos realizar lo siguiente:
```jsx
import React from 'react';
import GoogleMapReact from 'google-map-react';

const Map = ({ center, zoom }) => {
	return (
		<div className='map'>
			<GoogleMapReact
				bootstrapURLKeys={{
					key: process.env.REACT_APP_MAP_KEY,
				}}
				defaultCenter={center}
				defaultZoom={zoom}
			></GoogleMapReact>
		</div>
	);
};

Map.defaultProps = {
	center: {
		lat: -34.546342,
		lng: -58.451925,
	},
	zoom: 14,
};

export default Map;
```

Por supuesto que en caso de que el padre le pase este valor ignorará el valor default.

Otra opción podría ser utilizar el operador `||` para definir un *fallback value*, pero en ocasiones tendremos que ponerlo en muchos lugares y esto terminará ensuciando el código.

# Obtención de Mapa y Marker
El objetivo inicial es insertar un mapa centrado en las coordenadas recibidas en `center.lat` y `center.lng` (que tienen un valor inicial en caso de que no las reciban determinado por `defaultProps`) y un ícono como marcador centrado en la pantalla.

Creamos el componente `Map` que luego importaremos en `App`:
```jsx
import React from 'react';
import GoogleMapReact from 'google-map-react';
import Marker from './Marker';
const Map = ({ center, zoom }) => {
	return (
		<div className='map'>
			<GoogleMapReact
				bootstrapURLKeys={{
					key: process.env.REACT_APP_MAP_KEY,
				}}
				defaultCenter={center}
				defaultZoom={zoom}
			>
				<Marker lat={center.lat} lng={center.lng} />
			</GoogleMapReact>
		</div>
	);
};

Map.defaultProps = {
	center: {
		lat: -34.546342,
		lng: -58.451925,
	},
	zoom: 14,
};

export default Map;


```
Luego trabajamos con el componente `Marker` (a este componente hijo de `GoogleMapReact` le pasamos las props `lat` y `lng` y con ellas ubica automáticamente al elemento en pantalla, sin que tengamos que hacer nada en el componente propiamente dicho).

```jsx
import React from 'react';
import { Icon } from '@iconify/react';
import fireIcon from '@iconify/icons-mdi/fire-alert';

function Marker() {
	return (
		<div className='marker'>
			<Icon icon={fireIcon } className='marker__icon' />
		</div>
	);
}

export default Marker;
```

Para utilizar los íconos de Iconify, primero importamos el componente `Icon` desde  `import { Icon } from '@iconify/react';` y luego el ícono propiamente dicho `import fireIcon from '@iconify/icons-mdi/fire-alert';` que le pasamos como prop `icon` a `Icon`.

En cuanto al estilo, esto lo hacemos en `index.css`:
```css
.marker__icon {
	font-size: 2rem;
	color: red;
}
```

# Datos API
A continuación en `App` obtenemos los datos de la API y queremos mostrar una indicación "Loading" cuando esté obteniendo los datos. 

```jsx
import { useState, useEffect } from 'react';
import axios from 'axios';
import Map from './components/Map';
function App() {
	const [eventData, setEventData] = useState([]);
	const [loading, setLoading] = useState(false);

	useEffect(() => {
		const fetchEvents = async () => {
			try {
				setLoading(true);
				const res = await axios.get('https://eonet.sci.gsfc.nasa.gov/api/v2.1/events');
				const { events } = res.data;
				setEventData(events);
				setLoading(false);
				// console.log(eventData);
			} catch (error) {
				console.error(error);
			}
		};
		fetchEvents();
	}, []);

	return <div>{!loading ? <Map eventData={eventData} /> : <h1>Loading...</h1>}</div>;
}

export default App;

```

En lugar de mostrar el texto "Loading..." queremos mostrar un componente `Loader` con un gif animado de un spinner, este componente simplemente consistirá en una imagen con un título:
```jsx
import React from 'react';
import spinner from '../spinner.gif';

function Loader() {
	return (
		<div className='loader'>
			<img src={spinner} alt='Loading' />
			<h1>Fetching Data</h1>
		</div>
	);
}

export default Loader;

```

Luego en `index.css` le asignamos algunos estilos básicos a este componente:
```css
.loader {
	display: flex;
	flex-direction: column;
	align-items: center;
	justify-content: center;
	min-height: 100vh;
}

.loader h1 {
	margin: 0;
}

```

Ahora a partir del array de datos `eventData` recibido como prop en `Map` queremos mapear llamando tantas veces a `Marker` como puntos a marcar tengamos. 
Tener presente que los datos obtenidos de la API a la hora de presentar las coordenadas tiene primero la longitud y luego la latitud. Es decir que si leemos `"coordinates": [ -112.97499999999999, 44.665999999999997 ]` el elemento `[0]` es  `lng` y el `[1]` es `lat`.

```jsx
import React from 'react';
import GoogleMapReact from 'google-map-react';
import Marker from './Marker';
const Map = ({ eventData, center, zoom }) => {
	const markers = eventData.map(evt => {
		if (evt.categories[0].id === 8) {
			return <Marker key={evt.id} lat={evt.geometries[0].coordinates[1]} lng={evt.geometries[0].coordinates[0]} />;
		} else {
			return null;
		}
	});
	console.log(markers);
	return (
		<div className='map'>
			<GoogleMapReact
				bootstrapURLKeys={{
					key: process.env.REACT_APP_MAP_KEY,
				}}
				defaultCenter={center}
				defaultZoom={zoom}
			>
				{markers}
				<Marker lat={center.lat} lng={center.lng} />
			</GoogleMapReact>
		</div>
	);
};

Map.defaultProps = {
	center: {
		lat: -34.546342,
		lng: -58.451925,
	},
	zoom: 14,
};

export default Map;


```

Queremos que al hacer click sobre un ícono se muestre cierta información de la incencia, esto lo hacemos en el componente `LocationInfoBox`
```jsx
import React from 'react';

const LocationInfoBox = ({ info }) => {
	return (
		<div className='location-info'>
			<h2>Event Location Info</h2>
			<ul>
				<li>ID: {info.id}</li>
				<li>TITLE: {info.title}</li>
			</ul>
		</div>
	);
};

export default LocationInfoBox;

```

Luego asociamos a cada `Marker` un evento `onClick` que setee una variable de estado `locationInfo` con la información del elemento clickado. Luego `locationInfo` es pasado como prop a `LocationInfoBox` para mostrarla en pantalla.

```jsx
import React from 'react';
import GoogleMapReact from 'google-map-react';
import Marker from './Marker';
import LocationInfoBox from './LocationInfoBox';
import { useState } from 'react';

const Map = ({ eventData, center, zoom }) => {
	const [locationInfo, setLocationInfo] = useState(null);
	const markers = eventData.map(evt => {
		if (evt.categories[0].id === 8) {
			return (
				<Marker
					key={evt.id}
					lat={evt.geometries[0].coordinates[1]}
					lng={evt.geometries[0].coordinates[0]}
					onClick={() => setLocationInfo({ id: evt.id, title: evt.title })}
				/>
			);
		} else {
			return null;
		}
	});
	//console.log(markers);
	return (
		<div className='map'>
			<GoogleMapReact
				bootstrapURLKeys={{
					key: process.env.REACT_APP_MAP_KEY,
				}}
				defaultCenter={center}
				defaultZoom={zoom}
			>
				{markers}
			</GoogleMapReact>
			{locationInfo && <LocationInfoBox info={locationInfo} />}
		</div>
	);
};

Map.defaultProps = {
	center: {
		lat: -34.546342,
		lng: -58.451925,
	},
	zoom: 14,
};

export default Map;

```
En cuanto a estilos de `LocationInfoBox` para garantizar que esté arriba de la pantalla establecemos `position: absolute;`, siendo que en el div `.map` anteriormente establecimos `position: relative;` y la estructura es la siguiente:

```jsx
<div className='map'>
	<GoogleMapReact>
	<LocationInfoBox/>
</div>
```

Los estilos completos nos quedan:
```css
.location-info {
	position: absolute;
	background-color: rgba(0, 0, 0, 0.6);
	top: 50px;
	right: 50px;
	width: 400px;
	min-height: 100px;
	border-radius: 10px;
	font-size: 10px;
	padding: 10px;
	color: #fff;
}

.location-info ul {
	list-style-type: none;
	padding: 0;
}

.location-info li {
	padding: 5px 0;
}

```

Por último en `App` queremos agregar un componente `Header` a modo de título con `position: fixed;`

```jsx
import React from 'react';
import { Icon } from '@iconify/react';
import fireIcon from '@iconify/icons-mdi/fire-alert';

const Header = () => {
	return (
		<div className='header'>
			<h1>
				<Icon icon={fireIcon} /> Wildfire Tracker (Powered by NASA)
			</h1>
		</div>
	);
};

export default Header;

```

```css
.header {
	background-color: firebrick;
	padding: 10px;
	position: fixed;
	z-index: 1;
	left: 0;
	right: 0;
	color: #fff;
	text-align: center;
}

.header h1 {
	margin: 0;
}

```

# Centrado de Mapa Dinámico
En aquellas aplicaciones cuando de acuerdo a un `select` vamos a querer centrar el mapa de acuerdo a la opción elegida, podremos utilizar una variable de estado `mapCenter` y luego modificar la prop `center` de `GoogleMapReact` (notar que hasta el momento estabamos utilizando `defaultCenter` y al intentar modificar dicho valor obteníamos un warning).

Por ejemplo a continuación mostraremos el cambio del centro del mapa cada vez que hacemos click en un ícono marcador:

```jsx
import React from 'react';
import GoogleMapReact from 'google-map-react';
import Marker from './Marker';
import LocationInfoBox from './LocationInfoBox';
import { useState } from 'react';

const Map = ({ eventData, center, zoom }) => {
	const [locationInfo, setLocationInfo] = useState(null);
	const [mapCenter, setMapCenter] = useState(center);
	const handleClick = evt => {
		setMapCenter({ lat: evt.geometries[0].coordinates[1], lng: evt.geometries[0].coordinates[0] });
		setLocationInfo({ id: evt.id, title: evt.title });
	};

	const markers = eventData.map(evt => {
		if (evt.categories[0].id === 8) {
			return (
				<Marker key={evt.id} lat={evt.geometries[0].coordinates[1]} lng={evt.geometries[0].coordinates[0]} onClick={() => handleClick(evt)} />
			);
		} else {
			return null;
		}
	});
	//console.log(markers);
	return (
		<div className='map'>
			<GoogleMapReact
				bootstrapURLKeys={{
					key: process.env.REACT_APP_MAP_KEY,
				}}
				center={mapCenter}
				defaultZoom={zoom}
			>
				{markers}
			</GoogleMapReact>
			{locationInfo && <LocationInfoBox info={locationInfo} />}
		</div>
	);
};

Map.defaultProps = {
	center: {
		lat: 44.6659,
		lng: -112.9749,
	},
	zoom: 5,
};

export default Map;

```