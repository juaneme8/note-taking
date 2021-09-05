# TypeScript & React.js
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