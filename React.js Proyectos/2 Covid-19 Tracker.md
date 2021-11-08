# Covid-19 Tracker

Basado en el Video de Clever Programmer - [# Build a COVID-19 Tracker with REACT JS for Beginners (React Hooks and Material UI)](https://www.youtube.com/watch?v=cF3pIMJUZxM)

Cuando queremos trabajar en Visual Studio Code de manera colaborativa podemos utilizar la extensión de Microsoft [Visual Studio Live Share](https://visualstudio.microsoft.com/es/services/live-share/) que nos permitirá compartir el servidor local, compartir terminales y también será posible hacer llamadas de audio (otra extensión adicional es necesaria).

Utilizaremos la API opensource [https://disease.sh/](https://disease.sh/).

# --------
5. Creación de la Estructura del Proyecto
6. Creación de Header (título y dropdown de país)
7. Creación de Info boxes (casos totales, recuperados y muertes)
8. Creación de Tabla con listado de países y casos por país.
9. Creación de Chart
10. Creación de Mapa
>Para los mapas utilizaremos: [leafletjs](https://leafletjs.com/) que tiene la ventaja frente a Google Maps que es OpenSource y no requiere de API Keys.
11. Syling con Material UI
12. Deploy 

Utilizaremos **Firebase** [https://firebase.google.com/](https://firebase.google.com/)
**Iniciamos sesión**, luego hacemos click en **Ir a la consola** y en **Agregar proyecto**. Le damos el nombre "Covid 19 Tracker" luego en Continuar elegimos una de Google Analytics y en el siguiente paso estará creado el proyecto.

# ------

## Crear wireframe
Creamos un sket o wireframe de manera tal de tener una conceptualización de lo que vamos a crear antes de empezar a codear.
> Para realizar el wireframe recurrimos a draw.io (que al ingresar nos redirecciona a [app.diagrams.net](https://app.diagrams.net/))

## create-react-app
`npx create-react-app covid-19-tracker`
`cd covid-19-tracker`
`npm start`
`code .`

## Limpieza del Código
Es el proceso mediante el cual eliminamos una serie de archivos dentro del directorio.
En `src` eliminar `App.test.js`, `setupTests.js` y`logo.svg`

En app.js eliminar `import logo from './logo.svg';` y limpiar el contenido de  el contenido de `<div className="app"></div>` también cambiarle el nombre de `App` a `app` de manera de respetar *BEM naming convention*.

## Estilos Iniciales
Eliminar todo el contenido de `App.css` y agregamos: `* { margin: 0}` y le damos al body un color gris claro `body { background-color: #f5f6fa;}`

> En la parte inferior en Visual Studio Code si vemos JavaScript podemos hacer click con lo cual podremos elegir el lenguaje y elegimos JavaScript React de manera tal que al escribir `h1` y presionar tab obtengamos `<h1></h1>`

## Estructurar Proyecto
En nuestro proyecto tendremos los siguientes componentes, por lo que creamos comentarios para luego ir colocando el código en esas secciones.
```jsx
{ /* Header*/ }
{ /* Title + dropdown */ }

{ /* InfoBox */ }
{ /* InfoBox */ }
{ /* InfoBox */ }

{ /* Table */ }
{ /* Graph */ }

{ /* Map */ }
```

Utilizaremos [material-ui](https://material-ui.com/) que es un *design framework* creado y mantenido por google lo instalamos con: `npm install @material-ui/core`

## Dropdown Hardcodeado
Para mostrar un select con opciones primero importamos los elementos de material ui que vamos a utilizar:
```jsx
import { FormControl, Select, MenuItem } from '@material-ui/core';

<FormControl clasName="app__dropdown">
	<Select variant="outlined" value="worldwide">
		{/* A futuro Obtener lista de todos los paises y generar un dropdown con ellos*/}
		<MenuItem value="worldwide">Worldwide</MenuItem>
		<MenuItem value="pais1">Pais1</MenuItem>
		<MenuItem value="pais2">Pais2</MenuItem>
		<MenuItem value="pais3">Pais3</MenuItem>
	</Select>
</FormControl>
```

## Carga Dropdown Manual
Si quisieramos armar la lista de manera manual podríamos hacerlo de este modo:
Podemos pensar en `useState` como el modo de tener variables en React. Si quisiéramos que el valor inicial fuera un array vacío deberíamos poner `useState([]);`
```jsx
const [countries, setCountries] = useState(['USA', 'Argentina', 'India']);

<FormControl clasName="app__dropdown">
	<Select variant="outlined" value="worldwide">
		{countries.map((country) => (
			<MenuItem value={country}>{country}</MenuItem>
		))}
	</Select>
</FormControl>
```

## Llamada a API `disease.sh`
Utilizamos la API de `disease.sh` y en particular el endpoint `https://disease.sh/v3/covid-19/countries`

`UseEffect` Ejecuta una porción de código cuando el componente se carga y vuelve a ejecutarse o no basándose en una condición determinada. La condición la dejamos en blanco `[]` ya que queremos que se ejecute cuando el componente carga y que no vuelva a hacerlo. Si quisiéramos que se ejecute cada vez que cambia la variable `countries` o `abc` deberíamos poner `[countries, abc]`
También tenemos que importarlo `import React, { useState, useEffect } from 'react';`

Una vez obtenido el objeto con todos los paises conformamos un objeto con la función map de modo de tener `[{name:"Argentina", value: "AR"},{...},{...}]` una vez confeccionado ese array llamamos a `setCountries(countries)` para modificar la variable `countries` y así volver a renderizar el dropdown con todos los países.

> Podremos ver que cada vez que realiza una llamada al servicio, el autor utiliza el método `fetch` con promesas y a demás de los métodos `.then` utiliza `await` (lo cual no es necesario) "para estar seguro"

```jsx
useEffect(() => {
	const getCountriesData = async () => {
		await fetch('https://disease.sh/v3/covid-19/countries')
			.then((response) => response.json())
			.then((data) => {
				const countries = data.map((country) => ({
					name: country.country, //United States, United Kingdom
					value: country.countryInfo.iso2, //USA, UKA
				}));

				setCountries(countries);
			});
	};
	getCountriesData();
}, []);
```

## Worldwide
Queremos agregar en el dropdown la posibilidad de seleccionar **Worldwide**, para hacer esto agregamos manualmente el `<MenuItem value="worldwide">Worldwide</MenuItem>`.
Por otra parte queremos almacenar el estado del país seleccionado teniendo `worldwide` como default: `const [country, setcountry] = useState(['worldwide']);`. Para ello también cargamos esto como valor inicial del Select `<Select variant="outlined" value={country}>` con lo cual *mapeamos el dropdown al piece of state*. Pero en caso de seleccionar algún país no se modificará el estado. Para lograrlo debemos trabajar con `onChange` `<Select variant="outlined" onChange={onCountryChange} value={country}>`
```jsx
const onCountryChange = (event) => {
	const countryCode = event.target.value;
	console.log(countryCode);
	setcountry(countryCode);
};
```

## Contenedor de Componentes
Si tenemos un componente contenedor de otros componentes, una posible idea es crear en la carpeta `components` y en ella un archivo `index.js` con una función `Home` que luego importamos en `App` directamente con `import Home from './components/`

## Infobox
Crearemos un componente `<Infobox>` en `src/components` al cual utilizaremos tres veces ya que de acuerdo a las props que le pasemos mostrará casos activos, recuperados o fallecimientos. Luego ejecutamos `rfce` y utilizaremos varios recursos de Material UI: `Card, CardContent, Typography`

```jsx
import React from 'react';
import { Card, CardContent, Typography } from '@material-ui/core';

function InfoBox({ title, cases, total }) {
	return (
		<div>
			<Card className="infobox">
				<CardContent>
					<Typography className="infobox__title" color="textSecondary">
						{title}
					</Typography>
					<h2 className="infobox__cases">{cases}</h2>
					<Typography className="infobox__total" color="textSecondary">
						{total} Total
					</Typography>
				</CardContent>
			</Card>
		</div>
	);
}

export default InfoBox;

```

Luego utilizamos en `App.js` este componente dentro de un contenedor `app__stats`
```jsx
<div className="app__stats">
	<InfoBox title="Coronavirus Cases" cases={12} total={120} />
	<InfoBox title="Recovered" cases={12} total={120} />
	<InfoBox title="Deaths" cases={12} total={120} />
</div>
```

Luego en `App.css` le damos estilos a `.app__stats`

## Mapa
Como primer paso creamos un archivo `Map.js` en `/src/components` y creamos un *functional component* con un dummy text.


## Diseño Responsivo
En el diseño *desktop* dividimos la aplicación en dos contenedores y les damos el nombre `app__left` (donde tendremos el dropdown, los infobox, el mapa) y `app__rigth` (con una tabla de valores y los gráficos).
Por un lado `.app__left` la aplicaremos a un `div` mientras que `.app__rigth` la aplicaremos a un `Card` de material ui.
Cuando estamos en pantallas grandes queremeos que `app_left` crezca ocupando el 90% espacio disponible, es por eso que ponemos `flex: 0.9` (en definitiva al poner un sólo valor estamos estableciendo `flex-grow`). `app__rigth` en tanto tiene el valor default de `flex-grow`, o sea 0 por lo que no crecerá).
La idea es que una vez alcanzado un cierto tamaño de pantalla `990px` `app__left` se ubique debajo de `app__rigth`. Esto lo hacemos con el media querie `@media (max-width: 990px) `

## Obtención de Datos País
Continuamos trabajando con `onCountryChange` de manera tal que cuando el usuario selecciona un nuevo país hagamos un nuevo llamado al servicio: `https://disease.sh/v3/covid-19/countries/[COUNTRY_CODE]` a menos que el usuario haya seleccionado Worldwide en cuyo caso tendremos que hacer el llamado a `https://disease.sh/v3/covid-19/all`. 
Para esto utilizamos un *ternary operator* para confeccionar la url, luego la utilizaremos en conjunto con `fetch` para obtener los datos del pais en cuestión.
Notar que como dijimos antes a pesar de usar promesas el autor también usa `await` por lo que necesitaremos `async`.  

```jsx
const onCountryChange = async (event) => {
	const countryCode = event.target.value;
	//console.log(countryCode);

	const url =
		countryCode === 'worldwide'
			? 'https://disease.sh/v3/covid-19/all'
			: `https://disease.sh/v3/covid-19/countries/${countryCode}`;

	await fetch(url)
		.then((response) => response.json())
		.then((data) => {
			setcountry(countryCode);
			setCountryInfo(data);
		});
};
```

De esta manera ya tenemos los datos para pasarle como props a `<InfoBox>`

```jsx
<div className="app__stats">
	<InfoBox title="Coronavirus Cases" cases={countryInfo.todayCases} total={countryInfo.cases} />
	<InfoBox title="Recovered" cases={countryInfo.todayRecovered} total={countryInfo.recovered} />
	<InfoBox title="Deaths" cases={countryInfo.todayDeaths} total={countryInfo.deaths} />
</div>
```

# Valor Inicial
Al iniciar la aplicación queremos obtener los datos correspondientes a `worldwide` y eso lo hacemos con un `useEffect()` y un API call:
```js
useEffect(() => {
	fetch('https://disease.sh/v3/covid-19/all')
		.then((response) => response.json())
		.then((data) => {
			setCountryInfo(data);
		});
}, []);
```
Como segundo argumento indicamos una array vacío dado que queremos que se ejecute una vez al iniciar y no vuelva a ejecutarse.

# Tabla de casos por pais
En primer lugar debemos contar con `const [tableData, setTableData] = useState([]);`, luego al iniciar el programa en `getCountriesData()` obtenemos así como la lista de países para el dropdown, en el mismo endpoint la lista de casos por país. Con este valor hacemos `setTableData(data);`
Luego creamos un **componente funcional** `Table.js` al cual le pasamos como `props` a `<Table countries={tableData}/>` .

Componente `Table.js`
```jsx
import React from 'react';
import './Table.css';

function Table({ countries }) {
	return (
		<div className="table">
			{countries.map((country) => (
				<tr>
					<td>{country.country}</td>

					<td>
						<strong>{country.cases}</strong>
					</td>
				</tr>
			))}
		</div>
	);
}

export default Table;

```
Hasta el momento obtendremos la lista de todos los países con la cantidad de casos. Debemos corregir dos cosas: 
1. La lista es demasiado grande.
2. La lista está desordenada

En cuanto a la **longitud de la lista** lo vamos a trabajar con estilos. Creamos un archivo `Table.css` y lo importamos en el componente.
Establecemos `overflow-y: scroll` y establecemos el `heigth:400px`
Por otra parte queremos un efecto stripped el cual logramos trabajando con `.table tr:nth-of-type(odd) `

Cuando pasamos a la versión tablet/mobile veremos que aunque el `<div class="table">` ocupa todo el ancho, los `tr` ocupan solo el espacio determinado por el contenido. Es por eso que hacemos `.table tr { display: flex}`

En cuanto al **ordenamiento** crearemos un archivo `util.js` (notar u en minúscula) en `src` en el cual tendremos *helper functions* y una de ellas será una función encargada de ordenar los países por casos.

```js
export const sortData = (data) => {
	const sortedData = [...data];

	sortedData.sort((a, b) => {
		if (a.cases > b.cases) {
			return -1;
		} else {
			return 1;
		}
	});
	return sortedData;
};

```

Esto mismo podemos expresarlo en una única línea: `return sortedData.sort((a, b) => (a.cases > b.cases ? -1 : 1));`

Luego importamos esta función en `App.js` `import { sortData } from './util';`

```js
const sortedData = sortData(data);
setCountries(countries);
```

# LineGraph
Debajo de la tabla colocaremos un gráfico con la cantidad de casos diarios a nivel mundial y para ello creamos  un componente funcional llamado `LineGraph` en el cual utilizaremos `react-chartjs-2`

`npm i react-chartjs-2 chart.js` 

Notar que no le pasamos los datos via props sino que dentro del mismo componente hacemos el fetch del endpoint: `https://disease.sh/v3/covid-19/historical/all?lastdays=120`

De este servicio obtenemos:
```json
{
	"cases":{"5/12/20":4265367,"5/13/20":4350206....},
	"deaths":{"5/12/20":4265367,"5/13/20":4350206....},
	"recovered":{"5/12/20":4265367,"5/13/20":4350206....}
}
```
Y tenemos que convertirlo al formato esperado por `<Line>` de chart.js que podemos ver en la [documentación](https://www.chartjs.org/docs/latest/charts/line.html). Esto lo hacemos con una *helper function* `buildChartData` 

Instalamos el paquete `numeral` `npm i numeral`