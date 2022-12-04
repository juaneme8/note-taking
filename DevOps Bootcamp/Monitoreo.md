# Monitoreo

> Basado en el [video de Caos Binario](https://youtu.be/PCJwJpbln6Q)

## Introducción

En DevOps es fundamental el monitoreo de recursos, métricas y logs. Lo hacemos con Docker ya que nos permitirá generar nuestros archivos y luego simplemente tendremos que utilizarlos para monitorear nuestros servidores.



## Tipos de monitoreo

* Monitoreo a nivel servidor (RAM, CPU, Disco)
* Monitoreo a nivel contenedores (conocimiento de recursos por cada uno).
* Monitoreo a nivel aplicaciones (para por ejemplo conocer la cantidad de conexiones a la DB que está haciendo o la cantidad de requests x segundo que está recibiendo)
* Monitoreo de logs (a nivel servidor y a aplicación)

Vamos a centralizar todo esto en Grafana. Tendremos un servidor central que usamos para monitoreo y servidores donde están corriendo las aplicaciones.



## Contenedores de Monitoreo

![image-20221203151321691](Monitoreo.assets/image-20221203151321691.png)

> Los sentidos de las flechas indican desde dónde me conecto a dónde. Es decir que desde Grafana me conecto a Prometheus.

### node_exporter

Nos permite obtener las métricas del consumo de recursos (ram, cpu, disco) a nivel servidor. Herramienta creada por los creadores de Prometheus.



### cAdvisor

Nos permite obtener las métricas del consumo de recursos (ram, cpu, disco, networking) a nivel contenedores. Herramienta creada por Google.



### app_example

Aplicación de ejemplo con el endpoint de Prometheus activado. Importando una librería de modo que dentro de la aplicación exista un endpoint que devuelve info.



### prometheus

Es el encargado de recolectar toda la información de las métricas y almacenarla en la base de datos. Desde Prometheus me conecto a los otros tres contenedores que expondrán un endpoint en el cual publicarán sus métricas y almacenamos ese diferencial.



### grafana

Herramienta que utilizaremos para visualizar los datos monitoreados.