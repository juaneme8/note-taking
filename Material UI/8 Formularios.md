# Formularios
A la hora de trabajar con formularios utilizamos el tag standard de html `<form>`.
Podemos utilizar atributos como  `noValidate` para evitar la validación del navegador dado que realizaremos una propia o `autoComplete="off"` para evitar el autocompletado.

## `TextField`
```jsx
import TextField from '@material-ui/core/TextField'
const [title, setTitle] = useState(``);

<TextField 
	onChange={(e) => setTitle(e.target.value)}
	label="Título de la nota"
	variant="outlined"
	color="secondary"
	fullWidth
	required
/>
```
Con `fullWidth` nos aseguramos que ocupe la totalidad de la pantalla.
Con `required` nos agregará un asterisco al lado del label pero no agrega ninguna validación.
Con `multiline` podríamos determinar que se trata de un campo multilínea y si queremos tener cuatro filas lo establecemos con`rows={4}`

Queremos almacenar en una variable de estado el valor ingresado en los inputs y eso lo hacemos con el evento `onChange` de modo que ante cada cambio se actualice el valor.

A continuación mostramos un ejemplo de un formulario con validación de errores que se lleva a cabo cuando el usuario presiona el submit button y mediante el campo `error` que será puesto en `true` si el usuario no ha completado el campo.

```jsx
import React, { useState } from 'react'
import Button from '@material-ui/core/Button'
import { makeStyles } from '@material-ui/core'
import TextField from '@material-ui/core/TextField'

const useStyles = makeStyles({
  field: {
    marginTop: 20,
    marginBottom: 20,
    display: 'block'
  }
})

export default function Create() {
  const classes = useStyles()
  const [title, setTitle] = useState('')
  const [titleError, setTitleError] = useState(false)

  const handleSubmit = (e) => {
    e.preventDefault()
    setTitleError(false)

    if (title == '') {
      setTitleError(true)
    }
  }

  return (
      <form noValidate autoComplete="off" onSubmit={handleSubmit}>
        <TextField className={classes.field}
          onChange={(e) => setTitle(e.target.value)}
          label="Note Title" 
          variant="outlined" 
          color="secondary" 
          fullWidth
          required
          error={titleError}
        />

       <Button
          type="submit" 
          color="secondary" 
          variant="contained"
          endIcon={<KeyboardArrowRightIcon />}>
          Submit
        </Button>
      </form>
  )
}
```

## `Radio`
La forma más simple de crearlo es la siguiente:
```jsx
import Radio from '@material-ui/core/Radio'

<Radio
	value="opcion1"
/>
```

## `RadioGroup`
Si tuviéramos más de un `<Radio/>` sería posible seleccionar más de una opción a la vez, el cual no es el comportamiento esperado de un radio button pues sino usaríamos checkboxes en su lugar. Para lograr esto debemos envolverlos en un componente llamado `RadioGroup`

Nos permite agrupar múltiples `Radio` de modo tal que sólo sea posible seleccionar uno a la vez.
```jsx
import RadioGroup from '@material-ui/core/RadioGroup'

<RadioGroup>
	<Radio  value="opcion1"  />
	<Radio  value="opcion2"  />
</RadioGroup>
```

Si bien esto funciona, una opción mejor para implementarlo es haciendo uso del componente `FormControlLabel`

## `FormControlLabel`
Este componente nos permite contar con un label que envuelve al control que en nuestro caso es un componente `<Radio/>`

La variable de estado la inicializamos con aquel elemento que queremos que esté seleccionado por default.
```jsx
const [category, setCategory] = useState('');

 <RadioGroup value={category} onChange={(e) => setCategory(e.target.value)}>
   <FormControlLabel value="opcion1" label="Opción 1" control={<Radio />} />
   <FormControlLabel value="opcion2" label="Opción 2" control={<Radio />} />
 </RadioGroup>
```

## `FormLabel`
## `FormControl`
Para asignarle una etiqueta a este grupo de radio buttons utilizaremos los componentes `<FormLabel/>`  que se trata de una etiqueta genérica que podemos aplicar en diferentes secciones del formulario y `<FormControl/>` que es un wrapper también de ciertas secciones de form.
Veremos que gracias a la presencia del `FormControl` al cambiar los valores se producirá un destello en el texto de la etiqueta indicada en `FormLabel`.

