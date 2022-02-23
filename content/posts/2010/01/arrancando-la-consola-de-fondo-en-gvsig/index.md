---
title: "Arrancando la consola de fondo en gvSIG"
date: "2010-01-20"
categories: 
  - "gis"
  - "gvsig"
---

Un pequeñó truco de gvSIG,  rápido y sucio ;)

Si tenéis la instalación de Windows de gvSIG veréis que haciendo doble click en el .exe este arranca sin la conocida consola de comandos de fondo.

A veces, sin embargo, es preferible tenerla, para poder ver determinados mensajes, por ejemplo si estás conectando a un WMS que falla, poder ver el porque.

Para hacerlo debes arrancar gvSIG a la antigua (como en las primeras versiones) tirándolo contra java... este proceso no es trivial, de hecho yo casi siempre tengo que preguntar como se hace, así que para no seguir molestando a Xurxo, lo voy a apuntar en un sitio (o sea aquí) y de paso lo comparto con la comunidad.

Para "tirar" gvSIG contra java hay que crear un pequeño archivo .bat en la carpeta en la que se instala gvSIG y poner el siguiente contenido. `@echo oN SET BASEDIR="C:\Archivos de programa" SET GVSIG_HOME=%BASEDIR%\gvSIG_1.9 SET PATH=%GVSIG_HOME%;%PATH% SET JAVA_HOME=%BASEDIR%\Java\jre1.5.0_18 SET PROJ_LIB=%BASEDIR%\gvSIG_1.9\bin\gvSIG\extensiones\org.gvsig.crs\data SET GDAL_DATA=%BASEDIR%\gvSIG_1.9\lib\gdal_data`

cd %GVSIG\_HOME%\\bin

%JAVA\_HOME%\\bin\\java.exe -Djava.library.path=%GVSIG\_HOME%\\bin -cp andami.jar;.\\lib\\gvsig-i18n.jar;.\\lib\\beans.jar;.\\lib\\commons-codec-1.3.jar;.\\lib\\commons-collections-3.1.zip;.\\lib\\commons-dbcp-1.0-dev-20020806.zip;.\\lib\\commons-pool-1.2.zip;.\\lib\\log4j-1.2.8.jar;.\\lib\\iver-utiles.jar;.\\lib\\castor-0.9.5.3-xml.jar;.\\lib\\jcommon-1.0.10.jar;.\\lib\\jfreechart-1.0.6.jar;.\\lib\\jh.jar;.\\lib\\JUF-1.0.jar;.\\lib\\crimson.jar;.\\lib\\xerces\_2\_5\_0.jar;.\\lib\\javaws.jar;.\\lib\\xml-apis.jar;.\\lib\\xmlrpc-2.0.1.jar;.\\lib\\looks-2.1.4.jar;.\\lib\\JWizardComponent.jar;.\\lib\\tempFileManager.jar;.\\lib\\kxml2.jar;.\\lib\\org.gvsig.exceptions.jar;.\\lib\\org.gvsig.ui.jar;.\\lib\\xerces\_2\_5\_0.jar;.\\lib\\jcalendar.jar -Xmx500M com.iver.andami.Launcher gvSIG gvSIG\\extensiones gvSIG

pause

Dos caveat emptor:

1. Andaros con ojo al respecto de la configuración de variables, que dependen de las rutas de vuestro path.
2. Esta instalación usa el java del sistema, no el que viene con la instalación de gvSIG con su propio java, ténganlo en cuenta.

Un saludo
