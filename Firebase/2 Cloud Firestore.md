# Cloud Firestore
Cloud Firestore nos permite trabajar con bases de datos en Firebase.

# Creación DB
En **Project Overview** dentro de **Build** vamos a **Firestore Database** y luego hacemos click en **Create database**. 

En el modal de creación elegimos **Start in test mode** y presionamos Next. 

Luego dejamos **nam5 (us-central)** como Cloud Firestore location y presionamos **Enable**.

# Agregar al Proyecto
Haciendo uso de Firestore vamos a escribir en una colección y a leer de ella. Para eso en `components` creamos una carpeta `firestore` y dentro de ella dos archivos `Write.js` y `Read.js`.

## Escritura en DB
En `Write.js` realizaremos una escritura de cada uno de los tipos de datos que podemos utilizar en Firestore.

```jsx
import firebase from 'firebase/app'
import 'firebase/firestore'

const WriteToFirestore = () =>{
    const sendData = () =>{
        try{
            firebase
                .firestore()
                .collection('myCollection')
                .doc('my_document')
                .set({
                    string_data: 'Juan Ocho',
                    number_data: 2,
                    boolean_data: true,
                    map_data: { stringInMap: 'Hi', numberInMap: 7 },
                    array_data: ['text', 4],
                    null_data: null,
                    time_stamp: firebase.firestore.Timestamp.fromDate(new Date('December 17, 1995 03:24:00')),
                    geo_point: new firebase.firestore.GeoPoint(34.714322, -131.468435)
                })
                .then(alert('Data was successfully sent to firestore'))
        }   
        catch(error){
            console.log(error)
        }
    }

    return (
        <button onClick={sendData}>Send data to Firestore</button>
    )
}
export default WriteToFirestore;
```
Buscará la colección `myCollection` y la creará si no existe.
Hemos especificado `doc('my_document')`, por lo cual buscará un documento con ese nombre y si no existe lo creará. Si hubiéramos puesto `doc()` le asignaría un *unique name*, pero como queremos leerlo luego es que le ponemos ese nombre.

Si volviéramos a hacer un `.set()` en ese mismo documento sólo con un valor de `string_data` actualizado esto sobrescribiría todos los otros datos dejando sólo ese campo. Para evitar eso debemos utilizar como segundo argumento del set `{ merge: true }` de modo tal que sólo modifique esa propiedad y deje intacta las otras.

Luego en `index.js` importamos este componente `import WriteToFirestore  from '../components/firestore/Write'` y luego lo utilizamos.

## Lectura de DB
La lectura la efectuamos usando `.onSnapshot()` del siguiente modo  `firebase.firestore().collection('myCollection').doc('myDocument').onSnapshot(doc => console.log(doc.data())`  notar que no retorna una promesa sino que trabajamos con callback.

```jsx
import firebase from 'firebase/app'
import 'firebase/firestore'

const ReadFromFirestore = () => {
    const readData = () => {
        try {
            firebase.
            firestore()
            .collection('myCollection')
            .doc('my_document')
            .onSnapshot(doc => {
                
                console.log(doc.data())
                alert('Data was successfully fetched')
            });
            
        } catch (error) {
            console.log(error)
        }
      
    }
    return (
        <button onClick={readData}>Read data from Firestore</button>
    )
}

export default ReadFromFirestore;

```