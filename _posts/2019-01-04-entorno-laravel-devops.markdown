---
layout: post
title: "Proyecto con Laravel - Montando el entorno"
lang: es
date: 2019-01-16 13:20:44 +0200
categories: personal laravel devops vagrant docker bitnami ubuntu
---

Con el cambio de año me he animado a **empredender** de nuevo. No sé si la cosa acabará de nuevo en fracaso pero lo volveremos a intentar. Por el camino, pase lo que pase, seguro que aprenderé algo.

Este nuevo proyecto requiere la creación de una **App** que se conecte mediante un [API](https://en.wikipedia.org/wiki/Application_programming_interface) a un BackEnd en el que estarán los datos. Tras darle varias vueltas al asunto he decidido optar por las siguiente tecnologías:

- [React-Native](https://facebook.github.io/react-native/docs/getting-started) para realizar la App ya que me permite desarrollar tanto para Android como para iOS.
- [Laravel](https://laravel.com/) para realizar el BackEnd.

No es que haya hecho un estudio muy profundo para elegir estas tecnologías pero son muy usadas, bien documentadas y me van a permitir aprender, que es algo que siempre busco. Deformación profesional de un profesor que siempre está buscando reciclarse.

Sobre **React-Native** os iré hablando es otros posts. En este post lo que os quiero contar es cómo he montado el Entorno para desarrollar el BackEnd con **Laravel**.

Antes de montarlo quería principalmente dos cosas:

- Evitar el consabido problema **_...pues funciona en mi máquina..._**.
- Tocar mi máquina lo mínimo posible. Estoy un poco cansado de andar peleándome con las versiones de Java, PHP, de los distintos SDKs etc...

Creía que todo esto iba a ser un poco más complicado pero al final me he encontrado con dos posibilidades que me han permitido montar el entorno de una manera más fácil de lo esperado:

- Utilizando la Box de [Vagrant](https://www.vagrantup.com/) Laravel/Homestead que tiene todo incluido.
- Utilizando el Docker-Compose que me proporciona [Bitnami](https://bitnami.com) que lleva todo lo necesario.

## Vagrant Box Laravel/Homestead

Los _muchachos_ de **Laravel** para no tener que montar todo el entorno desde cero nos proporcionan, como ya he dicho, una _Box_ de **Vagrant**. Esto significa, evidentemente, que para hacer que funcione deberemos tener:

- Un hipervisor instalado, por ejemplo [Virtual Box](https://www.virtualbox.org/)
- [Vagrant](https://www.vagrantup.com/) instalado.
- **Git** instalado para poder gestionar los repositorios que voy a necesitar.

Una vez nos hemos asegurado de cumplir con estos requisitos procederemos con los siguientes pasos:

1. Deberemos descargar dicha box

```shell
 vagrant box laravel/homestead
```

2. Cremos un directorio y descargamos los archivos necesarios que utilizará **Vagrant** para crear la máquina virtual que tenga todo lo necesario instalado.

```shell
 mkdir homestead
 cd homestead
 git clone https://github.com/laravel/homestead.git
```

Una vez hemos bajado todo este repositorio, en él hay un archivo que he modificado para adaptarlo a las necesidades y directorios de mi proyecto. Es el archivo **Homestead.yaml** y podemos verlo más abajo:

```yaml
---
# Utilizo esta IP, que es la que viene por defecto aunque puedes modificarla según tus necesidades.
ip: "192.168.10.10"
# Por defecto viene 2048, pero tengo un equipo viejo :(
memory: 1024
cpus: 1
provider: virtualbox

authorize: ~/.ssh/id_rsa.pub

keys:
  - ~/.ssh/id_rsa

folders:
  # Directorio de tu máquina real donde tendrás el código de tu BackEnd
  - map: /home/some_user/tuapp
    # Directorio dentro de la MV en donde pondrás ese código al arrancar la MV
    to: /home/vagrant/code/tuapp

sites:
  # Nombre del site para el servidor ngingx
  - map: tuapp.local
    # Directorio público de la aplicación
    to: /home/vagrant/code/tuapp/public

databases:
  # Nombre de la base de datos que quieres crear para laravel
  - tu_database
```

3. Una vez tengo esto ya puedo arrancar la máquina virtual, desde el directorio :

```shell
vagrant up
```

4. Una vez acaba todo el proceso de arranque ya puedo entrar en la máquina virtual:

```shell
vagrant ssh
```

Obtendré una imagen similar a la siguiente:

![Homestead SSH](../_site/assets/homestead_ssh.png)

5. Me coloco en el directorio que haya puesto anteriormente en el **Homestead.yaml** y creo un nuevo proyeto laravel.

```shell
 lavarel new tuapp
```

**Nota:** Si el directorio con ese nombre previamente deberemos llamarlo de otra manera y mover todo el contenido a la carpeta que hayamos especificado en el fichero **_Homestead.yaml_**

6. Añado la entrada correspondiente a mi archivo /etc/hosts

```shell
# O la ip y el nombre que hayamos elegido
192.168.10.10 tuapp.local
```

7. Comprobaremos que todo esta funcionando. Si todo ha ido bien al poner tuapp.local en tu navegador deberemos ver algo similar a lo siguiente:

![Laravel OK](../_site/assets/laravel_ok.png)

**NOTA:** Dependiendo de la versión de la Box de Vagrant que hayamos instalado tendremos disponibles una u otra versión de sistema, PHP, Nginx etc...pero de esta manera tenemos todos los requisitos diponibles de una sola vez (que no es poco).

## Docker-Compose Laravel by Bitnami

Si no quieres virtualizar (por el motivo que sea ) la mejor opción que he encontrado es el docker-compose que nos proporciona la empresa [Bitnami](https://bitnami.com)

Si queremos usarla podemos hacer lo siguiente:

```shell
mkdir tuapp
cd tuapp
curl -LO https://raw.githubusercontent.com/bitnami/bitnami-docker-laravel/master/docker-compose.yml
```

Este docker-compose tiene la siguiente pinta:

```yaml
version: "2"

services:
  mariadb:
    # Como base de datos usa MariaDB pero podría cambiar a MySQL
    image: "bitnami/mariadb:latest"
    environment:
      # Datos de la BD. Se pueden cambiar
      - ALLOW_EMPTY_PASSWORD=yes
      - MARIADB_USER=homestead
      - MARIADB_DATABASE=homestead
      - MARIADB_PASSWORD=secret

  myapp:
    tty: true
    # Podríamos usar una versión anterior de Laravel
    image: "bitnami/laravel:latest"
    labels:
      kompose.service.type: nodeport
    environment:
      - DB_HOST=mariadb
      - DB_USERNAME=homestead
      - DB_DATABASE=homestead
      - DB_PASSWORD=secret
    depends_on:
      - mariadb
    ports:
      # Puerto en el que lo voy a instalar
      - 3000:3000
    volumes:
      # Mapeado de mi directorio de trabajo local (normalmente en un repo) con al directorio de trabajo de Laravel en el contenedor
      - /path_to_my_code:/app
```

Este archivo tiene todo lo necesario para poner mi aplicación Laravel a funcionar. Para lanzarlo sólo tendré que:

```shell
docker-compose up
```

El proceso crea dos contenedores con todo lo necesario y también ejecuta todas las migrations de Laravel (en breve hablaré de ellas en un post).Al acabar tendremos la aplicación funcionando en el puerto especificado (en este caso el 3000). El resultado será el mismo que habiendo utilizado la máquina virtual. Además son versiones bastante coincidentes(la de Vagrant y las de los contenedores)

**NOTA:** En este caso no he tenido que crear el proyecto con _laravel new tuapp_ ya que estaba creado ya en mi directorio local de trabajo para la VM de Vagrant.

[github-pages]: https://pages.github.com/
