# Spotify Clone

Basado en el [video](https://www.youtube.com/watch?v=3xrko3GpYoU) de Sonny Sangha



* Crear proyecto de Next.js con Tailwind CSS

```shell
npx create-next-app -e with-tailwindcss my-project
```



* Instalar la extensión Tailwind CSS IntelliSense de Brad Cornes

  

* Creación componente funcional con la extensión de ES7 snippets `_rfce` (guión bajo al inicio)

  

* Para los íconos utilizaremos heroicons.com 

  ```
  npm install heroicons
  ```

Luego importamos el ícono deseado de este modo:
  ```
  import {HomeIcon} from '@heroicons/react/outline'
  ```

  Para utilizarlo debemos especificar el alto y ancho cosa que hacemos usando clases de Tailwind:

```
<HomeIcon className="w-5 h-5"/>
```