```jsx
<FormControl>
  <FormLabel> Categoría</FormLabel>
  <RadioGroup
    value={category}
    onChange={(e) => setCategory(e.target.value)}
  >
    <FormControlLabel
      value="opcion1"
      label="Opción 1"
      control={<Radio />}
    />
    <FormControlLabel
      value="opcion2"
      label="Opción 2"
      control={<Radio />}
    />
  </RadioGroup>
</FormControl>
```



# Formularios Avanzados 
## Estructura
Para empezar creamos la carpeta `pages` y dentro de ella otra carpeta `Employees`. Por último colocamos `
```jsx
import { makeStyles, Paper } from "@material-ui/core";
import React from "react";
import EmployeeForm from "./EmployeeForm";

const useStyles = makeStyles((theme) => ({
  pageContent: {
    margin: theme.spacing(3),
    padding: theme.spacing(3)
  }
}));

export default function Employees() {
  const classes = useStyles();

  return (
    <>
      <Paper className={classes.pageContent} elevation={4}>
        <EmployeeForm />
      </Paper>
    </>
  );
}
```

En primer lugar creamos un objeto `initialValues` donde almacenamos los valores iniciales de cada uno de los ciedad coincidirá con el atributo `name` de cada uno de ellos. 
Luego una variable de estado usando el hook `useState()` una porcamos donde almacenaremos los valores de dichos campos luego de que sufran cambios `const [values, setValues] = useState(initialValues);` Esta tarea la llevará a cabo el método `handleInputChange` actualizando `values` con el valor del campo que sufrió cambios.

Cuando agregamos controles de un formulario como ser `TextInput` veremos que estos tienen la clase `.MuiFormControl-root` aplicada. Con esta clase podremos establecer el ancho y los márgenes por ejemplo. Definimos estos estilos y los aplicamos a `form` de modo que estos afecten individualmente a todos los elementos del formulario.En `/pages/Employe

```jsx
import { Grid, makeStyles, TextField } from "@material-ui/core";
import React, { useState, useEffect } from "react";

const useStyles = makeStyles((theme) => ({
  root: {
    "& .MuiFormControl-root": {
      width: "80%",
      margin: theme.spacing(1)
    }
  }
}));

const initialValues = {
  id: 0,
  fullName: "",
  email: "",
  mobile: "",
  city: "",
  gender: "male",
  departmentId: "",
  hireDate: new Date(),
  isPermanent: false
};

export default function EmployeeForm() {
  const classes = useStyles();
  const [values, setValues] = useState(initialValues);

  const handleInputChange = (e) => {
    const { name, value } = e.target;
    setValues({ ...values, [name]: value });
    //console.log(values);
    //console.log(name, value);
  };

  return (
    <form className={classes.root}>
      <Grid container>
        <Grid item xs={6}>
          <TextField
            variant="outlined"
	        name="fullName"
            label="Full Name"
            value={values.fullName}
            onChange={handleInputChange}
          />
          <TextField
            variant="outlined"
            name="email"
            label="Email"
            value={values.email}
            onChange={handleInputChange}
          />
        </Grid>
      </Grid>
    </form>
  );
}

```

# Reusable Input Controls
Creamos el hook `useForm` en la carpeta `/components` y este recibe como argumento `initialValues` y retornaremos un objeto con variables y funciones reutilizables. Dentro de este hook colocamos los estilos `useStyles = (...)`, a variable del estado `const [values, setValues] = useState(initialValues);` y el método que la actualiza `handleInputChange`.
La ventaja de hacer esto es que así no teneosr que repetir este código en cada componente que tenga un formulario y en caso de tener que realizar cambios en el estilo bastará con cambiarlo en un único lugar.

Creamos también un componente `Form` dentro del mismo archivo que recibe como `props.children` a los elementos que le pasamos internamente.

Entonces `useForm` nos queda:
```jsx
import { makeStyles } from "@material-ui/core";
import React, { useState } from "react";

const useStyles = makeStyles((theme) => ({
  root: {
    "& .MuiFormControl-root": {
      width: "80%",
      margin: theme.spacing(1)
    }
  }
}));

export function useForm(initialValues) {

  const [values, setValues] = useState(initialValues);
  
  const handleInputChange = (e) => {
    const { name, value } = e.target;
    setValues({ ...values, [name]: value });
  };

  return {
    values,
    setValues,
    handleInputChange
  };
}

export function Form(props) {
  const classes = useStyles();

  return <form className={classes.root}>{props.children}</form>;
}

```

