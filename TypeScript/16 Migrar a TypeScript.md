# Migrar aplicación Next.js a TypeScript

> Utilizamos como base el siguiente repositorio: 
>
> https://github.com/emiacerbi/basic-todo-list

* Instalar las dependencias y los tipos necesarios:

```typescript
npm install typescript @types/react @types/node --save-dev
```

* Renombrar`_app.js` a `_app.tsx` y ejecutamos `npm run dev`. Por estar utilizando Next.js nos dirá que detectó TypeScript y nos creo el archivo `tsconfig.json`. 

  > Para que la migración sea un poco más amena es aconsejable tener en `tsconfig.json` el campo `strict: false`. Sin embargo, con esta configuración si tenemos un campo que esperamos un booleano y colocamos `null` no nos arrojará ningún error.

La aplicación funcionará de la misma manera y si en `_app.tsx` hacemos hover en `pageProps` veremos que es de tipo `any`.

```typescript
function MyApp ({Component, pageProps}){
	...
}
```

Indicamos que el tipo recibido por `MyApp` es `AppProps` que importamos de `next/app`: `import type {AppProps} from next/app`.

> Al haberle puesto `type` no formará parte del código de producción.



```typescript
function MyApp ({Component, pageProps}:AppProps)
```

> `pageProps` seguirá siendo `any` dado que son las props que puede recibir cualquier página.



* Renombrar `index.js` a `index.tsx`



* Crear un archivo `types.ts` en la raíz y en el exportar las interfaces que necesitemos, por ejemplo una llamada  `Todo`.

```
interface Todo{
	
}
```

Modificar donde definimos los estados con `useState` de acuerdo al tipo de dato que sabemos que vamos a almacenar en esa variable.

```typescript
import data from '../utilities.data.json'

const [todoList, setTodoList] = useState<Todo[]>(data)
```



* Especificar el tipo del evento recibido en los handlers. 

   `handleChange` indicando que es de tipo `React.ChangeElement<HTMLInputElement>`

  `handleClick` indicando que es de tipo `React.MOuseEvent<HTMLButtonElement>`

Se verá así:

```typescript
const handleChange = (e:React.ChangeElement<HTMLInputElement>) => {
    setInputValue(e.target.value)
  }
```

La ventaja de esto será que cuando escribamos `e.target` nos sugerirá las propiedades reales que disponemos.



En el componente `TodoList` recibimos las siguientes props:

```
const TodoList = ({todoList, toggleComplete}) => {
```



Si nos fijamos en el componente padre las características de `toggleComplete` veremos:

```typescript
const toggleComplete = (id) => {
    return setTodoList(prevTodoList => prevTodoList.map(todo => todo.id === id ? { ...todo, complete: !todo.complete } : todo))
  }

```



A la hora de definir la interface de las Props:

```
interface TodoListProps{
	todoList: Todo[];
	toggleComplete: (id:number)=> void;
}
```

> Diremos que `toggleComplete` retorna void porque no nos interesa lo que retorna ya que la función no lo va a utilizar.