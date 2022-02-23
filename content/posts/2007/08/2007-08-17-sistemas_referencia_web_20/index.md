---
title: "Sistemas de referencia en la web 2.0"
date: "2007-08-17"
categories: 
  - "development"
  - "geodesy"
---

Seguro que trabajando con Sistemas de InformaciónGeográfica, alguna vez has tenido que lidiar con losSistemas de Referencia (SRS) y los Sistemas de Coordenadas (CRS).Además, casi con total seguridad te sonará labase de datos [EPSG](http://www.epsg.org/),creada por el antes llamado European Petroleum Survey Group ahoraconocido como [OGP](http://www.ogp.org.uk/)(Asociación internacional de productores de Petroleo y Gas).

Esta base de datos se ha convertido enprácticamente un estándar para codificar lamiríada de sistemas de coordenadas (UTM, Lambert, etc) ysistemas de referencia (ED50, NAD87, WGS84,...) por códigosmás sencillos de manejar. Bibliotecas como [Proj4](http://www.remotesensing.org/proj/ "Biblioteca Proj4") utilizanesta codificación para poder refererirnos a cualquiercombinación mediante un código. Por ejemplo,la ya casi obsoleta UTM zona 30, huso norte en ED50 secodifica como 23030.

La forma habitual de lidiar con estos códigos era,o bien te bajabas la base de datos Access (urgg) o bien (en mi caso) tebuscabas los códigos directamente en el fichero epsg queviene con Proj4.

Bueno, pues eso ha pasado a mejor vida porque [Howard Butler](http://hobu.biz/) y [Christopher Schmidt](http://crschmidt.net/)han creado [http://spatialreference.org](http://spatialreference.org),una página que permite consultar cualquier códigoEPSG y generar mediante urls muy sencillas la transformaciónde ese código a los diferentes formatos empleadoshabitualmente en los SIG.

Así, si pedimos el código 23030 tenemoslas siguientes direcciones web:

- [http://spatialreference.org/ref/epsg/23030/](http://spatialreference.org/ref/epsg/23030/) Página principal de la proyección en HTML estándar con incluso renderizados de dónde aplica la proyección (esto es muy muy interesante porque por ejemplo esta proyección es sólo válida en una zona concreta de la Tierra, mientras que otras proyecciones son globales).
- [http://spatialreference.org/ref/epsg/23030/prettywkt/](http://spatialreference.org/ref/epsg/23030/prettywkt/) OGC Well Known Text: un sistema de etiquetas definido por el OGC con retornos de carro para que se lea fácilmente
- [http://spatialreference.org/ref/epsg/23030/ogcwkt/](http://spatialreference.org/ref/epsg/23030/ogcwkt/) OGC Well Known Text: idem pero sin retornos de carro
- [http://spatialreference.org/ref/epsg/23030/proj4/](http://spatialreference.org/ref/epsg/23030/proj4/) El formato utilizado por la biblioteca Proj4
- [http://spatialreference.org/ref/epsg/23030/json/](http://spatialreference.org/ref/epsg/23030/json/) Formato orientado a ser empleado por programas escritos en JavaScript
- [http://spatialreference.org/ref/epsg/23030/gml/](http://spatialreference.org/ref/epsg/23030/gml/) Formato XML (increíblemente extenso) para definir CRS y SRS
- [http://spatialreference.org/ref/epsg/23030/esriwkt/](http://spatialreference.org/ref/epsg/23030/esriwkt/) Variante del Well Known Text específica de ESRI (la que se usa en sus ficheros .prj)
- [http://spatialreference.org/ref/epsg/23030/usgs/](http://spatialreference.org/ref/epsg/23030/usgs/) Este no lo conocía, un sistema utilizado supongo por el [USGS](http://www.usgs.gov/) (Servicio Geológico de los Estados Unidos)
- [http://spatialreference.org/ref/epsg/23030/mapfile/](http://spatialreference.org/ref/epsg/23030/mapfile/) Formato para ficheros MAP de UMN Mapserver
- [http://spatialreference.org/ref/epsg/23030/postgis/](http://spatialreference.org/ref/epsg/23030/postgis/) Sentencia SQL para insertar la combinación en la tabla de sistemas de referencia de PostGIS

En realidad algunas no son más que combinaciones deotras pero ¿por qué no hacerlas? seguro que aalguien le resulta útil.

Bueno, ahí queda el recurso que seguro que muchosencontráis tremendamente útil.
