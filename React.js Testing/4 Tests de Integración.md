# Tests de Integración
Los tests de integración nos permiten chequear como dos o mas componentes interactúan entre sí.
Por ejemplo en el caso de la TODO App luego de ingresar un texto al input y presionar el botón Add, este texto deberá aparecer en el componente listado.
Vamos a trabajar con el componente padre `Todo` que es quien muestra `AddInput` y `TodoList`. De manera similar a lo que hacemos para los tests unitarios, creamos una carpeta `__tests__` y en ella un archivo `Todo.test.js`.

Comenzamos con la estructura básica y vemos que el test es aprobado:
```jsx
import { render } from '@testing-library/react';
import Todo from "../Todo"

describe("Todo", () => {
	it("test description", () => {
	
	});
});
```
Sin embargo, ni bien agregamos la renderización de `Todo` falla y nos muestra el mensaje de error `You should not use <Link> outside a <Router>`

```jsx
import { render } from '@testing-library/react';
import Todo from "../Todo"

describe("Todo", () => {
	it("test description", () => {
		render(<Todo />);
	});
});
```

Si bien en este componente no usamos `<Link>` estamos usando `TodoList` que usa `TodoFooter` que sí usa `<Link>`. Es por esto que debemos crear un componente `MockTodo` que wrapee a `Todo` dentro de `<BrowserRouter>`

```jsx
import { render } from '@testing-library/react';
import Todo from "../Todo"
import { BrowserRouter } from "react-router-dom"

const MockTodo = () => {
    return (
        <BrowserRouter>
          <Todo/>
        </BrowserRouter>
    )
}

describe("Todo", () => {
	it("test description", () => {
		render(<MockTodo />);
	});
});
```

Para acceder al elemento Input trabajaremos con `screen.getByPlaceHolderText();` y para acceder al Input `screen.getByRole("button", {name: /Add/i}`
En la lista de tareas insertaremos la tarea nueva en un elemento `div` que lo buscamos de acuerdo al texto insertado con `screen.getByText()`

```jsx
describe("Todo", () => {
	it("should add the inserted text in the list", () => {
        render(<MockTodo />);
        const inputElement = screen.getByPlaceholderText(/Add a new task here.../i);
        const buttonElement = screen.getByRole("button", { name: /Add/i} );
        fireEvent.change(inputElement, { target: { value: "Go Grocery Shopping" } });
        fireEvent.click(buttonElement);
        const divElement = screen.getByText(/Go Grocery Shopping/i);
        expect(divElement).toBeInTheDocument();
    });
});
```

> La línea `const divElement = screen.getByText(/Go Grocery Shopping/i)` es una especie de *assertion* ya que si falla también fallará el test.

Si queremos testear la posibilidad de agregar múltiples elementos a la lista podríamos repetir las líneas:
```jsx
fireEvent.change(inputElement, { target: { value: "tarea 1" } });
fireEvent.click(buttonElement);
fireEvent.change(inputElement, { target: { value: "tarea 2" } });
fireEvent.click(buttonElement);
fireEvent.change(inputElement, { target: { value: "tarea 3" } });
fireEvent.click(buttonElement);
```
Una mejor práctica es insertarlos mediante una función `addTodo` que recibe un array de `tasks` que dentro ejecute un `forEach` y para cada uno dispare un evento para cambiar el input y otro para el click.

```jsx
const addTask = (tasks) => {
    const inputElement = screen.getByPlaceholderText(/Add a new task here.../i);
    const buttonElement = screen.getByRole("button", { name: /Add/i} );
    tasks.forEach((task) => {
        fireEvent.change(inputElement, { target: { value: task } });
        fireEvent.click(buttonElement);
    })
}
```
Utilizando esta función para refactorizar el test anterior que sólo agrega una tarea:
```jsx
it('should render a single item', () => {
    render(<MockTodo />);
    addTask(["Go Grocery Shopping"])
    const divElement = screen.getByText(/Go Grocery Shopping/i);
    expect(divElement).toBeInTheDocument()
});
```
Con el agregado de múltiples tareas (aunque con el mismo texto para usar `getAllByText`)
```jsx
it('should render multiple items', () => {
    render(
        <MockTodo />
    );
    addTask(["Go Grocery Shopping", "Go Grocery Shopping", "Go Grocery Shopping"])
    const divElements = screen.getAllByText(/Go Grocery Shopping/i);
    expect(divElements.length).toBe(3)
});
```

Otra opción sería poner 3 tareas distintas y utilizar `getAllByTestId("test-container")` pero para ello tendríamos que utilizar un *data-attribute* en el `div`, es decir `data-testid="task-container"` y luego como también obtenemos un array podemos utilizar el mismo *assertion* `expect(divElements.length).toBe(3)`

Ahora queremos testear que al hacer click sobre un elemento cambie el formato a tachado. El componente `TodoList` agrega la clase `todo-item-active` cuando le hacemos click. Es decir tenemos que chequear que inicialmente no tenga esta clase y al hacerle click al texto pase a tenerla. Para eso trabajaremos con `toHaveClass()`

```jsx

it('task should not have complete class when initally rendered', () => {
    render(<MockTodo />);
    addTask(["Go Grocery Shopping"])
    const divElement = screen.getByText(/Go Grocery Shopping/i);
    expect(divElement).not.toHaveClass("todo-item-active")
});

it('task should have complete class when clicked', () => {
    render(<MockTodo />);
    addTask(["Go Grocery Shopping"])
    const divElement = screen.getByText(/Go Grocery Shopping/i);
    fireEvent.click(divElement)
    expect(divElement).toHaveClass("todo-item-active")
});
```