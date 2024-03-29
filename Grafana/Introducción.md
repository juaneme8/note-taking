# Grafana

Grafana es una herramienta open source que nos permite visualizar métricas y logs en cualquier lugar donde estén almacenadas. Es posible convertir datos de una TSDB (time-series database) en gráficos.



## Grafana con Imagen Docker

A la hora de correr Grafana utilizando Docker, acuerdo a la [documentación](https://grafana.com/docs/grafana/latest/setup-grafana/installation/docker/) podemos elegir la imagen Grafana Enterprise (`grafana/grafana-enterprise`) o Grafana Open Source (`grafana/grafana-oss`), elegimos esta última.

```bash
docker run -d
-p 3000:3000
--name=grafana
grafana/grafana-oss
```



Grafana Cloud es una alternativa para evitar tener que hacer varios de estos procesos manualmente.



## Persistencia de Datos

> Basado en la [documentación de una versión anterior](https://grafana.com/docs/grafana/v9.0/setup-grafana/configure-docker/) donde encontramos información sobre configuraciones de la imagen de Docker. 

En la medida que no designemos una ubicación para almacenar la información, todos los datos de Grafana desaparecerán con el contenedor, es por eso que es aconsejable contar con un almacenamiento persistente para guardar esa información.

```bash
docker run -d
-p 3000:3000
-v grafana-storage:/var/lib/grafana
--name=grafana
grafana/grafana-oss
```

Con `docker volume ls` podemos ver que el volumen fue creado exitosamente.

## Instalar Plugins

Queremos importar datos de Google Sheets, para eso necesitamos usar el [plugin](https://grafana.com/grafana/plugins/grafana-googlesheets-datasource/) disponible para tal fin.

El comando que nos sugiere para tal fin es:

```
grafana-cli plugins install grafana-googlesheets-datasource
```

Podemos ejecutarlo en el contenedor que está corriendo con `docker exec -it <container_id> sh`.



Como podemos ver en la [documentación](https://grafana.com/docs/grafana/latest/setup-grafana/installation/docker/#install-official-and-community-grafana-plugins) a la hora de instalar plugins debemos usar la variable de entorno `GF_INSTALL_PLUGINS` pasándole los elementos separados por comas. Esto ejecutará para cada plugin el comando `grafana-cli plugins install ${plugin}` y los instalará cuando Grafana inicie.

```bash
docker run -d 
-p 3000:3000 
-e "GF_INSTALL_PLUGINS=grafana-clock-panel,grafana-simple-json-datasource"
--name=grafana 
grafana/grafana-oss
```



Por lo que en nuestro caso el comando completo nos queda:

```bash
docker run -d 
-p 3000:3000
-e "GF_INSTALL_PLUGINS=grafana-googlesheets-datasource"
-v grafana-storage:/var/lib/grafana
--name=grafana
grafana/grafana-oss
```



## Primeros Pasos

Luego vamos a http://localhost:3000 y nos pedirá el nombre de usuario y contraseña que completamos con usuario `admin` y password `admin` y luego nos indicará que ingresemos el password definitivo.



# Dashboard Interactivo

> :link: Basado en el [video de Caos Binario](https://youtu.be/GR8QdBrHhw0)
>
> Google Sheet Ejemplo: https://docs.google.com/spreadsheets/d/1TZlZX67Y0s4CvRro_3pCYqRCKuXer81oFp_xcsjPpe8



## Crear Dashboard

Supongamos que tenemos en un datasheet con datos de los errores que fueron arrojando nuestros servidores como vemos a continuación (no es exactamente igual al mostrado en el video pero a los fines didácticos sirve).

|       Time       |    Server    | Error code |    Path    |
| :--------------: | :----------: | :--------: | :--------: |
| 2022-10-05 10:30 | srv-front-01 |    404     | /files.txt |
| 2022-10-05 10:31 | srv-front-01 |    503     | /files.txt |
| 2022-10-05 10:32 |  srv-db-02   |    404     |   /.env    |
| 2022-10-05 10:33 | srv-back-03  |    503     |     /      |
| 2022-10-05 10:34 | srv-back-03  |    503     |   /.env    |
| 2022-10-05 10:35 |  srv-db-02   |    404     | /files.txt |
| 2022-10-05 10:36 | srv-front-02 |    404     | /files.txt |
| 2022-10-05 10:37 | srv-front-01 |    503     |     /      |
| 2022-10-05 10:38 |  srv-db-02   |    404     |   /.env    |
| 2022-10-05 10:39 | srv-back-02  |    503     |     /      |
| 2022-10-05 10:40 | srv-back-03  |    503     |   /.env    |
| 2022-10-05 10:41 |  srv-db-02   |    404     | /files.txt |
| 2022-10-05 10:42 | srv-front-01 |    404     | /files.txt |

Queremos obtener un **gráfico de tortas** que muestre la cantidad de errores que ha tenido cada servidor 

:warning: En el video se plantea que además al hacer click en cada uno de estas porciones nos muestre un deglose de qué tipo de errores ha tenido para lo cual se utilizan variables pero :rotating_light: **esto no es posible utilizando datos provenientes de google sheets.* *:rotating_light: 



### Agregar data source

#### Google Sheets

Vamos a **Configuration** :arrow_right:  **Add Data Source** y buscamos **Google Sheets**.

#### API Key Google

Tendremos que completar el apartado **API Key** para eso seguimos las siguientes instrucciones que nos aparecen en la misma UI:

1. Ir a la [página de  credenciales](https://console.developers.google.com/apis/credentials) en Google API Console.
2. Hacer click en **Create Credentials** y luego en **API key**.
3. Copiar la clave y pegarla en el campo API Key, los contenidos serán encriptados y guardados en la base de datos de Grafana.
3. Presionamos **Save & test** y nos aparecerá **Success**.



### Nuevo Dashboard

Luego vamos a **Dashboards**:arrow_right: **New Dashboard** y a continuación a **Add new Panel** donde veremos que ya nos aparece como data source Google Sheets.

* **SpreadsheetID**: colocamos el id de la URL de Google Sheet. 

  > Por ejemplo 1TZlZX67Y0s4CvRro_3pCYqRCKuXer81oFp_xcsjPpe8

  

* **Range**: especificamos las celdas desde y hasta donde queremos trabajar. 

  > Por ejemplo: A1:D13

> Si luego de hacerlo si creamos un nuevo Dashboard y vinculamos un Google Sheet nos mostrará:
>
> ```
> googleapi: Error 403: Google Sheets API has not been used in project 842066866158 before or it is disabled. Enable it by visiting https://console.developers.google.com/apis/api/sheets.googleapis.com/overview?project=842066866158
> ```
>
> Debemos visitar ese enlace y nos preguntará si queremos habilitar la API de Google Sheets. 
>
> :rotating_light: De todos modos probar si dejando pasar unos minutos después de cargar la API key este error desaparece solo (sin entrar al enlace).

* En el desplegable de los distintos tipos de gráficos elegimos Pie Chart

  ![image-20221031112203698](Introducción.assets/image-20221031112203698.png)



> Además el switch Table View nos permitirá pasar a una vista en modo de tabla.



* En el apartado de Transform elegimos las transformaciones que queremos realizar a los datos:

  **Add transformation** :arrow_right: **Group by**

  * Con la variable "Server" elegimos Group By
  * Con la variable "Error Code" elegimos Calculate :arrow_right: Count


<img src="Introducción.assets/image-20221031112438692.png" alt="image-20221031112438692" style="zoom:67%;" />



* Por último dentro de la configuración del Pie chart en Show elegimos **All values**.

<img src="Introducción.assets/image-20221031112329637.png" alt="image-20221031112329637" style="zoom: 67%;" />



Con algunos ajustes de las leyendas nos quedará:

<img src="Introducción.assets/image-20221031113725492.png" alt="image-20221031113725492" style="zoom:50%;" />





### Agregar Variables

Ahora queremos que al hacer click en una zona del gráfico nos muestre el detalle de en qué consistieron esos errores. Para eso tendremos que hacer uso de una variable.

En primer hacemos click en **Dashboard Settings** (engranaje de la imagen de abajo) :arrow_right: **Variables** y luego **New variable**.

![image-20221031114633843](Introducción.assets/image-20221031114633843.png)

En cuanto al **Type** elegimos **Text box** pero podríamos elegir por ejemplo query y determinar que sean todos los valores DISTINCT de nombre de servidor.

<img src="Introducción.assets/image-20221031114321788.png" alt="image-20221031114321788" style="zoom: 67%;" />



Ahora en la parte superior veremos el valor de la variable:

<img src="Introducción.assets/image-20221031114405783.png" alt="image-20221031114405783" style="zoom:67%;" />



### Agregar nuevo panel

![image-20221031114609387](Introducción.assets/image-20221031114609387.png)



Luego configuramos el mismo Spreadsheet ID, Range

A continuación configuramos la siguiente transformación:

# Alertas

> :link: Basado en el [video de Caos Binario](https://youtu.be/lWd4r44bHHo)

Vamos a crear alertas desde Grafana, si bien podríamos utilizar el Alert Manager de Prometheus en ocasiones estaremos utilizando también a [Loki](https://grafana.com/oss/loki/) por lo que no queremos restringirnos a hacerlo sólo desde una herramienta.

:warning: INCOMPLETO.