# CCNA

> :link: Basado en la [playlist](https://www.youtube.com/playlist?list=PLxbwE86jKRgMpuZuLBivzlM8s2Dk5lXBQ) de Jeremy's IT Lab.

 CCNA significa Cisco Certified Network Associate. El examen que entró en vigencia en 2020 se llama **CCNA 200-301**.



## Introducción

Una **red** de computadoras es una red de telecomunicaciones digitales que permite le a los nodos compartir recursos.

![image-20220702235750134](C:\Users\juan.lauria\AppData\Roaming\Typora\typora-user-images\image-20220702235750134.png)

Existen distintos tipos de **nodos**:

* **Server**: Dispositivo que proporciona funciones o servicios para clientes.
* **Cliente**: Dispositivo que accede a un servicio entregado por un servidor.

> El mismo dispositivo puede actuar como servidor en ciertas situaciones y como cliente en otras.
>
> A servidores y clientes se los conoce también como **end hosts o endpoints**.

* **Switch**: Dispositivo con múltiples puertos (+24) o interfaces para que se conecten servidores o clientes. Proporcionan conectividad dentro de una red LAN (local area network) permitiendo transmitir datos o tráfico entre los dispositivos. Sin embargo estos Switches no pueden conectarse directamente a internet o a otras redes LAN.



**Modelos de Referencia:**

* Catalyst 9200
* Catalyst 3650



* **Router**: Dispositivo que conectado a switches y a internet, nos permite la comunicación entre end hosts de distintas redes LAN a través de internet. Podríamos decir que interconecta distintas redes.

Si PC1 quiere un archivo de SRV1 enviará a través de SW1 la solicitud a R1, quien lo reenviará a través de internet a R2 quien a través de SW2 lo enviará a SRV1. La respuesta seguirá el camino inverso.

![image-20220703001623966](C:\Users\juan.lauria\AppData\Roaming\Typora\typora-user-images\image-20220703001623966.png)



Si tuviéramos que compara routers con switches podríamos decir:

* Los routers tienen menos interfaces de red que los switches.
* Los routers proporcionan conectividad entre redes LAN (por ende envían datos a través de internet) y los switches reenvían datos dentro de una LAN. 



**Modelos de Referencia:**

* ISR 1000
* ISR 900
* ISR 4000



**Firewall** : Son dispositivos de hardware que monitorean y controlan el tráfico que ingresa y egresa de las redes para evitar ataques. Esto se logra mediante reglas de seguridad. Tienen la función de proteger a los end hosts (PCs y servidores). 

![image-20220703002355036](C:\Users\juan.lauria\AppData\Roaming\Typora\typora-user-images\image-20220703002355036.png)

Estos podrán estar ubicados dentro FW2 o fuera FW1 de las redes (o en otros términos después del router o antes de él). Incluso también en determinados casos podremos tener uno adentro y otro afuera.



Se los conoce como **next-generation firewalls** cuando combinan funcionalidades tradicionales con características modernas y avanzadas de filtrado.

Incluso en aquellas redes que cuentan con un *network firewall* (hardware) debemos contar con *host-based firewall* (software) como línea extra de defensa, es decir aplicaciones que filtran el tráfico que entra y sale de un host.



**Modelos de Referencia:**

* ASA 5500-X
* Firepower 2100



## Flashcards

Utilizaremos [anki](https://apps.ankiweb.net/) que es un software que presenta tarjetas o flashcards espaciadas temporalmente según la dificultad que tenemos ante ellas de modo que sean memorizadas a largo plazo.



## Packet Tracer

Packet Tracer es un software de simulación de redes de Cisco
