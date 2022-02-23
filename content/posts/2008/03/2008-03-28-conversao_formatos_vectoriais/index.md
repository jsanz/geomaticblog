---
title: "Conversão entre formatos vectoriais"
date: "2008-03-28"
categories: 
  - "formats"
  - "foss"
---

O crescimento da importância dos Sistemas de Informação Geográfica(SIG) levou a um incremento de programas capazes de lidar com este tipo dedados e também ao surgimento de uma variedade de formatos que ossuportam. Mais todavia quando a captura de dados por vezes provêem dereceptoresGPS e cada marca de navegador implementa o seu formato dearmazenamento de dados.

Aconversão entre estes vários formatos de ficheiros com informação espacial torna-se fácil com o uso de apenas 3 pequenos programas:  [GPSBabel](http://www.gpsbabel.org/ "GPSBabel"), [gpx2shp](http://gpx2shp.sourceforge.jp/ "gpx2shp") e og2ogr de [FWTools](http://fwtools.maptools.org/ "FWTools"). Uma boa noticia é que estão disponíveis tanto para Window$ como para Linux. No meu caso, uso-os em [Ubuntu](http://www.ubuntu.com/ "Ubuntu") e posso adiantar que a instalação é trivial: Sistema ->Administração -> Gestor de pacotes Synaptic e instalar ospacotes: gpsbabel e gpx2shp. Quanto a FWTools ir à zona de descargas,descarregar "FWTools 2.0.6 (Linux x86 32bit)". Criar uma pasta, descomprimir ai o ficheiro descarregado e executar "install.sh" e já está.

Quanto ao seu uso, cada um tem a sua função. GPSBabel está pensado paraformatos de GPS (gpx, NMEA, [...](http://www.gpsbabel.org/capabilities.html "...")), ogr2ogr para os formatos usados nos SIG ("ESRI Shapefile", "MapInfoFile", "TIGER", "S57", "DGN", "Memory", "CSV", "GML", "KML","Interlis 1", "Interlis 2", "SQLite", "ODBC", "PostgreSQL", "MySQL") egpx2shp apenas para a conversão de gpx a shp, mas extremamente útil porque faz de ponte entre estes dois grupos deformatos. Outra opção seria utilizar um dos formatos suportados porambos como "KML" ou "CSV".

Para levar a cabo o uso de estes programas, abrimos a consola eescrevemos a sentencia como nos exemplos seguintes:

\[sourcecode language='bash'\] # nmea a gpx gpsbabel -i nmea -f "nome\_do\_ficheiro.txt" -o gpx -F "nome\_do\_ficheiro.gpx" # nmea a kml gpsbabel -i nmea -f "nome\_do\_ficheiro.txt" -o kml -F "nome\_do\_ficheiro.kml" # kml a gpx gpsbabel -i kml -f "nome\_do\_ficheiro.kml" -o gpx -F "nome\_do\_ficheiro.gpx" # gpx a shp gpx2shp nome\_do\_ficheiro.gpx # shp a kml ogr2ogr -f 'KML' nome\_do\_ficheiro.kml nome\_do\_ficheiro.shp \[/sourcecode\]
