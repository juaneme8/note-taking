# SOLID Principles

> :link: Basado en el [video de midulive](https://youtu.be/jKdt-BnTTR0)



## Enlaces

* [Applying SOLID principles in React](https://konstantinlebedev.com/solid-in-react/)

* [The S.O.L.I.D Principles in Pictures](https://medium.com/backticks-tildes/the-s-o-l-i-d-principles-in-pictures-b34ce2f1e898)



- **S**ingle responsibility principle (SRP)
- **O**pen-closed principle (OCP)
- **L**iskov substitution principle (LSP)
- **I**nterface segregation principle (ISP)
- **D**ependency inversion principle (DIP)



## **S**ingle responsibility principle (SRP)

En React en ocasiones tenemos un componente con diferentes responsabilidades que debe gestionar el estado, hacer un fetching de datos, renderizar el contenido, etc.

Como regla general en React podemos pensar que cuando tenemos un useEffect podemos crear un custom hook.