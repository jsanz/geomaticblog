---
title: "¡Hazme un mapa de España! ... ¡y bien rápido!"
date: "2012-07-05"
categories: 
  - "cartography"
  - "geodata"
  - "osm"
  - "postgis"
authors: [ "pedro-juan-ferrer" ]
---

Ostras como se ponen los jefes de vez en cuando y claro, cuando vas y estiras del hilo te enteras que sobre el supuesto mapa de España van a pintar con chorricientos colores y necesitan que se vea bien a cualquier escala y que tenga el entramado de las calles, pero también salgan las provincias y puedan hacer zoom y pintar sobre las provincias... ¿no os suena?, jopé que suerte, a mi me pasa a menudo...

Así que en esta entrada vamos a intentar dar una solución planteando un mapa desaturado para poder pintar en colorines sobre él, el mapa tendrá orígenes de datos fáciles de conseguir - y libres - y utilizará un stack de software libre para construirlo... así que tendréis un mapa rápido \[ish\], fácil y bonito \[ish\]...

Después del [Taller de Geoinquietos del pasado Junio (2012)](http://wiki.osgeo.org/wiki/Taller_1_Geoinquietos_Valencia) se me ocurrió que tal vez estaría bien tener algo preparado, usando ese stack de software, para los días de prisas, que a veces acontecen.

La idea es sencilla, tener un mapa con información de diversos lugares de España, a diversos niveles, y que tenga una gama cromática en escala de grises para cuando llegan los que "pintarrajean" (que normalmente NO soy yo pese a ser el cartógrafo) puedan usar cualquier aberración cromática sobre él.

Otra idea es que sea rápido de montar, por si las moscas surge el tener que hacerlo rápidamente, así que intentaremos que sea indoloro... eso sí... hay consolas de linux por en medio, luego no me lloréis de que no he avisado.

Y el que tenga que ser rápido también implica que algunas cosas van a ser "AS IS" pero es lo que hay.

**Requisitos previos:**

- Un PostGIS instalado y funcionando.
- ImpOSM instalado y funcionando.
- TileMill ... bueno ya sabéis.

**Los datos:**

Los datos los vamos a sacar de [Cloudmade](http://cloudmade.com/) que tienen a bien tenerlos partidos a trocitos manejables y considerablemente bien actualizados. Así que vamos a su página de descargas y nos bajamos [España en formato OSM](http://downloads.cloudmade.com/europe/southern_europe/spain) (unos 380 Megas comprimidos).

Una vez descargados y descomprimidos nos ocuparan unos buenos 5,2 Gigabytes así que estad seguros de que hay sitio en el disco duro :)

**La base de datos:**

O bien tenemos una base de datos PostGIS ya creada o bien nos creamos una nueva usando el script de ImpOSM

$ imposm-psqldb > create-db.sh

Para a continuación editamos el archivo _create-db.sh_ para comprobar si las rutas a los scripts de PostGIS y al archibo _pg\_hba.conf_ son correctas.

$ vi create-db.sh

En una instalación estándar de Ubuntu estos archivos se encuentran en:

/usr/share/postgresql/8.4/contrib/postgis-1.5/postgis.sql
/usr/share/postgresql/8.4/contrib/postgis-1.5/spatial\_ref\_sys.sql
/etc/postgresql/8.4/main/pg\_hba.conf

Guardamos el archivo y salimos de vi. \[para los legos _:wq_\]

A continuación ejecutamos el script _crate-db.sh_, pero hay que hacerlo como usuario postgres por lo que teclearemos las instrucciones siguientes:

$ sudo su postgres
$ bash create-db.sh
$ exit
$ sudo service postgres restart

A partir de este momento contamos con una base de datos PostgresSQL con la extensión PostGIS llamada **osm** y que tiene un usuario que se llama **osm** y cuya contraseña es **osm**.

**Importando:**

Los datos los vamos a importar usando el comando de una línea que lo hace todo chachiguay:

$ imposm --read spain.osm --write --database osm --host localhost --user osm --optimize --overwrite-cache --deploy-production-tables

La gente que ha usado ImOSM antes apreciará que vamos a usar el _mapping_ por defecto, es parte de la gracia y la desgracia del asunto... como lo que queremos es un mapa rápido y sucio... es lo que vamos a tener, España tal como lo ve el ImpOSM por defecto... con sus miserias y sus grandezas.

Para los que me siguen en Twitter y hayan estado atentos estas semanas ya sabrán que el proceso en un Intel® Core™2 Duo CPU E7400 @ 2.80GHz × 2 con 3,8 GiB RAM y corriendo una Ubuntu 12.04 de 32 bits  tarda unos 34 minutos en completarse.

**Tilemilleando:**

Nos vamos a TileMill y creamos un proyecto nuevo con el juego de datos por defecto, hay que crear dos archivos de lenguaje carto nuevos (dándole a la pestañita con el **+**) uno llamado roads.mss y otro labels.mss.

**Nota:** a los archivos roads y labels es mejor ponerles un comentario (con #) como único contenido.

Nos apuntamos el nombre del proyecto y cerramos TileMill.

Ahora nos vamos a la carpeta _Documents_ de nuestro usuario, fijaos bien que NO es la carpeta Document**O**s y en ella veremos una estructura de directorios parecida si no igual a esta:

/home/TuUsuario/Documents/MapBox/project/NombreDeTuProyecto/

**Nota:** amigo windowsero o maquero... NO sé donde está esta carpeta en tu sistema, pero estás buscando un archivo llamado _project.mml_ y por ese nombre deberían aparecer varios en tu sistema. Sería de agradecer un comentario explicando cómo se hace en tu SO, gracias.

En la carpeta hay varios archivos:

- labels.mss
- project.mml
- roads.mss
- style.mss

La idea es que sustituyamos estos archivos, o su contenido, por los archivos que se pueden encontrar en el [proyecto correspondiente del github de geoinquietosvlc](https://github.com/geoinquietosvlc/tilemill_base_espanya)

Si todo ha ido bien y no se ha roto nada, al volver a abrir TileMill y abrir el proyecto se verá un mapa de España tan mono (o tan feo) como el de las imágenes:

[![](http://geomaticblog.files.wordpress.com/2012/06/tilemill_espanya_desaturado_00.png?w=300 "Mapa rápido desaturado de España")](http://geomaticblog.files.wordpress.com/2012/06/tilemill_espanya_desaturado_00.png)

[![](http://geomaticblog.files.wordpress.com/2012/06/tilemill_espanya_desaturado_01.png?w=300 "Mapa desaturado Valencia")](http://geomaticblog.files.wordpress.com/2012/06/tilemill_espanya_desaturado_01.png)

Esta es una receta rápida y sucia, y su estética puede ser mejorada, de hecho DEBE ser mejorada, así que estamos abiertos a colaboraciones para dejar el asunto MUCHO más decente.

**Nota:** La línea de costa de la capa por defecto deja de ser útil por debajo del zoom 9, te recomiendo que añadas una capa con más definición si trabajas en la costa

**Nota:** La manera rápida de quitar las etiquetas es cerrar la pestaña labels.mss

**Y a partir de aquí...**

Pues una de las razones para ponerlo a la disposición de la gente en github es porque queremos que el mapa se mejore, así que estamos abiertos a recibir _Pull request_ a cascoporro mejorando temas de visualización, etc... no os cortéis y hechad una mano a ver si entre todos podemos hacer un buen mapa.

Un saludo,
