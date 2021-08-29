## Style Props
Style Props es un modo de alterar el estilo de un componente mediante el pasaje de props. 
Los componentes de Chakra extienden de un primitivo `Box` al cual podemos pasarle por props todas las propiedades que podríamos pasarle a un tag de CSS. 

Las propiedades CSS tienen un correlato con Props de chakra, por ejemplo:

## `_before`
Si queremos agregar contenido antes de un elemento podemos usar el pseudo-elemento before podemos usarlo con `_before` por ejemplo del siguiente modo `<Text _before={{content:"'...'}}>Hola</Text>"` como consecuencia de esto se mostrarán `...` antes del texto. 
 >Notar que pusimos `content:""` y luego el mensaje en comillas simples `'...'`

## `_after`
Si queremos agregar contenido después de un elemento podemos usar el pseudo-elemento after con `_after`.

## `_hover`
Podríamos establecer un hover de un cierto color en un texto de esta forma: `<Text _hover={{color: "red.500"}}>Hola</Text>`
`<Text _hover={{ color: 'red.500', fontWeight: 'bold' }}>Hola</Text>`

También podríamos agregar `transition=".2s"` para que no sea tan brusco el cambio.