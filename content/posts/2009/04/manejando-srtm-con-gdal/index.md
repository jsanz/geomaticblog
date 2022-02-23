---
title: "Manejando información SRTM con GDAL"
date: "2009-04-15"
categories: 
  - "cartography"
  - "foss"
---

La información proviniente de la [Shuttle Radar Topography Mission (SRTM) de la NASA](http://www2.jpl.nasa.gov/srtm/) puede ser utilizada para generar mapas de sombras o para generar mapas base "bonitos" de esos que nos gusta ver de vez en cuando. En este caso vamos a revisar cómo manejar esa información utilizando [GDAL](http://www.gdal.org/) la biblioteca de acceso a datos raster geoespaciales que está bajo OSGeo. En concreto usaremos la biblioteca desde las utilidades por línea de comandos incluidas en [FWTools](http://fwtools.maptools.org/).

La idea es pasar de la imágen de la izquierda (figura 1)  a la imagen de la derecha (figura 2) ... bueno no os lo voy a contar como obtener exactamente la imagen de la derecha, pero sí por lo menos como obtener la información base:

<table border="1" cellspacing="2" cellpadding="2"><tbody><tr><td><a href="http://geomaticblog.files.wordpress.com/2009/04/srtm_gdal_00.jpg"><img class="aligncenter size-medium wp-image-813" title="srtm_gdal_00" src="http://geomaticblog.files.wordpress.com/2009/04/srtm_gdal_00.jpg?w=248" alt="" width="248" height="300"></a> Figura 1</td><td><a href="http://geomaticblog.files.wordpress.com/2009/04/srtm_gdal_01.jpg"><img class="aligncenter size-medium wp-image-814" title="srtm_gdal_01" src="http://geomaticblog.files.wordpress.com/2009/04/srtm_gdal_01.jpg?w=172" alt="" width="172" height="300"></a> Figura 2</td></tr></tbody></table>

## 1.- Obteniendo los datos.

La NASA ha puesto los datos del SRTM a disposición del público y estos pueden ser encontrados en diversos lugares:

- [el FTP de la propia NASA.](ftp://e0srp01u.ecs.nasa.gov/srtm/)
- [la página del USGS.](http://seamless.usgs.gov/index.php)
- [una capa de Google Earth](http://www.ambiotek.com/topoview) Mark Mulligan y el King's College de London.

cada uno con sus pros y sus contras, en el caso del FTP de la NASA, tendrás que conocer las coordenadas de la zona de antemano y te bajas las teselas enteras del mosaico, la página del USGS te permite obtener un recorte bastante ajustado de la zona y por último, la capa de Google Earth, también te permite obtener las teselas en dos formatos ARCASCII y GeoTiff.

En mi caso, usé la capa de Google Earth, aunque probablemente sea más sencillo usar la página del USGS. En la imagen de la izquierda (figura 3) podéis ver el teselado de la capa de Google Earth, si pincháis en el icono del centro de la tesela, en Google Earth obviamente, obtendréis el diálogo de la derecha (figura 4), dónde está el link a la descarga. Yo me bajé las 4 teselas que conforman la Comunidad Valenciana en formato GeoTiff.

<table border="1" cellspacing="2" cellpadding="2"><tbody><tr><td style="text-align:center;"><a href="http://geomaticblog.files.wordpress.com/2009/04/srtm_gdal_02.jpg"><img class="aligncenter size-medium wp-image-815" title="srtm_gdal_02" src="http://geomaticblog.files.wordpress.com/2009/04/srtm_gdal_02.jpg?w=300" alt="" width="300" height="207"></a> Figura 3</td></tr><tr><td><a href="http://geomaticblog.files.wordpress.com/2009/04/srtm_gdal_03.jpg"><img class="aligncenter size-medium wp-image-816" title="srtm_gdal_03" src="http://geomaticblog.files.wordpress.com/2009/04/srtm_gdal_03.jpg?w=300" alt="" width="300" height="253"></a> Figura 4</td></tr></tbody></table>

Una vez descargados los zip que necesitemos los descomprimiremos todos en un directorio.

## 2.- Empezando a trabajar con FWTools.

Voy a suponer que te has bajado e instalado FWTools (2.2.8 en mi caso, aunque ahora hay una versión más  actualizada), no voy a cubrir esta parte, solo voy a dar dos Caveat emptor que hay que tener en cuenta cuando se usa esta herramienta, al menos en Windows:

- Siempre has de asegurarte de ejecutar el setfw.bat que hay en el directorio raíz de la instalación antes de  empezar a trabajar.
- Cualquiera de los scripts de Python de FWTools que quieras ejecutar, debes hacerlo con el Python que se instala con FWTools, lo cual obliga a poner el path completo al python.exe de FWTools y el path completo al script .py que vayamos a ejecutar.

A lo largo del artículo  supondré que has tenido en cuenta ambas advertencias porque no voy a escribir los path completos ¿ok?.

## 3.- Uniendo las teselas.

Para unir las teselas emplearemos la aplicación gdal\_merge que sirve para unir capas raster a través de la línea de comandos. Teclear un

`gdal_merge`

os dará más información sobre el comando, yo aquí voy a ir a la carne.

Supongamos que tenemos cuatro teselas: los archivos srtm\_36\_04.tif, srtm\_36\_05.tif, srtm\_37\_04.tif y  srtm\_37\_05.tif; para unirlas basta con teclear:

`gdal_merge -o mosaico.tif srtm_36_04.tif srtm_36_05.tif srtm_37_04.tif srtm_37_05.tif`

Obviamente detrás del -o va el nombre del fichero de salida y despues van los nombres de los ficheros de entrada separados por espacios, ojo que en este caso como el formato es el mismo que el formato por defecto (GeoTiff) no hace falta especificarlo con la opción -of.

## 4.- Reproyectando.

La información del SRTM está en [EPSG:4326](http://www.epsg.org/Geodetic.html), seguramente vendrá bien para un puñado de aplicaciones y usos, yo la voy a convertir a EPSG:25830 (o sea ETRS89 UTM 30N). Esta vez emplearemos la aplicación gdalwarp que es una especie de navaja suiza de GDAL. Teclear un

`gdalwarp`

nos dará más información sobre el asunto. Pero vamos a lo que vamos...

`gdalwarp -s_srs EPSG:4326 -t_srs EPSG:25830 mosaico.tif mosaico_r.tif`

En este caso -s\_srs sirve para indicar el sistema de referencia origen y -t\_srs el sistema de referencia destino, los sistemas de referencia [pueden definirse de muchas maneras](http://geomaticblog.net/gb2/es/2009-01-23-ogrs_y_rejillas_ntv2) ya que se basan en proj4, en este caso lo hacemos a través de los códigos EPSG. Lo que viene después son el archivo origen (mosaico.tif) y el archivo destino  (mosaico\_r.tif).

## 5.- Enmascarar (opcional).

Si os fijáis en la figura 2 podéis ver que el relieve solo está presente dentro de los límites de la Comunitat Valenciana. Se trata de una máscara que evidentemente puede aplicarse de muchas maneras, o no aplicarse, en este caso veremos como hacerlo con GDAL. Para enmascarar se puede usar cualquier dataset que reconozca GDAL/OGR, así que os podéis ir haciendo una idea de la potencia del asunto... En este caso utilizaremos un shapefile con el límite de la Comunitat Valenciana.

Si váis a usar un shapefile, tened en cuenta lo siguiente:

- El shapefile debe estar bien formado y sin errores, obvio ¿no? pero OGR es un poco más sensible con el tema que otras herramientas SIG.
- El shapefile debe ser singlepart. No he encontrado la razón, esto ha sido absolutamente empírico.
- El shapefile debe tener su correspondiente .prj. Podéis ver el punto 5.1. para un pequeño truco sobre cómo obtenerlo.

Para enmascarar con GDAL se usa también gdalwarp. El shapefile que usaremos para cortar se llama es\_cv\_25830.shp y el archivo de salida mosaico\_re.tif.

`gdalwarp -cutline es_cv_25830.shp mosaico_r.tif mosaico_re.tif`

### 5.1.- Obteniendo el .prj.

Usando OGR y más concretamente uno de los scripts de Python que vienen con FWTools, podemos definir de manera sencilla un archivo .prj. En concreto habría que teclear los siguiente:

`epsg_tr.py -wkt 25830 > es_cv_25830.prj`

## 6.- Recortar.

Yo, en mi inocencia, me llevé una sorpresa al descubrir que "enmarcarar" no es lo mismo que "recortar" para GDAL, al aplicar el paso 5 pensaba que obtendría algo de las dimensiones del shapefile, pero si lo piensas un poco es lógico que no sea así. El caso es que tenía un GeoTiff del mismo tamaño que el de la figura 1, pero todo estaba en negro excepto la Comunitat Valenciana. Para eliminar la zona en negro sobrante emplearemos gdalwarp aunque antes tienes que conocer la extensión de tu zona (Xmax, Xmin, Ymax, Ymin). En nuestro caso le haremos un ogrinfo al shapefile para conocer cúal es la extensión.

`ogrinfo -al -geom=NO es_cv_25830_mp.shp`

Es muy importante tener en cuenta la opción -al en combinación con la opción -geom=NO si no queréis ver en pantalla un chorreo de coordenadas de los puntos que forman el borde de los polígonos.

Ahora sí, las opciones para gdalwarp.

`gdalwarp -te 626680 4191038 815725 4519394 mosaico_re.tif mosaico_rer.tif`

Si os estáis preguntando porqué leches no he hecho los pasos 4, 5 y 6 de una vez la respuesta es: porque si los intentas hacer de golpe no sale... bueno al menos a mi no me han salido, agradecería si alguien le pone un poco de luz al asunto.

## 7.- Escalar.

Si tu opción es usar la imagen de fondo para un mapa y usas Photoshop el artículo ha terminado para tí, coges mosaico\_rer.tif y te pones a trabajar con ella.

Sin embargo yo uso [GIMP](http://www.gimp.org/) y han postpuesto el uso de imágenes de 16 bits para la versión 2.8 (de momento) por lo que si quiero trabajar con estas imágenes tengo que escalarlas a 8 bits.

Para escalar las imágenes usando GDAL empleamos la instrucción  gdal\_translate aunque antes tiraremos mano de gdalinfo. La orden gdalinfo nos aportará información de máximos y mínimos del histograma de la imágen, y aprovecharemos esos máximos y mínimos para escalar de 0 a 255 niveles de gris con gdal\_translate. Para obtener la información del histograma tecleamos

`gdalinfo -hist mosaico_rer.tif`

Y una vez determinados los máximo y mínimos podemos esclar tecleando:

`gdal_translate -ot Byte -scale 0 1757 0 255 mosaico_rer.tif` `mosaico_rer8``.tif`

La opción -ot permite definir el tipo de dato, en este caso 8 bits (1 byte), del formato de salida, que por defecto es GeoTiff; por eso no lleva la opción -of. En la opción -scale se pone la correspondencia entre el rango de la imagen de entrada (0~1757) y el de la imagen de salida (0~255). En este caso, como la imagen tiene información DEM el rango 0~1757 indica las altitudes máxima y mínima en las que se mueve la Comunitat Valenciana. Os recuerdo que en la imagen original las altitudes son ortométricas.

De esta forma habremos obtenido la imágen que nos puede servir para componer la base del mapa.

<table border="1" cellspacing="2" cellpadding="2"><tbody><tr><td><a href="http://geomaticblog.files.wordpress.com/2009/04/srtm_gdal_04.jpg"><img class="aligncenter size-medium wp-image-817" title="srtm_gdal_04" src="http://geomaticblog.files.wordpress.com/2009/04/srtm_gdal_04.jpg?w=172" alt="" width="172" height="300"></a><div>Figura 5</div></td></tr></tbody></table>

## 8.- Enlaces de interés.

El enlace que siempre hay que tener presente cuando se trabaja con las FWTools es [http://www.gdal.org/gdal\_utilities.html](http://www.gdal.org/gdal_utilities.html) donde se recoge la documentación de las utilidades de GDAL.