Luego utilizamos este hook `useForm` función en `EmployeeForm` y hacemos destructuring con lo que al tener los mismos nombres de variables logramos que el código siga funcionando sin modificar nada `const { values, setValues, handleInputChange } = useForm(initialValues);`

```jsx
import { Grid, TextField } from "@material-ui/core";
import React from "react";
import { useForm, Form } from "../../components/useForm";

const initialValues = {
  id: 0,
  fullName: "",
  email: "",
  mobile: "",
  city: "",
  gender: "male",
  departmentId: "",
  hireDate: new Date(),
  isPermanent: false
};

export default function EmployeeForm() {
  const { values, setValues, handleInputChange } = useForm(initialValues);

  return (
    <Form>
      <Grid container>
        <Grid item xs={6}>
          <TextField
            variant="outlined"
            label="Full Name"
            name="fullName"
            value={values.fullName}
            onChange={handleInputChange}
          />
          <TextField
            variant="outlined"
            name="email"
            label="Email"
            value={values.email}
            onChange={handleInputChange}
          />
        </Grid>
      </Grid>
    </Form>
  );
}

```
# Radio Group
Nuevamente en `EmployeeForm` comenzamos agregando un grupo de radio buttons que utilizaremos para seleccionar el gender y posteriormente será movido a un componente reutilizables.
```jsx
<FormControl>
  <FormLabel>Gender</FormLabel>
  <RadioGroup>
    <FormControlLabel value="male" control={<Radio />} label="Male" />
    <FormControlLabel
      value="female"
      control={<Radio />}
      label="Female"
    />
    <FormControlLabel
      value="other"
      control={<Radio />}
      label="Other"
    />
  </RadioGroup>
</FormControl>
```

Para poder actualizar la propiedad correspondiente al gender cuando hagamos una selección en el componente `<RadioGroup />`debemos tener `name`, `value` y `onChange`

Además si queremos mostrarlas en una única fila podemos agregar al `<RadioGroup>` el atributo `row`

```jsx
<FormControl>
  <FormLabel>Gender</FormLabel>
  <RadioGroup
    row
    name="gender"
    value={values.gender}
    onChange={handleInputChange}
  >
    <FormControlLabel  control={<Radio />} label="Male" />
    <FormControlLabel
      value="female"
      control={<Radio />}
      label="Female"
    />
    <FormControlLabel
      value="other"
      control={<Radio />}
      label="Other"
    />
  </RadioGroup>
</FormControl>
```

Para evitar tener que importar todos estos componentes de Material UI cada vez que queremos utilizar un grupo de *radio buttons* y algo similar sucederá cuando hagamos drop-downs, selects, date pickers, etc, es que queremos mover todo esto a componentes reutilizables.

En este momento `EmployeeForm`
```jsx
import {
  FormControl,
  FormControlLabel,
  FormLabel,
  Grid,
  Radio,
  RadioGroup,
  TextField
} from "@material-ui/core";
import React from "react";
import { useForm, Form } from "../../components/useForm";

const initialValues = {
  id: 0,
  fullName: "",
  email: "",
  mobile: "",
  city: "",
  gender: "male",
  departmentId: "",
  hireDate: new Date(),
  isPermanent: false
};

export default function EmployeeForm() {
  const { values, /*setValues,*/ handleInputChange } = useForm(initialValues);

  return (
    <Form>
      <Grid container>
        <Grid item xs={6}>
          <TextField
            variant="outlined"
            label="Full Name"
            name="fullName"
            value={values.fullName}
            onChange={handleInputChange}
          />
          <TextField
            variant="outlined"
            name="email"
            label="Email"
            value={values.email}
            onChange={handleInputChange}
          />
        </Grid>
        <Grid item xs={6}>
          <FormControl>
            <FormLabel>Gender</FormLabel>
            <RadioGroup
              row
              name="gender"
              value={values.gender}
              onChange={handleInputChange}
            >
              <FormControlLabel 
              value="male" 
             value="male" control={<Radio />} 
              label="Male" />
              <FormControlLabel
                value="female"
                control={<Radio />}
                label="Female"
              />
              <FormControlLabel
                value="other"
                control={<Radio />}
                label="Other"
              />
            </RadioGroup>
          </FormControl>
        </Grid>
      </Grid>
    </Form>
  );
}

```

