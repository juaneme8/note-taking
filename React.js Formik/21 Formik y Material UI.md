# Integrar Formik con Material UI
> Basado en el video https://www.youtube.com/watch?v=ziWJ4k_3BLk&t=212s&ab_channel=JustinKim
> sandbox propio: https://codesandbox.io/s/mui-formik-login-form-lfq3f

Trabajaremos con un formulario de Registro, creamos un componente llamado `SignUpForm` e incorporamos Formik en primer lugar.

```jsx
import React from "react";
import { Formik, Form, Field, ErrorMessage } from "formik";
import * as Yup from "yup";

const initialValues = {
  name: "",
  position: ""
};

const validationSchema = Yup.object().shape({
  name: Yup.string().min(2, "Muy corto").required("Requerido"),
  position: Yup.string().required()
});

const handleSubmit = (values) => {
  alert(JSON.stringify(values));
};

export default function LoginForm() {
  return (
    <Formik
      initialValues={initialValues}
      onSubmit={handleSubmit}
      validationSchema={validationSchema}
    >
      {({ dirty, isValid }) => {
        return (
          <Form>
            <div>
              <label htmlFor="name">Name:</label>
              <Field name="name" id="name" as="input" />
              <ErrorMessage name="name" />
            </div>
            <div>
              <label htmlFor="position">Position:</label>
              <Field name="position" id="position" as="select">
                <option value=""></option>
                <option value="front-end">Front End</option>
                <option value="back-end">Back End</option>
                <option value="dev-ops">Dev Ops</option>
                <option value="qa">QA</option>
              </Field>
              <ErrorMessage name="position" />
              <button type="submit" disabled={!dirty || !isValid}>
                Submit
              </button>
            </div>
          </Form>
        );
      }}
    </Formik>
  );
}

```

# `FormikField`
Queremos obtener componente reutilizables para ello primero creamos al componente `FormikField.js` (si bien tendrá también Material UI consideramos a la hora de elegir el nombre que sin Formik el componente no tendría sentido).

> **TIP Estructura de Trabajo**
> En lugar de crear un archivo en `components` llamado `FormikField.js` podríamos crear una carpeta `FormikField` y dentro de ella un archivo `index.js` y otro `FormikField.css` de esta manera tendríamos una carpeta para cada componente. Luego retornamos un div exterior con `className='FormikField'`

En primer lugar nos aseguramos renderizar el componente de Material UI `<TextField/>`
```jsx
import { TextField } from "@material-ui/core";
import React from "react";

export default function FormikField({ label }) {
  return <TextField id="standard-basic" label={label} fullWidth />;
}

```
Luego utilizaremos el componente de Formik `<Field/>` con el atributo `as={TextField}`

```jsx
import { TextField } from "@material-ui/core";
import { Field } from "formik";
import React from "react";

export default function FormikField({ name, label }) {
  return <Field as={TextField} name={name} label={label} fullWidth />;
}

```
Para mostrar los errores utilizamos el elemento de Material UI que dice "Some Important Text" que utiliza el atributo `helperText` y esto lo vinculamos con el componente de Formik `ErrorMessage` de la siguiente forma

```jsx
 helperText={<ErrorMessage name={name} /
```

De esta manera hemos logrado obtener un componente reutilizable:
```jsx
import { TextField } from "@material-ui/core";
import { ErrorMessage, Field } from "formik";
import React from "react";

export default function FormikField({ name, label }) {
  return (
    <Field
      as={TextField}
      name={name}
      label={label}
      fullWidth
      helperText={<ErrorMessage name={name} />}
    />
  );
}

```

Para asegurarnos que sea verdaderamente reutilizable y que por ejemplo si queremos hacer un formulario con un campo de contraseñas no nos muestre los caracteres ingresados, agregamos una prop mas llamada `type`la cual será opcional cosa que hacemos con : `export default function FormikField({ name, label, type = "text" }) {`

En `SignUpForm` reutilizaremos `FormikField` de la siguiente forma;
```jsx
<FormikField label="Name" name="name" />
<FormikField label="Email" name="email" type="email" />
<FormikField label="Password" name="password" type="password" />
```

# `FormikSelect`
Creamos el componente `FormikSelect` y en la documentación **Inputs -> Select** elegimos como base el ejemplo que utiliza `<FormHelperText>`
https://material-ui.com/components

Copiamos el snippet y consideramos que recibimos por el momento sólo una prop `label`.
```jsx
import {
  FormControl,
  InputLabel,
  MenuItem,
  Select,
  FormHelperText
} from "@material-ui/core";
import React from "react";

export default function FormikSelect({ label }) {
  return (
    <FormControl fullWidth>
      <InputLabel>{label}</InputLabel>
      <Select>
        <MenuItem value={10}>Front End</MenuItem>
        <MenuItem value={20}>Back End</MenuItem>
        <MenuItem value={30}>Dev Ops</MenuItem>
        <MenuItem value={30}>QA</MenuItem>
      </Select>
      <FormHelperText>Requerido</FormHelperText>
    </FormControl>
  );
}

```

En este momento tenemos hardcodeados las opciones del select, por lo que en `SignUp` creamos un array de objetos llamado `positionItems` que luego le pasaremos como prop a `FormikSelect`
```jsx
const positionItems = [
  { label: "Front End", value: "front-end" },
  { label: "Back End", value: "back-end" },
  { label: "Dev Ops", value: "dev-ops" },
  { label: "QA", value: "qa" }
];
```

