---
title: "geoRSS en gvSIG"
date: "2007-07-28"
categories: 
  - "development"
  - "gvsig"
authors: ["jorge-sanz"]
---

Hace algún tiempo, vi en el blog de [Meneame](http://blog.meneame.net/2007/07/02/geolocalizacion-de-usuarios-y-noticias/)quepublicaban sus noticias añadiendo informaciónsobre la localización de las noticias gracias alestándar [geoRSS](http://georss.org/).Automáticamente (esdeformación profesional) pensé queestaría bien poder presentar en [gvSIG](http://www.gvsig.gva.es) las noticiascomo side una capa más se tratara. De hecho ya lo habíapensado antes para las fotos de [Panoramio](http://www.panoramio.com)(para el quetambién hay un [API](http://www.panoramio.com/api/)que permite estas cosas) pero en sumomento me dio algo de pereza.

Pero esta vez, se dieron los condicionantes, a saber:

- Un curso sobre desarrollo con Software Libre que realicé hace poco: los comienzos
- En verano resido en un chalet: no hay Internet
- Jornada intensiva en el trabajo: tardes libres
- Tampoco hay televisión: mi padre ve el «tomate» y «betty la fea».
- Alternancia entre piscina y nuevo libro de Harry Potter: _un poco de programación no hace daño a nadie ¿no?_

Así que nada, manos a la obra.

## Objetivos

- Crear un nuevo origen de datos en gvSIG: un canal geoRSS
- Poder fácilmente añadir dicho recurso a gvSIG como una capa vectorial (¿tal vez especializando alguna clase existente?)
- Profundizar en la metodología de configuración de proyectos de desarrollo sobre gvSIG, para conseguir la forma **más sencilla y rápida** de empezar a programar (luego diré el por qué de esto).
- Probar bibliotecas que no conocía para evaluarlas ([renderizado de HTML](http://html.xamjwg.org/cobra.jsp), [parseo de documentos XML](http://www.jdom.org/)).
- Probar sistemas de documentación de código ([Doxygen](http://www.stack.nl/%7Edimitri/doxygen/)).
- Probar sistemas de documentación técnica ([Docbook](http://www.docbook.org/)).
- Aprender más sobre la arquitectura de gvSIG, y si puede ser divertirse programando.

En fin, muchas cosas, algunas conseguidas y otras no.

## Estado actual

¿Que tengo hecho? Pues más o menos hayuna versión funcional del plugin con lo siguiente:

- Un conjunto de clases que modelan un origen de datos geoRSS utilizando como parser XML la biblioteca [JDOM](http://www.jdom.org/).
- Una especialización de la clase MemoryDriver que recibe como origen un objeto de las clases anteriores.
- Una interfaz para presentar una noticia RSS, utilizando una plantilla HTML configurable. Esta interfaz lanza todos los enlaces al navegador del usuario.
- Para probar esto, un botón que carga las noticias de Meneame.net.

[![geoRSSLayer 01](images/geoRSSLayer01.preview.png "geoRSSLayer 01")](/gb2/files/images/geoRSSLayer01.png "geoRSSLayer 01<br /><br /><a mce_thref="http://geomaticblog.net/gb2/es/2007-jul-28-georsslayer_01">View Image Information</a>")

## Cosas que quiero terminar

Estas son las cosas que seguro voy a terminar porque creo quevale la pena dejar algo a mi entender redondo. Como esto essólo un prototipo que he realizado por mi cuenta (y gratis)pues no creo que siga trabajando mucho más en el proyecto.Eso no quiere decir que como _software_ que selicencia bajo la **GPL**,cualquiera pueda continuar el trabajo para mejorarlo o adaptarlo a loque le dé la gana.

En orden de prioridad, tengo estas tareas autoimpuestas:

- Una ventana con información sobre el canal RSS
- Un nuevo botón para añadir un geoRSS cualquiera (ahora se podría hacer cambiando la url del fichero text.properties).
- Revisar los _javadocs_ para no dejar nada importante sin comentar
- Tal vez en la funcionalidad de obtener información de la noticia podría hacer que saliera el título de la noticia en el _tooltip_ del ratón o en la barra de estado.
- Implementar los métodos que permiten que se pueda guardar la capa (actualmente no se guarda).

Seguro que quedará con montones de bugs y cosas porhacer pero ya digo, esto lo he planteado como **unprototipo** paraaprender un poco más de ciertas áreas de gvSIGque quería explorar, así que si alguien se animay quiere contribuir, es bienvenido.

## Algunas consideraciones finales

- A ver si tenemos pronto una **infraestructura** para que la comunidad gvSIG aporte este tipo de trabajos, más o menos profesionales, pero siempre bienvenidos, tanto plugins escritos en Java como para los distintos lenguajes de _scripting_ que gvSIG soporta. Yo de momento me he apañado con un repositorio [SVN](http://project.knowledgeforge.net/geomaticlabs/svn/) aportado por el proyecto [KnowledgeForge](http://www.knowledgeforge.net/).
- Queda pendiente por mi parte, si lo consigo, encontrar una metodología de configuración para no tener que compilar **TODO GVSIG** antes de empezar a desarrollar. No es tan complicado, y seguro que alguno de los que lean esto saben cómo hacerlo, pero creo que se puede **facilitar bastante más** la tarea de empezar a programar en gvSIG a la gente recién llegada.
- Sobre las experiencias en el desarrollo y tal, pues contaré algo en función de las ganas y el **feedback** que haya. Yo ya he aprendido, así que lo escriba aquí ya es por puro altruismo bloguero.
- En cuanto al _plugin_..., si habéis llegado hasta aquí merecéis probarlo :D, estos son los pasos a seguir:
    1. descomprime [este zip](/gb2/files/geoRSS/net.geomaticblog.georss.zip) en tu carpeta extensiones de gvSIG
    2. activa una vista en 4326
    3. cuenta por aquí tus impresiones
    4. Si te atreves o tienes curiosidad, aquí están los [fuentes](/gb2/files/geoRSS/net.geomaticblog.georss.src.zip) a fecha de hoy
- Uis, casi se me olvida, esto va sobre gvSIG 1.1rc1 y java 1.5 ![Avergonzado](images/smiley-embarassed.gif "Avergonzado")
