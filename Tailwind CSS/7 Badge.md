# Badge

Primero diseñamos el badge que consiste en un `div` con un `span` anidado:
`rounded-full` equivale a poner `border-radius: 9999px;` lo que da un efecto tipo cápsula.

Como queremos que esté sobre el card le damos posicionamiento `absolute` y al padre le daremos `relative`. Luego lo posicionamos con `top-0`.
```html
<div class="absolute top-0 ml-2 mt-2 p-2 bg-secondary-100 text-secondary-200 text-xs uppercase font-bold rounded-full">
	<span>25 mins</span>
</div>
```

De esta manera en la tarjeta completa:
```html
<div class="rounded bg-white border-gray-200 shadow-md overflow-hidden relative"> 
  <img src="img/stew.jpg" alt="stew" class="h-32 sm:h-48 w-full object-cover">
  <div class="m-4">
    <span class="font-bold">5 Bean Chili Stew</span>
    <span class="block text-gray-500 text-sm">Recipe by Mario</span>
  </div>
  <div class="absolute top-0 ml-2 mt-2 p-2 bg-secondary-100 text-secondary-200 text-xs uppercase font-bold rounded-full">
    <span>25 mins</span>
  </div>
</div>
```

Como segunda opción podemos ver la implementación en la documentación oficial: https://tailwindcss.com/course/designing-a-badge Usando flex-box y algunos conceptos interesantes en cuanto a diseño.