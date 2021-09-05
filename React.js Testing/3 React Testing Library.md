# React Testing Library
> Basada en [videos](https://www.youtube.com/watch?v=7dTTFW7yACQ) de The Net Ninja. ==AGREGAR PLAYLIST==

## Introducción
React Testing Library como su nombre lo indica nos permite testear aplicaciones React.
[Documentación](https://testing-library.com/docs/react-testing-library/intro/)

## Motivación
Si bien el desarrollo de los tests puede llevarnos incluso más tiempo que el desarrollo de la *feature* que estamos testeando, hay varias razones que nos motivan a hacerlo:

* **Detección de bugs de manera rápida**. 
Cuando hayamos escrito tests para cada parte de la aplicación, cuando estemos desarrollando una nueva funcionalidad podremos tener la certeza de que no rompa otros componentes de la aplicación.

* **Aumenta la confianza en la aplicación**
Decimos que aumenta la confianza en la aplicación ya que si supera todos los tests, esto implica que todas los *features* de la app están funcionando bien.

* **Acelera el proceso de QA**
En los procesos de **Quality assurance** (**QA**) en caso de disponer de tests para cada funcionalidad, no habrá que testearlas manualmente.

* **Puede servir como documentación**.
En ocasiones el analisis de los tests nos permite tener la certeza del propósito de la aplicación.

## Primer Test
En caso de utilizar **Create React App** veremos en `package.json` que RTL viene instalado como dependencia de desarrollo por defecto, en caso de no utilizar dicha herramienta debemos instalar RTL con `npm install --save-dev @testing-library/react`.

Además veremos que tenemos el script `"test":"react-scripts test"` por lo que con `npm run test` podremos ejecutar los tests. Luego de ejecutar ese comando veremos un menu en la terminal que que nos dirá que presionemos `a` para ejecutar todos los tests alojados en archivos que tengan en su nombre la estructura `Nombre.test.js`. Por ejemplo `App.test.js` que viene con un test inicial que revisa que el componente `App`  tenga un elemento con el texto "learn react":

```jsx
import {render, screen} from @testing-library/react
import App from './App';

test('descripción del test', ()=>{
	render(<App/>)
	cont linkElement = screen.getByText(/learn react/i);
	expect(linkElement).toBeInTheDocument();
});
```

> Una vez que ejecutemos los tests, la terminal quedará en modo *watch* a la espera de cambios para volver a correr los tests.

## Estructura de un test
De acuerdo a lo que vimos en `App.test.js` la estructura de un test está compuesta por el **Bloque del test** y para lograrlo podremos utilizar  `test()` o `it()`
```jsx
test('descripción del test', ()=>{

});
```
```jsx
it('descripción del test', ()=>{
	
});
```

Dentro del bloque tendremos que llevar a cabo las siguientes tareas:
1. Renderizar el componente que queremos testear en el Virtual DOM de ese test. Esto lo hacemos con el método `render()` y el componente a testear se lo pasamos como argumento `render(<App/>)`. 

2. Buscar los elementos con los que queremos interactuar: Por ejemplo si queremos buscar un elemento con el texto "learn react" lo haremos de esta forma: `cont linkElement = screen.getByText(/learn react/i);` 
3. Interactuar con esos elementos. Esto incluirá introducir texto o hacer click en ellos.
4. Afirmar (*assert*) que los resultados sean los esperados con el método `expect()` por ejemplo `expect(linkElement).toBeInTheDocument();`
> Notar que no lo importamos este método ya que es interpretado por Jest que es la librería que estamos usando con RTL.

> Tanto el método `render()` como el objeto `screen` los importamos de `import {render, screen} from @testing-library/react`


## Query Methods
Así como usamos `screen.getByText(/learn react/i);` tenemos muchos otros métodos que podemos utilizar. Bastará con poner `scren.` para que el Intellisence de Visual Studio Code nos muestre todos los métodos disponibles.

### `getBy...`
Los métodos que comienzan con `getBy`  nos permiten consultar por elementos de acuerdo al valor de un atributo, por ejemplo `getByLabelText`, `getByText` nos entrega **el** elemento que matchea con la condición especificada.
En caso de matchear con más de un elemento tira **error** por lo que el test fallará.
En caso de no matchear con ningún elemento también tira **error**.

### `getAllBy...`
Los métodos que comienzan con `getAllBy` también nos permiten consultar por elementos de acuerdo al valor de un atributo pero al matchear con uno o mas elementos devuelven un **array**. 
En caso de no matchear con ningún elemento tira **error**.

> De manera similar tenemos métodos con `find` y `query`. En la mayoría de los casos utilizaremos los métodos de `get` mientras que en casos específicos usaremos los de `find` (para tareas asincrónicas) y `query` (paa testear ausencia de valor).

### `findBy...`
Idem `getBy` pero debemos utilizarlo con `await` cuando estemos realizando tareas asincrónicas.

### `findAllBy...`
Idem `getAllBy` pero debemos utilizarlo con `async` y `await`. Es ideal para cuando estemos realizando tareas asincrónicas.

### `queryBy...`
idem `getBy` pero retorna `null` (en lugar de error) en caso de no matchear con ningún elemento. Esto es importante a la hora de testear la ausencia de valor de un elemento.

### `queryAllBy...`
Idem `getAllBy` pero retorna un array vacío (en lugar de error) en caso de no matchear con ningún elemento.

### Priority
La mayoría de las veces podremos acceder a un elemento por más de un atributo asociado a los métodos  `get`, `find` y `query`. Sin embargo, a la hora de elegir *la porción de atributo del método* tendremos en cuenta que como queremos imitar el comportamiento del usuario lo ideal será usar siempre que sea posible aquellos que son **accesibles por todos** como `getByRole` (podremos especificar el tipo de elemento y su texto), `getByLabelText`, `getByPlaceholderText`, `getByText`. En segundo lugar los **atributos semánticos** que si bien el usuario no los verá pueden ser leídos por los *screen readers* como `getByAltText`, `getByTitle` y como última alternativa podemos usar `getByTestId`.  

# Test de Componente
Supongamos que tenemos un componente bien simple que recibe `title` como props y lo muestra en un `<h1>`.
Para ello tenemos un directorio `Header`que dentro tiene un archivo `Header.js` otro `Header.css`. La convención es crear dentro otra carpeta `__test__`y en ella el archivo `Header.test.js`

El componente `Header` tiene el siguiente código:
```jsx
import React from 'react'
import "./Header.css"

export default function Header({title}) 
{
    return <h1 className="header">{title}</h1>
}
```
Ya en `Header.test.js` tenemos distintas formas de crear un test unitario del componente `Header`.

La primera forma es utilizando `getByText()`:
```jsx
it('should render same text passed into title prop', () => {
    render(<Header title="todo"/>);
    const h1Element = screen.getByText(/todo/i);
    expect(h1Element).toBeInTheDocument();
});
```

> Estamos utilizando una RegEx pero podríamos poner directamente también un string.

Utilizando `getByRole`:
```jsx
it('should render same text passed into title prop', () => {
	render(<Header title="todo"/>);
	const h1Element = screen.getByRole("heading");
	expect(h1Element).toBeInTheDocument();
});
```

Si en lugar de sólo un `h1` tuviéramos más de un encabezado, el test fallaría. En ese caso agregamos como segundo parámetro de `getByRole()` un objeto de opciones, con la propiedad `name` con el contenido del texto del *heading* que quiero seleccionar. Esta es una implementación muy buena ya que imita lo que el usuario va a querer realizar, es decir buscar un encabezado y luego comparar con el texto.
```jsx
// getByRole
it('should render same text passed into title prop', () => {
	render(<Header title="todo"/>);
    const h1Element = screen.getByRole("heading", { name: /todo/i });
    expect(h1Element).toBeInTheDocument();
});
```

Para utilizar las alternativas semánticas, suponemos que en el componente tenemos `<h1 title="Header">{title}</h1>` y aunque el usuario ignore de la existencia del title, podemos hacer la *query* en base a el. 
```jsx
// getByTitle
it('should render same text passed into title prop', () => {	
	render(<Header title="todo"/>);
	const h1Element = screen.getByTitle("Header");
	expect(h1Element).toBeInTheDocument();
});
```

Como última alternativa podemos testear por `testId` para ello tenemos que contar con un *data attribute* en el componente, por ejemplo: `<h1 className="header" data-testid="header-1">{title}</h1>`
```jsx
// getByTestId
it('should render same text passed into title prop', () => {
	render(<Header title="todo"/>);
	const h1Element = screen.getByTestId("header-1");
	expect(h1Element).toBeInTheDocument();
});
```

Notar que `findBy` funciona sólo con `async` y `await`, de lo contrario falla el test.
```jsx
// findByText
it('should render same text passed into title prop', async () => {
render(<Header title="todo"/>);
const h1Element = await screen.findByText(/todo/i);
	expect(h1Element).toBeInTheDocument();
});
```
Como dijimos anteriormente `queryBy` lo utilizamos cuando queremos testear que no haya un elemento de tal tipo en la pantalla.
```jsx
// queryByText
it('should render same text passed into title prop', () => {
	render(<Header title="todo"/>);
	const h1Element = screen.queryByText(/dogs/i);
	expect(h1Element).not.toBeInTheDocument
// });
```
Si en lugar de la línea `const h1Element = screen.queryByText(/dogs/i);` tuviéramos `const h1Element = screen.getByText(/dogs/i);` el test fallaría en esa línea simplemente porque `getByText` no matchea con ningún elemento.

Si queremos asegurarnos tener cierta cantidad de elementos de tipo `heading` podemos utilizar `getAllByText` que retorna una array y luego en la parte de *assertion* utilizamos `expect(h1Elements.length).toBe(1);` 
```jsx
// getAllByText
it('should render same text passed into title prop', () => {
	render(<Header title="My Header"/>);
	const headingElements= screen.getAllByRole("heading");
	expect(headingElements.length).toBe(2);
});
```

```jsx
// getAllByText
it('should render same text passed into title prop', () => {
	render(<Header title="todo"/>);
	const h1Elements = screen.getAllByText(/todo/i);
	expect(h1Elements.length).toBe(1);
});
```

# Tests con Librerías Externas
Supongamos que ahora queremos testear un componente `TodoFooter.js` que utiliza `Link` de React Router DOM para navegar a otra sección:

```jsx
import React from 'react'
import "./TodoFooter.css"
import { Link } from "react-router-dom"

function TodoFooter({
    numberOfIncompleteTasks
}) {
    return (
        <div className="todo-footer">
            <p>{numberOfIncompleteTasks} {numberOfIncompleteTasks === 1 ? "task" : "tasks"} left</p>
            <Link to="/followers">Followers</Link>
        </div>
    )
}

export default TodoFooter
```
Este componente recibe una prop `numberOfIncompleteTasks` y muestra en pantalla **# tasks left**.

Creamos la carpeta `__test__` y un archivo  `TodoFooter.test.js`
```jsx
import { render, screen } from '@testing-library/react';
import TodoFooter from "../TodoFooter"

it('should render the correct amount of incomplete tasks', () => {
    render(
        <TodoFooter 
          numberOfIncompleteTasks={5}
        />
    );
    const pElement = screen.getByText(/5 tasks left/i);
    expect(pElement).toBeInTheDocument();
});
```

Al ejecutar `npm run test` veremos que el test falla y nos muestra el mensaje 
`[Error: Invariant failed: You should not use <Link> outside a <Router>]`.
Como podemos ver en `index.js` todos los componentes están wrapeados por `<BrowserRouter>` lo que nos permite utilizar `<Link>` en ellos, pero en el test estamos testeando al componente completamente aislado.

Para solucionarlo creamos un componente que wrapea a `TodoFooter` con `BrowserRouter`. 

```jsx
import { render, screen } from '@testing-library/react';
import TodoFooter from "../TodoFooter"
import { BrowserRouter } from "react-router-dom"

const MockTodoFooter = ({ numberOfIncompleteTasks }) => {
    return (
        <BrowserRouter>
          <TodoFooter 
            numberOfIncompleteTasks={numberOfIncompleteTasks}
          />
        </BrowserRouter>
    )
}

it('should render the correct amount of incomplete tasks', () => {
    render(
        <MockTodoFooter 
          numberOfIncompleteTasks={5}
        />
    );
    const pElement = screen.getByText(/5 tasks left/i);
    expect(pElement).toBeInTheDocument();
});
```

Si queremos también testear que el componente funcione bien en singular (cuando tengo una sóla tarea) podría agregar otro test:
```jsx
it('should render correct text content', () => {
  render(
      <MockTodoFooter 
        numberOfIncompleteTasks={1}
      />
  );
  const pElement = screen.getByText(/1 task left/i);
  expect(pElement).toHaveTextContent("1 task left");
});
```

# Assertions
Hasta ahora estamos trabajando con el método `toBeInTheDocument()`, pero existen muchos otros que nos permiten hacer el proceso de *assertion*. 

```jsx
const pElement = screen.getByText(/5 tasks left/i);
expect(pElement).toBeInTheDocument();
```

Bastará con escribir `expect(pElement).` para que el Intellisence nos sugiera todos ellos: `toBe()`, `toBeCalledWith()`, `toBeFalsy()`, `toBeTruthy()`, `toBeInvalid()`, `toBeLessThan()`.
Luego de elegir uno podremos posicionar el mouse encima para obtener más información sobre cuándo usarlo.

Por ejemplo podríamos realizar el mismo test utilizando `toBeTruthy()`
```jsx
it('p element should be truthy when the number of incomplete tasks is one', () => {
  render(
      <MockTodoFooter 
        numberOfIncompleteTasks={1}
      />
  );
  const pElement = screen.getByText(/1 task left/i);
  expect(pElement).toBeTruthy();
});
```

El método `toBeVisible()` es muy interesante porque puede que un elemento esté en el documento (por lo que `toBeInTheDocument()` pasará el test) pero no sea visible (por ejemplo si tengo `style={{opacity:0}}`, por lo que con `toBeVisible()` no pasaremos el test.

```jsx
it('"task" should be visible when the number of incomplete tasks is one', () => {
  render(
      <MockTodoFooter 
        numberOfIncompleteTasks={1}
      />
  );
  const pElement = screen.getByText(/1 task left/i);
  expect(pElement).toBeVisible();
});
```
Con `toContainHTML()`
```jsx
it('should contain p tag with correct text', () => {
  render(
      <MockTodoFooter 
        numberOfIncompleteTasks={1}
      />
  );
  const pElement = screen.getByText(/1 task left/i);
  expect(pElement).toContainHTML('p');
});
```
Con `toHaveTextContent()`
```jsx
it('should render correct text content', () => {
  render(
      <MockTodoFooter 
        numberOfIncompleteTasks={1}
      />
  );
  const pElement = screen.getByText(/1 task left/i);
  expect(pElement).toHaveTextContent("1 task left");
});
```
El método `not()` nos permite obtener un comportamiento opuesto a la afirmación que hemos realizado. Por ejemplo el uso de `not()` junto con `toBeFalsy()` será equivalente a usar `toBeTruthy()`
```jsx
it('should render correct text content', () => {
  render(
      <MockTodoFooter 
        numberOfIncompleteTasks={1}
      />
  );
  const pElement = screen.getByText(/1 task left/i);
  expect(pElement).not.toBeFalsy();
});
```
También podemos trabajar con los atributos del elemento, por ejemplo en el caso del texto dentro de un párrafo podremos usar  `textContent`, `value` si se tratara de un input, etc. Esto lo acompañamos con el método `toBe()`
```jsx
it('should render correct text content', () => {
  render(
      <MockTodoFooter 
        numberOfIncompleteTasks={1}
      />
  );
  const pElement = screen.getByText(/1 task left/i);
  expect(pElement.textContent).toBe("1 task left");
});
```

> Si bien es posible tener más de un assert por test, lo ideal es tener uno sólo así es posible aislar rápidamente donde está el error dentro del test.

# Describe Blocks
Los **describe blocks** o bloques de descripción nos permiten organizar mejor los tests, colocando juntos aquellos tests que están relacionados.

Por ejemplo en `TodoFooter.test.js` podemos crear un `describe`

```jsx
describe("TodoFooter",() => {
	if("test 1", ()=>{
	
	});
	if("test 2", ()=>{
	
	});
});
```

No sólo es posible tener un describe block y dentro los tests sino que si deseamos más separación también es posible tener un describe block padre y otros describe blocks hijos siendo estos últimos los que tienen los tests.
```jsx
describe("TodoFooter",() => {
	describe("funcionalidad 1",() => {
		if("descripción del test 1", ()=>{
		
		});
		if("descripción del test 2", ()=>{
		
		});
	});
	describe("funcionalidad 2",() => {
		if("descripción del test  3", ()=>{
		
		});
		if("descripción del test 4", ()=>{
		
		});
	});
});
```

# Fire Events
Hasta ahora hemos aprendido como obtener elementos, pero la mayoría de las veces vamos a querer interactuar o realizar una acción en ellos. Por ejemplo ingresar texto en un input, presionar un botón, hacer click en una tarea para marcarla como completada, etc.

Supongamos que queremos testear el componente `AddInput` que consiste en un input en el cual ingresamos la tarea y un botón **Add** con el cual la agregamos a la lista.

Como variables de estado tenemos a `todo` y `setTodo`. En la medida que ingresamos caracteres al input (`onChange`) actualizamos el valor de `todo`.

Como props recibimos `todos` y `setTodos`. Cuando presionamos el botón Add con `addTodo()` agregamos un elemento al array `todos` con `setTodos` y limpiamos `todo` para el próximo ingreso.

```jsx
import React, { useState } from 'react'
import "./AddInput.css"
import { v4 } from "uuid"
import TodoList from '../TodoList/TodoList'

function AddInput({setTodos, todos}) {

    const [todo, setTodo] = useState("")

    const addTodo = () => {
        let updatedTodos = [
            ...todos,
            {
                id: v4(),
                task: todo,
                completed: false
            }
        ]
        setTodos(updatedTodos);
        setTodo("")
    }

    return (
        <div className="input-container">
            <input 
                className="input" 
                value={todo} 
                onChange={(e) => setTodo(e.target.value)}
                placeholder="Add a new task here..."
            />
            <button className="add-btn" onClick={addTodo}>
                Add
            </button>
        </div>
    )
}

export default AddInput

```

En `AddInput.test.js` debemos renderizar lógicamente `AddInput` y este como dijimos recibe como props `todos` y `setTodos`.

En cuanto a `todos` le podemos pasar un array vacío

Si bien sabemos de analizar `Todo.js` (el componente que usa `AddInput`) que `setTodos` es un hook `const [todos, setTodos]=useState([]);` como no se trata de un test de integración y nos interesa `AddInput` de manera aislada, no nos importa lo que sucede en este componente padre. Es por eso que podemos poner `setTodos={()=>{}}` es decir una función que no hace nada. Sin embargo, un método mejor es mockearlo usando `const mockedSetTodo = jest.fn()` y luego `setTodos={mockedSetTodo}`

> Recordar que estamos usando la librería RTL que utiliza la librería Jest

En primer lugar lo que hacemos será testear que el Input tenga un placeholder que matchee con la RegEx `/Add a new task here.../i`
```jsx
import { render, screen} from '@testing-library/react';
import AddInput from "../AddInput"

const mockedSetTodo = jest.fn();

it('should render input element', () => {
    render(
        <AddInput 
            todos={[]}
            setTodos={mockedSetTodo}
        />
    );
    const inputElement = screen.getByPlaceholderText(/Add a new task here.../i);
    expect(inputElement).toBeInTheDocument();
});
```

A continuación queremos revisar que cuando ingresamos texto, este aparezca dentro del input.
Importamos `fireEvent` y luego si ponemos `fireEvent.` veremos todos los eventos disponibles.
En nuestro caso vamos a utilizar el evento de `change()` para cambiar el valor del input, debemos pasarle como primer parámetro el elemento y como segundo un objeto de opciones indicando aquello que queremos cambiar que será el `target` y dentro del `target` el `value`: `{ target: { value: "Go Grocery Shopping" } }`. Luego hacemos el assert verificando que `inputElement.value` sea lo que acabamos de poner.
```jsx
import { render, screen, fireEvent } from '@testing-library/react';
import AddInput from "../AddInput"

it('should be able to type into input', () => {
    render(
        <AddInput 
            todos={[]}
            setTodos={mockedSetTodo}
        />
    );
    const inputElement = screen.getByPlaceholderText(/Add a new task here.../i);
    fireEvent.click(inputElement)
    fireEvent.change(inputElement, { target: { value: "Go Grocery Shopping" } })
    expect(inputElement.value).toBe("Go Grocery Shopping");
});
```

Además queremos que al hacer click en **Add** el input se vacíe, para ello usamos `getByRole` para localizar el botón y luego `fireEvent.click()`
```jsx
it('should have empty input when add button is cliked', () => {
    render(
        <AddInput 
            todos={[]}
            setTodos={mockedSetTodo}
        />
    );
    const inputElement = screen.getByPlaceholderText(/Add a new task here.../i);
    fireEvent.change(inputElement, { target: { value: "Go Grocery Shopping" } });
    const buttonElement = screen.getByRole("button", { name: /Add/i});
    fireEvent.click(buttonElement)
    expect(inputElement.value).toBe("")
});
```

> Como tenemos un sólo botón en vez de `const buttonElement = screen.getByRole("button",{name: /Add/i});` podríamos haber puesto `const buttonElement = screen.getByRole("button");`