# TypeScript & React.js

## Introducción

El uso de TypeScript en React nos permite la **detección temprana de bugs** potenciales en la medida que ingresamos el código en lugar de detectarlos en *runtime*. 

Nos permite describir la estructura de un objeto y de es manera tener mejor **documentación y autocompletado**.

Por último **favorece el mantenimiento y refactoreo** del código.

Como aspectos negativos exige ingresar mucho más código y estar atentos a los errores mostrados por el compilador pero a la larga el beneficio es notable y nos permitirá evolucionar como desarrolladores React.



## Proyecto con TypeScript
Si queremos utilizar create-react-app podemos crear un template que viene con TypeScript de la siguiente forma:

```bash
npx create-react-app react-typescript-demo --template typescript
```

Como en cualquier aplicación CRA en `src` veremos `index.tsx` que es el entrypoint de nuestra aplicación donde montamos el componente `App.tsx` en el nodo `root` del DOM. 



Algunas observaciones:

* La extensión de los componentes es `.tsx`
* Si en `App.tsx` nos posicionamos sobre el componente funciona, veremos `function App(): JSX.Element` esto básicamente nos  está diciendo que devuelve JSX. Esto se trata de una **inferencia de tipo**.



## Prop Types

### Props de tipo string, number, boolean

Supongamos que tenemos un componente `Greet` que recibe via props un string `name` y lo muestra en pantalla.

```tsx
export const Greet = (props) => {
    return (
        <div>
            <h2>Bienvenido {props.name}</h2>
        </div>
    )
}
```

En ese momento obtendremos el mensaje `'props' implicitly has 'any' type`. Esto significa que debemos indicar la estructura de las props a recibir. 

Esto lo hacemos utilizando la palabra reservada `type` con la cual definimos la estructura de las `props` que recibirá el componente. Luego la utilizamos indicando `(props: GreetProps)`.

```tsx
type GreetProps = {
    name: string
}
export const Greet = (props: GreetProps) => {
    return (
        <div>
            <h2>Bienvenido {props.name}</h2>
        </div>
    )
}
```

Luego de hacer esto tendremos dos ventajas:

* En el componente en sí nos proporciona el **autocompletado** ya que ni bien escribimos `props.` nos sugerirá `name` y lo mismo en el componente padre nos ayudará a escribir las props apenas ingresemos la primera letra.
* En el componente padre nos permitirá una rápida detección de errores si le pasamos un tipo incorrecto a `name`.



> Es prácticamente indistinto utilizar `type` o `interface` para lo que acabamos de hacer. Sin embargo, hay quienes recomiendan usar `types` en aplicaciones y `interfaces` en bibliotecas.



Si queremos recibir más propiedades via props debemos agregarlas al tipo `GreetProps`

```tsx
type GreetProps = {
    name: string,
    messageCount: number,
    isLoggedIn: boolean
}
export const Greet = (props: GreetProps) => {
    return (
        <div>
            <h2>
                {
                    props.isLoggedIn ? 
                    `Welcome ${props.name} you have ${props.messageCount} messages`
                    : `Welcome Guest`
                }
            </h2>
        </div>
    )
}
```



Apenas modifiquemos `GreetProps` obtendremos un error en el componente que utiliza a `Greet` indicándonos que no hemos utilizado todas las props.



### Props de tipo Objeto

Supongamos que tenemos un componente `Person` que recibe como props un objeto `name` con una propiedad `first` y otra `last`.



```tsx
type PersonProps = {
    name: {
        first: string,
        last: string
    }
}
export const Person = (props: PersonProps) => {
    return (
        <div>
            <h2>My name is {props.name.first} {props.name.last}</h2>
        </div>
    )
}
```



### Props de tipo array de objetos

Supongamos que tenemos un componente `PersonList` que en lugar de un nombre debe mostrar una lista de nombres que recibe como un array de objetos `names` con una propiedad `first` y una `last` via props. 

```tsx
type PersonListProps = {
    names: {
        first: string,
        last: string
    }[]
}
export const PersonList = (props: PersonListProps) => {
    return (
        <div>
            {props.names.map(name => (
                <h2 key={name.last}>{name.first}{name.last}</h2>))
            }
        </div>
    )
}
```



### Props de tipo unión de strings

Supongamos que tenemos un componente `Status` que recibe mediante una prop `status` un string que no puede tomar cualquier valor, sino que debe ser uno de los siguientes:`loading`, `sucess` o `error`.

> Notar que no usamos `type StatusProps = {status: string}` pues eso haría que aceptáramos cualquier tipo de string y no sólo los tres mencionados.



```tsx
type StatusProps = {
    status: 'loading' | 'success' | 'error'
}
export const Status = (props: StatusProps) => {
    return (
        <div>
            {props.status}
        </div>
    )
}

```



