---
title: "TileCache en Windows"
date: "2008-01-24"
categories: 
  - "foss"
---

Las últimas semanas he estado trasteando con MapServer y OpenLayers en el curro y llegué a la conclusión de que tenía que "cachear" algunas de las grandes imágenes que usamos.

El problema llegó cuando intenté montar un TileCache de MapLabs y no estaba tan fácil como sugerían en su página. En parte por el famoso [proxydel curro](http://runas.cap.gva.es/pipermail/gvsig_usuarios/2005-November/000437.html) en parte porque voy un poco a bastonazos con este asunto y no encuentro un solo sitio dónde me expliquen cómo se hace. Al final después de marear mucho la perdiz conseguí que funcionara y ahora os explico, en modo recetario, qué es lo que hice:

Primero:

Bajar la aplicación de [la página de TileCache](http://www.tilecache.org/ "http://www.tilecache.org/")

Segundo:

Descomprimir en un directorio accesible vía web

C:\\xampp\\htdocs\\tilecache

Tercero:

Permitir la ejecución CGI en el directorio,añadiendo la siguiente sección en **httpd.conf**

\# Tilecache
<Directory "C:/xampp/htdocs/tilecache"> 
AddHandler cgi-script .cgi Options +ExecCGI +Indexes
</Directory>

Cuarto:

Editar la primera línea de **tilecache.cgi**para que apunte al ejecutable de Python

#!C:/Python24/python.exe

Quinto:

Modifica **tilecache.cfg** para que guarde elcache en el directorio temporal deseado

\[cache\]
type=Disk
base=d:/temp/tilecache

Sexto:

Modifica **tilecache.cfg** para que cargue lacapa WMS correspondiente (Con diferencia este es el paso que más por el... dió). Los ¶ son retornos de carro que hay que eliminar en tu fichero, puestos aquí para que quepan en el ancho del blog.

\[rhyncho\]
type=WMSLayer
url=http://tuservidor/cgi-bin/rhynchoms? ¶
        map=c:/xampp/htdocs/mapas/tuarchivo.map
layers=BaseCarto
extension=png
srs=EPSG:23030
extent\_type=loose
bbox=620193, 4189704, 819285, 4519881
resolutions=599.7218983723972,¶
                  88.19439681947017,¶
                  17.63887936389403,¶
                  8.819439681947015,¶
                  3.527775872778806,¶
                  1.763887936389403,¶
                  0.8819439681947016

Desde el primer día la configuración del Mapserver y de Openlayers me ha llevado por la calle de la amargura, sobretodo con el tema de las resoluciones y las escalas, para obtener qué resoluciones correspondían a los niveles de zoom que tengo definidos me tocó añadir en el código estándar de Openlayers (en la función init) las siguientes líneas:

var str = "";
for (var i = 0; i < myScales.length; i++) {
    str +=
      OpenLayers.Util.getResolutionFromScale(
          myScales\[i\], 'm'
       ) + ',';
}
var element = document.getElementById('output'); 
element.innerHTML = str;

y añadir un contenedor <div>llamado outputpara que mostrara el resultado.

Séptimo:

Carga en el openlayers

base = new OpenLayers.Layer.WMS( "Base cartografica",
   "http://tuservidor/tilecache/tilecache.cgi",
  {layers: 'rhyncho' });

La receta es un poco "as is", espero que os sirva, aunque uno de los inconvenientes de dar palos de ciego es que no voy a poder contestarlas preguntas con soltura, sin embargo si hay preguntas o sugerencias serán bien atendidas

¡Saludos!
