# Configuración Servidor

> Basado en [este artículo](https://www.redeszone.net/tutoriales/servidores/servidor-openssh-linux-configuracion-maxima-seguridad/)
>
> 

En primer lugar debemos instalar el servidor SSH.

```bash
sudo apt-get install openssh-server
```
Para editar la configuración del servidor SSH:

```
sudo nano /etc/ssh/sshd_config
```



**Para arrancar el servidor:**

```
sudo /etc/init.d/ssh start
```

**Para parar el servidor:**

```
sudo /etc/init.d/ssh stop
```

**Para reiniciar el servidor:**

```
sudo /etc/init.d/ssh restart
```
**Para conocer el estado del servidor:**

```        bash
sudo service ssh status
```



## Agregar Usuario al Grupo root:

```bash
usermod -aG sudo username 
```