### Props  `children`

Supongamos que tengo un componente `Heading` y queremos pasarle un texto entre los tags de apertura y cierre de modo que lo reciba en `props.children` y lo muestre en pantalla.

```tsx
type HeadingProps = {
    children: string
}
export const Heading = (props: HeadingProps) => {
    return (
        <h2>
            {props.children}
        </h2>
    )
}
```



En el componente padre tendremos `<Heading>Mensaje que queremos mostrar</Heading>`



En el caso anterior hemos utilizado como `children` un string pero también podría haber sido un componente React. Supongamos que tenemos un componente `Oscar` que queremos que reciba como `props.children` a un componente `Heading` con el mensaje a mostrar por el primero.

En `Oscar.tsx`

```tsx
type OscarProps = {
	children: React.ReactNode
}
```

> Este tipo proviene de @types/react y si estamos utilizando React 17 en adelante no habrá necesidad de importar React.



## Props opcionales

Si queremos indicar que una prop es opcional lo hacemos colocando un signo de pregunta `?` después del nombre de la prop al definir el `type`. Por ejemplo si `messageCount` es opcional:

```tsx
type GreetProps = {
	name: string,
	messageCount?: number,
    isLoggedIn: boolean
}
```

> Luego podríamos darle un valor por default `const {messageCount = 0}=props;`



### Props de eventos

Trabajaremos con dos de los eventos más usados click event de un `button` y change event de un elemento `input`.

#### Click Event

Trabajamos con un componente `Input` que recibe como props `handleClick`.

En primer lugar consideramos el caso en que no recibimos ningún parámetro y tampoco no retornamos ninguno.

```tsx
type ButtonProps = {
    handleClick: () => void
}
export const Button = (props: ButtonProps) => {
    return (
        <button onClick={props.handleClick}>Button</button>
    )
}
```

> Notar que al poner `handleClick: () => void` indicamos que no recibe ningún parámetro y no retorna nada, por ejemplo consideramos que lo usaremos para hacer un llamado a una API.

Luego en `App.tsx` tendremos ` <Button handleClick={() => console.log('Button clicked')} />`



En segundo lugar consideramos el caso en el que necesitamos usar el evento en el handler.

```tsx
type ButtonProps = {
    handleClick: (event:React.MouseEvent<HTMLButtonElement>) => void
}
export const Button = (props: ButtonProps) => {
    return (
        <button onClick={props.handleClick}>Button</button>
    )
}
```

Luego en `App.tsx` tendremos `<Button handleClick={(event) => console.log('Button clicked', event)} />`



Por último consideramos el caso en que queremos utilizar también un id.

```tsx
type ButtonProps = {
    handleClick: (event: React.MouseEvent<HTMLButtonElement>, id: number) => void
}
export const Button = (props: ButtonProps) => {
    return (
        <button onClick={event => props.handleClick(event, 1)}>Button</button>
    )
}
```

En `App.tsx` tendremos `      <Button handleClick={(event, id) => console.log('Button clicked', event, id)} />`



#### Change Event

Trabajamos con un archivo `Input.tsx` que recibe como props `value` y `handleChange`

```tsx
type InputProps = {
    value: string,
    handleChange: (event: React.ChangeEvent<HTMLInputElement>) => void
}
export const Input = (props: InputProps) => {
    return (
        <input type="text" value={props.value} onChange={props.handleChange}></input>
    )
}
```

En `App.tsx` lo utilizamos de esta forma `<Input value="test" handleChange={event => console.log(event.target.value)} />`

### Props de estilos

Trabajamos con un componente `Container.tsx`

```tsx
type ContainerProps = {
    styles: React.CSSProperties,
}
export const Container = (props: ContainerProps) => {
    return (
        <h1 style={props.styles}>Container</h1>
    )
}
```



En `App.tsx` tendremos `<Container styles={{ color: "red" }} />` en caso de que intentemos utilizar una propiedad CSS inválida nos indicará el error lo mismo si usamos un valor incorrecto.

### Destructuring de Props

### Exportar tipos

### Reutilizar tipos

### useState Hook

#### Valor inferido

#### Valor inicial y valor futuro de distinto tipo





# Contenido para Revisar

En`App.tsx` debemos especificar que se trata de un componente funcional de React y esto lo hacemos con `React.FC`

```jsx
import React from 'react';

const App: React.FC = () => {
  return <h1>Hello World!</h1>;
};

export default App;
```

Así mismo si se trata de un componente que va a recibir props  podemos crear una interfaz y así nos sugerirá las props al hacer destructuring:
```jsx
import React from 'react';

interface Props {
	name: String
}
const About: React.FC<Prop> = ({name}) => {
  return <h1>Hello {name}!</h1>;
};

export default About;
```



