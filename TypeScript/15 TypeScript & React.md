# TypeScript & React.js

## Contenido para Revisar

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

Es posible hacer el destructuring de las props al definir el componente como podemos ver a continuación, retomando el ejemplo de `Input.tsx`

```tsx
type InputProps = {
    value: string,
    handleChange: (event: React.ChangeEvent<HTMLInputElement>) => void
}
export const Input = ({value,handleChange}: InputProps) => {
    return (
        <input type="text" value={value} onChange={handleChange}></input>
    )
}
```



### Exportar tipos

En componentes simples tiene sentido definir los tipos en la parte superior, sin embargo cuando estos tienen múltiples tipos nos conviene hacerlo en un archivo aparte.

Por ejemplo tenemos `Person.tsx` con el siguiente código:

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



Luego copiamos los tipos a un archivo `Person.types.ts`

```tsx
export types PersonProps = {
    name: {
        first: string,
        last: string
    }
}
```

Y los importamos en `Person.tsx`

```tsx
import {PersonProps} from './Person.types'

export const Person = (props: PersonProps) => {
    return (
        <div>
            <h2>My name is {props.name.first} {props.name.last}</h2>
        </div>
    )
}
```



### Reutilizar tipos

Para asegurar la reutilización y evitar repeticiones, es importante reutilizar los tipos siempre que sea posible. 

Por ejemplo en `Person.types.ts` hasta ahora tenemos:

```tsx
export types PersonProps = {
    name: {
        first: string,
        last: string
    }
}
```

 Podríamos definir un tipo `Name` y exportarlo para utilizarlo en otros lados.

```tsx
export type Name = {
	{
        first: string,
        last: string
    }
}
export types PersonProps = {
    name: Name
}
```

Por ejemplo en `PersonList` donde hasta ahora tenemos:

```tsx
type PersonListProps = {
    names: {
        first: string,
        last: string
    }[]
}
```

Podemos utilizar el tipo `Name`

```tsx
import {Name} from './Person.types'

type PersonListProps = {
    names: Name[]
}
```



## Tipos en el hook `useState`

Estudiaremos como establecer los tipos de los hooks, comenzando por el `useState` hook.

#### `useState` con tipo inferido de valor inicial

El primer caso que consideramos será aquel en el cual el tipo es inferido en función del valor inicial. Para ello creamos un componente `LoggedIn.tsx`. En él tendremos un botón Login y uno Logout y debemos mostrar un mensaje acorde a ese estado.

```tsx
import { useState } from "react"

export const LoggedIn = () => {
    const [isLoggedIn, setIsLoggedIn] = useState(false)
    const handleLogin = () => {
        setIsLoggedIn(true)
    }
    const handleLogout = () => {
        setIsLoggedIn(false)
    }
    return (
        <>
            <button onClick={handleLogin}>Login</button>
            <button onClick={handleLogout}>Logout</button>
            <div>the user is {isLoggedIn ? "logged in" : "logged out"}</div>
        </>
    )
}
```

Si ponemos el cursor sobre `isLoggedIn` veremos que se trata de un `boolean` y por ende sólo nos permitirá asignarle ese tipo de valores con `setIsLoggedIn`.

#### 

#### `useState` con tipo del valor inicial distinto del valor futuro

En ocasiones no conocemos el valor inicial de una variable de estado y la asignaremos luego. 

Supongamos que trabajamos con un componente `User.tsx` que cuenta también con dos botones de Login y Logut y queremos que al loguearnos se almacene en una variable de estado `user` los datos de un usuario de tipo `AuthUser`. 

Como inicialmente no disponemos de estos datos le damos valor `null`

 `const [user, setUser] = useState(null)` 

Sin embargo, debido a la inferencia de tipo, cuando el botón de login sea clickeado no nos dejará setear el usuario con como un objeto de tipo `AuthUser` es decir: `setUser({name:'juaneme8', email:'j8@example.com'})` sino que esperará que le pasemos `null`. 

En este caso no podremos confiar en la inferencia de tipo sino que tendremos que especificarle que el tipo podrá ser `null` o de tipo `AuthUser` y esto lo hacemos con:

`const [user, setUser] = useState<AuthUser|null>(false)`



```tsx
import { useState } from "react"

type AuthUser = {
    name: string,
    email: string
}

export const User = () => {
    const [user, setUser] = useState<AuthUser | null>(null)

    const handleLogin = () => {
        setUser({
            name: 'j8',
            email: 'j8@example.com'
        })
    }
    const handleLogout = () => {
        setUser(null)
    }
    return (
        <>
            <button onClick={handleLogin}>Login</button>
            <button onClick={handleLogout}>Logout</button>
            <div>User is {user?.name}</div>
            <div>Email is {user?.email}</div>
        </>
    )
}
```



