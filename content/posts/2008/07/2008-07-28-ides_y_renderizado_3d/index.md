---
title: "IDEs y renderizado 3D"
date: "2008-07-28"
categories: 
  - "gvsig"
  - "ide"
---

A veces lo que a uno le parece evidente a otros lespuedesuponer un cambio de planteamiento brutal en ciertos tipos detrabajos.Es el típico «hostia, pues si llego a saber que esto era taninteresante para ti ¡¡**te lo cuento antes**!!».El caso es que uno de mismejores amigos es arqueólogo en proceso de reconversión a opositor yespecialista en modelado yanimación 3D, que diréis «qué leches tiene que ver la arqueología conel 3D» pues algo pero no viene a cuento :-)

El objetivo final, por hacer la introducción corta y así nomolestaros demasiado, es generar un **renderizado 3D**de una zonacualquiera del territorio español utilizando en primer lugar losrecursos puestos a nuestra disposición por la [IDEE](http://www.idee.es) (utilizando [gvSIG](http://www.gvsig.gva.es)como perfecto cliente) y a continuación un software de modelado yanimación, en este caso [Ligthwave3D](http://www.newtek.com/lightwave/), pero podría seguro realizarse concualquier otro. El resultado final, tras no más de 25 minutos detrabajo es la figura siguiente.

[![Lightwave render](images/2695680021_9ec243e4f8.jpg)](http://www.flickr.com/photos/xurxosanz/2695680021/ "Lightwave render por XuRxO, en Flickr")

El caso es que llevo vendiéndole la burra de [gvSIG](http://www.gvsig.gva.es) desde haceyatiempo.Mucho antes, hace ya unos cuantos años le vendí la de AutoCAD cuando loveía trabajar con acetato y vaya si le ha sacado rendimiento, así quealgo de razón voy a tener... Así que en estas que le estoy contandoacerca de gvSIG, los servidores WMS saliendo además a colación el temade los[ModelosDigitales del Terreno](http://es.wikipedia.org/wiki/Modelo_digital_del_terreno) y me acuerdo del más que interesante[servidorWCS](http://www.idee.es/show.do?to=pideep_desarrollador_wcs.ES) que la IDEE proporciona con el MDT de 25 metros de España.

Y me pregunta «¿podríamosconseguir una ortofoto y un MDT de un área más o menos extensa?» a loque yo contesto con una sonrisa y levantándolo del asiento delescritorio.

En un momento abrimos una sesión de gvSIG y añadimosel servidor WMS con la ortofoto del [PNOA](http://www.idee.es/show.do?to=pideep_desarrollador_wms.ES#PNOA)y el servidor WCS con el [MDT](http://www.idee.es/show.do?to=pideep_desarrollador_wcs.ES)de 25 metros. Y aquí viene un truquito. Él necesitaba tanto una buenaimagen como textura como el MDT en valores de gris (el valor absolutode los niveles de gris de cada celda no es importante). Así que siabrimos una buena ventana con la vista pongamos de unos 1000x1300píxeles con las dos capas.

En cierta carpeta de nuestro disco durogvSIG está alojando las dos imágenes que al añadir capas hemosconfigurado como JPEG y geoTIFF respectivamente. La carpeta estaba enalgo como (lo pongo de memoria) C:Document andSettingsUSUARIOConfiguración Localtemptmp-andami.Ahí gvSIG va descargando todos sus ficherostemporales, si losordenamos por fecha y si no hacemos nada después de colocar nuestravista con el tamaño y la extensión deseada, los últimos ficheroscorresponderán con las imágenes, que copiaremos a cualquier otro sitiodel disco añadiendo las extensiones pertinentes.

En definitiva hemos conseguido una ortofoto y un MDT de**exactamente la misma extensión de terreno**,con una calidad suficientepara una visualización 3D que no requiera demasiada precisión.

[![gvSIG WMS and WCS](images/2695672431_45842a1b9f.jpg)](http://www.flickr.com/photos/xurxosanz/2695672431/ "gvSIG WMS and WCS por XuRxO, en Flickr")

Y hasta aquí entra mi intervención, **¡poco másde 5 minutos entotal en la práctica!**

La segunda fase le he pedido que me la explique brevementeporque yo lo que vi fue una sucesión de rápidos clicks de ratón en unsoftware con una interfaz de usuario infernal, llena de menús y botonesque riete tú de cualquier CAD. Así que trancribo lo que me ha comentadoque hizo:

Vino a crear un cuadrado plano, que partió para convertirlo enuna malla densa, como una cuadrícula. A continuación colocó la imagendel MDT envima de dicho cuadrado, y elevó los vértices de la malla deforma relativa al color de la imagen. Es lo que en la jerga de modeladose _llama mapa de desplazamiento_. Luego hubo queajustar la exageraciónvertical para mostrar algo vistoso, como es habitual en cualquierrenderizado del terreno, aunque este sea escarpado.

Una vez dispuso de la representación 3D pasó a añadir latextura de la ortofoto. Hasta ahí nada sorprendente o diferente de loque ya podemos conseguir con diversos programas GIS tanto libres comoprivativos. Pero ahora viene el toque de distinción.

[![Lightwave MDT 3D](images/2696488744_0ec60e9143.jpg)](http://www.flickr.com/photos/xurxosanz/2696488744/ "Lightwave MDT 3D por XuRxO, en Flickr")

Utilizando de forma adecuada la posición de las luces (no nosvamos a poner tiquismiquis con calcular la posición del sol!),añadiendo una textura de nubes que en realidad también ilumina laimagen, y ajustando finalmente algunos parámetros de iluminación comola [radiosidad](http://es.wikipedia.org/wiki/Radiosidad),se consigue un efecto **bastante bueno**, enapenas unos 10 o 15minutos.

¿Qué os parece? No hay nada como los equipos**multidisciplinares** para conseguirresultados de, a veces, **insospechadacalidad**.
