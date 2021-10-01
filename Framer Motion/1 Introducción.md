# Framer Motion 
> ==Al estudiarlo podemos basarnos en== [Tutorial de The Net Ninja](https://youtu.be/2V1WK-3HQNk?list=PL4cUxeGkcC9iHDnQfTHEVVceOEBsOf07i)

Framer Motion es una librería para animaciones en React, nos permite realizar tanto animaciones y transiciones a elementos del DOM definidos en nuestros componentes React.

Se caracteriza por tener una sintaxis muy intuitiva y nos permitirá realizar animaciones bastante complejas con muy pocas líneas de código comparado con si las hiciéramos con CSS.

En la [documentación](https://www.framer.com/motion/) podemos encontrar varios ejemplos de lo que es capaz esta librería.

## Instalación
`npm install framer-motion`

> Notar que al instalar Chakra-UI con los `npm i` que nos dice la documentación también estaremos instalando Framer Motion.

## Animaciones Básicas
En primer lugar debemos importar `motion` y luego en cada elemento que queremos debemos modificar sus tags por ejemplo si queremos animar un `<h2>...</h2>` debemos poner `<motion.h2>...</motion.h2>`. Esto nos retorna un motion component que será un h2 con habilidades extra para animar.
Luego agregamos la prop `animate` en la cual le pasamos un objeto con los valores finales que deseamos que tenga la animación.

### `x` e `y`
En cuanto a las traslaciones que hacemos con `x` e `y` debemos tener presente que representan el offset respecto a la posición actual. No son propiedades de CSS sino propiedades de framer-motion. 

Si `x` es positivo será un desplazamiento hacia la derecha (y si es negativo a la izquierda) y si `y` es positivo irá hacia abajo (y hacia arriba si es negativo). `scale` también es una propiedad de framer-motion

### `px` y otras unidades
A la hora de definir valores de las propiedades podemos ver que ponemos `fontSize: 50` sin especificar `px` y por default sabe que se trata de `px`. 
En cambio si queremos otra unidad por ejemplo `100vw` podremos poner el valor y la unidad entre comillas `animate={{'100vw'}}`.

```jsx
import React from 'react';
import { Link } from 'react-router-dom';
import { motion } from 'framer-motion';

const Home = () => {
  return (
    <motion.div className="home container"
      animate={{ rotateZ: 180, opacity: 0.2, marginTop: 200 }}
    >
      <motion.h2 
        animate={{ fontSize: 50, color: '#ff2994', x: 100, y: -100 }}
      >
        Welcome to Pizza Joint
      </motion.h2>
      <Link to="/base">
        <motion.button
          animate={{ scale: 1.5 }}
        >
          Create Your Pizza
        </motion.button>
      </Link>
    </motion.div>
  )
}

export default Home;
```
## Initial State
Así como con `animate` establecimos el estado final de la animación en ocasiones también vamos a querer definir el estado inicial y eso lo haremos con el atributo `initial`.
Suponiendo que queremos animar un `div` de modo tal que pase de opacidad 0 a 1. Luego veremos que también es posible regular el tiempo en el cual hace dicha transición.
```jsx
import React from 'react';
import { Link } from 'react-router-dom';
import { motion } from 'framer-motion';

const Home = () => {
  return (
    <motion.div className="home container"
      initial={{ opacity: 0 }}
      animate={{ opacity: 1 }}
    >
      <h2>Welcome to Pizza Joint</h2>
      <Link to="/base">
        <motion.button
          animate={{  }}
        >
          Create Your Pizza
        </motion.button>
      </Link>
    </motion.div>
  )
}

export default Home;
```

## Transition Options
A la hora de controlar las características de la animación vamos a utilizar la prop `transition`, a esta le pasaremos un objeto `Transition` que define cómo animamos de un estado a otro.

* con `type` definimos el tipo de animación entre los posibles valores: `Tween` (movimiento uniforme), `Spring` (movimiento con rebote) o `Inertia`. El valor default dependerá del tipo de animación que estemos haciendo.
* con `delay` establecemos el tiempo que debe transcurrir para que comience.
* con `duration` establecemos la duración de la transición y solo puede ser usado si el tipo es `Tween` como veremos a continuación:
```jsx
import React from 'react';
import { Link } from 'react-router-dom';
import { motion } from 'framer-motion';

const Home = () => {
  return (
    <motion.div className="home container"
      initial={{ opacity: 0 }}
      animate={{ opacity: 1 }}
      transition={{ delay: 1.5, duration: 1.5 }}
    >
      <h2>Welcome to Pizza Joint</h2>
      <Link to="/base">
        <motion.button
          animate={{  }}
        >
          Create Your Pizza
        </motion.button>
      </Link>
    </motion.div>
  )
}

export default Home;
```
 Si tenemos `type: 'spring'` (es el tipo por default en la transición que veremos a continuación pero lo especificamos para verlo de un golpe de vista) podremos definir `stiffness` y cuando mayor sea este número mayor será la cantidad de rebotes. El valor default de `stiffness` es cercano a 100 por lo que con 120 lo hacemos rebotar un poco más.
```js
<motion.div className="title"
  initial={{ y: -250}}
  animate={{ y: -10 }}
  transition={{ delay: 0.2, type: 'spring', stiffness: 120 }}
>
  <h1>Pizza Joint</h1>
</motion.div>
```
## Hover Animations
Cuando queremos animar un elemento cuando el usuario le pasa el mouse por encima debemos utilizar el atributo `whileHover` y especificar los estilos que queremos que tenga en ese momento de similar a lo visto hacia ahora.

Por ejemplo si queremos agregarle un cambio en la escala y un `textShadow` en las letras y `boxShadow` en el botón en sí:
```jsx
<motion.button
  whileHover={{ 
    scale: 1.1, 
    textShadow: "0px 0px 8px rgb(255,255,255)",
    boxShadow: "0px 0px 8px rgb(255,255,255)",
  }}
>
  Create Your Pizza
</motion.button>
```

También podemos utilizarlo si tenemos una lista de elementos uno arriba de otro, en los cuales queremos cambiar su escala y color:

```jsx
<ul>
 {toppings.map(topping => {
   let spanClass = pizza.toppings.includes(topping) ? 'active' : '';
   return (
     <motion.li key={topping} onClick={() => addTopping(topping)}
       whileHover={{ scale: 1.3, originX: 0, color: '#f8e112' }}
       transition={{ type: 'spring', stiffness: 300 }}
     >
       <span className={spanClass}>{ topping }</span>
     </motion.li>
   )
 })}
</ul>
```
> En este caso como tenemos varios elementos uno arriba de otro debemos especificar el origen de la transformación para que no se muevan los elementos al escalarlos.

```jsx
```

## Basado en Twitch Goncy (e-commerce)
`import { motion, AnimatePresence, AnimateSharedLayout } from 'framer-motion';` 

`AnimatePresence` nos permite animar la aparición y desaparición de un elemento.
`AnimateSharedLayout` nos permite agregar transiciones entre elementos que tienen el mismo `layoutId`.

Suponiendo que queremos agregar una animación una imagen que tenemos con este código:

```jsx
<Grid gap={6} templateColumns="repeat(auto-fill, minmax(200px, 1fr))">
        {products.map((product) => (
          <Stack key={product.id} padding={4}>
            <Image src={product.image} />    
            <Button onClick={() => handleAddToCart(product)}>
              Agregar
            </Button>
         </Stack>
       ))}
</Grid>
```

## Variants
En primer lugar nos permiten extraer `initial`, `animate` y `transition` en un objeto externo que luego referenciamos, logrando así un código más limpio. 

En segundo lugar nos permiten los valores de `intial` y `animate` por todo el DOM resultando también un código más limpio.

 Por último nos permiten temporizar animaciones entre padres e hijos usando propiedades de *transition orchestration*

> A las propiedades de los objetos que utilizamos como `variants` podemos ponerle cualquier nombre pero por comodidad le ponemos `hidden` y `visible`.
```jsx
import React from 'react';
import { Link } from 'react-router-dom';
import { motion } from 'framer-motion';

const containerVariants = {
  hidden: { 
    opacity: 0, 
    x: '100vw' 
  },
  visible: { 
    opacity: 1, 
    x: 0,
    transition: { type: 'spring', delay: 0.5 }
  },
};

const nextVariants = {
  hidden: { 
    x: '-100vw' 
  },
  visible: {
    x: 0,
    transition: { type: 'spring', stiffness: 120 }
  } 
}

const Base = ({ addBase, pizza }) => {
  const bases = ['Classic', 'Thin & Crispy', 'Thick Crust'];

  return (
    <motion.div className="base container"
      variants={containerVariants}
      initial="hidden"
      animate="visible"
    >
      <h3>Step 1: Choose Your Base</h3>
      <ul>
        {bases.map(base => {
          let spanClass = pizza.base === base ? 'active' : '';
          return (
            <motion.li key={base} onClick={() => addBase(base)}
              whileHover={{ scale: 1.3, originX: 0, color: '#f8e112' }}
              transition={{ type: 'spring', stiffness: 300 }}
            >
              <span className={spanClass}>{ base }</span>
            </motion.li>
          )
        })}
      </ul>

      {pizza.base && (
        <motion.div className="next"
          variants={nextVariants}
          initial="hidden"
	      animate="visible"
        >
          <Link to="/toppings">
            <motion.button
               whileHover={{ 
                scale: 1.1, 
                textShadow: "0px 0px 8px rgb(255,255,255)",
                boxShadow: "0px 0px 8px rgb(255,255,255)",
              }}
            >
              Next
            </motion.button>
          </Link>
        </motion.div>
      )}
    </motion.div>
  )
}

export default Base;
```

Con el atributo `variants` le indicamos el objeto que contiene los variants y luego busca la propiedad correspondiente para `initial` y `animate`.

En el objeto `visible` podemos ver que tenemos una propiedad `transition`  embebida por lo que no es necesario colocarlo como atributo separado.

La propagación de atributos de animación hacia abajo en el DOM que mencionamos como segunda característica consiste en que los hijos con con elementos motion heredan de los padres los valores de `initial` y `animate` que será buscado en las propiedades del objeto que le hayamos pasado como `variants` a esos hijos. 

Es por eso que a continuación veremos que cuando ponemos ` <motion.div className="next" variants={nextVariants}>` no  es necesario especificar `initial` y `animate` sino que buscará `hidden` y `visible` en `nextVariants`. De esta manera nos ahorramos unas líneas de código.

```jsx
import React from 'react';
import { Link } from 'react-router-dom';
import { motion } from 'framer-motion';

const containerVariants = {
  hidden: { 
    opacity: 0, 
    x: '100vw' 
  },
  visible: { 
    opacity: 1, 
    x: 0,
    transition: { type: 'spring', delay: 0.5 }
  },
};

const nextVariants = {
  hidden: { 
    x: '-100vw' 
  },
  visible: {
    x: 0,
    transition: { type: 'spring', stiffness: 120 }
  } 
}

const Base = ({ addBase, pizza }) => {
  const bases = ['Classic', 'Thin & Crispy', 'Thick Crust'];

  return (
    <motion.div className="base container"
      variants={containerVariants}
      initial="hidden"
      animate="visible"
    >
      <h3>Step 1: Choose Your Base</h3>
      <ul>
        {bases.map(base => {
          let spanClass = pizza.base === base ? 'active' : '';
          return (
            <motion.li key={base} onClick={() => addBase(base)}
              whileHover={{ scale: 1.3, originX: 0, color: '#f8e112' }}
              transition={{ type: 'spring', stiffness: 300 }}
            >
              <span className={spanClass}>{ base }</span>
            </motion.li>
          )
        })}
      </ul>

      {pizza.base && (
        <motion.div className="next"
          variants={nextVariants}
        >
          <Link to="/toppings">
            <motion.button
               whileHover={{ 
                scale: 1.1, 
                textShadow: "0px 0px 8px rgb(255,255,255)",
                boxShadow: "0px 0px 8px rgb(255,255,255)",
              }}
            >
              Next
            </motion.button>
          </Link>
        </motion.div>
      )}
    </motion.div>
  )
}

export default Base;
```

En cuanto a la orquestación entre animaciones de padres e hijos, podemos decir que esto será particularmente útil cuando queremos aplicar un efecto en el padre y a continuación un efecto en el hijo.

Si tuviéramos una situación como la siguiente, no llegaríamos a ver el *fade-in* en el párrafo hijo. Esto se debe a que cuando hace al transición de opacidad 0 a 1, todavía se encuentra fuera de la pantalla. Una posible solución sería agregar un delay a la animación del hijo o bien podríamos utilizar las **propiedades de orquestación de transición** en los `variants`. 

```jsx
import React from 'react';
import { motion } from 'framer-motion';

const containerVariants = {
  hidden: { 
    opacity: 0, 
    x: '100vw',
  },
  visible: { 
    opacity: 1, 
    x: 0,
    transition: { 
      type: 'spring',
      delay: 0.5
    }
  },
};

const childVariants = {
  hidden: {
    opacity: 0,
  },
  visible: {
    opacity: 1,
  }
}

const Order = ({ pizza }) => {
  return (
    <motion.div className="container order"
      variants={containerVariants}
      initial="hidden"
      animate="visible"
    >
      <h2>Thank you for your order :)</h2>
      <motion.p variants={childVariants}>You ordered a {pizza.base} pizza with:</motion.p>
      <motion.div variants={childVariants}>
        {pizza.toppings.map(topping => <div key={topping} >{topping}</div>)}
      </motion.div>
      
    </motion.div>
  )
}

export default Order;
```

En cambio usando la orquestación

```jsx
import React from 'react';
import { motion } from 'framer-motion';

const containerVariants = {
  hidden: { 
    opacity: 0, 
    x: '100vw',
    transition: {
      staggerChildren: 0.5,
    } 
  },
  visible: { 
    opacity: 1, 
    x: 0,
    transition: { 
      type: 'spring',
      mass: 0.4,
      damping: 8,
      staggerChildren: 0.4,
      when: "beforeChildren",
    }
  },
};

const childVariants = {
  hidden: {
    opacity: 0,
  },
  visible: {
    opacity: 1,
  }
}

const Order = ({ pizza }) => {
  return (
    <motion.div className="container order"
      variants={containerVariants}
      initial="hidden"
      animate="visible"
    >
      <h2>Thank you for your order :)</h2>
      <motion.p variants={childVariants}>You ordered a {pizza.base} pizza with:</motion.p>
      <motion.div variants={childVariants}>
        {pizza.toppings.map(topping => <div key={topping} >{topping}</div>)}
      </motion.div>
      
    </motion.div>
  )
}

export default Order;
```

En el objeto `containerVariants` colocamos en `transition` la propiedad `when: "beforeChildren"` con lo cual indicamos que queremos que termine la animación el padre antes de que comiencen las de los hijos.

Para que no resulta tan lento eliminamos el `delay: 0.5` y agregamos otras propiedades que podemos usar con el `type: 'spring'`de `mass` (cuanto mayor es, más lento se mueve el objeto) y el `damping` (un número grande implica menos oscilación, con 0 oscilaría permanentemente) .

Con la propiedad `staggerChildren: 0.4` indicamos que queremos que las animaciones de los hijos sean escalonas con un tiempo de 0.4 segundos por lo que existirá ese tiempo entre el comienzo de la animación del `p` y la del `div` con la lista.

==VIDEO 8 COMPLETO==

## Keyframes