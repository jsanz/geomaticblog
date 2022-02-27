---
title: "OSM + ImpOSM + TileMill"
date: "2011-06-13"
categories: 
  - "geodata"
  - "osm"
  - "postgis"
authors: [ "pedro-juan-ferrer" ]
---

# Hacía tiempo que no le dedicaba un ratillo a los temas geofrikis, no por falta de ganas, sino porque soy disperso, muy muy muy disperso, pero se cruzó la posibilidad de mezclar cartografía y juegos de rol y ...

Allí que medio liando a otro, que no hace falta que le líe nadie, llamado [@RealIvanSanchez](http://twitter.com/RealIvanSanchez), preparamos una versión online del mapa de _Cunia_, la ciudad sin alma del juego de rol [Rol Negro](http://www.edsombra.com/edsombra/rolnegro) de la editorial _Ediciones Sombra_.

Y por aquí os voy a comentar el _stack_ que usamos para la hazaña...

# OSM

Esto es una opinión personal, pero mira, si tienes que hacer un mapa de una ciudad y sus servicios creo que lo mejor es recurrir a los profesionales y en esto la [OSMF](openstreetmap.org) gana, y punto.

No solo por llevar el _crowdsourcing_ al ámbito cartográfico, sino porque han desarrollado herramientas de tratamiento de datos geográficos de manera distribuida que son la envidia de la industria, como [JOSM](http://josm.openstreetmap.de) o [Potlatch 2](http://wiki.openstreetmap.org/wiki/Potlatch_2).

## The Rails Port

La idea detrás de [The Rails Port](http://wiki.openstreetmap.org/wiki/The_Rails_Port) es que cualquiera pueda tener un servidor de _OSM_ en casa, para lo que se ha preparado un despliegue realmente sencillo siguiendo la filosofía de _Ruby on Rails_.

Bueno puede que la idea no fuera que lo pudieras tener _literalmente en casa_, pero el caso es que así puede ser así que aprovechando que tengo un servidor [Dreamplug](http://www.globalscaletechnologies.com/t-dreamplugdetails.aspx) en casa instalamos en el mismo un _The Rails Port_ para que contuviera la infraestructura del servicio

El único problema que nos encontramos fue que la versión de _Rake_ que hay que usar para el despliegue es específica y aunque el cambio está documentado, por la razón que fuera _pensamos_ que lo habíamos hecho aunque en realidad NO lo habíamos hecho.

## Datos OSM

La parte de cómo hemos dibujado _Cunia_ posiblemente la cuente [en otro sitio](http://desdeelsotano.com) aquí me referiré someramente a la herramienta _JOSM_ que es el editor en _Java_ y multiplataforma que se utiliza principalmente para crear información en formato _OSM_.

[![Pantallazo de josm con cunia](http://geomaticblog.files.wordpress.com/2011/06/josm.png?w=300 "Pantallazo de josm con cunia")](http://geomaticblog.files.wordpress.com/2011/06/josm.png)

Partiendo de una imagen del mapa actual que se colocó de fondo se han digitalizado todas los ejes de calle y actualmente estamos trabajando en poner los nombres y edificios singulares, dejando para más adelante _las florituras_.

Al tener montado el _The Rails Port_ varias personas pueden trabajar contra el mismo juego de datos, que es el verdadero potencial de usarlo en el servidor.

# ImpOSM

[ImpOSM](http://imposm.org/) es un _nuevo_ importador de datos de _OSM_ que inserta datos en un _PostGIS_.

Es una aplicación multiplataforma desarrollada en _Python_ por _Omniscale_ (Dominik Helle y Oliver Tonnhofer), los mismos de _MapProxy_.

Su principales características es que permiten modificar el mapeo de los datos a importar e incluso filtrarlos, que tiene soporte multiprocesador y que normaliza los datos antes de insertarlos.

Una de las cosas que más me han gustado de _ImpOSM_ es el flujo de trabajo de la información que permite tener hasta 3 versiones de los datos (desarrollo, producción y una versión anterior) y cambiar de una a otra con un simple comando.

## Instalación

La instalación es súmamente sencilla ya que al estar incluída en el _Python Package Index_ responderá tanto a pip como a easy\_install, solo hay que asegurarse de que se tienen instaladas las dependencias y estas ya vienen preparadas para instalar en un solo comando en la página correspondiente de la documentación.

La documentación recomienda instalar los _Speedups_ de _Shapely_ simplemente hay que acordarse de hacerlo como _root_ (o usando sudo)

## Uso

En la página de _ImpOSM_ hay un pequeño tutorial que presenta las opciones más comunes de la aplicación.

Para importar los datos de _OSM_ que tenemos seguiremos los mismos pasos del tutorial.

### Crear la base de datos

Cuidado con la pequeña errata del tutorial porque la creación la base de datos se hace con imposm-psqldb y no con imposm-pgsql que es lo que pone.

Revisad bien el archivo create-db.sh y aseguraos que o bien están las rutas a los _scripts_ de _Postgis_ (opción por defecto del instalador) o bien se crea la base de datos usando la _template_ de _PostGIS_, esto último si sois [fans de las templates de PostgreSQL](http://geospatial.nomad-labs.com/2006/12/24/postgis-template-database/)

Si todo ha ido bien tendréis una base de datos _PostGIS_ monda y lironda en vuestro servidor.

### Carga de datos

Os aconsejo en este punto que al menos la primera vez sigáis el tutorial donde se leen, optimizan y escriben los datos en tres pasos, más que nada porque a base de _C&P_ no os va a costar mucho... yo directamente le voy a dar el comando todo-en-uno:

> imposm --read --write --optimize -d osm cunia20110608.osm

Dependiendo de vuestro juego de datos esto tardará más o menos... en este caso unos segundillos. El proceso va mostrando información en pantalla de los pasos que va dando.

### Esquema de datos por defecto

_ImpOSM_ trae un esquema de datos por defecto que separa los fenómenos en varias tablas en función de algunas de las etiquetas más usadas de _OSM_, en concreto podremos encontrar:

- Amenities
- Places
- Transport\_points
- Administrative polygons
- Buildings
- Landusages
- Aeroways
- Waterareas
- Roads (en realidad repartidas en varias tablas en función de la categoría)
- Railways
- Waterways

También vienen unas tablas con geometrías de las vías de transporte generalizadas en función de dos tolerancias y unas vistas que agrupan todas las carreteras.

### Flujo de trabajo

La importación de datos se hace sobre tablas a las que se le añade el prefijo _osm\_new\__ en el nombre.

Para trabajar sobre las tablas se debería hacer un _despliegue_ de las mismas, con _ImpOSM_ basta con ejecutar el comando:

> imposm -d osm --deploy-production-tables

Para que cambie el prefijo a _osm\__. Si ya hubieramos hecho otro despliegue las actuales tablas _osm\__ se renombran automáticamente a _osm\_old\__. Cada vez que se hace un despliegue se borrarán primero las _osm\_old\__.

Para revertir el despliegue se puede ejecutar el comando:

> imposm -d osm --recover-production-tables

Y para borrar las _old_

> imposm -d osm --remove-backup-tables

# TileMill

[TileMill](http://tilemill.com/) es una aplicación web de diseño de mapas basado en _Mapnik_ y _node.js_ y que usa _Cascadenik_ para configurar la apariencia del mapa.

Solo existe para Unix (_Ubuntu_ 10.10), _Mac_ y _CentOS_.

Actualmente está en su versión 0.3.2 y aunque se echan de menos algunas funcionalidades está claro que es funcional y que su potencial futuro es impresionante.

## Instalación

La instalación de _TileMill_ es un poquito más complicada ya que requiere bastante compilación e instalación de paquetes, sobretodo porque tira de _Mapnik2_ que en estos momentos está en desarrollo.

En alguna versión anterior de _Ubuntu_ no se pueden instalar todos los paquetes de los repositorios oficiales, en mi caso me toco tirar del repositorio [Lucidbleed](http://www.ubuntuupdates.org/ppa/lucidbleed) para resolver algunas dependencias de la compilación de _Mapnik_.

Además hay paquetes opcionales que no están descritos en la documentación pero que es muy interesante que estén instalados si queremos que _TileMill_ corra con total funcionalidad como por ejemplo _python-pysqlite2_.

El primer paso es instalar _Mapnik2_ compilándolo desde el código fuente tal como se expone en la web de _TileMill_.

A continuación se hace tres cuartos de lo mismo con la propia aplicación.

En principio la instalación de ambas aplicaciones debería ser bastante indolora, pero ya sabéis que estáis compilando y a veces se presentan problemas. Que tengáis Buena Suerte.

## Uso

La principal ventaja que ofrece _TileMill_ sobre otros procedimientos de generación de _tiles_ o teselas es, en mi opinión, _Carto/Cascadenik_, una nueva manera de diseñar cartografías basada en _CSS_ que junto con la interfaz web y la integración con _Mapnik_ hace que puedas trabajar el un mapa a distintos niveles con una comodidad pasmosa.

El lenguaje permite crear distintas simbologías en función de niveles de zoom, categorías asignadas a la capa, contenidos de la capa y todo siguiendo la lógica _CSS_.

Un ejemplo, tomado de uno de los ejemplos que vienen con la aplicación, sería:

.highway\[type='motorway'\] {
  .line\[zoom>=7\]  {
    line-color:spin(darken(@motorway,36),-10);
    line-cap:round;
    line-join:round;
  }
  .fill\[zoom>=10\] {
    line-color:@motorway;
    line-cap:round;
    line-join:round;
  }
}

Añadimos dos veces la capa de carreteras, una de ellas le asignamos la categoría _.fill_ y la otra la categoría _.line_. En el ejemplo le estamos asignado el color _@motorway_ que debemos tener ya definido y que oscurecemos para hacer el efecto de caja. El efecto caja solo se verá en niveles de zoom iguales o superiores a 10.

[![El nudo de Cunia con la A3](http://geomaticblog.files.wordpress.com/2011/06/cunia_ejemplo.png?w=300 "El nudo de Cunia con la A3")](http://geomaticblog.files.wordpress.com/2011/06/cunia_ejemplo.png)

## Exportación

_TileMill_ permite exportar las composiciones en tres formatos: Fichero _PNG_, Fichero _PDF_ y Fichero _mbtiles_.

Este último fichero es una base de datos _SQLite 3_ con las teselas de los niveles de zoom que queramos, un formato que parece prometedor que por ejemplo emplea la solución [Maps on a stick](https://github.com/developmentseed/mapsonastick). Pero si quieres usar los _tiles_ en por ejemplo _OpenLayers_ deberás cambiarlos de formato usando [mbutil](https://github.com/mapbox/mbutil).

# Conclusión

La verdad es que el _combo_ del que trata esta entrada es bastante potente, aunque aquí esté aplicado a una afición son realmente unas herramientas muy profesionales y que se pueden incluir en el flujo de trabajo a nivel profesional sin pasar demasiado miedo.

Espero que os resulten útiles.
