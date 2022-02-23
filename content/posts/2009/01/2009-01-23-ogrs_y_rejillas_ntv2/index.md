---
title: "De OGRs y Rejillas NTv2"
date: "2009-01-23"
categories: 
  - "cartography"
  - "foss"
---

Después de 7 años me han vuelto a sacar a campo. Así comosuena. Llevo 7 años criando culo, encadenado a la mesa, picando código y desgastando la vista y de un día para otro me dejan encimade la mesa un marrón de esos que molan, porque en el fondo, hay un topógrafo en mi y me mola más el campo que a Geppetto una lijadora eléctrica. Bueno, que me pierdo.

Me mandan a campo con un GPS peeeero a la vueltaquieren unos datos en ED50UTM30N (EPSG:23030). Que pesaos son, a ver cuando se enteran de [qué es el ETRS89](http://www.fomento.es/MFOM/LANG_CASTELLANO/DIRECCIONES_GENERALES/INSTITUTO_GEOGRAFICO/Geodesia/red_geodesicas/etrs89.htm) y de que las cosas habría que pedirlas en [EPSG:25830](http://spatialreference.org/ref/epsg/25830/),que [ya se lo hemos dicho en alguna ocasión](http://geomaticblog.net/node/112) pero vamos, que [ni publicándolo en el BOE](http://www.boe.es/aeboe/consultas/bases_datos/doc.php?coleccion=iberlex&id=2007/15822&t).

Muy bien, puesto que [gvSIG te permite hacer la conversión usando la _rejilla_en formato NTv2 del IGN](http://www.gvsig.org/web/docusr/userguide-gvsig-1-1/extension-jcrs-gestion-de-sistemas-de-referencia-de-coordenadas/transformaciones-1/transformacion-por-fichero-rejilla/) a priori no hay problema y la cosa queda muy bien, en la imágen podéis ver los datos en azul que son los datos WGS84 (EPSG:4326) reproyectados usando la rejilla y los datos en rojo que son los mismos datos pero en EPSG:23030, coincidencia total, color morado, que es el de la ingeniería.

[![](http://geomaticblog.files.wordpress.com/2009/01/artogr_p001.png?w=300 "Morado, el color de la ingeniería")](http://geomaticblog.files.wordpress.com/2009/01/artogr_p001.png)

Pero a mi me asaltó una duda, ¿y si no tengo gvSIG? ¿con qué lo hago?. Pues como gvSIG usa [proj.4](http://trac.osgeo.org/proj/) para esos menesteres y [GDAL](http://www.gdal.org/)también, y como [ya nos explico Eloi](http://geomaticblog.net/gb2/en/2008-03-28-convers%C3%A3o_formatos_vectoriais) se puede usar GDAL en línea de comandos con OGR, me he decidido a realizar la misma transformación a mano y usando ogr2ogr.

Así que me miro un par de webs, cazo un par de ideas por aquí y por alla y compongo la siguiente instrucción lista para teclear en la consola (cuidado que he añadido un salto de línea para mayor claridad):

`ogr2ogr -t_srs "+proj=utm +zone=30 +ellps=intl +units=m +nadgrids=./sped2et.gsb" -f "ESRI Shapefile" DatCam04_23030_proj.shp DatCam04.shp`

Explico por partes: "\-t\_srs" indica que se deben reproyectar los datos de salida, la cadena de texto que viene detrás son los argumentos de proj.4 que indican el SRS de salida, ojo al argumento "+nadgrids" que debe tener la ruta completa al fichero de rejilla (yo lo copié al mismo directorio donde estaba el archivo SHP por comodidad, por ello el "./"). "\-f "ESRI Shapefile"" indica el formato de salida. Por último el nombre del archivo de salida y el de entrada.

Voy todo ilusionado, le doy al enter y...

[![](http://geomaticblog.files.wordpress.com/2009/01/artogr_p002.png?w=300 "El viñedo yendose a por uvas")](http://geomaticblog.files.wordpress.com/2009/01/artogr_p002.png)

Para el que no tenga ganas de sacar proporciones de las imágenes, se trata de un desplazamiento de unos 170m en dirección SW.¿Que diablos ha pasado?

Aja, OGR no está usando la rejilla para hacer la transformación, ¿pero si la documentación y los ejemplos de transformación [NAD27y NAD83](http://en.wikipedia.org/wiki/NAD83) funcionan? ¿qué pasa?

Bueno, os ahorro un rato de pruebas futiles y googleo: [aquí](http://n2.nabble.com/nasty-gotcha-in-alternate-ogr2ogr-solution-comment-td1928022.html#none) está la pista y [aquí](http://trac.osgeo.org/gdal/changeset/15681) la explicación del problema y la solución, que os detallo acontinuación.

Resulta que el problema es de OGR, que no es capaz de parsear todos los comandos de proj.4, así que cuando no entiende algo, lo ignora, como pasa en la imagen de arriba. Como esa es una fea costumbre, los programadores de OGR ha incluido una pequeña puerta de atrás: el parámetro "+wktext". Al incluir este parámetro en la cadena que le damos a OGR le indicamosque NO tiene que parsear la cadena, si no más bien pasársela tal cual a proj.4 para que esté haga la magia.

Así pues, ni corto ni perezoso incluí el dichoso parámetro en la instrucción:

`ogr2ogr -t_srs "+proj=utm +zone=30 +ellps=intl +units=m +nadgrids=./sped2et.gsb **+wktext**" -f "ESRI Shapefile" DatCam04_23030_proj.shp DatCam04.shp`

Y de esta manera obtuve la imagen que deseaba:

[![](http://geomaticblog.files.wordpress.com/2009/01/artogr_p003.png?w=300 "Todo en su sito")](http://geomaticblog.files.wordpress.com/2009/01/artogr_p003.png)

Que ustedes lo reproyecten bien.

**ACTUALIZACIÓN (20/01/2012):** [Nacho Varela](https://twitter.com/nachouve)  nos informó de que se habían perdido las imágenes, y en el proceso de reconstruirlas nos hemos dado cuenta que el famoso "+wktext" ya no es necesario y que GDAL/OGR ya parsea la cadena completa y se la manda a proj4.