# TextField Reutilizable
En primer lugar queremos mover el `TextField` control a un componente reutilizable (si bien no trabaja con múltiples componentes que se traducen en múltiples imports como el *radio button*), tiene la ventaja que si a futuro queremos hacer algún cambio tenemos que hacerlo en un único lugar.

Creamos una carpeta `/components/controls` y en ella `Input.js` (ojo al hacer el import que Material UI tiene un componente con el mismo nombre).
```jsx
import { TextField } from "@material-ui/core";
import React from "react";

export default function Input(props) {
  const { label, name, value, onChange } = props;
  return (
    <TextField
      variant="outlined"
      label={label}
      name={name}
      value={value}
      onChange={onChange}
    />
  );
}

```

Luego lo utilizamos en `EmployeeForm`
```jsx
<Input
   label="Full Name"
   name="fullName"
   value={values.fullName}
   onChange={handleInputChange}
 />
```

# Radio Group Reutilizable
Ahora repetimos lo mismo para los radio group buttons, nuevamente creamos un componente `RadioGroup` pero otra vez existe el conflicto de nombres ya que Material UI tiene un componente con el mismo nombre que tenemos que usar en ese archivo.
Para evitar estos inconvenientes creamos en `/controls` un componente `Controls`
```jsx
import Input from "./Input";
import RadioGroup from "./RadioGroup";

export default Controls = {
  Input,
  RadioGroup
};
```

Por lo que luego directamente en `EmployeeForm` importaremos el objeto `import Control from '../../components/controls/Controls` y luego utilizaremos los componentes con `<Controls.Input/>` y `<Controls.RadioGroup/>`

En `RadioGroup.js` notar que a la hora de importar el componente de Material UI con idéntico nombre debimos asignar un alias por lo que ponemos `import {RadioGroup as MuiRadioGroup} from '@material-ui/core'`
```jsx
import {
  FormControl,
  FormControlLabel,
  FormLabel,
  Grid,
  Radio,
  RadioGroup as MuiRadioGroup
} from "@material-ui/core";
import ReacInput from "react";

export default function RadioGroup(props) {
  const { label, name, value, onChange, items } = props;
  return (
    <FormControl>
      <FormLabel>{label}</FormLabel>
      <MuiRadioGroup row name={name} value={value} onChange={onChange}>
        {items.map((item) => (
          <FormControlLabel
            key={item.value}
            value={item.value}
            control={<Radio />}
            label={item.label}
          />
        ))}
      </MuiRadioGroup>
    </FormControl>
  );
}

```

Si queremos evitar el autocompletado en `useForm` podemos poner `autocomplete="off"`

# Select Reutilizable
Repetimos lo mismo para un componente en `/src/components/controls/Select`
Las opciones del dropdown normalmente provendrán de una API pero utilizaremos un array de objetos para simular de manera simple ese proceso.
Creamos una carpeta `services` en `/src` y dentro de ella `EmployeeService.js` que por el momento tendrá un array hardcodeado de departamentos.

```jsx
export const getDepartmentCollection = () => [
  { id: "1", title: "Development" },
  { id: "2", title: "Marketing" },
  { id: "3", title: "Accounting" },
  { id: "4", title: "HR" }
];
```

En `EmployeeForm` lo utilizamos importamos `import * as employee from "../../services/EmployeeService";` y luego lo utilizamos de este modo>

```jsx
<Controls.Select
   label="Department"
   name="departmentId"
   value={values.departmentId}
   onChange={handleInputChange}
   options={employee.getDepartmentCollection()}
 />
```


Luego en `Select.js`
```jsx
import {
  FormControl,
  InputLabel,
  MenuItem,
  Select as MuiSelect
} from "@material-ui/core";
import React from "react"
  hireDate: new Date(),
  isPermanent: false
};

export default function Select(propsEmployeeForm() {
  const { label, name, value, onChange, options } = props;

  return (
    <FormControl variant="outlined">
      <InputLabel>{label}</InputLabel>

      <MuiSelect name={name} value={value} onChange={onChange} label={label}>
        <MenuItem value="None" />

        {options.map((option) => (
          <MenuItem key={option.id} value={option.id}>
            {option.title}
          </MenuItem>
        ))}
      </MuiSelect>values, /*setValues,*/ handleInputChange } = useForm(initialValues);

  return (
    </FormControl>
  );
}
```

En este momento en la consola tendremos un error indicando **FindDOMNode is deprecated in StrictMode ** y para solucionarlo podemos sacar el strict mode en `index.js`

