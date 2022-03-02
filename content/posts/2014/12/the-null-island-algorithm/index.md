---
title: "The Null Island Algorithm"
date: "2014-12-12"
categories: 
  - "gis"
authors: ["ivan-sanchez"]
---

We geomaticians like to gather around a mythical place called [Null Island](http://www.nullisland.com/). This island has everything: airports, train stations, hotels, postcodes, all kinds of shops, a huge lot of geocoded addresses, and whatever geographical feature ends up with null coordinates due to whatever buggy geoprocessing pipeline and ends up in the (0,0) coordinates.

But earlier this year, some geonerds such as [@mizmay](http://twitter.com/mizmay) and [@schuyler](http://twitter.com/schuyler) realized that there is no one Null Island, but one Null Island per datum / coordinate system (depending on who you ask). And [@smathermather](http://twitter.com/smathermather) had the spare time to find out [how the "Null Archipielago" looks like](http://smathermather.wordpress.com/2014/09/10/null-archipelago-null-islands-for-all-coordinate-reference-systems/):


{{< figure
 src="https://smathermather.files.wordpress.com/2014/09/null_archipelago.png"
 caption="Null archipielago image by [@smathermather](http://twitter.com/smathermather), containing Map tiles by [Stamen Design](http://stamen.com), under [CC BY 3.0](http://creativecommons.org/licenses/by/3.0). Data by [OpenStreetMap](http://openstreetmap.org), under [CC BY SA](http://creativecommons.org/licenses/by-sa/3.0)"
>}}


Fast forward a few months. I received an e-mail from one of my university peers, asking for help with a puzzle:

> A friend of mine received a puzzle with some coordinates. He has to find a place on earth represented by 861126.41, 941711.64.
> 
> It's supposed to be a populated place. Any ideas?

Well, off the top of my head, those looked slightly like UTM coordinates - two digits after the decimal point, suggesting centimeter precision... but the easting is way off the valid range for UTM coordinates.

And I realized this is the Null Archipielago problem, all over again; but instead of plotting (0,0) on a map, let's plot all points having (861126.41, 941711.64) coordinates in any reference system.

Cue PostGIS. We can create a point in every CRS like so:

{{< highlight sql>}}
 select srid, 
        ST_GeomFromText(
          'POINT(861126.41 941711.64)',
          srid
        ) as geom 
   from spatial_ref_sys; 
 {{< /highlight >}}

Note the complete absence of PL/SQL in there.

But it will be much easier to work with the data if all the points are in our beloved EPSG:4326 latitude-longitude coordinate system. And while we're at it, let's materialize that data into a table:

{{< highlight sql>}}
 select srid, 
        ST_Transform(
          ST_GeomFromText(
            'POINT(861126.41 941711.64)',
            srid
          ),
          4326
        ) as geom
   from spatial_ref_sys; 
{{< /highlight >}}

But there is a problem with this - the PostGIS query will crash due to some CRSs having an empty Proj4 string. This took me a while to trace and fix:

{{< highlight sql>}}
 select srid, 
        ST_Transform(
          ST_GeomFromText(
            'POINT(861126.41 941711.64)',
            srid
          ),
          4326
        ) as geom 
   from spatial_ref_sys 
  where proj4text!=''; 
{{< /highlight >}}

And now we can take this data out into a file... but once again, there's a catch: some of the coordinates are out of bounds and represented by (∞, ∞) coordinate pair. Even though file formats can handle ∞/-∞ values (good thing we know how IEEE floating point format works, right folks?), some mapping software can not accommodate for these values. And I'm looking at you, CartoDB upload page.

In this particular case, there are only points for (∞, ∞) so the data can be cleaned up in just one pass:

{{< highlight sql>}}
delete from archipielago where ST_X(geom)>180;
{{< /highlight >}}

Then just add a tiny bit of CartoDB magic, and publish a map:


{{< figure
src="/imgs/2014/12/nullarchipielago.png?w=285)](/imgs/2014/12/nullarchipielago.png"
link="https://ivansanchez.cartodb.com/viz/1ac4a786-805a-11e4-bc48-0e853d047bba/public_map"
caption="CartoDB archipielago map"
>}}

<https://ivansanchez.cartodb.com/viz/1ac4a786-805a-11e4-bc48-0e853d047bba/public_map>

I still don't know if the original puzzle has anything to do with any obscure used-in-the-real-world CRS, but at least it's worth a try.
