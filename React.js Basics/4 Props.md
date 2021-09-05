# Props
Las props representan un modo a través del cual los compontes padre le pasarán datos a los componentes hijos.

Los componentes son reutilizables y gracias al uso de props podemos lograr que su contenido sea dinámico. Estas propiedades son especificadas como atributos en la invocación del componente `<Greet name="Juan" />` y luego las recibimos como un objeto parámetro de la función en el caso de componentes funcionales o como parte de `this.props` en el caso de los componentes de clase.


## Props en Componentes Funcionales
```jsx
const Greet = (props) => {
	return <h1>Hello {props.name}!</h1>;
};
```

### Destructuring
Es posible usar **destructuring** al recibirlas lo cual podemos hacerlo en el parámetro de la función:
```jsx
const Greet = ({name}) => {
	return <h1>Hello {name}!</h1>;
};
```
O en el cuerpo de la función:
```jsx
const Greet = (props) => {
	const {name} = props;
	return <h1>Hello {name}!</h1>;
};
```
Notar que `props` puede tener muchas propiedades y sólo hacemos destructuring de aquellas que queremos usar.

### Destructuring y asignación de Alias
`const {name : nombre} = props;`

> Como queremos que React evalúe la expresión `props.name` y no que nos muestre ese texto, lo ponemos dentro de llaves `{props.name}`

> Cuando el JSX tiene líneas múltiples en lugar de poner: 
> `return <h1> Hello {props.name} </h1>` debemos poner el valor retornado entre paréntesis y siempre deberá retornar un único elemento contenedor o *wrapper element* es decir:
```jsx
return (
		<div>
			<h1>{name}</h1>
			<h2>{name}</h2>
		</div>
	);
```

## Passing Children
Existe una prop especial llamada `children` y desde el padre para usarla debemos pasar su contenido entre los tags de apertura y cierre del componente.
Luego en el componente lo referenciamos usando la propiedad reservada `children`.

Esto es particularmente útil cuando trabajamos con *Dialog Boxes* donde el consumidor del componente entre tags de aquello que quiere renderizar como contenidoen dicho modal.

En `App.js`
```jsx
<Greet name="Juan">
	<h1>titulo</h1>
	<p>lorem ipsum dolor sit amet</p>
</Greet>
```

Luego en `Greet.js`
```jsx
const Greet = (props) => {
	return (
		<div>
			<h1>Hola {props.name}</h1>
			{props.children}
		</div>
	);
};
```
Si no le pasamos  nada entre los tags, `props.children` valdrá `undefined` y no mostrará nada.

## Props en Componentes de Clase
Al invocar componentes de clase le pasamos las props de la misma manera que a los funcionales mediante atributos y luego para utilizarlos en el componente los tenemos disponibles automáticamente en `this.props`

```jsx

function App() {
	return (
		<div className="App">
			<Welcome name="Miguel" />
		</div>
	);
}
```

```jsx
class Welcome extends Component {
	render() {
		return <h1>Bienvenido {this.props.name}</h1>;
	}
}
```

En caso de querer hacer destructuring en los componentes de clase (tanto de props como de `state`), generalmente lo haremos dentro del método `render()`. 

```jsx
class Welcome extends Component {
	render() {
		const {name} = this.props;
		return <h1>Bienvenido {name}</h1>;
	}
}
```