# Checkbox
Creamos un archivo `Checkbox.js` en `/src/components/controls`

EN `/src/components/controls/Controls.js` agregamos:
```jsx
import Input from "./Input";
import RadioGroup from "./RadioGroup";
import Select from "./Select";
import Checkbox from "./Checkbox";

const Controls = {
  Input,
  RadioGroup,
  Select,
  Checkbox
};

export default Controls;
```

Luego en `Checkbox` nuevamente como tenemos que utilizar un componente de Material UI con el mismo nombre utilizamos un alias. `import {Checkbox as MuiCheckbox} from @material-ui/core`

```jsx
import {
  FormControl,
  Checkbox as MuiCheckbox,
  FormControlLabel
} from "@material-ui/core";
import React from "react";

export default function Checkbox(props) {
  const { label, name, value, onChange } = props;
  return (
    <FormControl>
      <FormControlLabel
        control={
          <MuiCheckbox
            name={name}
            color="primary"
            checked={value}
            onChange={onChange}
          />
        }
        label={label>
      <Grid container>
        <Grid item xs={6}>
          <Input
            label="Full Name"
            name="fullName"
            value={values.fullName}
      />
    </FormControl>
  );
}

```

Sin embargo al hacer click en el checkbox obtenemos el siguiente error: 
> Warning: Failed prop type: Invalid prop `checked` of type `string` supplied to `ForwardRef(Checkbox)`, expected `boolean`.

El problema se origina en `handleInputChange` ya que como podemos observar con un `console.log(e)` no tenemos `name` ni `value` sino `checked` es por eso que creamos una función auxiliar `convertToDefaultEventParameter`

Finalmente `Checkbox` nos queda:
```jsx
import {
  FormControl,
  Checkbox as MuiCheckbox,
  FormControlLabel
} from "@material-ui/core";
import React from "react";

export default function Checkbox(props) {
  const { label, name, value, onChange } = props;

  const convertToDefaultEventParameter = (name, value) => ({
    target: {
      name,
      value
    }
  });

  return (
    <FormControl>
      <FormControlLabel
        control={
          <MuiCheckbox
            name={name}
            color="primary"
            checked={value}
            onChange={(e) =>
              onChange(convertToDefaultEventParameter(name, e.target.checked))
            }
          />
        }
        label={label}
      />
    </FormControl>
  );
}

```

# Datepicker
https://material-ui-pickers.dev/getting-started/installation

Instalamos `npm i @material-ui/pickers`

Luego
```
npm i @date-io/date-fns@1.x date-fns
```

Creamos `DatePicker.js` en `/controls`
```jsx
import DateFnsUtils from "@date-io/date-fns";
import {
  KeyboardDatePicker,
  MuiPickersUtilsProvider
} from "@material-ui/pickers";
import React from "react";

export default function DatePicker(props) {
  const { label, name, value, onChange } = props;
  return (
    <MuiPickersUtilsProvider utils={DateFnsUtils}>
      <KeyboardDatePicker
        disableToolbar
        variant="inline"
        inputVariant="outlined"
        label={label}
        formate="MMM/dd/yyyy"
        value={value}
        onChange={onChange}
      />
    </MuiPickersUtilsProvider>
  );
}

```

Actualizamos `Controls.js`
```jsx
import Input from "./Input";
import RadioGroup from "./RadioGroup";
import Select from "./Select";
import Checkbox from "./Checkbox";
import DatePicker from "./DatePicker";

const Controls = {
  Input,
  RadioGroup,
  Select,
  Checkbox,
  DatePicker
};

export default Controls;

```

Nuevamente tendremos que hacer una conversión para que el valor recibido en `handleInputChange` tenga las propiedades `name` y `value`

Finalmente `DatePicker` nos queda:
```jsx
import DateFnsUtils from "@date-io/date-fns";
import {
  KeyboardDatePicker,
  MuiPickersUtilsProvider
} from "@material-ui/pickers";
import React from "react";

export default function DatePicker(props) {
  const { label, name, value, onChange } = props;

  const convertToDefaultEventParameter = (name, value) => ({
    target: {
      name,
      value
    }
  });

  return (
    <MuiPickersUtilsProvider utils={DateFnsUtils}>
      <KeyboardDatePicker
        disableToolbar
        variant="inline"
        inputVariant="outlined"
        label={l          <FormLabel>Gender</FormLabel>
            <RadioGroup
              row
              name="gender"
              value={values.gender}
              onChange={handleInputChange}
            >
              <FormControlLabel value="male" control={<Radio />} label="Male" />
              <FormControlLabel}
        formate="MMM/dd/yyyy"
        value={value}"female"
        onChange={(date) =>
          onChange(convertToDefaultEventParameter(name, date))
                control={<Radio />}
      />
    </MuiPickersUtilsProvider>
  );
}

```

