# Next.js con MongoDB Mongoose
Para trabajar con Next.js y mongoose debemos basarnos en el siguiente [repositorio](https://github.com/vercel/next.js/blob/canary/examples/with-mongodb-mongoose).

## Conexión a la Base de Datos:
En `utils` creamos un archivo `dbConnect.js` en el cual exportamos como defauit una función con el mismo nombre.
```js
import mongoose from 'mongoose'

async function dbConnect() {
  // check if we have a connection to the database or if it's currently
  // connecting or disconnecting (readyState 1, 2 and 3)
  if (mongoose.connection.readyState >= 1) {
    return
  }

  return mongoose.connect(process.env.MONGODB_URI, {
    useNewUrlParser: true,
    useUnifiedTopology: true,
    useFindAndModify: false,
    useCreateIndex: true,
  })
}

export default dbConnect
```

La propiedad `readyState` tiene cuatro posibles valores: 0 = desconectado, 1 = conectado, 2 = conectando y 3 = desconectando por lo que cualquier valor `>=1` estará conectado y es por eso que en ese caso no hacemos nada.

Cuando utilicemos MongoDB en la nube en la cadena de conexión estará nuestra contraseña, para evitar que esta sea subida a GitHub y vista por otras personas, trabajamos con una variable de entorno que tenemos en un archivo `.env.local` (al trabajar con Next.js ya tenemos este archivo en el `.gitignore`).


## Creación del Modelo
En la raíz también creamos un directorio `models` y dentro un achivo `Pet.js` donde colocaremos el modelo.
```js
import mongoose from 'mongoose'

/* PetSchema will correspond to a collection in your MongoDB database. */
const PetSchema = new mongoose.Schema({
  name: {
    type: String,
    required: [true, 'Please provide a name for this pet.'],
    maxlength: [20, 'Name cannot be more than 60 characters'],
  },
  owner_name: {
    type: String,
    required: [true, "Please provide the pet owner's name"],
    maxlength: [20, "Owner's Name cannot be more than 60 characters"],
  },
  species: {
    type: String,
    required: [true, 'Please specify the species of your pet.'],
    maxlength: [30, 'Species specified cannot be more than 40 characters'],
  },
  age: {
    type: Number,
  },
  poddy_trained: {
    type: Boolean,
  },
  diet: {
    type: Array,
  },
  image_url: {
    required: [true, 'Please provide an image url for this pet.'],
    type: String,
  },
  likes: {
    type: Array,
  },
  dislikes: {
	type: Array,
  },
})

export default mongoose.models.Pet || mongoose.model('Pet', PetSchema)
```

> Prestar atención al modo en que exportamos `export default mongoose.models.Pet || mongoose.model('Pet', PetSchema)` para evitar mensaje de error **“Cannot overwrite model once compiled” **.

> Si modificamos este archivo y los cambios no se reflejan probar eliminando la carpeta `.next`.

# Listado y Agregado de Documentos
En `pages/api/pets` creamos un archivo `index.js`

```js
import dbConnect from '../../../utils/dbConnect'
import Pet from '../../../models/Pet'

export default async function handler(req, res) {
  const { method } = req

  await dbConnect()

  switch (method) {
    case 'GET':
      try {
        const pets = await Pet.find({}) /* find all the data in our database */
        res.status(200).json({ success: true, data: pets })
      } catch (error) {
        res.status(400).json({ success: false })
      }
      break
    case 'POST':
      try {
        const pet = await Pet.create(
          req.body
        ) /* create a new model in the database */
        res.status(201).json({ success: true, data: pet })
      } catch (error) {
        res.status(400).json({ success: false })
      }
      break
    default:
      res.status(400).json({ success: false })
      break
  }
}
```