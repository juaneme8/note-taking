# `getStaticProps`
En aplicaciones React donde utilizamos por ejemplo **create-react-app** hacemos el fetch de datos de una API en el  `useEffect` hook es decir que e request lo estamos haciendo en el navegador. Sin embargo al utilizar **Next.js** como los componentes ya están pre-renderizados cuando llegan al navegador por lo que idealmente vamos queremos obtener los datos antes para que ya estén en el template. Es posible hacer esto con una función especial provista por Next.js llamada `getStaticProps` que se ejecuta antes de que se renderice el componente, obtiene los datos mediante el `fetch` y luego se los pasa via `props` al componente que luego será renderizado.

> Como fake API es posible utilizar https://jsonplaceholder.typicode.com/

```jsx
import styles from '../../styles/Jobs.module.css'

export const getStaticProps = async () => {
  const res = await fetch('https://jsonplaceholder.typicode.com/users');
  const data = await res.json();

  return {
    props: { ninjas: data }
  }
}

const Ninjas = ({ ninjas }) => {
  console.log(ninjas)

  return (
    <div>
      <h1>All Ninjas</h1>
      {ninjas.map(ninja => (
        <div key={ninja.id}>
          <a className={styles.single}>
            <h3>{ ninja.name }</h3>
          </a>
        </div>
      ))}
    </div>
  );
}
 
export default Ninjas;
```

Esta función se ejecuta *at build-time* por lo que no debemos escribir código que esperamos que se ejecute en el navegador.

