# SWR

> Basado en el [video](https://youtu.be/feJtBI_HL6M) de midudev 

## Introducción

SWR (stale while revalidate) es una estrategia popularizada por HTTP que hace referencia a entregar lo que tengo cacheado en la medida que voy a recuperar la última información y la muestro cuando sea posible.

* **Fast page navigation** ya que le mostramos rápidamente al usuario lo que tenemos en caché que se conoce como **stale**, mientras ocurre el **revalidate**.
* **Polling on interval** nos permite realizar un fetching de datos cada cierto tiempo.

* **Revalidation on focus** nos permite que cuando nos movemos otra pestaña y luego volvemos se revaliden los datos.
* Local mutation (**Optimistic** UI) nos permite mutar la caché incluso antes de hacer la mutación en el servidor.



## Utilización

#### Documentación 

https://swr.vercel.app/



#### Instalación

```
npm install swr
```



#### Ejemplo de Uso

En primer lugar hacemos un import default de `useSWR`

```
import useSWR from 'swr'
```



Luego debemos definir un `fetcher` de manera global

```
const fetcher = (...args) => fetch(...args).then(res => res.json())
```



Por último dentro del componente utilizamos el hook y obtenemos los datos:

```
const {data} = useSWR('/api/challenge', fetcher)
```

