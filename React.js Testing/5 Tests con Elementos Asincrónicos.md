# Tests con Elementos Asincrónicos
Supongamos que realizamos un GET request a una API externa y cargamos datos en una lista. Como la carga de estos elementos llevará un tiempo, si testeamos que una de esas cards aparezca del modo que venimos trabajando, esto no funcionará ya que durante un tiempo no existirá debido al tiempo que le lleva obtener esos datos.

Como dijimos anteriormente en situaciones donde tenemos datos asincrónicos debemos utilizar `findBy`

Suponemos que tenemos el siguiente componente `FollowersList`
```jsx
import React, { useEffect, useState } from 'react'
import "./FollowersList.css"
import axios from "axios"
import { Link } from 'react-router-dom';

export default function FollowersList() {
    const [followers, setFollowers] = useState([]);

    useEffect(() => {
        const fetchFollowers = async () => {
            const { data } = await axios.get("https://randomuser.me/api/?results=5")
            setFollowers(data.results)
        }
        fetchFollowers()
    }, []);

    return (
        <div className="followerslist-container">
            <div>
                {followers.map((follower, index) => (
                    <div className="follower-item" data-testid={`follower-item-${index}`}>
                        <img src={follower.picture.large}/>
                        <div className="followers-details">
                            <div className="follower-item-name">
                                <h4>{follower.name.first}</h4> <h4>{follower.name.last}</h4>
                            </div>
                            <p>{follower.login.username}</p>
                        </div>
                    </div>
                ))}
            </div>
            <div className="todo-footer">
                <Link to="/">Go Back</Link>
            </div>
        </div>
    )
}

```

> En Visual Studio Code si queremos crear un directorio y dentro de el un archivo podemos hacerlo con `__test__/FollowerList.test.js`

Si creamos un test simple veremos que nuevamente falla y nos muestra el error `You should not use <Link> outside a <Router>]` debido a que en `FollowersList` estamos usando el componente `Link` para navegar a `/`.

```jsx
import { render } from '@testing-library/react';
import FollowersList from '../FollowersList';

describe('FollowersList', () => {
	it('should render component', () => {
		render(<FollowersList />);
	});
});
```

Por ese motivo creamos el componente `MockFollowers` y queremos chequear que aparezca el primer elemento de la lista de personas.

```jsx
const MockFollowersList = () => {
    return (
        <BrowserRouter>
            <FollowersList />
        </BrowserRouter>
    )
}
```