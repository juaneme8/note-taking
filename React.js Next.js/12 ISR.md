# (ISR) Incremental Static Regeneration
> https://www.youtube.com/watch?v=aD8g-IDXyhg&ab_channel=WatchandLearn
> https://blog.logrocket.com/incremental-static-regeneration-with-next-js/

En versiones anteriores de Next.js sólo teníamos dos estrategias **SSR** (*server side rendering)* o **SSG** (static site generation) en Next.js 9.5 se introdujo una tercera llamada **ISR** (Incremental Static Regeneration) que es un híbrido de estas dos. ISR nos permite actualizar páginas regenerándolas en la medida que reciben tráfico (es decir *at run-time*).
La página es generada en el primer request y a diferencia de SSR donde el usuario tiene que esperar por los datos, se le sirve una *fallback page* (con placeholders y *skeleton page* hasta que todo se resuelva). Una vez que los datos son resueltos la página final es cacheada y los visitantes subsiguientes verán la versión cacheada de manera inmediata como con SSG.

Los datos que obteníamos en el momento de hacer el build via `getStaticProps` no se actualizaban, por lo que si estos cambiaban (por ejemplo si cambio el contenido de la página de about si estuviera usando algo como Strapi) no vería dichos cambios. A partir de la versión 9.5 podemos agregar como segundo `revalidate: 1` siendo en este caso el 1 el tiempo en segundos luego del cual una regeneración puede ocurrir.

```jsx
export async function getStaticProps() {
	const {API_URL} = proces.env;

	const res = await fetch(`${API_URL}/pages/1`);
	const data = await res.json();

	return {
		props: {
			page: data,
		},
		revalidate: 1
	}
}
```

En caso de que los datos hayan cambiado, cuando el usuario ingresa a la página este verá los datos viejos pero luego de un segundo refresh se revalidarán los datos. Esto es lo mejor de los dos mundos ya que son páginas generadas estáticamente las cuales son muy rápidas y también obtenemos datos dinámicos.

Esto lo utilizaremos en aquellos casos donde no nos importa si algunos usuarios no ven los datos completamente actualizados, mientras que si queremos asegurarnos que vean datos completamente actualizados vamos a usar `getServerSideProps`