# Button
Una vez más agregamos un componente reutilizable en `controls` llamado `Button`
```jsx
import { Button as MuiButton } from "@material-ui/core";
import React from "react";

export default function Input(props) {
  const { text, variant, size, color, onClick } = props;
  return (
    <MuiButton variant={variant} size={size} color={color} onClick={onClick}>
      {text}
    </MuiButton>
  );
}
```

Luego actualizamos `Controls`
```jsx
import Input from "./Input";
import RadioGroup from "./RadioGroup";
import Select from "./Select";
import Checkbox from "./Checkbox";
import DatePicker from "./DatePicker";
import Button from "./Button";

const Controls = {
  Input,
  RadioGroup,
  Select,
  Checkbox,
  DatePicker,
  Button
};

export default Controls;
```

Como la mayoría de las veces `variant="contained"`, `color="primary"`, `size="large"` los definimos como valores default.
```jsx
import { Button as MuiButton } from "@material-ui/core";
import React from "react";

export default function Input(props) {
  const { text, variant, size, color, onClick } = props;
  return (
    <MuiButton
      variant={variant || "contained"}
      size={size || "small"}
      color={color || "primary"}
      onClick={onClick}
    >
      {text}
    </MuiButton>
  );
}

```
Por último lo incorporamos en `EmployeeForm`
```jsx
<div>
  <Controls.Button text="Submit" />
  <Controls.Button text="Reset" />
</div>
```

Si quisiéramos que el componente `Button` reciba `type="submit"` podríamos hacer lo siguiente en:
```jsx
import { Button as MuiButton } from "@material-ui/core";
import React from "react";

export default function Input(props) {
  const { text, variant, size, color, onClick, ...other } = props;
  return (
    <MuiButton
      variant={variant || "contained"}
      size={size || "small"}
      color={color || "primary"}
      onClick={onClick}
      {...other}
    >
      {text}
    </MuiButton>
  );
}

```

luego simplemente se lo pasaríamos de este modo:
```jsx
  <Controls.Button text="Submit" type='submit} />
```

Si quisiéraeremos darle estilos como por ejemplo un *padding* entre los botones, podemos hacerlo en el componente reutilizable `Button`.      InputBase
`InputBase` trata de un componente que podemos utilizar para personalizar inputs. 