> A la hora de renderizar estos campos veremos que al hacer click en el Intellicense se incluye el *optional chaining operator* automáticamente  ya que `user` puede ser `null` y sólo queremos acceder a las propiedades `name` e `email` si no lo es.



#### `useState` Type Assertion

Anteriormente vimos que la posibilidad de que el valor sea `null` nos exige chequear siempre que no lo sea antes de acceder a las propiedades, es por eso que si  tenemos absoluta certeza de que `user` nunca será `null` (por ejemplo si se seteará en un efecto `useEffect` y no tenemos el método logout) podemos definir como valor inicial un objeto vacío como si fuera de tipo `AuthUser` y esto lo hacemos con el keyword `as`.

`const [user, setUser] = useState<AuthUser>({} as AuthUser)`

De esta manera podremos acceder a `name` y `email` sin chequear que sea `null` es decir sin el optional chainig operator `{user.name}`



## Tipos en el hook `useReducer` 

Si bien `useState` es un hook útil cuando manejamos valores simples de estado, mientras que es conveniente utilizar `useReducer` cuando el estado es más complejo y el estado futuro depende del estado anterior.

Trabajaremos con un componente `Counter.tsx` que cuenta con botones para incrementar o decrementar el valor de cuenta. Inicialmente lo haremos sin agregar conceptos de TypeScript:

```tsx
import { useReducer } from "react";

const initialState = { count: 0 }

function reducer(state, action) {
    switch (action.type) {
        case 'increment': {
            return { count: state.count + action.payload };
        }
        case 'decrement': {
            return { count: state.count - action.payload };
        }
        default:
            return state;
    }
}

export const Counter = () => {
    const [state, dispatch] = useReducer(reducer, initialState);
    return (
        <>
            <div>{state.count}</div>
            <button onClick={() => dispatch({ type: 'increment', payload: 10 })}>Increment 1 </button>
            <button onClick={() => dispatch({ type: 'increment', payload: 1 })}>Increment 10 </button>
            <button onClick={() => dispatch({ type: 'decrement', payload: 1 })}>Decrement 1 </button>
            <button onClick={() => dispatch({ type: 'decrement', payload: 10 })}>Decrement 10 </button>
        </>
    )
}
```

Veremos que tenemos líneas rojas indicandonos errores en la línea `function reducer(state, action) {` debido a que debemos especificar el tipo de `state` y `action`. Como sabemos el tipo de datos que van a almacenar estar variables definimos los `type`



```tsx
import { useReducer } from "react";

type CounterState = {
    count: number
}

type CounterAction = {
    type: string,
    payload: number
}

const initialState = { count: 0 }

function reducer(state: CounterState, action: CounterAction) {
    switch (action.type) {
        case 'increment': {
            return { count: state.count + action.payload };
        }
        case 'decrement': {
            return { count: state.count - action.payload };
        }
        default:
            return state;
    }
}

export const Counter = () => {
    const [state, dispatch] = useReducer(reducer, initialState);
    return (
        <>
            <div>{state.count}</div>
            <button onClick={() => dispatch({ type: 'increment', payload: 1 })}>Increment 1 </button>
            <button onClick={() => dispatch({ type: 'increment', payload: 10 })}>Increment 10 </button>
            <button onClick={() => dispatch({ type: 'decrement', payload: 1 })}>Decrement 1 </button>
            <button onClick={() => dispatch({ type: 'decrement', payload: 10 })}>Decrement 10 </button>
        </>
    )
}
```

Luego debido a la inferencia de tipos veremos que si posicionamos el mouse en `const [state, dispatch] = useReducer(reducer, initialState);` en `state` sabrá que es de tipo `CounterState`, en `dispatch` de tipo `React.Dispatch<CounterAction>` (esto podría ser útil si tenemos que pasar a un componente `state` o `dispatch` como props).



Hemos definido el `type` como `string`

```
type CounterAction = {
    type: string,
    payload: number
}
```

Esto a futuro puede traernos inconvenientes porque en lugar de "increment" y "decrement" podríamos poner "reset" sin que nos marque ningún error (a pesar de que esto lo atajaríamos con el `default` case del switch). Esto lo solucionamos utilizando template literals en lugar de `string`.

```
type CounterAction = {
    type: 'increment'|'decrement',
    payload: number
}
```



Si ahora quisiéramos agregar la funcionalidad `reset` podríamos hacerlo agregando `reset` al `CounterAction` y agregando un botón para tal fin y un caso en el reducer. Sin embargo, al hacerlo obtendríamos un error por no estar usando payload cosa que no necesitamos en este caso. La forma más simple sería pasarle un `payload: 0` que luego no usamos en el reducer.

