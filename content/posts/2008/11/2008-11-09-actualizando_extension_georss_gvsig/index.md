---
title: "Actualizando la extensión para geoRSS de gvSIG"
date: "2008-11-09"
categories: 
  - "gvsig"
  - "ogc"
authors: ["jorge-sanz"]
---

Hace ya [más deun año](/gb2/es/2007-08-15-georss_gvsig_%282a_parte%29) desde la última vez que le dediqué algo de tiempo a laextensión y tenía algunos errores y cosas que quería tocar. Por un ladohace ya algún tiempo [salióen las listas de gvSIG](http://runas.cap.gva.es/pipermail/gvsig_usuarios/2008-May/004954.html) el tema del bloqueo activo que Googlerealiza en Cuba cumpliendo las leyes del embargo. Esto fue bastantecriticado y en ese momento, como [SEXTANTE](http://www.sextantegis.com), ya meplateé que el servicio era bueno pero no se justificaba si había otrasopciones.

Un compañero de gvSIG me comentó que [JavaHispano](http://www.javahispano.org/)tenía una [forja](http://www.javahispano.net),así que me puse manos a la obra y efectivamente, aunque con algunosproblemas al principio, pude migrar el código a un [nuevoproyecto](http://javahispano.net/projects/georss4gvsig/) en la forja de JavaHispano y ahí quedó la cosa.Durante ese tiempo salió [OSOR](http://osor.eu/),una forja también de proyectos libres que creo que se queda grande paralo que yo tengo entre manos, aunque me comentaron que si seguía con elproyecto podría usar sus infraestructuras. No creo que cambie, si todova bien, JavaHispano funciona, no muy rápido pero suficiente y tienemucho más de lo que necesito.

En fin, nada que ayer le dediqué unas horas a conseguir que laextensión funcione correctamente sobre gvSIG 1.1.2 (dejo para másadelante la adaptación a gvSIG 2.0) y ya la tenéis disponible en el [catálogode extensiones de gvSIG](https://gvsig.org/plugins/downloads/georss-support) y en la [páginade la forja](http://javahispano.net/projects/georss4gvsig/).

Por otro lado he intentado sin éxito integrar la ventana deinformación de geoRSS en la herramienta y el diálogo de gvSIG. La razónprincipal es que me obliga a llevar al ámbito de gvSIG (a su carpeta com.iver.cit.gvsig/lib)partes de la extensión que no deberían estar ahí. Esto es por un temabastante peliagudo y que se está resolviendo. SEXTANTE también losufre, a ver si se soluciona (con un trabajo duro, no sale porgeneración espontánea, creedme que lo sé) y tenemos un gvSIG aún másmodular y extensible.

Otra cosa que he hecho y que me tenía un poco mosqueado desdehace tiempo es el tema de la documentación. Está hecha con [DocBook](http://www.docbook.org/) pero noconseguía organizarlo de forma sencilla para que cualquiera pudieracompilarla (aunque dudo mucho que nadie vaya a hacerlo). Finalmente ygracias a un trabajo del equipo de [Apache Velocity](http://velocity.apache.org)llamado [DocBookFramework](http://velocity.apache.org/docbook/), he conseguido que sólo haya que descargar el _framework_y si lo pones en el mismo _workspace_ que tuproyecto funciona a la primera ya que tiene todos los componentes. La [documentación](http://georss4gvsig.javahispano.net/)es la misma, pero la forma de montarla es muchísimo más sencilla.

Cosas por mejorar: pues las mismas de antes, mejores javadocs,la documentación técnica del bicho (tampo esto es una obra de laingeniería del software francamente, cualquiera que le pegue un poco agvSIG lo comprenderá fácilmente) y tal vez un poco más de **cariño**en las Interfaces de Usuario.

Dejo unas capturas de pantalla: una de la extensiónfuncionando con el [RSSinternacional](http://www.ELPAIS.com/rss/rss_section.html?anchor=elpporint) del periódico [ElPaís](http://www.elpais.es) (previo paso por geonames, la extensión se encarga, tusabes) y del [geoRSS](http://api.flickr.com/services/feeds/geo/?tags=colorful&lang=es-us&format=rss_200)de las fotos con la etiqueta [colorful](http://flickr.com/photos/tags/colorful/) de flickr (el geoRSS se puede ver abajo del todo de la página, junto alKML) y otras dos con ejemplos de información de una noticia de cadauno, pinchando en la imagen se puede ir a la noticia real. He dereconocer que la sección Internacional del País es para no leerla, quelo más suave que haya encontrado (sin muertos y esas cosas)sea de Sarah Palin...

![gvSIG con dos orígenes geoRSS](images/screenshot-georss-0.2.png "gvSIG con dos orígenes geoRSS")

[![Noticia geoRSS del País](images/georss-palin.png)](http://www.elpais.com/articulo/internacional/Palin/llama/estupidos/asesores/McCain/culpan/derrota/elpepiint/20081109elpepiint_4/Tes)

[![Noticia geoRSS de flickr colorful](images/georss-colorful.png "Noticia geoRSS de flickr colorful")](http://www.flickr.com/photos/olibac/3012125403/)
