# `Box`
Es el componente más abstracto que tiene Chakra y sobre él se construyen todos los otros componentes. Por default renderiza un `div` y como dijimos anteriormente haciendo uso de las Style Props puede recibir todo tipo de props para cambiar su estilo. También es posible utilizar la propiedad `as="p"` cuando queremos que el `Box` renderice un `p` en lugar de un `div`

# `Container`
El componente container nos permite establecer un `maxWidth` y que el contenido quede centrado para pantallas muy grandes.
Lo usaremos habitualmente en el componente `Layout` que contendrá los elementos que queremos que se repitan para todas las páginas.

```jsx
<Container maxWidth="container.xl" centerContent>
	Contenido
</Container>
```

Con `centerContent` centramos a `children` independientemente de su ancho.

# `Stack`
Se trata de un componente que nos permite agregar un espaciado configurable entre elementos.
Esto es útil por ejemplo cuando tenemos varios botones ubicados en la misma línea y queremos que tengan un cierto espaciado entre ellos pero que sigan estando centrados. Si esto lo resolviéramos utilizando `margin-left` el primer botón no querríamos que tenga margen y luego si ocultamos ese primero y otro pasa a ser primero tendríamos que quitarle el margen.

El `Stack` puede recibir como prop `divider` que establece un divisor:
Podríamos pasarle `divider={<StackDivider/>}` o incluso podríamos pasarle directamente el marcado deseado  `divider={<span>Hola</span>}`

```jsx
<Stack direction="row" divider={<StackDivider borderColor="gray.700" orientation="vertical" />} spacing={4}>
	(...)
</Stack>
```

Podemos indicar la dirección del apilado con la prop `direction` usando `row` o `column`.
Este componente soporta valores responsivos por lo que podremos tener un `Stack` que para pantallas grandes tiene dirección `row` y para chicas `column`.

También existen los componentes `HStack` (para dirección horizontal) y `VStack` (para dirección vertical) que además se encargan del centrado.


# Iconos
Para utilizar los íconos de Chakra debemos instalar `npm i @chakra-ui/icons`, sin embargo es posible instalar cualquier tipo de librería de íconos que queramos.

Por ejemplo podemos trabajar con con **React Icons** que instalamos con `npm install react-icons`:
```jsx
import {FaTwitter} from 'react-icons/fa'

<Icon as={FaTwitter} height={6} width={6}/>
```
> Modificamos el tamaño del ícono con las props `height` y `width`

# `Grid`
Podemos usar las mismas propiedades que usaríamos en CSS Grid.

# `Button`
## `colorScheme`
A la hora de darle color a un `button` utilizaremos `colorScheme` con lo cual tendremos un esquema de colores apropiado tanto de fondo, letra, hover, etc. 

`<Button colorScheme="red">1</Button>`
`<Button colorScheme="telegram">1</Button>`

## `variant` de Button
Por default los button tendrán la variante `solid` pero también podremos especificar mediante el atributo `variant` para que sea `ghost`, `outline`, `unstyled`, `link`

## `leftIcon` y `rightIcon` en Button
Los botones pueden venir acompañados de íconos, de hecho es posible pasarle cómo icono cualquier componente React:
`<Button leftIcon={<span>H</span>}` (también podemos usar `rightIcon`)

## `isLoading` y `loadingText`
Con la prop `isLoading` podremos agregar un estado de loading al botón, con `loadingText` especificamos el texto que queremos que acompañe al ícono de carga.

```jsx
<Button
    isLoading
    loadingText="Submitting"
    colorScheme="teal"
    variant="outline"
  >
    Submit
  </Button>
```



Por default será un spinner (pero podemos cambiarlo con la prop `spinner` por ejemplo `spinner={<BeatLoader size={8} color="white" />}`) y el ancho del botón no cambiará.

```jsx
<Button
  isLoading
  colorScheme="blue"
  spinner={<BeatLoader size={8} color="white" />}
>
  Click me
</Button>
```

También podemos pasarle `loadingText` con el texto que queremos que aparezca mientras está cargando.

## Enlaces Externos y `as`
Cuando queremos tener un enlace externo podemos utilizar un `Button` con la prop `as={Link}` y un `href` con la dirección a la cual queremos ser redireccionados:

```jsx
 <Button
  isExternal
  as={Link}
  botton={0}
  colorScheme="whatsapp"
  href={`https://wa.me/54911XXXXXXXX?text=${encodeURIComponent(text)}`}
  position="sticky"
>
  Completar pedido ({cart.length} productos)
