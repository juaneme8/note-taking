# TypeScript & React.js

> Basado en el [video](https://youtu.be/15VKbky2gB4) de midudev.
>
> Basado en la [lista de reproducción](https://www.youtube.com/playlist?list=PLC3y8-rFHvwi1AXijGTKM0BKtHzVC-LSK) de Codevolution. 

## Introducción

El uso de TypeScript en React nos permite la **detección temprana de bugs** potenciales en la medida que ingresamos el código en lugar de detectarlos en *runtime*. 

Nos permite describir la estructura o contrato de un objeto y de esta manera tener mejor **documentación y autocompletado**.

Por último **favorece el mantenimiento y refactoreo** del código.

También tiene la ventaja que hace que no sea necesario utilizar la librería `prop-types` puesto que TypeScript cumple una tarea similar pero es mucho más potente. Existe una diferencia y es que `prop-types` en desarrollo funciona en *run-time* por lo que veríamos los errores en la consola, mientras que en TS funciona en *build-time* por lo que los errores los veremos en el editor o en la compilación. 

Como aspectos negativos exige ingresar mucho más código y estar atentos a los errores mostrados por el compilador pero a la larga el beneficio es notable y nos permitirá evolucionar como desarrolladores React.



### Observaciones 

* La extensión de los componentes es `.tsx` y para aquellos archivos que no tenemos jsx usamos `.ts`
* Si en `App.tsx` nos posicionamos sobre el componente (:sparkles: en TS el hover es fundamental :sparkles:) veremos `function App(): JSX.Element` esto básicamente nos  está diciendo que devuelve JSX. Esto se trata de una **inferencia de tipo**.
* Si ejecutamos `npm run build` en la carpeta `build/static/js` veremos que tenemos el código compilado a JavaScript.
* Muchas veces los errores de TS conviene leerlos de abajo hacia arriba, pues encontraremos el motivo de los mismos más rápidamente.



# Proyecto con TypeScript
## TypeScript y Create React App

Si queremos utilizar create-react-app podemos crear un template que viene con TypeScript de la siguiente forma:

```bash
npx create-react-app react-typescript-demo --template typescript
```

Como en cualquier aplicación CRA en `src` veremos `index.tsx` que es el entrypoint de nuestra aplicación donde montamos el componente `App.tsx` en el nodo `root` del DOM. 



## TypeScript y Vite

```
npm init vite@latest
```

Luego elegimos el nombre del proyecto, luego `react` como framework y por último indicamos que queremos usar TypeScript  eligiendo `react-ts`.

Finalmente `cd nombre-proyecto` y ejecutamos `npm i`  y `npm run dev`



> Si queremos hacerlo en un sólo paso: 
>
> ```
> npx @vitejs/app mi-app --template react-ts
> ```



## TypeScript y Next.js

```
npx create-next-app@latest --ts
```



# Tipado Componentes Funcionales

Cuando tenemos un componente funcional una opción es expresarlo de este modo:

```typescript
interface Props {
	name: String
}
const About = ({name}:Props) => {
  return <h1>Hello {name}!</h1>;
};

export default About;
```



Otra opción es especificar que se trata de un componente funcional de React y esto lo hacemos con `React.FC`. Esto nos permitiría acceder a ciertas propiedades del componente como por ejemplo habilitarnos para recibir children aún sin especificarlo en la interface de las Props. Esto según algunos autores puede no ser aconsejable.

Si posicionamos el mouse por encima del nombre del componente no nos aparecerá más `JSX.Element` y nos dirá en cambio `React.FC<Props>`.

```jsx
interface Props {
	name: String
}
const About: React.FC<Props> = ({name}) => {
  return <h1>Hello {name}!</h1>;
};

export default About;
```

> También es posible en lugar de poner `React.FC<Props>` poner `React.FunctionComponent<Props>`



## Tipar Métodos

Aunque no es una buena práctica y normalmente conviene extraerlo en componentes aparte, en ocasiones tendremos funciones que retornan JSX. En esos casos es aconsejable tiparlas lo cual nos ayudará a detectar errores a futuro. Por ejemplo suponemos un caso en el que tenemos un método `renderList` que retorna una lista de elementos de tipo JSX.

```typescript
const List = ({subs}: Props) => {
    
	const renderList = ():JSX.Element[] => {
		return subs.map(sub => {
            return (
            	<li key=...>
                     <h4>...</h4>
                     <p>...</p>
                </li>
            )
        }
	}
	
	return (
		<ul>
			{renderList()}
		</ul>
	)
}
```



# Tipar Eventos

Consideramos un componente `Form` que consiste en una serie de inputs. 



## Change Event

Ni bien creamos el método `  const handleChange = evt => {};` veremos que nos aparece **Parameter 'evt' implicitly has an 'any' type.** 

Para ayudarnos a obtener el tipo que debe ser `evt` podemos utilizar una expresión en línea como vemos a continuación y luego posicionar el mouse sobre `evt` de donde obtendremos: `React.ChangeEvent<HTMLInputElement>`

```typescript
 <input
        onChange={evt => {
          setInputValues({ ...inputValues, [evt.target.name]: evt.target.value });
        }}
        type='text'
        name='nick'
        placeholder='nick'
        value={inputValues.nick}
      />
```

Luego el bloque de código nos queda:

```typescript
import { useState } from 'react';

const Form = () => {
  const [inputValues, setInputValues] = useState({
    nick: '',
    subMonths: 0,
    avatar: '',
    description: '',
  });

  const handleChange = (evt: React.ChangeEvent<HTMLInputElement>) => {
    setInputValues({ ...inputValues, [evt.target.name]: evt.target.value });
  };

  console.log({ inputValues });

  return (
    <form>
      <input onChange={handleChange} type='text' name='nick' placeholder='nick' value={inputValues.nick} />
      <input
        onChange={handleChange}
        type='number'
        name='subMonths'
        placeholder='subMonths'
        value={inputValues.subMonths}
      />
      <input onChange={handleChange} type='text' name='avatar' placeholder='avatar' value={inputValues.avatar} />
      <input
        onChange={handleChange}
        type='text'
        name='description'
        placeholder='description'
        value={inputValues.description}
      />
      <button>Enviar</button>
    </form>
  );
};

export default Form;
	
```

Supongamos que decidimos cambiar el `input` de `description` por un `textarea`, en ese caso nos aparecería un error que leyéndolo de abajo hacia arriba diría: **...Type 'HTMLTextAreaElement' is missing the following properties from type 'HTMLInputElement'...**

Entonces modificamos el tipo de esta forma:

```typescript
const handleChange = (evt: React.ChangeEvent<HTMLInputElement | HTMLTextAreaElement>) => {
    setInputValues({ ...inputValues, [evt.target.name]: evt.target.value });
  };
```



## Reutilizar Interfaces

Las interfaces que hacen a la lógica de negocios es conveniente exportarlas a un archivo `types.d.ts` (que creamos en `src` no en `components`) de modo tal que las reutilicemos. 

Este archivo tiene unas características:

* Las exportaremos para que sean utilizadas por cualquier paquete que utilice nuestra biblioteca.
* En ese archivo sólo podemos tener definiciones

```typescript
export interface Sub {
	nick: string
	subMonths: number
	avatar:string
	description?:string
}
```

Lo anterior como forma parte de la lógica de negocios lo hemos extraído a un archivo aparte.

La idea será luego reutilizar esta interface en todos los lugares donde podamos. Notar que **a la hora de importar lo hacemos sin la extensión** por lo que en este caso eliminamos `.d.ts` y quedará `import {Sum} from '../types'`.



```typescript
import { useState } from 'react';
import { Sub } from '../types';

interface FormState {
  inputValues: Sub;
}

const Form = () => {
  const [inputValues, setInputValues] = useState<FormState['inputValues']>({
    nick: '',
    subMonths: 0,
    avatar: '',
    description: '',
  });

  const handleChange = (evt: React.ChangeEvent<HTMLInputElement | HTMLTextAreaElement>) => {
    setInputValues({ ...inputValues, [evt.target.name]: evt.target.value });
  };

  console.log({ inputValues });

  return (
    <form>
      <input onChange={handleChange} type='text' name='nick' placeholder='nick' value={inputValues.nick} />
      <input
        onChange={handleChange}
        type='number'
        name='subMonths'
        placeholder='subMonths'
        value={inputValues.subMonths}
      />
      <input onChange={handleChange} type='text' name='avatar' placeholder='avatar' value={inputValues.avatar} />
      <textarea onChange={handleChange} name='description' placeholder='description' value={inputValues.description} />
       <button>Enviar</button>
    </form>
  );
};

export default Form;

```

Notar que `FormState` lo dejamos preparado para si en un futuro tenemos que manejar otros estados, con `const [inputValues, setInputValues] = useState<FormState['inputValues']>({..})`



## Submit Event

Suponemos que desde la función padre le pasamos a `Form` una prop `onNewSub` con referencia a una función`handleNewSub` que utiliza el setter. 

```typescript
import React, { useState } from 'react';
import { Sub } from '../../types';

interface FormState {
  inputValues: Sub;
}

interface FormProps {
  onNewSub: (sub: Sub) => void;
}

const Form = ({ onNewSub }: FormProps) => {
  const [inputValues, setInputValues] = useState<FormState['inputValues']>({
    nick: '',
    subMonths: 0,
    avatar: '',
    description: '',
  });

  const handleChange = (evt: React.ChangeEvent<HTMLInputElement | HTMLTextAreaElement>) => {
    setInputValues({ ...inputValues, [evt.target.name]: evt.target.value });
  };

  const handleSubmit = (evt: React.FormEvent<HTMLFormElement>) => {
    evt.preventDefault();
    onNewSub(inputValues);
  };


  return (
    <form onSubmit={handleSubmit}>
      //...
      //...
      //...
      <button>Enviar</button>
    </form>
  );
};

export default Form;

```

En el componente padre (`Subs`) donde usamos a `Form`

```typescript
import React, { useEffect } from 'react';
import { Sub } from '../../types';
import Form from './Form';

const INITIAL_STATE = [
  //...
  //...
];

const Subs = () => {
  const [subs, setSubs] = React.useState<Sub[]>([]);

  useEffect(() => {
    setSubs(INITIAL_STATE);
  }, []);

  const handleNewSub = (newSub: Sub): void => {
    setSubs([...subs, newSub]);
  };

  return (
    <>
      <ul>
        {subs.map(sub => (
          <li key={sub.nick}>
            //...
            //...
          </li>
        ))}
      </ul>
      <Form onNewSub={handleNewSub} />
    </>
  );
};

export default Subs;

```

Nunca debemos pasarle el set state "hacia abajo" al componente hijo, porque el tipado se volverá más complejo de manera innecesaria. En caso de que lo hiciéramos nos ayudamos del hover sobre `onNewSub={setSubs}` para saber que el tipo de esta prop que será  `Dispatch<SetStateAction<Sub[]>` no olvidar agregarle `React.` a lo que copiamos: `React.Dispatch<React.SetStateAction<Sub[]>`



# :rotating_light: 

Luego en `handleSubmit` trabajaremos con `setSubs(subs => [..subs,inputValues])`. :rotating_light: **Notar que recibo como props sólo a `setSubs` y también accedo al valor del estado `subs`** 



# `type` vs `interface`

Es prácticamente indistinto utilizar `type` o `interface` para definir los tipos recibidos por Props. Sin embargo, hay quienes recomiendan usar `types` en aplicaciones y `interfaces` en bibliotecas. De los autores estudiados midudev y Goncy utilizan `interface` y vishwas `type`.

`interface` tiene la ventaja de que es extensible y además se puede poner dos veces la definición y se combinarán por lo que la mayoría de las veces será aconsejable utilizarlo en lugar de `type`.

Algunas personas a las interfaces les ponen una `I`mayúscula al comienzo esto es algo que viene de otros lenguajes con tipado pero está desaconsejado en TS.



# :tada:`useReducer`

Queremos trabajar con un reducer que tenga como acciones actualizar un campo modificado en un formulario y resetearlos todos luego de hacer submit.

Primero agregamos un botón Clear que al presionarlo invoque a `handleClear` que limpie todos los campos del formulario. A su vez invocamos este método al realizar el submit del formulario.

```
 const handleClear = () => {
    setInputValues({
      nick: '',
      subMonths: 0,
      avatar: '',
      description: '',
    });
  };
```

Como algo parecido también hacemos al inicializar la variable de estado, nos conviene utilizar una constante `INITIAL_STATE`.



Queremos refactorizar para utilizar un `useReducer`:

```typescript
import { useReducer, useState } from 'react';
import { Sub } from '../types';

const INITIAL_STATE = {
  nick: '',
  subMonths: 0,
  avatar: '',
  description: '',
};

interface FormProps {
  onNewSub: (sub: Sub) => void;
}

interface FormState {
  inputValues: Sub;
}

type FormReducerAction =
  | {
      type: 'change_value';
      payload: { inputName: string; inputValue: string };
    }
  | {
      type: 'clear_form';
    };

const formReducer = (state: FormState['inputValues'], action: FormReducerAction) => {
  switch (action.type) {
    case 'change_value':
      const { inputName, inputValue } = action.payload;
      return {
        ...state,
        [inputName]: inputValue,
      };
    case 'clear_form':
      return INITIAL_STATE;
  }
};

const Form = ({ onNewSub }: FormProps) => {
  // const [inputValues, setInputValues] = useState<FormState['inputValues']>(INITIAL_STATE);

  const [inputValues, dispatch] = useReducer(formReducer, INITIAL_STATE);

  const handleChange = (evt: React.ChangeEvent<HTMLInputElement | HTMLTextAreaElement>) => {
    // setInputValues({ ...inputValues, [evt.target.name]: evt.target.value });

    const { name, value } = evt.target;
    dispatch({
      type: 'change_value',
      payload: {
        inputName: name,
        inputValue: value,
      },
    });
  };

  const handleSubmit = (evt: React.FormEvent<HTMLFormElement>) => {
    evt.preventDefault();
    onNewSub(inputValues);
    handleClear();
  };

  const handleClear = () => {
    dispatch({ type: 'clear_form' });
  };

  console.log({ inputValues });

  return (
    <form onSubmit={handleSubmit}>
      <input onChange={handleChange} type='text' name='nick' placeholder='nick' value={inputValues.nick} />
      <input
        onChange={handleChange}
        type='number'
        name='subMonths'
        placeholder='subMonths'
        value={inputValues.subMonths}
      />
      <input onChange={handleChange} type='text' name='avatar' placeholder='avatar' value={inputValues.avatar} />
      <textarea onChange={handleChange} name='description' placeholder='description' value={inputValues.description} />
      <button onClick={handleClear} type='button'>
        Clear
      </button>
      <button type='submit'>Send</button>
    </form>
  );
};

export default Form;

```

> A la hora de definir `FormReducerAction` utilizamos `type` en lugar de `interface` pero por una cuestión de usar uno y el otro.
>
> En el switch no colocamos default ya que TypeScript debido a lo que hemos especificado en `FormReducerAction` nos ayudará a que sólo ingresemos las dos opciones aceptadas, pero bien podríamos poner `default: return state`

## :tada: Custom Hook

Creamos en la raíz una carpeta `hooks` y extramos  `formReducer` y todos los tipos y constantes que de él dependan en un custom hook `useNewSubForm`.

Ese archivo nos queda:

```typescript
import { useReducer } from 'react';
import { Sub } from '../types';

const INITIAL_STATE = {
  nick: '',
  subMonths: 0,
  avatar: '',
  description: '',
};
interface FormState {
  inputValues: Sub;
}

type FormReducerAction =
  | {
      type: 'change_value';
      payload: { inputName: string; inputValue: string };
    }
  | {
      type: 'clear_form';
    };

const formReducer = (state: FormState['inputValues'], action: FormReducerAction) => {
  switch (action.type) {
    case 'change_value':
      const { inputName, inputValue } = action.payload;
      return {
        ...state,
        [inputName]: inputValue,
      };
    case 'clear_form':
      return INITIAL_STATE;
  }
};
const useNewSubform = () => {
  return useReducer(formReducer, INITIAL_STATE);
};

export default useNewSubform;


```

Mientras que `Form.tsx` nos queda:

```typescript
import useNewSubform from '../hooks/useNewSubform';
import formReducer from '../hooks/useNewSubform';
import { Sub } from '../types';

const INITIAL_STATE = {
  nick: '',
  subMonths: 0,
  avatar: '',
  description: '',
};

interface FormProps {
  onNewSub: (sub: Sub) => void;
}

const Form = ({ onNewSub }: FormProps) => {
  // const [inputValues, setInputValues] = useState<FormState['inputValues']>(INITIAL_STATE);

  const [inputValues, dispatch] = useNewSubform();

  const handleChange = (evt: React.ChangeEvent<HTMLInputElement | HTMLTextAreaElement>) => {
    // setInputValues({ ...inputValues, [evt.target.name]: evt.target.value });

    const { name, value } = evt.target;
    dispatch({
      type: 'change_value',
      payload: {
        inputName: name,
        inputValue: value,
      },
    });
  };

  const handleSubmit = (evt: React.FormEvent<HTMLFormElement>) => {
    evt.preventDefault();
    onNewSub(inputValues);
    handleClear();
  };

  const handleClear = () => {
    dispatch({ type: 'clear_form' });
  };

  console.log({ inputValues });

  return (
    <form onSubmit={handleSubmit}>
      <input onChange={handleChange} type='text' name='nick' placeholder='nick' value={inputValues.nick} />
      <input
        onChange={handleChange}
        type='number'
        name='subMonths'
        placeholder='subMonths'
        value={inputValues.subMonths}
      />
      <input onChange={handleChange} type='text' name='avatar' placeholder='avatar' value={inputValues.avatar} />
      <textarea onChange={handleChange} name='description' placeholder='description' value={inputValues.description} />
      <button onClick={handleClear} type='button'>
        Clear
      </button>
      <button type='submit'>Send</button>
    </form>
  );
};

export default Form;
```



### Ocultar Implementación

La idea de utilizar custom hooks es no mostrar fuera de ellos qué implementación tienen. Por ello quizás en lugar de 



# Prop Types

## Props de tipo string, number, boolean

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

* En el componente en sí nos proporciona el **autocompletado** ya que ni bien escribimos `props.` nos sugerirá `name` y lo mismo en el componente padre nos ayudará a escribir las props apenas ingresemos la  primera letra.
* En el componente padre nos permitirá una rápida detección de errores si le pasamos un tipo incorrecto a `name`.



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



## Props de tipo Objeto

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



## Props de tipo array de objetos

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



Podríamos haber utilizado también la siguiente notación:

```typescript
type PersonListProps = {
    names: <Array{
        first: string,
        last: string
    }>
}
```



## Props de tipo unión de strings

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



## Props  `children`

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



En el componente padre tendremos `<Heading>Mensaje que queremos mostrar</Heading>`. 



En el caso anterior hemos utilizado como `children` un string pero también podría haber sido un componente React. Supongamos que tenemos un componente `Card` que queremos que reciba como `props.children` a un componente `Heading` con el mensaje a mostrar por el primero.

En `Card.tsx`

```tsx
type CardProps = {
	children: React.ReactNode
}
```

> Otra opción sería poner `children: JSX.Element` y aceptaría sólo componentes.
>
> `React.ReactNode` proviene de @types/react y si estamos utilizando React 17 en adelante no habrá necesidad de importar React.



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



## Props de eventos

Trabajaremos con dos de los eventos más usados click event de un `button` y change event de un elemento `input`.

### Click Event

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



### Change Event

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

## Props de estilos

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



# Exportar tipos

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



# Reutilizar tipos

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



# Tipos en el hook `useState`

Estudiaremos como establecer los tipos de los hooks, comenzando por el `useState` hook.

## `useState` con tipo inferido de valor inicial

El primer caso que consideramos será aquel en el cual el tipo es inferido en función del valor inicial. 

Para ello creamos un componente `LoggedIn.tsx`. En él tendremos un botón Login y uno Logout y debemos mostrar un mensaje de acuerdo a la variable de estado `isLoggedIn`.

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



### Tipo un poco más complejo

Suponemos ahora que queremos almacenar en una variable de estado `const [subs, setSubs]` los sucriptores a un canal. En primer lugar le asignamos como valor inicial un array con dos elementos.

```typescript
import React, { useState } from 'react';

type Props = {};

const Subs = (props: Props) => {
  const [subs, setSubs] = useState([
    {
      nick: 'juan',
      subMonths: 3,
      avatar: 'https://i.pravatar.cc/150?u=juan',
      description: 'he loves to code',
    },
    {
      nick: 'ocho',
      subMonths: 12,
      avatar: 'https://i.pravatar.cc/150?u=ocho',
    },
  ]);
  return (
    <ul>
      {subs.map(sub => (
        <li key={sub.nick}>
          <img src={sub.avatar} alt={sub.nick} />
          <p>{sub.description?.substring(0, 6)}</p>
        </li>
      ))}
    </ul>
  );
};

export default Subs;
```



# :bar_chart: pravatar.cc

Como avatar placeholder utilizamos el servicio https://pravatar.cc



En ese caso como en el valor inicial uno de los elementos del array tiene el campo `description` y el otro no lo tiene, si nos posicionamos sobre `subs` inferirá que su tipo es la unión de ambos tipos de objetos:

```typescript
const subs: ({
    nick: string;
    subMonths: number;
    avatar: string;
    description: string;
} | {
    nick: string;
    subMonths: number;
    avatar: string;
    description?: undefined;
})[]
```

Como `description` es opcional si queremos usar un método debemos utilizar *optional chaining* ya que podrá ser *undefined*:

```typescript
<h2>{sub.description?.substring(0, 6)}</h2>
```



Si queremos hacer lo mismo pero sin que tenga que inferir el tipo sino que utilizamos un tipo genérico mediante una interface `Subscriptor`.

```typescript
import React, { useState, useEffect } from 'react';

type Props = {};

interface Subscriptor {
  nick: string;
  subMonths: number;
  avatar: string;
  description?: string;
}

const INITIAL_STATE = [
  {
    nick: 'juan',
    subMonths: 3,
    avatar: 'https://i.pravatar.cc/150?u=juan',
    description: 'he loves to code and play with his dog',
  },
  {
    nick: 'ocho',
    subMonths: 12,
    avatar: 'https://i.pravatar.cc/150?u=ocho',
  },
];

const Subs = (props: Props) => {
  const [subs, setSubs] = useState<Subscriptor[]>([]);

  // useEffect(() => {
  //   setSubs(INITIAL_STATE);
  // }, []);

  return (
    <ul>
      {subs.map(sub => (
        <li key={sub.nick}>
          <img src={sub.avatar} alt={sub.nick} />
          <p>{sub.subMonths} months with us</p>
          <p>{sub.description?.substring(0, 20)}...</p>
        </li>
      ))}
    </ul>
  );
};

export default Subs;

```

Hemos puesto `const [subs, setSubs] = useState<Subscriptor[]>([]);` pero podríamos haber puesto también `const [subs, setSubs] = useState<Array<Subscriptor>>([]);`



### Manejo del Estado

Puede ser conveniente tener una interface que usemos para manejar el estado del componente:

```
interface AppState{
	subs: Array<Subscriptor>
	newSubsNumber: number
}
```

De modo tal que luego al definir las variables de estado tengamos:

```
const [subs, setSubs] = useState<AppState["subs"]>([]);
const [newSubsNumber, setNewSubsNumbers] = useState<AppState["newSubsNumber"]>(0)
```



## `useState` con tipo del valor inicial distinto del valor futuro

En ocasiones no conocemos el valor inicial de una variable de estado y la asignaremos luego. 

Supongamos que trabajamos con un componente `User.tsx` que cuenta también con dos botones de Login y Logut y queremos que al loguearnos se almacene en una variable de estado `user` los datos de un usuario de tipo `AuthUser`. 

Como inicialmente no disponemos de estos datos le damos valor `null`

 `const [user, setUser] = useState(null)` 

Sin embargo, debido a la inferencia de tipo, cuando el botón de login sea clickeado no nos dejará setear el usuario con como un objeto de tipo `AuthUser` es decir: `setUser({name:'juaneme8', email:'j8@example.com'})` sino que esperará que le pasemos `null`. 

En este caso no podremos confiar en la inferencia de tipo sino que tendremos que utilizar **tipos genéricos** indicándole que el tipo podrá ser `null` o de tipo `AuthUser` y esto lo hacemos con:

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



## `useState` Type Assertion

Anteriormente vimos que la posibilidad de que el valor sea `null` nos exige chequear siempre que no lo sea antes de acceder a las propiedades, es por eso que si  tenemos absoluta certeza de que `user` nunca será `null` (por ejemplo si se seteará en un efecto `useEffect` y no tenemos el método logout) podemos definir como valor inicial un objeto vacío como si fuera de tipo `AuthUser` y esto lo hacemos con el keyword `as`.

`const [user, setUser] = useState<AuthUser>({} as AuthUser)`

De esta manera podremos acceder a `name` y `email` sin chequear que sea `null` es decir sin el optional chainig operator `{user.name}`



# Tipos en el hook `useReducer` 

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

```typescript
type CounterAction = {
    type: string,
    payload: number
}
```

Esto a futuro puede traernos inconvenientes porque en lugar de "increment" y "decrement" podríamos poner "reset" sin que nos marque ningún error (a pesar de que esto lo atajaríamos con el `default` case del switch). Esto lo solucionamos utilizando template literals en lugar de `string`.

```typescript
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

```typescript
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



# Tipos en el hook `useContext`

A la hora de trabajar con TypeScript y React Context usando hooks trabajaremos con `useContext` para consumir el valor del contexto.

## `useContext` con tipo inferido de valor inicial

A los fines didácticos consideraremos el uso de React Context para proporcionar un tema a nuestros componentes usando Context API. Para ello asumimos que tenemos los archivos `theme.ts`, `Box.tsx` y `ThemeContext.tsx`

Queremos usar el tema como un contexto y usarlo para estilar el `div` dentro de `Box`



En primer en `theme.ts` exportamos un objeto `theme` que contiene la paleta de colores que queremos usar en nuestros componentes.

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



A continuación en `ThemeContext.tsx` (notar que el componente se llama `ThemeContextProvider`) creamos el contexto `ThemeContext` haciendo uso de `createContext` (esto sucede fuera del componente y quizás podamos recordarlo más fácil si vemos que se exporta para ser usado luego en `Box.tsx`).



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



Como podremos notar no hemos tenido que escribir ninguna línea específica de TypeScript en este componente.

En este caso simplifica mucho las cosas el hecho de que conocemos el valor del contexto al crearlo y este no cambia de tipo en el futuro, distinto sería si no lo conocemos inicialmente y lo seteamos luego.



## `useContext` con tipo del valor inicial distinto del valor futuro

* `UserContext.tsx` que es el encargado de manejar el estado de autenticación de un usuario. Contará con una variable de estado `user` (que valdrá null inicialmente y al loguearse tendrá el nombre de usuario y mail) y una función para loguearse. Recibirá como prop `children` un componente al cual le pasará este contexto.

  > El archivo se llama `UserContext.tsx`
  >
  > El contexto se llama `UserContext`
  >
  > El componente se llama `UserContextProvider` 

```tsx
import React, { useState, createContext } from 'react'

type AuthUser = {
  name: string
  email: string
}

type UserContextType = {
  user: AuthUser | null
  setUser: React.Dispatch<React.SetStateAction<AuthUser | null>>
}

type UserContextProviderProps = {
  children: React.ReactNode
}

// export const UserContext = createContext<UserContextType | null>(null)
export const UserContext = createContext({} as UserContextType)

export const UserContextProvider = ({ children }: UserContextProviderProps) => {
  const [user, setUser] = useState<AuthUser | null>(null)
  return (
    <UserContext.Provider value={{ user, setUser }}>
      {children}
    </UserContext.Provider>
  )
}
```



* En `App.tsx` envolvemos al componente `User` con `UserContextProvider`

```tsx
<UserContextProvider>
	<User />
</UserContextProvider>
```



* `User.tsx` contiene un botón Login, uno Logout y muestra el nombre y el email de un usuario logueado. Hacemos uso del contexto para setear que el usuario sea `null` o el valor deseado. Es aqui donde seteamos el valor futuro.

```tsx
import { useContext } from 'react'
import { UserContext } from './UserContext'

export const User = () => {
  const userContext = useContext(UserContext)
  const handleLogin = () => {
    // if (userContext) {
    userContext.setUser({
      name: 'Vishwas',
      email: 'asd@asd.com'
    })
    // }
  }
  const handleLogout = () => {
    // if (userContext) {
    userContext.setUser(null)
    // }
  }
  return (
    <div>
      <button onClick={handleLogin}>Login</button>
      <button onClick={handleLogout}>Logout</button>
      <div>User name is {userContext.user?.name}</div>
      <div>User email is {userContext.user?.email}</div>
      {/* <div>User name is {userContext?.user?.name}</div>
      <div>User email is {userContext?.user?.email}</div> */}
    </div>
  )
}
```

Notar que haciendo uso de **type assertion** `createContext({} as UserContextType)` no tenemos que chequear que sea null ni usar el optional chaining operator, cosa que sí sucedería si le decimos que puede ser `null`: `createContext<UserContextType | null>(null)`

La ventaja del uso del contexto está en que todos los componentes tendrán acceso al estado sin que tengan que recibirlo mediante props.



# :warning: TUTORIAL CODEVOLUTION INCOMPLETO



# Tipos en el hook `useRef`

Suponemos que queremos tener una referencia a un div, en ese caso comenzaríamos escribiendo algo así:

```
const divRef = useRef()
```



```
<div ref={divRef}>
	...
	...
	...
</div>
```



Como en la referencia guardaremos un div, colocamos `useRef<HTMLDivElement>()`, pero si ponemos solo esto nos dirá que **type undefined  is not assignable to type HTMLDivElement | null**, por lo tanto debemos darle un valor inicial a la referencia:

```
const divRef = useRef<HTMLDivElement>(null)
```

> Otra opción podría ser `const divRef = useRef<HTMLDivElement | undefined>()`



# Fetching de Datos

Inicialmente planteamos el fetching de la misma manera que siempre realizándolo dentro de un `useEffect`

```typescript
useEffect(()=> {
	fetch(http://localhost:3001/subs)
		.then(res => res.json())
		.then(subs => {
			setSubs(subs)
		})
},[])
```

Sin embargo, debemos tener presente que como TypeScript no funciona en *run-time* sino en *build-time*, si la API está devolviendo unas propiedades distintas a las que esperamos no obtendremos ningún error en pantalla pero la aplicación no funcionará del modo esperado. 
En este caso `subs` deberá ser un array de `Sub` y como lo que me está llegando no respeta ese contrato tendré problemas.

Si nos fijamos posicionando el mouse sobre  `res.json()` veremos que es de tipo `any`. Esto lo solucionamos de la siguiente manera:

```typescript
useEffect(()=> {
	const fetchSubs = ():Promise<SubResponseFromApi[]> => {
		return 
		fetch(http://localhost:3001/subs)
			.then(res => res.json())
	}
	
	fetchSubs()
		.then(subs => setSubs(subs))
	
},[])
```

Ahora sí, obtendremos un error en pantalla si hacemos esto.

```typescript
useEffect(()=> {
	const fetchSubs = ():Promise<SubResponseFromApi[]> => {
		return 
		fetch(http://localhost:3001/subs)
			.then(res => res.json())
	}
	const mapFromApiToSubs = (apiResponse: SubResponseFromApi[]):Array<Sub> => {
		return apiResponse.map(subFromApi => {
			const {
				nick,
				months: subMonths,
				profileUrl: avatar
				description
			}=subFromApi;
			
			return {
				nick, 
				subMonths, 
				avatar, 
				description
			}
		})
	}
	
	fetchSubs()
		.then(apiSubs => {
			 const subs = mapFromApiToSubs(apiSubs))
			 setSubs(subs))
		});
	
},[])
```

La separación en dos métodos facilita el testing a futuro, no necesariamente tendríamos que pegarle a la API real podría ser una data obtenida de local storage por ejemplo.

Lo mismo expresado de una manaera mucho más compacta:

```
fetchSubs()
	.then(mapFromApiToSubs)
	.then(setSubs)
		
```



* Otra opción podría ser utilizando `as`:

```typescript
useEffect(()=> {
	fetch(http://localhost:3001/subs)
		.then(res => res.json() as Promise<SubResponseFromApi[]>)
		.then(subs => {
			setSubs(subs)
		})
},[])
```



En `types.d.ts` además de `Sub` agregamos otro tipo `SubResponseFromApi`

```typescript
export interface Sub {
  nick: string
  subMonths: number
  avatar: string
  description?: string
}

export interface SubResponseFromApi {
	nick:string
	months: number
	profileUrl: string
	description: string
}
```

También podríamos haber puesto directamente en plural `SubsResponseFromApi` y que fuera una array:

```typescript
export type SubsResponseFromApi = Array<{
	nick:string
	months: number
	profileUrl: string
	description: string
}>
```



## Especificando tipo devuelto con  axios

Utilizando **axios** podremos especificar el dato devuelto por la API de otra manera:

```typescript

useEffect(()=> {
	const fetchSubs = () => {
		return 
		axios.get<SubResponseFromApi[]>("http://localhost:3001/subs")
			.then(res => res.data)
	}
	const mapFromApiToSubs = (apiResponse: SubResponseFromApi[]):Array<Sub> => {
		return apiResponse.map(subFromApi => {
			const {
				nick,
				months: subMonths,
				profileUrl: avatar
				description
			}=subFromApi;
			
			return {
				nick, 
				subMonths, 
				avatar, 
				description
			}
		})
	}
	
	fetchSubs()
		.then(apiSubs => {
			 const subs = mapFromApiToSubs(apiSubs))
			 setSubs(subs))
		});
	
},[])
```

Son dos formas de hacer lo mismo pero en la anterior no tenemos que ver la implementación y desde fuera de la función podemos obtener información y esto puede ser ventajoso en determinados casos.



## Consumo API

A continuación crearemos un ejemplo con TypeScript del consumo de una API con la estructura de archivos que tendría una aplicación real.

Utilizaremos en el ejemplo Punk API (https://punkapi.com/) que devuelve un listado de cervezas cuando le pegamos al endpoint https://api.punkapi.com/v2/beers.

En `src` creamos los archivos `types.ts` y `api.ts` 



En `types.ts`

```typescript
export interface Gift {
  text: string;
  owner: string;
  image: string;
  count: number;
}
```

En `api.ts` suponemos que queremos retornar los primeros tres elementos:

```typescript
import { Gift } from "./types";

export default {
  gifts: {
    list: async():Promise<Gift[]> => {
      const beers = await fetch("https://api.punkapi.com/v2/beers").then(res => res.json());
      
      return beers.slice(0,3).map((beer:any) => ({
        id: beer.id,
          text: beer.name,
          image: beer.image_url,
          count: beer.abv,
          owner: "Juancho",
      }));
    }
    
  }
}
```



> En caso de utilizar LocalStorage para guardar los elementos evaluar la posibilidad de usar un Set para evitar duplicados



En `App.tsx`

```typescript
import './App.css'
import api from './api'
import { useEffect, useState } from 'react'
import { Gift } from "./types";

function App() {
  const [gifts, setGifts] = useState<Gift[]>([])
  
  useEffect(() => {
    api.gifts.list().then(gifts=> setGifts(gifts))
  }, [])


  console.log(gifts)

  return (
    <>
      {gifts.length && gifts.map(gift => (
        <div key={gift.id}>
          <img src={gift.image} alt={gift.text} />
          <p>{gift.text}</p>
          <p>{gift.owner}</p>
          <p>{gift.count}</p>
        </div>
      ))}
    </>
  );
}

export default App

```



# Repositorio Recomendado

https://github.com/typescript-cheatsheets/react

Encontraremos un montón de casos de uso de React y TypeScript, como ser diferencia entre `interface` o `type`
