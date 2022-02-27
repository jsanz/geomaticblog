---
title: "Comparing Oracle GeoRaster with PostGIS WKT Raster"
date: "2010-09-03"
categories: 
  - "foss4g"
  - "postgis"
authors: ["jorge-sanz"]
---

If you are interested in [WKT Raster](http://trac.osgeo.org/postgis/wiki/WKTRaster) or «raster-in-databases» in general, you should check the recent posts by [Jorge Arévalo](http://twitter.com/jorgeas80) in his [GIS4Free](http://gis4free.wordpress.com) blog site. I specially love this quote (after a long write about how to add your raster into Oracle):

> All things we’ve done to load our raster data in Oracle GeoRaster and create an index, can be done in PostGIS WKT Raster by executing these two lines
> 
> \> gdal2wktraster.py" -r C:\\orcl\_tut\\\*.tif -t spain\_images -s 4326 -k 50x50 -I 
> -o C:\\orcl\_tut\\srtm.sql
> > psql -d tutorial01 -f C:\\orcl\_tut\\srtm.sql

More awesomeness at  [Comparing Oracle GeoRaster with PostGIS WKT Raster (II)](http://gis4free.wordpress.com/?p=343 "GIS4Free Blog") and also at [his presentation](http://2010.foss4g.org/presentations_show.php?id=3814) next week at [FOSS4G](http://2010.foss4g.org).

By the way I don't know why on Earth [that blog](http://gis4free.wordpress.com/) is not at [http://planet.osgeo.org](http://planet.osgeo.org/), I think I have to arrange a quick meeting between [Mateusz](http://mateusz.loskot.net/) and [Jorge](http://twitter.com/jorgeas80) next week ;-)