Si queremos que tenga un ícono al comienzo podemos utilizar la prop `startAdornment` (`endAdornment` si lo queremos al final) [API](https://material-ui.com/api/input-base/)

# Estilos en Formularios
Cuando agregamos controles de un formulario como ser `TextInput` veremos que estos tienen la clase `.MuiFormControl-root` aplicada. Con esta clase podremos establecer el ancho y los márgenes por ejemplo.

```jsx
useStyles = makeStyles(theme => ({
	root: {
		"& .MuiFormControl-root":{
			width: '80%',
			margin: theme.spacing(1),
		}
		
	}
}));
```



# Validación en Formularios
https://www.youtube.com/watch?v=4YLa9iuN43c&ab_channel=CodAffection

En la documentación de Material UI podemos descubrir que para el caso de `TextField` con `error` para establecer que hay un error y con `helperText` para escribir el texto que indique de qué se trata.

En cuanto a la validación queremos tener las siguientes:
* `name` es requerido
* `email` tiene que tener el formato de correo electrónico
* `mobile` tiene que tener más de 9 caracteres
* `departmentId` debe tener un valor seleccionado.

En `EmployeeForm` creamos una función `validate`

Para validar el email creamos la siguiente RegEx `/$|.+@.+..+/` notar que con `$` indicmaos que el campo no es requerido por lo que puede estar vacío. Luego viene un `|` y finalmente `.+@.+..+` tendremos uno o mas caracteres un **arroba**, uno o más caracteres un **punto** y uno o más caracteres.

En `useForm` agregamos una nueva variable de estado ` const [errors, setErrors] = useState({});` y también los retornamos de modo de poder usarlos en `EmployeeForm`
```jsx
  const validate = () => {
    let temp = {};
    temp.fullName = values.fullName ? "" : "Requerido";
    temp.email = /$|.+@.+..+/.test(values.email) ? "" : "Correo Inválido";
    temp.mobile = values.mobile.length > 9 ? "" : "Cant. mín 9 caracteres";
    temp.departmentId = values.departmentId.length ? "" : "Requerido";

    setErrors({ ...temp });
  };
```

Queremos que la validación se produzca al hacer Submit del formulario, por lo que agregamos el método `handleSubmit` y el atributo`onSubmit` en el componente `Form`. Luego en `useForm` (en particular en el componente `Form`) debemos garantizar que esas props le lleguen al `<form>` y lo hacemos mediante el spread operator
```jsx
export function Form(props) {
  const { children, ...other } = props;
  const classes = useStyles();

  console.log(other);

  return (
    <form className={classes.root} {...other}>
      {children}
    </form>
  );
}
```

En `EmployeeForm` por el momento queremos mostrar un alerta en pantalla al enviar el formulario si las validaciones están bien.
Nótese que en `validate` con `return Object.values(temp).every((x) => x === "");` nos aseguramos que sea true si no hay errores y false si hay al menos un error. `Object.values(temp)` entrega un array con los valores de las propiedades de `temp` es decir algo así: `["Requerido","","",""]` y con el método `every()` chequeamos si existe algun error.

```jsx
import React from "react";
import { Grid } from "@material-ui/core";

import { useForm, Form } from "../../components/useForm";
import Controls from "../../components/controls/Controls";
import * as employee from "../../services/EmployeeService";

const genderItems = [
  { label: "Male", value: "male" },
  { label: "Female", value: "female" },
  { label: "Other", value: "other" }
];
const initialValues = {
  id: 0,
  fullName: "",
  email: "",
  mobile: "",
  city: "",
  gender: "male",
  departmentId: "",
  hireDate: new Date(),
  isPermanent: false
};

export default function EmployeeForm() {
  const {
    values,
    /*setValues,*/ errors,
    setErrors,
    handleInputChange
  } = useForm(initialValues);

  const validate = () => {
    let temp = {};
    temp.fullName = values.fullName ? "" : "Requerido";
    temp.email = /^$|.+@.+..+/.test(values.email) ? "" : "Correo Inválido";
    temp.mobile = values.mobile.length > 9 ? "" : "Cant. mín 10 caracteres";
    temp.departmentId = values.departmentId.length !== 0 ? "" : "Requerido";

    setErrors({ ...temp });
    console.log(temp);
    return Object.values(temp).every((x) => x === "");
  };

  const handleSubmit = (e) => {
    e.preventDefault();
    if (validate()) {
      alert("Valid");
    }
  };

  return (
    <Form onSubmit={handleSubmit}>
      <Grid container>
        <Grid item xs={6}>
          <Controls.Input
            label="Full Name"
            name="fullName"
            value={values.fullName}
            onChange={handleInputChange}
          />
          <Controls.Input
            label="Email"
            name="email"
            value={values.email}
            onChange={handleInputChange}
          />
          <Controls.Input
            label="Mobile"
            name="mobile"
            value={values.mobile}
            onChange={handleInputChange}
          />
          <Controls.Input
            label="City"
            name="city"
            value={values.city}
            onChange={handleInputChange}
          />
        </Grid>
        <Grid item xs={6}>
          <Controls.RadioGroup
            label="Gender"
            name="gender"
            value={values.gender}
            onChange={handleInputChange}
            items={genderItems}
          />
          <Controls.Select
            label="Department"
            name="departmentId"
            value={values.departmentId}
            onChange={handleInputChange}
            options={employee.getDepartmentCollection()}
          />
          <Controls.Checkbox
            label="Is Permanent?"
            name="isPermanent"
            value={values.isPermanent}
            onChange={handleInputChange}
          />
          <Controls.DatePicker
            label="Hire Date"
            name="hireDate"
            value={values.hireDate}
            onChange={handleInputChange}
          />

          <div>
            <Controls.Button text="Submit" type="submit" />
            <Controls.Button text="Reset" color="default" />
          </div>
        </Grid>
      </Grid>
    </Form>
  );
}
```

Para lograr mostrar los mensajes de error en los componentes reutilizables les agregamos la prop `error={errors.fullName}` 

```jsx
<Controls.Input
  label="Full Name"
  name="fullName"
  value={values.fullName}
  onChange={handleInputChange}
  error={errors.fullName}
/>
```

En el control `Input` recibir dicha prop con valor null por default y dependiendo de `error` cargar `error` y `helperText`

```jsx
import { TextField } from "@material-ui/core";
import React from "react";

export default function Input(props) {
  const { label, name, value, error = null, onChange } = props;
  return (
    <TextField
      variant="outlined"
      label={label}
      name={name}
      value={value}
      onChange={onChange}
      {...(error && { error: true, helperText: error })}
    />
  );
}

```
Repitiendo esto mismo para el `Select` nuevamente recibimos una prop con valor default null `error = null` y en caso de que tenga un valor distinto agregamos el atributo `error` a `<FormControl>` y de manera similar si existe `error` mostramos el mensaje de error con `{error && <FormHelperText>{error}</FormHelperText>}`

```jsx
import {
  FormControl,
  FormHelperText,
  InputLabel,
  MenuItem,
  Select as MuiSelect
} from "@material-ui/core";
import React from "react";

export default function Select(props) {
  const { label, name, value, onChange, options, error = null } = props;

  console.log(error);

  return (
    <FormControl variant="outlined" {...(error && { error: true })}>
      <InputLabel>{label}</InputLabel>

      <MuiSelect name={name} value={value} onChange={onChange} label={label}>
        <MenuItem value="None" />

        {options.map((option) => (
          <MenuItem key={option.id} value={option.id}>
            {option.title}
          </MenuItem>
        ))}
      </MuiSelect>
      {error && <FormHelperText>{error}</FormHelperText>}
    </FormControl>
  );
}

```

En `useForm` desarrollamos el método para resetear el formulario y lo retornamos para poder usarlo en `EmployeeForm`
```jsx
const resetForm = () => {
    setValues(initialValues);
    setErrors({});
  };
```
Luego lo invocamos en `EmployeeForm`
```jsx
<Controls.Button text="Reset" color="default" onClick={resetForm} />

```

# Validación en Tiempo Real
Hasta el momento solo estamos validando al enviar el formulario pero queremos hacerlo al presionar una tecla. Para ello agregamos dos parámetros mas en `useForm()` una es `validateOnChange` con la cual indicamos si queremos este tipo de validación en tiempo real o no (valor default `false`) y la otra `validate` para llamar cada vez que hay un cambio (en `handleInputChange`)

Hasta el momento tenemos esta función de validación:
```jsx
const validate = () => {
  let temp = {};
  temp.fullName = values.fullName ? "" : "Requerido";
  temp.email = /^$|.+@.+..+/.test(values.email) ? "" : "Correo Inválido";
  temp.mobile = values.mobile.length > 9 ? "" : "Cant. mín 10 caracteres";
  temp.departmentId = values.departmentId.length !== 0 ? "" : "Requerido";

  setErrors({ ...temp });

  return Object.values(temp).every((x) => x === "");
};
```

Sin embargo queremos que si ingresamos un caracter en un campo sólo chequee la validación en ese, por lo que desde `useForm()` la invocamos con `validate({[name]:value})`
Notar que ponemos `let temp = {...errors};` para evitar que una validación individual nos borre errores de validación de otros campos.
Por último solo retornamos `true` si todas las validaciones son válidas si invocamos a este método desde `handleInput` y no con la validación individual en tiempo real: `if(fieldValues === values)`
```jsx
 const validate = (fieldValues = values) => {
    let temp = {...errors};
    if ("fullName" in fieldValues)
      temp.fullName = values.fullName ? "" : "Requerido";
    if ("email" in fieldValues)
      temp.email = /^$|.+@.+..+/.test(values.email) ? "" : "Correo Inválido";
    if ("mobile" in fieldValues)
      temp.mobile = values.mobile.length > 9 ? "" : "Cant. mín 10 caracteres";
    if ("departmentId" in fieldValues)
      temp.departmentId = values.departmentId.length !== 0 ? "" : "Requerido";

    setErrors({ ...temp });

	if(fieldValues === values)
	    return Object.values(temp).every((x) => x === "");
  };
```

Si ingresamos un caracter por ejemplo en `fullName` veremos que aparece el mensaje Requerido recién al agregar un segundo caracter este desaparece. Esto se debe a la naturaleza asincrónica de setValues y que por consecuencia la validación se efectúa antes de actualizar el valor del estado.
Usamos `fieldValues` en lugar de `values` en `validate`
```jsx

```