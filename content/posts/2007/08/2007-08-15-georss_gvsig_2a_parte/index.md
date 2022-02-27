---
title: "geoRSS en gvSIG (2a parte)"
date: "2007-08-15"
categories: 
  - "development"
  - "gvsig"
authors: ["jorge-sanz"]
---

Bueno, ya han pasado unas semanas desde que publicamos laprimera versión del _plugin_ para gvSIGpara poder ver capas geoRSS.

Algo de trabajo he hecho y ya va siendo hora de publicarlo,aunque (como no) queda mucho por hacer, pero como rezan loscánones del _software_libre, hay que publicar a menudo, aunque haya pocos avances. Pero no esel caso, porque creo que esta versión es bastante usable yañade bastantes funcionalidades.

Además, para hacer pruebas con el sistema dedocumentación técnica [DocBook](http://www.docbook.org "Sitio web de DocBook") (y porque es imprescindible documentar, claro) he escrito unpequeño [manualde usuario](/gb2/files/geoRSS/doc/index.html) y en un futuro algo de documentacióntécnica sobre este pequeño proyecto.

Por cierto, ya que que Alejando en la [Cartoteca](http://alpoma.net/carto/ "La Cartoteca") ha estado haciendo [experimentos](http://alpoma.net/carto/?p=385) con geoRSS y [Flickr](http://www.flickr.com), le invitamos desde aquí a que los continúe con [gvSIG](http://www.gvsig.gva.es), que puede dar bastante juego ![Gui&ntilde;o](images/smiley-wink.gif "Gui&ntilde;o").

[![geoRSS en gvSIG](images/geoRSSLayer02.preview.png)](/gb2/files/images/geoRSSLayer02.png)

Para no aburrir las características principales delplugin:

- Acepta orígenes RSS y Atom
- Acepta noticias geocodificadas con puntos en el estándar geoRSS
- Permite ver en gvSIG el contenido de la noticia RSS y proporciona el enlace a la noticia completa (abriéndose el navegador web).
- Las capas se pueden almacenar en el proyecto, al abrirse de nuevo recuperará las noticias actuales.
- Se pueden recargar las noticias en cualquier momento (me acabo de dar cuetna de que esto no está en la doc de usuario, cachis).

Cosas que **no** van bien

- Por alguna extraña razón, cuando te acercas mucho las geometrías no se pintan en pantalla.
- Al guardar da un error con respecto a las tablas, pero la operación termina correctamente
- Seguro que hay muchas más pero no lo he probado a fondo, a fin de cuentas esto es por amor al arte y para apreder ![Gui&ntilde;o](images/smiley-wink.gif "Gui&ntilde;o").

Cosas que se podrían **mejorar**

- Como no, los javadocs... eso da una pereza terrible pero hay que hacerlo **SIEMPRE** y **BIEN**.
- El diálogo de añadir capa debería integrarse con gvSIG
- El botón de información por punto debería integrarse con el de gvSIG
- Podrían marcarse con un tono más saturado los items visitados, al estilo de los visores de RSS
- Podría hacerse un temático con lasnoticias más viejas y las más nuevas
- En general, se podría trabajar más con la capa para ofrecer una funcionalidad más parecida a la de un lector de RSS, ya que actualmente la capa es simplemente un tema de puntos en memoria sin persistencia de ningún tipo.

Finalmente, los ficheros de distribución:

- [Binarios](/gb2/files/geoRSS/gvSIG-geoRSS-bin-BN38.zip) (las instrucciones de instalación están en la [documentación](/gb2/files/geoRSS/doc/index.html))[  
    ](/gb2/files/geoRSS/gvSIG-geoRSS-bin-BN38.zip)
- [Fuentes](/gb2/files/geoRSS/gvSIG-geoRSS-src-BN38.zip) (el proyecto de [Eclipse](http://www.eclipse.org/))[  
    ](/gb2/files/geoRSS/gvSIG-geoRSS-src-BN38.zip)
- [Documentación](/gb2/files/geoRSS/extGeoRSSLayerDoc_v0.5.0.tar.gz) (proyecto Eclipse con los fuentes [DocBook](http://www.docbook.org "Sitio web de DocBook") y la tarea [Ant](http://ant.apache.org/) que genera todo) cuidado porque hay referencias que no se podrán cumplir al faltar los recursos (DTD, XSL, etc) que están fuera del proyecto. Si alguien está interesado en esta parte puedo más adelante contar como funciona.

Lo siguiente en lo que me gustaría meterme (cuando vuelva de las vacaciones) es integrar la documentación de usuario **DENTRO** de gvSIG, ya que me parece **IMPRESCINDIBLE** que un buen _software_ tenga una ayuda en línea, ni siquiera apuntando a un sitio _web_ (o al menos no sólo) sino que debe estar descargada en la máquina. A ver qué sale, iremos informando.

Bueno, con esto cierro este proyecto de momento,. Espero queos sea útil como usuarios y a alguno como desarrollador.
