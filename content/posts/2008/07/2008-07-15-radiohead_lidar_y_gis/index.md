---
title: "Radiohead, LIDAR y GIS"
date: "2008-07-15"
categories: 
  - "gvsig"
  - "photogrammetry"
---

Radiohead es un grupo ya veterano pero que desde luego nodejan de sorprender. El [últimovídeo](http://code.google.com/creative/radiohead/) en lugar de grabarlo con cámarasde vídeo convencionales ha sido grabado con un láser escáner terrestre.Así, cada fotograma es en realidad un escaneo formando una verdaderacapa 4D. Pero lo mejor es que los [materiales](http://code.google.com/p/radiohead/downloads/list)con los que han generadoel vídeo han sido colgados en la página de este peculiar proyecto conlo que tenemos dos partes del vídeo House of Cards en sendos ficheroscomprimidos con un archivo separado por comas (CSV) por cada frame delvídeo.

Si a eso le añadimos un poquito de operativa en gvSIG (por elcontrapunto con el [blogger](http://thes.wordpress.com/2008/07/14/radiohead-gis/)que lo hizo con tecnología ESRI), a saber:

- Tomemos por ejemplo el primer fichero, el 1.csv
- Reemplazamos las comas por punto y comas (;)
- Añadimos una fila de títulos, yo qué se X;Y;Z;L porejemplo
- Importamos a gvSIG el fichero CSV
- Lo pasamos a tema de eventos asignando a las dos primerascolumnas la X y la Y (con esto tenemos la cara girada)
- Luego exportamos a shape y ponemos en edición la tabla
- Creamos 4 campos tres de tipo Double y uno de tipo Integer
- Con la calculadora de campos y usando la simple expresión toNumber(\[COLUMNA\])\*-1"giramos" las dos primeras y lo mismo pero sininvertir para la tercera y la cuarta
- Guardamos y borramos la capa
- Abrimos el dbf del shape generado, **sólo eldbf** como unanueva tabla
- Volvemos a crear la capa de eventos, esta vez con lascolumnas con doubles invertidos
- Ya podemos aplicar un **temático**sobre la cuarta columna, lade enteros para dar un poco de color
- Listo

  

[![RadioHead Frame1](images/2672409938_f23a3d90a8.jpg)](http://www.flickr.com/photos/xurxosanz/2672409938/ "RadioHead Frame1 por XuRxO, en Flickr")

Es un juego de datos de centenares de frames, con el que a másde uno que yo me sé se le van a ocurrir procesos de análisis de lo másvariopintos, que aquí el que esté libre de pecado que tire la primerapiedra (y el valle del Jerte lo tenemos un poquito visto :-P).

**¿¿Leaplicamos un modelo gaussiano al variograma??**
