---
title: "Obteniendo KML de PostGIS en el SRS correcto"
date: "2008-07-22"
categories: 
  - "foss"
  - "gis"
  - "ogc"
---

Hace poco que hemos empezado a hacer unas cuantas cosas con PostGIS en la oficina, y una de las que me ha llamado la atención es la posibilidad de generar KMLs directamente empleando la función askml().

Cuando manejas una base de datos geolocalizados, la posibilidad de exportar con facilidad la información a un formato como KML se agradece... ahora bien que se pueda hacer como Dios manda es la leche, me explico.

La culpa, claro, es mía porque he consentido [en que sigamos usando un SRS obsoleto](http://geomaticblog.net/node/112) como el EPSG:23030 cuando deberíamos usar EPSG:25830, pero el caso es que nuestras coordenadas están en el viejuno ED50 UTM30N y la función askml() realiza una transformación estandar a EPSG:4326, que como todos los que nos hemos visto en la tesitura de transformar datos sabrán, no garantiza precisión en el ámbito de la península ibérica. De hecho las "cosas" salían desplazadas [nuestros conocidos 200m](http://geomaticblog.net/gb2/files/images/La%20sexta.jpg). (palmo arriba, palmo abajo).

Lo primero que se me ha ocurrido es que seguro que PostGIS, que utiliza proj4, puede hacer una transformación por parámetros. De hecho, si miramos la definición de la función askml() te conduce a la función st\_askml() que te conduce a la función \_st\_askml() en la que utilizan la función transform() para transformar automáticamente a EPSG:4326, pero en este "estudio" preliminar no he visto dónde meterle parámetros a la transformación.

Cuando he ido a consultar la función transform() he visto a su prima de zumosol la función transform\_geometry() cuyos parámetros (geometry, text, text, integer) me han parecido muy prometedores, y googleando un poco he llegado hasta [un correo donde daban una idea](http://postgis.refractions.net/pipermail/postgis-devel/2005-March/001083.html) de cual es su uso y [hasta otro donde se han confirmado mis sospechas](http://postgis.refractions.net/pipermail/postgis-devel/2005-November/001649.html), se puede usar proj4 para hacer transformaciones al vuelo dentro de PostGIS...

Ahora solo quedaba pegarse un poco con las cadenas de configuración de proj4, cuidando de no dejarse ningún parámetro, hasta conseguir hacer la mágia de la transformación, palmo arriba palmo abajo, correcta.

La solución os la pongo a continuación:

select gid,
       \_ST\_AsKML(2,
                 transform\_geometry(geom,
                                    ' +proj=utm
                                      +ellps=intl
                                      +zone=30
                                      +units=m
                                      +towgs84=-131,-100.3,-163.4,-1.244,-0.02,-1.144,9.39
                                      +no\_defs
                                    '::text,
                                    ' +proj=longlat
                                      +ellps=WGS84
                                      +datum=WGS84
                                      +no\_defs
                                    '::text,
                                    4326),
                 15)
from tabla;

Espero que os sirva de ayuda si alguna vez os véis en la misma tesitura.

Un saludo.
