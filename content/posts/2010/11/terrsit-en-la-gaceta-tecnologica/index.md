---
title: "Terr@sit en la Gaceta Tecnológica"
date: "2010-11-07"
categories: 
  - "geodata"
  - "ide"
  - "opinion"
---

De vuelta de la [LSWC](http://www.libresoftwareworldconference.org/) me entretuve muy temprano en el aeropuerto de Málaga leyendo un ejemplar de la [Gaceta Tecnológica](http://www.gacetatecnologica.com) que había cogido en el congreso y que todavía no había podido leer. Andaba yo con mi café con leche y bizcocho cuando en el apartado dedicado a la geomática de esta estupenda publicación veo una noticia que me termina de despertar: "[La Comunidad Valenciana pone en marcha la primera cartografía digital en 3D de España](www.gacetatecnologica.com/sig/1436-la-comunidad-valenciana-pone-en-marcha-la-primera-cartografia-digital-en-3d-de-espana.html)".

Así que nada me pongo a leer la noticia y cada párrafo me hacía abrir un centímetro más la mandíbula, básicamente va transcribiendo frases de la presentación del proyecto [Terr@sit](http://terrasit.gva.es) por parte del Muy Honorable Señor. Está claro que un político no suele acertar demasiado con las orientaciones técnicas reales de un proyecto cuando se presenta, pero hay bastantes perlas en el artículo que me han sorprendido (para mal, claro). De hecho tantas que en lugar de ponerlas aquí directamente las he puesto de una forma alternativa y muy "web 2.0". He subrayado y anotado la noticia, y se pueden visitar [aquí](http://awurl.com/GozBp6HDU).

Y es que me resultan muy tristes una serie de puntos:

- Que uno vaya a la web de [servicios WMS del ICV](http://www.icv.gva.es/ICV/secciones/noticias/noticias/wms/wms.html) y tenga cuatro capas, con 2004 como el año de la última ortofoto publicada
- Que el portal Terr@sit ofrezca un visor 3D que sólo funciona en Windows, y al resto de usuarios de MacOS y Linux que nos den[![Terr@sit fail](images/4855893485_3f0769f274.jpg)](http://www.flickr.com/photos/xurxosanz/4855893485/ "Terr@sit fail by XuRxO, on Flickr")
- Que la esperanzadora etiqueta de [Geoservicios](http://terrasit.gva.es/es/acceder-geoservicios) lleve a una página de descargas de archivos [GeoPDF](http://en.wikipedia.org/wiki/GeoPDF) un formato que sólo puede visualizarse correctamente con el visor de Adobe (a partir de la versión 6) y que por tanto de _servicio_ tiene poco.
- Que se diga que los autónomos y pymes se beneficiarán de este visor, ¿alguien me puede explicar cómo?
- Que se diga que todos los valencianos vamos a gozar de la misma información, cuando mucha de la información de ese portal sólo está accesible mediante el visor (bueno esto no es del todo cierto, luego hablo más) o directamente hay que ir a comprarla al ICV.

En fin, el artículo tiene más perlas que se pueden ver en mis notas, pero estas eran las más gordas. Al menos de todo esto he sacado algo positivo. En primer lugar comprobar que el uso del software libre en el proyecto, ya que el portal está realizado usando [OpenLayers](http://www.openlayers.org) y [JQuery](http://www.jquery.org) y en el servidor el ICV sigue apostando por [UMN Mapserver](http://mapserver.org/) como tecnología base.

Y la segunda viene derivado del uso de UMN Mapserver, y esto hay que agradecerlo seguramente a los técnicos que hay detrás del proyecto que se han preocupado de configurarlo correctamente. Resulta que realmente no sólo tenemos un visor, sino un servidor WMS **no publicitado** para acceder a la cartografía del portal. Desgraciadamente gvSIG no negocia bien el acceso a este servidor por algún problema entre versiones del protocolo WMS que por lo que sé, debería resolverse a medio plazo, pero al menos desde otras aplicaciones de webmapping podemos usar estas pocas capas, ya que el [capabilities](http://terramapas.icv.gva.es/cgi-bin/mapserv.fcgi?map=/var/www/portal/map/servidor.map&REQUEST=GetCapabilities&SERVICE=WMS&VERSION=1.1) no dice nada al respecto. La url del servicio es

[http://terramapas.icv.gva.es/cgi-bin/mapserv.fcgi?map=/var/www/portal/map/servidor.map](http://terramapas.icv.gva.es/cgi-bin/mapserv.fcgi?map=/var/www/portal/map/servidor.map)

En fin, esta perorata al final va de que Terr@sit ni ofrece abiertamente como servicios estándar la cartografía de la que se nutre, y que para cumplir con INSPIRE al menos debería estar catalogada en algún servidor (¿[el de la CMA](http://geocatalogo.cma.gva.es/geonetwork/srv/es/main.home) por ejemplo?) ni cumple con la necesaria neutralidad tecnológica al limitar sus servicios a usuarios de un sistema operativo concreto. Esto último probablemente incumpla alguna directiva nacional pero ahí ya no entro porque la verdad es que no tengo ni idea, si alguien puede confirmar o desmentir esto se agradecería un comentario.
