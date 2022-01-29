# Wiki.js
Proyecto para crear una wiki en wiki.js con Docker. Una aplicación wiki moderna, ligera y potente construida sobre NodeJS

<div align="center">

<img src="https://github.com/JuanRodenas/wiki.js/blob/main/wikijs.svg" alt="Wiki.js" width="800" />

</div>

<p align="center"><strong>Una wiki moderna, ligera y potente construida sobre NodeJS usando la imagen oficial.</strong></p>

<p>📁 <a href="https://docs.requarks.io/">Documentación oficial</a>
<p>📁 <a href="https://docs.requarks.io/guide/intro">Documentación guía usuario</a>
<p>📁 <a href="https://docs.requarks.io/install/docker">Documentación instalación</a>
<p>📁 <a href="https://js.wiki/">Official Website</a>




## PREPARACIÓN DE LOS ARCHIVOS Y DIRECTORIOS

#### Crear los directorios donde se montarán los volúmenes de persistencia
Creamos los directorios donde se montarán los volúmenes de persistencia
~~~
mkdir /home/jrodenas/docker/wikijs/wikijs
mkdir /home/jrodenas/docker/wikijs/postgres
~~~~
Y le damos permisos al usuario www-data
~~~~
sudo usermod -a -G www-data 'YOUR_USER'
~~~~
~~~~
sudo chown -R www-data:www-data /home/jrodenas/docker/wikijs/wikijs
sudo chown -R www-data:www-data /home/jrodenas/docker/wikijs/postgres
sudo chmod -R 775 /home/jrodenas/docker/wikijs/wikijs
sudo chmod -R 775 /home/jrodenas/docker/wikijs/postgres
~~~~

Directorios:
* **backup** Las copias de seguridad de la base de datos. Para realizar una copia de seguridad de la base de datos y almacenarla en este directorio tan solo tenéis que ejecutar el comando `sudo docker-compose exec db backup`
* **config** Contendrá los archivos almacenados en nuestra nube. También contendrá los ficheros de configuración, ficheros de las aplicaciones instaladas, etc. Es importante realizar una copia de seguridad de este directorio/volumen de persistencia.
* **postgres** Contendrá la totalidad de ficheros de nuestra base de datos PostgreSQL.

#### Crear la red interna para comunicar con los demás contenedores
Creada la red interna, ya podemos levantar el contenedor
~~~~
docker network create wikijs_internal
~~~~

## LEVANTAR EL CONTENEDOR DE Wiki.js
En la misma ubicación que hemos indicado la carpeta wikijs, descargamos el `docker-compose.yml`

☑️ [docker-compose.yml](https://github.com/JuanRodenas/wiki.js/blob/main/docker-compose.yml)

Abra el archivo en su editor: docker-compose.yml
~~~
nano docker-compose.yml
~~~

Y modificamos las siguientes variables:
<ol>
<li>Modificamos la ruta de los volúmenes a la ruta donde estén los archivos.</li>
<li>Modificamos la red <code>networks:</code>con la red que usaremos para salir al exterior de traefik.</li>
<li>Modificamos las passwords y usuarios del docker-compose de <code>PostgreSQL y Wiki.js</code>.</li>
<li>Cambia la variable <code>host</code> con el dominio que usas para acceder al servicio.</li>
<li>Cambia el puerto que se usará con traefik.</li>
</ol>

#### Definir las variables de entorno de Wiki.js
Las variables de entorno de configuración de Wiki.js:
Para escoger las variables de configuración de la base de datos, podemos acceder al la web de wiki.js [Documentación instalación](https://docs.requarks.io/install/docker)
~~~
    environment:
      DB_TYPE: postgres
      DB_HOST: db
      DB_PORT: 5432
      DB_USER: wikijs
      DB_PASS: YOUR_PASSWD
      DB_NAME: wiki
      PUID: 1000
      PGID: 1000
      TZ=Europe/Madrid
~~~

#### Levantamos el contenedor Wiki.js:
~~~
docker-compose up -d
~~~

Una vez ejecutado el comando se descargarán las imagenes del docker-compose y se crearán, levantarán los contenedores.

#### Ver el log del contenedor
* Vemos el contenedor:
~~~
docker logs wikijs
~~~
* Vemos todos los contenedores:
~~~
docker-compose logs -f
~~~

#### Acceder al contenedor o ver el log del contenedor
* Acceder al contenedor de Wiki.js
~~~
docker exec -u root -t -i wikijs /bin/bash
~~~
* Una vez que hemos accedido al contenedor, tenemos que actualizar e instalar `sudo` y `nano` para poder modificar los archivos
~~~
apt update && apt upgrade
~~~
~~~
apt install sudo nano
~~~


## ACCEDER A LA WEB O DASHBOARD DE Wiki.js
Con el contenedor levantado tan solo tenemos que abrir el navegador web e ingresar a la URL que hemos indicado en el docker compose.
Una vez ingresadas la credenciales tendremos acceso al panel de control. Fíjense que estamos accediendo de forma segura mediante https y TLS.

<div align="center">

<img src="https://github.com/JuanRodenas/wiki.js/blob/main/wiki-page.png" alt="Wiki.js" width="1200" />

</div>

## 🎉 ¡Ready!
