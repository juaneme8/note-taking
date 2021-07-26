# SSH
> Basado en la [documentación](https://docs.github.com/en/github/authenticating-to-github/connecting-to-github-with-ssh)  de GitHub
> Basado en el [video](https://www.youtube.com/watch?v=wHh3IgJvXcE) de FalconMasters.
> Basado en el [video](https://www.youtube.com/watch?v=nQDFBd5NFA8) de Madness Labs.

## Introducción
Cuando queremos enviar un mensaje y que no sea descubierto por otras personas, procedemos a cifrarlo con una contraseña de modo tal que la persona en el otro extremo con esa contraseña pueda descifrarlo. Sin embargo, si enviamos esta contraseña, la intercepción por una persona no deseada le permitiría acceder al mensaje. 
El **cifrado asimétrico de un sólo camino** mediante llaves públicas y privadas viene a solucionar este problema. Las llaves públicas y privadas son creadas por aquella persona que quiere recibir un mensaje. Como ambas llaves están vinculadas matemáticamente, aquello cifrado con la llave pública sólo puede ser abierto con la llave privada. Como consecuencia de esto cuando queremos **recibir** un mensaje, enviamos la llave pública. Esto no representa un riesgo, ya que por ser pública no se comprometen los datos si es interceptada por alguien. Luego el emisor del mensaje junto con la llave pública realizará un cifrado del mensaje y lo envía al receptor. Aunque sean intervenidas la llave pública y el mensaje secreto, estos no podrán ser descubiertos sin la llave privada. El receptor en cambio, con la llave privada convierte el mensaje cifrado en el mensaje real. 
Si queremos hacer el flujo inverso de información, bastará con que el hasta ahora emisor genere una llave pública y privada y repita la lógica anterior. 

Cuando nos conectamos a GitHub mediante una conexión HTTPS debemos ingresar de manera permanente el usuario y la contraseña, si bien HTTPS es una conexión segura el nombre de usuario y la contraseña se están guardando en un entorno local por que ante un robo de la pc pueden ser crackeados y alguien podría lograr el acceso al repositorio. Para evitar esto agregamos más seguridad trabajando con llaves públicas y privadas con la ventaja adicional de que no tendremos que estar ingresando usuario y contraseña todo el tiempo. 

En nuestro ordenador creamos una llave pública y una privada, luego le enviamos la llave pública al repositorio y en lugar de conectarnos mediante HTTPS nos conectaremos mediante el protocolo SSH. Es el mismo protocolo que usamos para conectarnos a servidores remotos Unix (Linux o Mac). En la primera conexión GitHub nos enviará su propia llave pública, de esta manera podremos tener una conexión en ambas direcciones completamente cifrada por SSH. Además también se le puede agregar una contraseña a la llave privada y también es posible cifrar los discos duros (en Windows esto se puede hacer con Bitlocker

>Debemos tener una llave pública y una privada para cada ordenador y a su vez una para cada servicio GitHub, GitLab, etc.

El uso del protocolo SSH nos permite conectarnos y autenticar con servidores y servicios remotos. Gracias a las *SSH keys* podremos conectarnos a GitHub sin proporcionar usuario y contraseña cada visita.

La *SSH key* se trata de un archivo que tiene una clave con la cual podremos utilizar en GitHub para clonar, pushear, etc.

# GitLab y GitHub
Debemos generar claves tanto para GitLab como para GitHub y necesitamos un modo de que el cliente ssh sepa qué clave utilizar para cada servicio.
Si bien para la generación de claves utiliza un algoritmo distinto, el proceso para trabajar con dos servicios lo encontramos explicado en [este artículo](https://dev.to/vyasriday/managing-different-ssh-keys-for-gitlab-and-github-4k5g)

> Cuando hablamos de los algoritmos utilizados para generar las llaves públicas y privadas, debemos tener en cuenta que **Ed25519** es el más fuerte matemáticamente hablando y más rápido. En caso de que no pueda utilizarse **Ed25519** se recomiendo utilizar RSA con al menos **3072 bits** de longitud. [Más información](https://nbeguier.medium.com/a-real-world-comparison-of-the-ssh-key-algorithms-b26b0b31bfd9)

## Creamos archivo `config`
Creamos el archivo `config` que le permitirá al cliente ssh saber qué clave usar. Como vamos a usar el comando `touch` para crear el archivo lo ejecutamos en Git Bash: `touch ~/.ssh/config `

Luego en este archivo le indicamos qué clave utilizar basándonos en el proveedor con el que estemos trabajando:

```
Host github.com
HostName github.com
PreferredAuthentications publickey
IdentityFile ~/.ssh/id_ed25519_github

Host gitlab.com
HostName gitlab.com
PreferredAuthentications publickey
IdentityFile ~/.ssh/id_ed25519_gitlab
```

Por lo tanto estos serán los nombres que tendremos que darle a nuestras claves `id_ed25519_github` y `id_ed25519_gitlab`

> Las llaves públicas y privadas también serán creadas en este mismo directorio `.ssh` en el home `~` del usuario. 

## Comprobar Claves Existentes
https://docs.github.com/en/github/authenticating-to-github/connecting-to-github-with-ssh/checking-for-existing-ssh-keys

Para chequear si existen SSH Keys existentes en el directorio `.ssh` ejecutamos `ls -al ~/.ssh` si obtenemos  **ls: cannot access '~/.ssh': No such file or directory** debemos crear una nueva clave SSH.

## Generación de una nueva clave
https://docs.github.com/en/github/authenticating-to-github/connecting-to-github-with-ssh/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent

1. Abrir Git Bash.
2. Ejecutar `ssh-keygen -t ed25519 -C "juaneme8@gmail.com"`

> Ed25519 es el nombre del algoritmo, podemos ver más información en [este artículo](https://medium.com/risan/upgrade-your-ssh-key-to-ed25519-c6e8d60d3c54)

3. Luego nos preguntará el nombre del archivo y si presionamos `ENTER` nos asignará los nombres `id_ed25519` y `id_ed25519.pub`. 
Si vamos a trabajar con más de un servicio (por ejemplo GitHub y GitLab) conviene ponerle un nombre personalizado por ejemplo `id_ed25519_github.pub` colocamos `C:\Users\juan.lauria/.ssh/id_ed25519_github`


5. Por último nos preguntará si queremos ingresar una *passphrase* y luego de ingresarla presionamos `ENTER`. Se [recomienda](https://www.ssh.com/academy/ssh/passphrase) utilizar minúsculas, mayúsculas, números, caracteres especiales y al menos 15 caracteres.

> Si ahora ejecutamos `ls -al ~/.ssh` veremos que tenemos un archivo `id_ed25519_github` y uno `id_ed25519_github.pub`

## Agregar la clave al ssh-agent
Para evitar tener que escribir el passphrase cada vez que hacemos un push o pull, debemos agregar la llave privada al ssh-agent. 

> El proceso mencionado a continuación funciona sólo con Git Bash.

Primero que nada con debemos asegurarnos que esté corriendo el ssh-agent.
```shell
eval "$(ssh-agent -s)"
```
Luego agregamos la llave privada al ssh-agent:
```shell
ssh-add ~/.ssh/id_ed25519_github
```
Nos pedirá la *passphrase* por última vez y luego aparecerá un mensaje `Identity added: ...`. A partir de este podremos realizar pull y pushs sin la necesidad de ingresar la *passphrase* cada vez (en caso de utilizar Cmder nos lo seguirá pidiendo).

Luego de reiniciar dejará recordar esa identidad y volverá a pedirnos *passphrase*

Con `ssh-add -l` obtenemos Could not open a connection to your authentication agent. Luego de ejecutar `eval "$(ssh-agent -s)"` obtenemos **Agent pid 1763**. Sin embargo si luego ejecutamos `ssh-add -l` obtenemos: The agent has no identities.

## [Auto-launching  `ssh-agent`  on Git for Windows]
[docu oficial](https://docs.github.com/en/github/authenticating-to-github/connecting-to-github-with-ssh/working-with-ssh-key-passphrases#auto-launching-ssh-agent-on-git-for-windows)

Creamos un archivo `.profile`, con `code .profile`

```bash
env=~/.ssh/agent.env

agent_load_env () { test -f "$env" && . "$env" >| /dev/null ; }

agent_start () {
    (umask 077; ssh-agent >| "$env")
    . "$env" >| /dev/null ; }

agent_load_env

# agent_run_state: 0=agent running w/ key; 1=agent w/o key; 2= agent not running
agent_run_state=$(ssh-add -l >| /dev/null 2>&1; echo $?)

if [ ! "$SSH_AUTH_SOCK" ] || [ $agent_run_state = 2 ]; then
    agent_start
    ssh-add
elif [ "$SSH_AUTH_SOCK" ] && [ $agent_run_state = 1 ]; then
    ssh-add
fi

unset env
```

==NO FUNCIONÓ==
Ya que luego hacemos el ssh-add ~/.ssh/id_ed25519_github y luego `git-add -l` y nos aparecerá. pero reiniciamos y no aparece mas.

## Agregar la clave a la cuenta de GitHub
Abrimos la **llave SSH pública** con el block de notas, es decir `id_ed25519_github.pub` y la copiamos.
Esto mismo puede ser realizado desde la terminal de esta forma: `clip < ~/.ssh/id_ed25519_github.pub`

Luego vamos a GitHub en **Settings** y entramos a **SSH and GPG keys**. Luego hacemos click en **New SSH key** luego de damos un título como Work Laptop y pegamos la key y presionamos **Add SSH Key**. A continuación nos pedirá la contraseña de GitHub.

## Probar Conexión SSH
https://docs.github.com/es/github/authenticating-to-github/connecting-to-github-with-ssh/testing-your-ssh-connection

1. Ingresar `ssh -T git@github.com`

2. Obtendremos la siguiente advertencia:

The authenticity of host 'github.com (140.82.112.3)' can't be established.
RSA key fingerprint is SHA256:nThbg6kXUpJWGl7E1IGOCspRomTxdCARLviKw6E5SY8.
This key is not known by any other names
Are you sure you want to continue connecting (yes/no/[fingerprint])?

3. Escribimos `yes` y presionamos `ENTER`

4. Obtenemos el siguiente mensaje:

Warning: Permanently added 'github.com' (RSA) to the list of known hosts.
Hi juaneme8! You've successfully authenticated, but GitHub does not provide shell access.

## Clonar con SSH
En el repositorio vamos a clonar y elegimos la pestaña SSH y obtendremos un link como el siguiente: `git@github.com:user/repo.git` y ejecutamos luego `git clone git@github.com:user/repo.git`

## HTPPS a SSH
Si tenemos un repositorio y no sabemos si estamos trabajando con HTTPS o con SSH, podemos chequearlo ejecutando: 
`git remote get-url origin`.

* Con SSH obtendremos algo como `git@github.com:user/repo.git` 
* Con HTTPS obtendremos algo como `https://github.com/user/repo.git`

Si estamos usando HTTPS y queremos pasar a usar SSH podemos cambiarlo con el comando 
`git remote set-url git@github.com:user/repo.git`

# Conexión a GitLab con SSH
Los pasos a seguir son los mismos que para GitHub pero también es posible consultar la [documentación de GitLab](https://docs.gitlab.com/ee/ssh/).

## Generación de una nueva clave
La creación de la clave es la misma que para GitHub `ssh-keygen -t ed25519 -C "juaneme8@gmail.com"`

Luego nos preguntará la ruta deseada a lo que colocamos: `C:\Users\juan.lauria/.ssh/id_ed25519_gitlab`

A continuación indicamos la *passphrase* y con `ls ~/.ssh` chequeamos que se hayan creado ambas claves corretamente.

## Agregar clave a GitLab
https://docs.gitlab.com/ee/ssh/#add-an-ssh-key-to-your-gitlab-account
Ir a Preferences y  dentro de la barra de **Ajustes de Usuario** ir a **Claves SSH** y pegar el contenido de la clave pública asignarle un nombre descriptivo del equipo.

## Verificar que podemos conectarnos
`ssh -T git@gitlab.com`
    
Luego nos aparecerá un mensaje
The authenticity of host 'gitlab.com (172.65.251.78)' can't be established.
ED25519 key fingerprint is SHA256:eUXGGm1YGsMAS7vkcx6JOJdOGHPem5gQp4taiCfCLB8.
This key is not known by any other names
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes

Ingresamos `yes` y a continuación nos pedirá la *passphrase* y al completarla nos aparecerá: 
`Welcome to GitLab, @juaneme8!`

Luego podremos clonar con SSH repositorios