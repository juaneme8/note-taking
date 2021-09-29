## Rendering Lists

Creamos una nueva clase que utilizaremos para mostrar los elementos en pantalla. Esta clase en su constructor recibe un `<ul>` y cuenta con el método `render()` que recibe `item` que es un objeto implementado con la interface `HasFormatter`, `heading` que es un título y `pos` que puede valer `start` o `end` (se trata de un **union type**) e indica si queremos insertar el elemento al comienzo o al final 

```ts
import { HasFormatter } from "../interfaces/HasFormatter.js";

export class ListTemplate {
  constructor(private container: HTMLUListElement){}

  render(item: HasFormatter, heading: string, pos: 'start' | 'end'){
    const li = document.createElement('li');
  
    const h4 = document.createElement('h4');
    h4.innerText = heading;
    li.append(h4);

    const p = document.createElement('p');
    p.innerText = item.format();
    li.append(p);

    if(pos === 'start'){
      this.container.prepend(li);
    } else {
      this.container.append(li);
    }
  }
}
```

Si pasamos el mouse por encima de la variable `li` o `h4` veremos nuevamente que infiere el tipo automáticamente.


Luego en `app.ts` debemos pasarle a `ListTemplate` la lista en la cual queremos mostrar los datos.
El signo `!` recordemos que es para establecer que estamos seguros de la existencia de ese elemento y que no nos indique que posiblemente sea `null`.

```ts
const ul = document.querySelector('ul')!;
const list = new ListTemplate(ul);
list.render(doc, type.value,'end');
```
