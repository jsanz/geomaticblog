---
title: "Documentar, adiós Word/Writer hola Firefox"
date: "2008-09-16"
categories: 
  - "opinion"
---

A lo largo de los últimos años para la redacción de documentación he utilizado infinidad de sistemas. Entiéndase sistema de documentación por algún _software_ que a partir de un fichero de texto convenientemente redactado genere una salida que sea ya **publicable** bien en Internet (como documento HTML en general), bien para imprimir (por ejemplo en PDF).

En función de las necesidades (a veces impuestas por otros, otras por iniciativa propia) he ido abordando los diferentes sistemas. Ciertamente todos tienen puntos en común aunque hay dos grupos principales: los de _semántica fuerte_ y los de _semántica débil_.

Por otro lado, la proliferación del mantenimiento diferentes repositorios de documentación en gestores de contenidos como [Plone](http://www.plone.org) o **wikis** como [Confluence](http://www.atlassian.com/software/confluence/) o [MediaWiki](http://www.mediawiki.org/wiki/MediaWiki/es) provoca que acabemos utilizando como 3 o 4 sistemas de marcado. En cualquier caso estas soluciones son claramente superiores frente a las ya anticuadas carpetas de archivos en _Word_ o _[Writer](http://www.openoffice.org)_, y esto el que haya tenido que escribir un documento de forma colaborativa lo comprenderá perfectamente. La verdad es que tras unos minutos te haces a cualquiera de ellos y permite concentrarse bastante bien en lo importante **la documentación**.

### Sistemas de semántica fuerte

Estos lenguajes marcan claramente los diferentes tipos de contenidos con algún tipo de comando. Por ejemplo, con [LaTeX](http://es.wikipedia.org/wiki/LaTeX) es más que conveniente crear comandos para palabras extranjeras o comandos de una terminal (en [DocBook](http://www.docbook.org/) viene de [serie](http://www.docbook.org/tdg/en/html/foreignphrase.html)).

Son sistemas complejos, difíciles de aprender en general pero a la vez bastante potentes. Personalmente he utilizado mucho LaTeX y algo DocBook, aunque ciertamente cada vez menos en favor de los semánticamente más ligeros.

Ambos son excelentes sistemas con sus puntos fuertes y sus puntos débiles. Por ejemplo: LaTeX no tiene rival en la elaboración de documentación científica con fórmulas y exigentes necesidades de maquetación mientras que DocBook es un sistema orientado perfectamente para la publicación de documentación técnica (especialmente en el área de la ingeniería del _software_).

Producir documentación en estos sistemas requiere de bastante tiempo, más que los que a continuación comentaré, pero en determinados ambientes son una apuesta a medio plazo bastante conveniente, existen decenas de proyectos de _software_ que mantienen su documentación en DocBook como por ejemplo [PostgreSQL](http://www.postgresql.org/docs/8.3/static/index.html) o [GeoNetwork OpenSource](http://geonetwork-opensource.org/documentation/manual/geonetwork-manual).

### Sistemas de semántica débil

Estos sistemas tienen poca a ninguna semántica en sus comandos, fuera de las mínimas necesarias para producir tablas, crear enlaces, negritas, cursivas y demás. En este área ahora mismo manejo, y espero no olvidarme de ninguno:

- Plone, [reStructured Text](http://docutils.sourceforge.net/rst.html)
- Wiki de [OSGeo](http://wiki.osgeo.org), [mediawiki](http://www.mediawiki.org/wiki/MediaWiki/es)
- Wiki del trabajo, [Confluence](http://www.atlassian.com/software/confluence/)
- [Trac](http://trac.edgewall.org/)'s varios que de vez en cuando edito, el suyo
- Edición de este blog, [txt2tags](http://txt2tags.sourceforge.net/) (fuera, los contenidos se suben como xhtml) y antes directamente en [textile](http://www.textism.com/tools/textile/)

De estos, el más evolucionado es sin duda [reStructured Text](http://docutils.sourceforge.net/rst.html), con algunas marcas para escribir notas, comandos para que se genere de forma automatizada la tabla de contenidos y cosas así. Por otro lado, hacer tablas en reStructured Text es prácticamente _[ASCII-art](http://docutils.sourceforge.net/docs/ref/rst/restructuredtext.html#tables)_ y un **auténtico fastidio**.

La ventaja de estos sistemas es que en general los ficheros o textos que se escriben son altamente legibles (al contrario de DocBook por ejemplo) y se pueden redactar con cualquier editor de textos, del viejo [Vim](http://www.vim.org/) a cosas más modernas como [JEdit](http://www.jedit.org/).

Por ejemplo, para redactar este artículo estoy utilizando un marcado débil muy sencillito llamado [txt2tags](//txt2tags.sourceforge.net/) que funciona con Python. Si estás en Windows, creo que puedes bajarte una versión que es un ejecutable que lleva ya un pequeño Python dentro, si estás en Linux y MacOS seguro que tienes Python instalado y no tienes más que dejar caer el **script** en algún sitio de tu PATH del sistema.

Por último, para escribir, como editor de texto (independientemente de que escriba en txt2tags o reStructured Text) utilizo [este ordenador](http://oblongomirihi.wordpress.com/2008/08/24/presentando-al-chiquitin/) [PyRoom](http://pyroom.org/), un clon libre del maquero [WhiteRoom](http://hogbaysoftware.com/products/writeroom) (en Windows tienes el bonito y barato [Q10](http://www.baara.com/q10/)). Todos ellos te dejan una buena pantalla donde no hay ni botones ni menús, **¡¡sólo un cursor parpadeante que espera que escribas cosas interesantes!!**
