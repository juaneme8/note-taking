# Refactorizado
Cuando tenemos un método que lo único que hace es renderizar un componente es una buena práctica directamente extraerlo a un nuevo componente.

## Componentes Reutilizables con `children`
Supongamos que queremos crear un componente reutilizable que muestre o no un contenido alternativamente, dependiendo del valor de una variable. 

```jsx
import React, { useState} from 'react'
import PropTypes from 'prop-types'
import i18n from '../i18n/index'

const Togglable = forwardRef(({children, buttonLabel = 'Muestrame esto, desde default values'}, ref) => {
  const [visible, setVisible] = useState(false)

  const hideWhenVisible = { display: visible ? 'none' : '' }
  const showWhenVisible = { display: visible ? '' : 'none' }

  const toggleVisibility = () => setVisible(!visible)

  return (
    <div>
      <div style={hideWhenVisible}>
        <button onClick={toggleVisibility}>{buttonLabel}</button>
      </div>

      <div style={showWhenVisible}>
        {children}
        <button onClick={toggleVisibility}>
          Cancel
        </button>
      </div>

    </div>
    )
})


export default Togglable
```

Luego para utilizar este componente:
```jsx
<Togglable buttonLabel='Show Login'>
  <form onSubmit={handleSubmit}>
    <div>
      <input
        type='text'
        value={props.username}
        name='Username'
        placeholder='Username'
        onChange={props.handleUsernameChange}
      />
    </div>
    <div>
      <input
        type='password'
        value={props.password}
        name='Password'
        placeholder='Password'
        onChange={props.handlePasswordChange}
      />
    </div>
    <button>
      Login
    </button>
  </form>
</Togglable>
```
Es decir que el `form` en sí constituirá el `children` en `Togglable` y además tendrá la prop `buttonLabel`.