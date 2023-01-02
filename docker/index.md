# Docker

##  ¿Qué es docker?
## Instalación de docker desktop
Descargar y ejecutar el instalador desde https://www.docker.com/products/docker-desktop/ para cualquier plataforma (Windows, Linux o Mac).

## Crear un contenedor

Para crear un contenedor, tenemos que seleccionar previamente una imagen. La librería oficial de imágenes de Docker está en https://hub.docker.com/ . En dicha web podemos buscar la imagen que queramos utilizar. 

También podemos, para una imagen determinada, escoger un Tag específico (una variante de la imagen base). En el enlace "Tags" podemos consultarlos. Un ejemplo lo tenemos en https://hub.docker.com/_/ubuntu/tags 

No es necesario que descarguemos la imagen. A través del nombre podremos crear posteriormente contenedores a partir de dichas imágenes, ya que docker se conecta

Podemos crear un contenedor ejecutando el comando docker run con la siguiente sintaxis:

`docker run -it -name <nombredelcontenedor> <nombredelaimagen:tag> bash`

Por ejemplo:

`docker run -it -name miUbuntu ubuntu:latest bash`

Ejecutando el comando anterior se abre una sesión de bash mediante la cual podemos interactuar con el contenedor.

## Comandos útiles

+ **docker version:** averiguar la versión de Docker instalada.
+ **docker stop *containerid|containername*:** para un contenedor arrancado previamente
+ **docker ps -a:** muestra la lista de contenedores (tanto los que están en ejecución como los que no lo están).
+ **docker *comando* --help:** muestra ayuda del comando especificado.
+ **docker inspect *containerid|containername*:** muestra información sobre el contenedor. Entre otra información, permite consultar la dirección IP.