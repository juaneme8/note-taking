# Migración a TypeScript

## de Next.js a TypeScript

> Basado en el [stream de Goncy](https://youtu.be/DgXjfdadHXA)
>
> Utilizamos como base el siguiente repositorio: 
>
> https://github.com/emiacerbi/basic-todo-list

* Instalar las dependencias y los tipos necesarios:

```typescript
npm install typescript @types/react @types/node --save-dev
```

> Gracias a que instalamos por ejemplo los tipos de react al hacer hover sobre `useEffect` tendremos datos sobre los parámetros que recibe, devuelve, etc sino sería de tipo `any` o typescript nos diría que debemos instalarlos. En algunos paquetes los tipos ya vienen incluídos en la librería como es el caso de Chakra-UI.

* Renombrar`_app.js` a `_app.tsx` y ejecutamos `npm run dev`. Por estar utilizando Next.js nos dirá que detectó TypeScript y nos creo el archivo `tsconfig.json`. 

  > Para que la migración sea un poco más amena es aconsejable tener en `tsconfig.json` el campo `strict: false`. Sin embargo, con esta configuración si tenemos un campo que esperamos un booleano y colocamos `null` no nos arrojará ningún error. **Si en cambio vamos a comenzar un proyecto de cero lo aconsejable es hacerlo con `strict: true`**. En ese caso no compilará cuando encuentre algún tipo de dato que es `any` hasta que definamos el tipo correcto.

La aplicación funcionará de la misma manera y si en `_app.tsx` hacemos hover en `pageProps` veremos que es de tipo `any`.

```typescript
function MyApp ({Component, pageProps}){
	...
}
```

Indicamos que el tipo recibido por `MyApp` es `AppProps` que importamos de `next/app`: `import type {AppProps} from next/app`.

> Al haberle puesto `type` no formará parte del bundle final al compilar de TypeScript a JavaScript, por lo que ni siquiera el compilador lo analizará. Si no se lo pusiéramos habría un gasto de tiempo hasta que el compilador detecte que se trata de tipos.



```typescript
function MyApp ({Component, pageProps}:AppProps)
```

> `pageProps` seguirá siendo `any` dado que son las props que puede recibir cualquier página.



* Renombrar `index.js` a `index.tsx`



* Crear un archivo `types.ts` en la raíz y en el exportar las interfaces que necesitemos, por ejemplo una llamada  `Todo`.

```typescript
interface Todo{
	
}
```

Modificar donde definimos los estados con `useState` de acuerdo al tipo de dato que sabemos que vamos a almacenar en esa variable.

```typescript
import data from '../utilities.data.json'

const [todoList, setTodoList] = useState<Todo[]>(data)
```



### Tipos de Eventos

Especificar el tipo del evento recibido en los handlers. 

`handleChange` indicando que es de tipo `React.ChangeElement<HTMLInputElement>`

`handleClick` indicando que es de tipo `React.MOuseEvent<HTMLButtonElement>`

Se verá así:

```typescript
const handleChange = (e:React.ChangeElement<HTMLInputElement>) => {
    setInputValue(e.target.value)
  }
```

La ventaja de esto será que cuando escribamos `e.target` nos sugerirá las propiedades reales que disponemos.



### Simplificación de tipos

Supongamos que tenemos un componente con las siguientes características:

```typescript
const TodoList = ({todoList, toggleComplete}) => {
	...
```



Como podemos ver recibe una función `toggleComlete` como props, por lo que nos proponemos definir sus tipos mediante una interface.

Si nos fijamos en el componente padre las características de `toggleComplete` veremos:

```typescript
const toggleComplete = (id) => {
    return setTodoList(prevTodoList => prevTodoList.map(todo => todo.id === id ? { ...todo, complete: !todo.complete } : todo))
  }

```

Si bien vemos que tiene un `return` a la hora de definir la interface diremos que retorna `void` porque no nos interesa lo que retorna ya que la función no lo va a utilizar.

```typescript
interface TodoListProps{
	todoList: Todo[];
	toggleComplete: (id:number)=> void;
}
```



### Union Types vs Enums

Supongamos que tenemos una variable de estado `status` que no queremos que tome cualquier string sino que solo pueda tomar los valores `init`, `resolved` y `rejected`. Esto tenemos dos formas de tiparlo. Una es mediante **union types** y otra es mediante el uso de **enums**.

> [Interesante artículo](https://blog.bam.tech/developer-news/should-you-use-enums-or-union-types-in-typescript) que compara ambos enfoques y termina recomendando union types.



* Con **enum types**

```
const [status, setStatus] = useState<"init"|"resolved"|"rejected">("init")
```



* Con **enums**

Fuera del componente definimos el enum.

```typescript
enum Status = {
	Init = "init",
	Resolved = "resolved",
	Rejected = "rejected"
}
```

```typescript
const [status, setStatus] = useState<Status>(Status.Init)
const [todoList, setTodoList] = useState<Todo[]>([])

useEffect(()=> {
	setTodoList(data);
	setStatus(Init.Resolved)
},[])

if(status=== Status.Init){
	return "Cargando..."
}

return (
	//
)
```



## de Vite.js a TypeScript

Segun la documentación de Vite soporta TypeScript out of the box, por lo que no debemos realizar nada en particular:

```
npm install typescript @types/react
```

> Notar que en este caso no necesitamos instalar `@types/node`.



> Basado en el repositorio [FourtyFourChallenge](https://github.com/matias3981/FourtyFourChallenge)