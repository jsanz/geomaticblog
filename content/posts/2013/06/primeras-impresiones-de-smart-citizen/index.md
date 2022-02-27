---
title: "Primeras impresiones de Smart Citizen"
date: "2013-06-16"
categories: 
  - "geodata"
  - "hardware"
  - "osm"
authors: ["jorge-sanz"]
---

[Smart Citizen](http://smartcitizen.me) es un proyecto que apareció en [goteo](http://goteo.org/project/smart-citizen-sensores-ciudadanos) hace un año. Goteo es una web de _crowdfunding_ al estilo de Kickstarter. Apareció poco después del [Air Quality Egg](http://airqualityegg.com/), al cual llegué tarde para participar. Así que nada, cuando vi una idea similar me apunté sin pensármelo mucho. ¿Cuál es la idea? Bien es sencillo, imaginad una red **completamente voluntaria** para la medición de variables medioambientales, sobre todo aquellas relacionadas con la contaminación tanto acústica como de cualquier otro tipo. Esa red no estaría controlada por ningún organismo, realmente es una red porque existe una forma común de acceder a todos los datos, pero los miembros ni se conocen, ni tienen por qué tener los mismos objetivos, ni las mismas motivaciones. ¿A que recuerda a otras actividades similares? Efectivamente, se trata al igual que en [OSM](http://osm.org) por ejemplo, de _«mapear»_ el territorio solo que de una forma diferente, un paso más allá de la representación estática de la realidad, hacia un conocimiento **más profundo** de nuestro entorno al entrar en el mundo de los sensores en tiempo real, de la famosa Internet de las Cosas.

Tanto [Smart Citizen](http://smartcitizen.me) como [AQE](http://airqualityegg.com) solo son los comienzos de una nueva generación de _hardware_, _software_ y servicios que nos harán ser más conscientes (y espero concienciados) de nuestro entorno, de la calidad del mismo y de cómo evoluciona tanto en el corto plazo como con miras un poco más alejadas. Con todo este conjunto de datos en bruto publicados en tiempo real, ¿quién sabe qué innovaciones veremos en los próximos años y cómo éstas afectarán nuestras vidas?.

En fin, volviendo al caso concreto de Smart Citizen, hace poco me llegó la placa. Básicamente es una placa [Arduino](http://arduino.cc/) con una placa superior de sensores (_shield_ en el argot Arduino) y una batería. La placa ya viene con el software precargado aunque ciertamente van a ir saliendo actualizaciones del mismo que hay que cargar con el entorno de desarrollo estándar de Arduino, sin mucho más misterio.

[![SCK](images/9060074052_8fc963cb78.jpg)](http://www.flickr.com/photos/xurxosanz/9060074052/ "SCK by XuRxO, on Flickr")

Para poner en marcha la placa solo hay que configurar la red wifi a la que se conectará. Esto se hace a través de la web mediante un _applet_ java que realiza todo el proceso. El único problema que tuve es que en mi caso, cuando se genera el puerto para el dispositivo no tengo permisos para usarlo por lo que tuve que ejecutar un sencillo sudo chmod 777 /dev/ttyACM0 para que fuera capaz de cargar la configuración. Una vez cargada, se reinicia la placa y empieza a enviar datos sin mayor inconveniente, quedando publicados de forma automática en [la web](http://test.smartcitizen.me/devices/view/139).

La web todavía está en desarrollo, bueno **TODO** está aún en desarrollo, incluyendo el _firmware_ que ciertamente aún no presenta los datos de los sensores todo lo bien que debiera. Por ejemplo los valores de calidad de aire vienen en Ohmios, en lugar de las más típicas «partes por millón». Todo esto estoy seguro que se irá puliendo y aún cuando algún sensor no vaya del todo fino (me temo que el de sonido, por ejemplo), solo como primera aproximación a lo que puede ser el disponer de una red de sensores publicando en tiempo real toda esta información es más que interesante.

La red [Smart Citizen](http://smartcitizen.me) está sobre todo (de momento) enfocada en Barcelona, de hecho la mía es la única placa hasta la fecha activada en la provincia de Valencia.

[![Smart CItizen en Barcelona](http://geomaticblog.files.wordpress.com/2013/06/2013-06-16-192707-seleccic3b3n.png?w=300)](http://test.smartcitizen.me/)

Por otro lado todavía hay mucho que hacer en cuanto a la presentación de los datos que se van subiendo a su plataforma. Además de [la web del sensor](http://test.smartcitizen.me/devices/view/139) hay algún [punto de acceso](http://data.smartcitizen.me/testjson?device=139) para descargar en formato JSON todos los registros y también [una web](http://data.smartcitizen.me/test?device=139) para ver datos más antiguos que los escasos 20 minutos que se pueden ver desde la web oficial.

En fin de momento eso es todo, la placa ahora mismo la tengo «indoor» porque aún tengo que ver cómo le conecto un panel solar o bien saco un cable USB para poder tenerla en el balcón de casa, y que haga medidas lo más estables posible. Ya iré contando.