Ahora recibimos esta prop en `FormikSelect` y en lugar de tener:
```jsx
<Select>
  <MenuItem value={10}>Front End</MenuItem>
  <MenuItem value={20}>Back End</MenuItem>
  <MenuItem value={30}>Dev Ops</MenuItem>
  <MenuItem value={30}>QA</MenuItem>
</Select>
```
Tendremos:
```jsx
  <Select>
  {items.map((item) => (
    <MenuItem key={item.value} value={item.value}>
      {item.label}
    </MenuItem>
  ))}
</Select>
```

Dentro del `FormikSelect` crearemos otro componente `MaterialUISelectField` y lo hacemosdentro de ese archivo ya que no será exportado, en ese componente retornamos todo el JSX que teníamos hasta el momento `<FormControl...>`

```jsx
import {
  FormControl,
  InputLabel,
  MenuItem,
  Select,
  FormHelperText
} from "@material-ui/core";
import { ErrorMessage, Field } from "formik";
import React from "react";

const MaterialUISelect = ({ label, children, errorString }) => {
  return (
    <FormControl fullWidth>
      <InputLabel>{label}</InputLabel>
      <Select>{children}</Select>
      <FormHelperText>{errorString}</FormHelperText>
    </FormControl>
  );
};

export default function FormikSelect({ name, label, items }) {
  return (
    <Field
      name={name}
      as={MaterialUISelect}
      label={label}
      errorString={<ErrorMessage name={name} />}
    >
      {items.map((item) => (
        <MenuItem key={item.value} value={item.value}>
          {item.label}
        </MenuItem>
      ))}
    </Field>
  );
}

```
Notar que hacemos uso de la prop `children` y el `map` lo hacemos en `FormikSelect`
Agregamos la prop `errorString` con la cual por el momento hardcodeamos un mensaje y lo pasamos como prop.
Agregamos la prop `name` que la asociamos a `<Field>` y `<ErrorMessage>`.

Para que esté conectado con Formik debemos asegurarnos que reciba `value`, `onBlur`, `onChange` con las que contamos por usar el componente `<Field/>` por lo que las agregamos como props recibidas y luego las incorporamos a `<Select/>`

```jsx
import {
  FormControl,
  InputLabel,
  MenuItem,
  Select,
  FormHelperText
} from "@material-ui/core";
import { ErrorMessage, Field } from "formik";
import React from "react";

const MaterialUISelect = ({
  label,
  children,
  errorString,
  value,
  name,
  onChange,
  onBlur
}) => {
  return (
    <FormControl fullWidth>
      <InputLabel>{label}</InputLabel>
      <Select name={name} value={value} onChange={onChange} onBlur={onBlur}>
        {children}
      </Select>
      <FormHelperText>{errorString}</FormHelperText>
    </FormControl>
  );
};

export default function FormikSelect({ name, label, items }) {
  return (
    <Field
      name={name}
      as={MaterialUISelect}
      label={label}
      errorString={<ErrorMessage name={name} />}
    >
      {items.map((item) => (
        <MenuItem key={item.value} value={item.value}>
          {item.label}
        </MenuItem>
      ))}
    </Field>
  );
}

```

Con el atributo `required` que le pasamos a `<Field>` colocaremos un asterisco al costado del label lo cual nos dará la idea de que se trata de un campo requerido. Implementamos esto mediante props para que el componente sea también reutilizable. Para esta prop utilizamos un valor por default de `false`
En el caso de `FormikSelect` se lo pasamos también a `MaterialUISelect` y lo colocamos no en `Field` sino en `InputLabel`

Finalmente `FormikSelect` nos queda:
```jsx
import {
  FormControl,
  InputLabel,
  MenuItem,
  Select,
  FormHelperText
} from "@material-ui/core";
import { ErrorMessage, Field } from "formik";
import React from "react";

const MaterialUISelect = ({
  label,
  children,
  errorString,
  value,
  name,
  onChange,
  onBlur,
  required
}) => {
  return (
    <FormControl fullWidth>
      <InputLabel required={required}>{label}</InputLabel>
      <Select name={name} value={value} onChange={onChange} onBlur={onBlur}>
        {children}
      </Select>
      <FormHelperText>{errorString}</FormHelperText>
    </FormControl>
  );
};

export default function FormikSelect({ name, label, items, required = false }) {
  return (
    <Field
      required={required}
      name={name}
      as={MaterialUISelect}
      label={label}
      errorString={<ErrorMessage name={name} />}
    >
      {items.map((item) => (
        <MenuItem key={item.value} value={item.value}>
          {item.label}
        </MenuItem>
      ))}
    </Field>
  );
}

```

## Incorporar Button
En primer lugar importamos el componente:
```jsx
import  {  Button  }  from  "@material-ui/core";
```

Luego reemplazamos `button` por `Button` indicando además de las props que ya tenemos `variant="contained"` y `color="primary"`:
```jsx
<Button
  variant="contained"
  color="primary"
  type="submit"
  disabled={!dirty || !isValid}
>
  Submit
</Button>
```

# `FormikCheckbox`


Creamos un componente `FormikSelect`

