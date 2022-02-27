---
title: "Instalando ka-Map en un XAMPP... pegándose con php_mapscript en un Apache que ya está en marcha."
date: "2008-04-28"
categories: 
  - "development"
  - "foss"
  - "gis"
authors: [ "pedro-juan-ferrer" ]
---

Como [ya os conté en su momento](http://geomaticblog.net/gb2/es/2008-01-24-tilecache_windows), tengo instalado un mapserver y un tilecache en el ordenador, lo que no os conté es que está encima de un [XAMPP](http://www.apachefriends.org/en/xampp.html). Recientemente se me ha ocurrido probar [ka-Map](http://www.google.com/search?source=ig&hl=es&rlz=&q=php_mapscript+xampp&btnG=Buscar+con+Google&meta=&aq=f), a raíz del [taller de las IIas Jornadas de SIG Libre](http://www.sigte.udg.es/jornadassiglibre/index.php?page=talleres),y cuando lo instalé, siguiendo rigurosamente las instrucciones, me encontré con  un problemilla que me ha llevado un poco resolver, sobretodo porque pese a que hay bastante gente que le ha pasado, no he sido capaz de encontrar la solución [en San Google](http://www.google.com/search?source=ig&hl=es&rlz=&q=php_mapscript+xampp&btnG=Buscar+con+Google&meta=&aq=f). Se trata de un problema para instalar MapScript sobre un Apache ya existente (o sobre un XAMPP).

Todo empieza con este error cuando pedimos la siguiente dirección: http://localhost/ka-map/init.php

 **Warning**: dl() \[function.dl\]: Not supported in multithreaded Web servers - use extension=php\_mapscript.dll in your php.ini in **C:xampphtdocska-mapinit.php**  on line **118**

 **Fatal error**: Call to undefined function ms\_newmapobj()   in **C:xampphtdocska-mapinit.php** on line **124**

Para empezar la distribución estándar de xampp no tiene la dll php\_mapscript así que vamos a [la página de PHP MapScript](http://www.maptools.org/php_mapscript/) decididísimo a descargarme lo que haga falta... te llegas a la página de PHP MapScript y descubres que, para MSW$, ahora solo se distribuye [junto con ms4w](http://www.maptools.org/ms4w/),pero claro, yo ya tenía un Apache y un MapServer funcionando así que no quería instalar otro servicio Apache... cojo, [la versión en zip de ms4w](http://www.maptools.org/ms4w/index.phtml?page=downloads.html) y rapiño la dll...

 Ojo, acordaros de modificar convenientemente el config.php de ka-Map 

 $szPHPMapScriptModule = 'php\_mapscript.'.PHP\_SHLIB\_SUFFIX; $szPHPGDModule = 'php\_gd2.'.PHP\_SHLIB\_SUFFIX;

... retoco el php.ini del xampp (usad el phpinfo() para saber dónde está) para que cargue php\_mapscript como extensión,  reinicio Apache y ...

 **Warning**: dl() \[function.dl\]: Not supported in multithreaded Web servers   - use extension=php\_mapscript.dll in your php.ini in **C:xampphtdocska-mapinit.php** on line **118**

 **Fatal error**: Call to undefined function ms\_newmapobj()   in **C:xampphtdocska-mapinit.php** on line **124**

vale, al parecer estoy haciendo algo mal... tras unas búsquedas en Google veo que es un mal bastante común y que no parece que nadie sepa cómo solucionarlo, así que me pongo a investigar y descubro que ms4w tiene montado PHP como FastCGI en vez de como módulo (opción por defecto en XAMPP), bueno, pues probemos si es eso...

Vamos a C:/xampp/apache/conf/extra/httpd-xampp.conf y modificamos las primeras líneas del archivo para que recen así:

 ScriptAlias /php/ "C:/xampp/php/" Action application/x-httpd-php "/php/php-cgi.exe" #LoadModule php5\_module "C:/xampp/apache/bin/php5apache2.dll" AddType application/x-httpd-php-source .phps AddType application/x-httpd-php .php .php5 .php4 .php3 .phtml .phpt

si lo reiniciáis y ejecutáis "as is" lo más fácil es que os de un error de Acceso prohibido cuando intentéis ejecutar cualquier archivo php ya que por defecto C:/xampp/php/ NO tiene permisos asignados en Apache (ojo con esto que puede producir fallos GORDOS de seguridad)

 <Directory "C:/xampp/php/"> AllowOverride AuthConfig Order allow,deny Allow from all </Directory> 

reiniciamos Apache y "sorpresa"

 **Warning**: dl() \[[function.dl](http://localhost/ka-map/function.dl)\]: Unable to load dynamic library  'C:xamppphpextphp\_mapscript.dll' - No se puede encontrar el módulo especificado.in **C:xampphtdocska-mapinit.php** on line **118**

 **Fatal error**: Call to undefined function ms\_newmapobj() in  **C:xampphtdocska-mapinit.php** on line **124**

El hecho de que hayamos cambiado de error indica que estamos progresando :-D aunque el error que indica es que no puede cargar php\_mapscript.dll la realidad es un poco más compicada, lo que no puede cargar es alguna de sus dependencias pero ¿cuales? pues para contestar eso tendremos que ejecutar un programita que "escarba" las dependencias de las dll el [Dependency Walker](http://www.dependencywalker.com/)(esto lo averigüe leyendo la ayuda de Apache).

Cuando falta una dll, la podemos obtener de [la versión en zip de ms4w](http://www.maptools.org/ms4w/index.phtml?page=downloads.html), descomprimiendo lo que necesitemos, las dlls están todas en ms4w\_2.2.7.zipms4wApachecgi-bin. Conforme vamos añadiendo dlls a C:/xampp/php/nos irá señalando nuevas faltas (refrescando Dependency walker con F5). Una forma de "acelerar" el proceso es descomprimir directamente todas las dlls que hay en el .zip en la carpeta (básicamente GDALes y PROJes)... Hay una dependencia que no es necesario resolver para que php\_mapscript funcione "MSJAVA.dll".

Fijaros que cómo PHP está montado como FastCGI no hace falta reiniciar Apache para que surtan efecto los cambios. Una vez hecho esto el mensaje de error de ka-Map es distinto, haciendo referencia a que no encuentra el mapa de ejemplo... señal de que funciona.

 **Warning**: \[MapServer Error\]: msLoadMap():  (C:xampphtdocska-map/../../gmap/htdocs/gmap75.map) in  **C:xampphtdocska-mapinit.php** on line **124**

 **Warning**: Failed to open map file ../../gmap/htdocs/gmap75.map in  **C:xampphtdocska-mapinit.php** on line **124**

 **Fatal error**: Call to a member function getMetaData() on a non-object in  **C:xampphtdocska-mapinit.php** on line **130**

Bueno, pues con esto en un periquete tenéis ka-Map montado sobre un XAMPP ya existente (ojito,  again, con las configuraciones de seguridad, este es un ejemplo para un puesto de desarrollo, no de producción). Espero que os sirva la recetilla y si tenéis dudas... ya sabéis.

¡Un saludo!

PD. A mis fans, os juro que ahora mismo me pongo con la 2a parte de lo de Girona, pero como esto lo tenía a mano...