```tsx
import { useReducer } from "react";

type CounterState = {
    count: number
}

type CounterAction = {
    type: 'increment' | 'decrement' | 'reset',
    payload: number
}

const initialState = { count: 0 }

function reducer(state: CounterState, action: CounterAction) {
    switch (action.type) {
        case 'increment': {
            return { count: state.count + action.payload };
        }
        case 'decrement': {
            return { count: state.count - action.payload };
        }
        case 'reset': {
            return initialState;
        }
        default:
            return state;
    }
}

export const Counter = () => {
    const [state, dispatch] = useReducer(reducer, initialState);
    return (
        <>
            <div>{state.count}</div>
            <button onClick={() => dispatch({ type: 'increment', payload: 1 })}>Increment 1 </button>
            <button onClick={() => dispatch({ type: 'increment', payload: 10 })}>Increment 10 </button>
            <button onClick={() => dispatch({ type: 'decrement', payload: 1 })}>Decrement 1 </button>
            <button onClick={() => dispatch({ type: 'decrement', payload: 10 })}>Decrement 10 </button>
            <button onClick={() => dispatch({ type: 'reset', payload: 0 })}>Reset </button>
        </>
    )
}
```



Otra opción sería indicar que `payload` es opcional:

```
type CounterAction = {
    type: 'increment' | 'decrement' | 'reset',
    payload?: number
}
```

Pero esto traería el incoveniente que de que dentro del case `increment` y `decrement` como esos sí usan `action.payload` nos aparecería "Object is possibly 'undefined'", lo cual podríamos solucionar colocando `(action.payload || 0)`.

Una solución mas conveniente es usar lo que se conoce como **discriminated unions** sería creando dos tipos `UpdateAction` (con `type` y `payload`), `ResetAction` )(sólo con `type `) y declarando `CounterAction` como la unión de los dos.

 

```tsx
import { useReducer } from "react";

type CounterState = {
    count: number
}

type UpdateAction = {
    type: 'increment' | 'decrement',
    payload: number
}

type ResetAction = {
    type: 'reset'
}


type CounterAction = UpdateAction | ResetAction

const initialState = { count: 0 }

function reducer(state: CounterState, action: CounterAction) {
    switch (action.type) {
        case 'increment': {
            return { count: state.count + action.payload };
        }
        case 'decrement': {
            return { count: state.count - (action.payload || 0) };
        }
        case 'reset': {
            return initialState;
        }
        default:
            return state;
    }
}

export const Counter = () => {
    const [state, dispatch] = useReducer(reducer, initialState);
    return (
        <>
            <div>{state.count}</div>
            <button onClick={() => dispatch({ type: 'increment', payload: 1 })}>Increment 1 </button>
            <button onClick={() => dispatch({ type: 'increment', payload: 10 })}>Increment 10 </button>
            <button onClick={() => dispatch({ type: 'decrement', payload: 1 })}>Decrement 1 </button>
            <button onClick={() => dispatch({ type: 'decrement', payload: 10 })}>Decrement 10 </button>
            <button onClick={() => dispatch({ type: 'reset' })}>Reset </button>
        </>
    )
}
```



## Tipos en el hook `useContext`

A la hora de trabajar con TypeScript y React Context usando hooks trabajaremos con `useContext` para consumir el valor del contexto.

A los fines didácticos consideraremos el uso de React Context para proporcionar un tema a nuestros componentes usando Context API. Para ello asumimos que tenemos los archivos `theme.ts`, `Box.tsx` y `ThemeContext.tsx`

Queremos usar el tema como un contexto y usarlo para estilar el `div` dentro de `Box`



En primer lugar definimos `theme.ts`

```tsx
export const theme = {
    primary: {
        main: '#3f51b5',
        text: '#fff',
    },
    secondary: {
        main: '#f50057',
        text: '#fff',
    },
}
```



Luego en `ThemeContext`

```tsx
import { createContext } from "react"
import { theme } from './theme'

export const ThemeContext = createContext(theme)

type ThemeContextProviderProps = {
    children: React.ReactNode
}

export const ThemeContextProvider = ({ children }: ThemeContextProviderProps) => {
    return (
        <ThemeContext.Provider value={theme}>
            {children}
        </ThemeContext.Provider>
    )
}
```



En `App.tsx`

```tsx
<ThemeContextProvider>
	<Box />
</ThemeContextProvider>
```



En `Box.tsx`

```tsx
import { useContext } from "react"
import { ThemeContext } from "./ThemeContext"

export const Box = () => {
    const theme = useContext(ThemeContext)
    return (
        <div style={{ backgroundColor: theme.primary.main, color: theme.primary.text }}>Box</div>
    )
}
```

