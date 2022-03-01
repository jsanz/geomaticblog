---
title: "Instalando MapProxy en windows, paso a paso"
date: "2012-10-10"
categories: 
  - "foss"
  - "ogc"
  - "osgeo"
authors: ["ivan-sanchez"]
---

La semana pasada tuve el placer de formar parte de los formadores de los [voluntarios de EUROSHA](http://hot.openstreetmap.org/updates/2012-10-04_become_a_tutor_of_the_eurosha_volunteers), un grupo de 25 jóvenes destinados a levantar cartografía en diversos países de África, como parte de las actividades del [HOT](http://hot.openstreetmap.org/). Uno de los problemas a los que se enfrentan estos voluntarios es una conexión a internet no muy fiable.

Es perfectamente posible editar datos de OSM offline (guardando los datos a fichero, editando, y resolviendo conflictos de versiones a posteriori), pero lo que no se puede hacer es consultar cartografía de fondo para comparar. Había que hacer algo al respecto. Y la solución fue instalar [MapProxy](http://mapproxy.org/), que permite tomar imágenes ráster de varias fuentes y servirlas como WMS, en local. En un portátil con linux (y python, python-pil y python-pip), instalarlo y probar la configuración por defecto fue una cuestión de minutos.

Ahora bien, los ordenadores que el HOT va a desplegar en África van con windows, principalmente por no disponer del tiempo suficiente para hacer una instalación completa con las herramientas adecuadas para la situación. Improvisemos pues, e instalemos MapProxy tal y como sugiere el [manual](http://mapproxy.org/docs/latest/install_windows.html)...

> We advise you to install MapProxy into a [virtual Python environment](http://guide.python-distribute.org/virtualenv.html).

Bueno, pues **no hagáis esto**. Al instalar python desde cero, lo más probable es que os encontréis con problemas a la hora de instalar las librerías necesarias, en concreto PIL (Python Imaging Library). La manera sencilla de instalar Python para hacer funcionar MapProxy encima es [OSGeo4W](http://trac.osgeo.org/osgeo4w/). Así que descargamos el instalador, elegimos una instalación avanzada, y nos aseguramos de que al menos los paquetes para python y python-pil se van a instalar:

[![](/imgs/2012/10/mapproxy-howto-1.png?w=300 "mapproxy-howto-1")](/imgs/2012/10/mapproxy-howto-1.png)

El siguiente paso es descargarse [distribute-setup.py](http://pypi.python.org/pypi/distribute#distribute-setup-py) y ejecutarlo dentro de una shell de OSGeo4W _como administrador_:

[![](/imgs/2012/10/mapproxy-howto-3.png?w=184 "mapproxy-howto-3")](/imgs/2012/10/mapproxy-howto-3.png)

[![](/imgs/2012/10/mapproxy-howto-2.png?w=300 "mapproxy-howto-2")](/imgs/2012/10/mapproxy-howto-2.png)

En esa misma consola, ejecutamos un _easy\_install mapproxy_, y justo después un _easy\_install pyproj_:

[![](/imgs/2012/10/mapproxy-howto-4.png?w=300 "mapproxy-howto-4")](/imgs/2012/10/mapproxy-howto-4.png)

En este punto, los ejecutables de MapProxy ya están instalados. Lo podemos comprobar ejecutando _mapproxy-util_:

[![](/imgs/2012/10/mapproxy-howto-5.png?w=300 "mapproxy-howto-5")](/imgs/2012/10/mapproxy-howto-5.png)

Ahora bien, MapProxy es inútil sin un fichero de configuración que le diga qué servicios tiene que cachear. Así que hacemos copia-pega de una [configuración de MapProxy para OpenStreetMap](https://wiki.openstreetmap.org/wiki/Mappproxy_setup), guardamos el fichero resultante como (por ejemplo) _C:\\OSGeo4W\\mapproxy.yaml_, y lanzamos _mapproxy-util_:

[![](/imgs/2012/10/mapproxy-howto-6.png?w=300 "mapproxy-howto-6")](/imgs/2012/10/mapproxy-howto-6.png)

¡Et voilà! Nuestro MapProxy está funcionando y respondiendo a peticiones desde localhost:8080, cacheando tiles de OSM para convertirlas en un servicio WMS:

[![](/imgs/2012/10/mapproxy-howto-7.png?w=293 "mapproxy-howto-7")](/imgs/2012/10/mapproxy-howto-7.png)

El resto de opciones se pueden consultar en el manual de MapProxy, pero hay unas cuantas cosas a tener en cuenta:

- MapProxy siempre debe ejecutarse dentro del entorno de OSGeo4W.
- ... lo que quiere decir que si queremos que se ejecute automáticamente, se puede hacer un _.bat_ haciendo copia-pega de _C:\\OSGeo4W\\osgeo4w.bat_, y modificando el comando que se lanza en la última línea de ese script.
- La utilidad para inicializar o refrescar la caché, _mapproxy-seed.exe_, ha de ejecutarse también dentro del entorno de OSGeo4W.
- Los datos cacheados se almacenan en el directorio que se especifique en el fichero de configuración, y es relativo a la ruta donde se lanza mapproxy.