</Button>
```

# `Drawer`
Basándonos en el ejemplo que aparece en la [documentación](https://chakra-ui.com/docs/overlay/drawer)
```jsx
import React from 'react';

import {
  Drawer,
  DrawerBody,
  DrawerFooter,
  DrawerHeader,
  DrawerOverlay,
  DrawerContent,
  DrawerCloseButton,
  useDisclosure,
  Button,
  Input,
} from '@chakra-ui/react';

export default function DrawerExample() {
  const { isOpen, onOpen, onClose } = useDisclosure();
  const btnRef = React.useRef<any>();

  return (
    <>
      <Button ref={btnRef} colorScheme="teal" onClick={onOpen}>
        Open
      </Button>
      <Drawer finalFocusRef={btnRef} isOpen={isOpen} placement="right" onClose={onClose}>
        <DrawerOverlay>
          <DrawerContent>
            <DrawerCloseButton />
            <DrawerHeader>Create your account</DrawerHeader>

            <DrawerBody>
              <Input placeholder="Type here..." />
            </DrawerBody>

            <DrawerFooter>
              <Button mr={3} variant="outline" onClick={onClose}>
                Cancel
              </Button>
              <Button colorScheme="blue">Save</Button>
            </DrawerFooter>
          </DrawerContent>
        </DrawerOverlay>
      </Drawer>
    </>
  );
}
```
En este ejemplo usamos `const btnRef = React.useRef<any>();` (el `any` no es aconsejable pero lo hicimos en este caso para no definir tipos). El propósito de eso es tener la referencia al botón para que luego de cerrar el drawer pongamos el foco en ese botón nuevamente.

# Hooks
## `useDisclosure`
En los Drawer podemos utilizar el *custom hook* `useDisclosure` ideal para aquellos casos donde trabajamos con `open`, `close` o `toggle`. Lo usamos de este modo `const { isOpen, onOpen, onClose } = useDisclosure();`. Al botón para abrir el modal le pasamos `onClick={onOpen}`, al botón para cancelar le pasamos `onClick={onClose}` y al `Drawer` a la propiedad `isOpen={isOpen}`.
Gracias a esto nos ahorramos crear los estados con `useState` y las funciones.

## `useBreakpointValue`
 `useBreakpointValue` se trata de un *custom hook* que retorna un valor del objeto responsivo según el breakpoint actual. Por ejemplo la prop `variant` de los `Button` no acepta valores responsivos pero podemos usar `useBreakpointValue` para especificar un valor u otro dependiendo del tamaño de pantalla.

```jsx
function Example() {
  const variant = useBreakpointValue({ base: "outline", md: "solid" })

  return (
    <VStack align="flex-start">
      <Text>Resize your window to see the button variant change</Text>
      <Button colorScheme="teal" variant={variant}>
        Button
      </Button>
    </VStack>
  )
}
```
Veremos que para pantallas chicas es `outline` y para pantallas grandes `solid`.

## Toggle color mode con `useColorMode`
`useColorMode` es un hook on el cual podremos togglear el *color mode*, cambiar el *color mode* al deseado u obtener el color de modo en el que estamos:

Podremos hacer destructuring de:
Con `toggleColorMode` como su nombre lo indica si estamos en *dark mode* nos pasa a *light mode* y viceversa.
Con `setModeColor` podríamos pasarle el modo deseado `"dark"` o `"light"`.
Con `colorMode` obtenemos el valor del *color mode* actual.

```jsx
const {toggleColorMode} = useColorMode()
```

En este caso luego en ícono que queramos usar para cambiar de modo, hacemos:

```jsx
<Icon as={FaTwitter} heigth={7} width={7} onClick={toggleColorMode}/>
```

Si queremos mostrar un ícono de acuerdo al modo en el cual estamos:

```jsx
const {toggleColorMode, colorMode} = useColorMode()

 <IconButton
      aria-label="Toggle Dark Switch"
      icon={colorMode === 'dark' ? <BiSun /> : <BiMoon />}
      onClick={toggleColorMode}
    />
```

## `useColorModeValue`
Nos permite definir una serie de valores de acuerdo al *color mode* en el cual nos encontremos
```jsx
const logoColor = useColorModeValue('primary.500', undefined);

```
Luego en el componente le paso `color={logoColor}` logrando así tener un color personalizado.

```jsx
<Icon color={logoColor] ... />
```

# Chakra Factory
Nos permite convertir en componentes Chakra los componentes de una librería externa o elementos jsx en general y de esta manera podrán ser personalizados mediante style props. 