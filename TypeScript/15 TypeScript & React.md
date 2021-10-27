# TypeScript & React.js

## Introducción

El uso de TypeScript en React nos permite la **detección temprana de bugs** potenciales en la medida que ingresamos el código en lugar de detectarlos en *runtime*. 

Nos permite describir la estructura de un objeto y de es manera tener mejor **documentación y autocompletado**.

Por último **favorece el mantenimiento y refactoreo** del código.

Como aspectos negativos exige ingresar mucho más código y estar atentos a los errores mostrados por el compilador pero a la larga el beneficio es notable y nos permitirá evolucionar como desarrolladores React.



## Proyecto con TypeScript
Si queremos utilizar create-react-app podemos crear un template que viene con TypeScript de la siguiente forma:

```
npx create-react-app react-typescript-demo --template typescript
```

Como en cualquier aplicación CRA en `src` veremos `index.tsx` que es el entrypoint de nuestra aplicación donde montamos el componente `App.tsx` en el nodo `root` del DOM. 



Algunas observaciones:

* La extensión de los componentes es `.tsx`
* Si en `App.tsx` nos posicionamos sobre el componente funciona, veremos `function App(): JSX.Element` esto básicamente nos  está diciendo que devuelve JSX. Esto se trata de una **inferencia de tipo**.



## Prop Types

### Props de tipo string, number, boolean

Supongamos que tenemos un componente `Greet` que recibe via props un string `name` y lo muestra en pantalla.

```jsx
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

```jsx
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

```jsx
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

Supongamos que tenemos un componente `Person` que recibe como props un objeto `name` con una propiedad `firstName` y otra `lastName`.



```jsx
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



Para utilizarlo en `App.tsx`

```
 <Person name={personName} />
```

Siendo `personName`

```
const personName = {
    first: 'Juan',
    last: 'Ocho'
}
```



### Props de tipo array de objetos

Supongamos que tenemos un componente `PersonList` que en lugar de un nombre debe mostrar una lista de nombres que recibe via props. 

```
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



Para utilizarlo en `App.tsx`

```
<PersonList names={nameList} />
```

Siendo `nameList`

```
const nameList = [
    {
        first: 'Juan',
   		last: 'Ocho'
    },
    {
    	first: 'Juan',
    	last: 'Nueve'
    },
    {
    	first: 'Juan',
    	last: 'Diez'
    },
]
```



### Props con unión de strings

Supongamos que tenemos un componente `Status` que recibe mediante una prop `status` un string que no puede tomar cualquier valor, sino que debe ser uno de los siguientes:`loading`, `sucess` o `error`.

> Notar que no usamos `type StatusProps = {status: string}` pues eso haría que aceptáramos cualquier tipo de string y no sólo los tres mencionados.



```jsx
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



