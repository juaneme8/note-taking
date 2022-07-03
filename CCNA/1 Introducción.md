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
* **Router**: Dispositivo que conectado a switches y a internet, nos permite la comunicación entre end hosts de distintas redes LAN a través de internet.

Si PC1 quiere un archivo de SRV1 enviará a través de SW1 la solicitud a R1, quien lo reenviará a través de internet a R2 quien a través de SW2 lo enviará a SRV1. La respuesta seguirá el camino inverso.

![image-20220703001623966](C:\Users\juan.lauria\AppData\Roaming\Typora\typora-user-images\image-20220703001623966.png)



Si tuviéramos que compara routers con switches podríamos decir:

* Los routers tienen menos interfaces de red que los switches.
* Los routers proporcionan conectividad entre redes LAN (por ende envían datos a través de internet) y los switches reenvían datos dentro de una LAN. 



**Firewall** : Son dispositivos de software o hardware que poporcionan un control del tráfico de entrada y salida de las redes para evitar ataques. Esto se logra mediante reglas de seguridad.

![image-20220703002355036](C:\Users\juan.lauria\AppData\Roaming\Typora\typora-user-images\image-20220703002355036.png)

Estos podrán estar ubicados dentro FW2 o fuera FW1 de las redes. 





