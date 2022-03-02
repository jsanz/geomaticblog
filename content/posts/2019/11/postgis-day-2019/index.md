---
title: "PostGIS Day 2019"
date: "2019-11-20"
categories: 
  - "events"
  - "postgis"
coverImage: "ejwzpeux0ambkg9.jpeg"
authors: ["jorge-sanz"]
---

This year I was happy to attend the [PostGIS Unconference](https://postgisdag.nl/) organized by the good folks from [Geodan](https://www.geodan.nl/) and [Webmapper](https://webmapper.nl/). The idea for the event was to cover the middle ground between typical introductory activities and developer-oriented ones, targeting then mainly PostGIS power users.

The event was held at [Science Park Amsterdam](https://www.openstreetmap.org/way/26778806#map=15/52.3544/4.9555), hosted around 30 people and was organized in a very open way, meaning there's an [open Google Document](https://docs.google.com/document/d/1mby7bcDVCGVEkKky6zngtOE6mZm0hhu0mQqAmdhUbMQ/edit#heading=h.ef7laxwhu4pr) for anyone to jump in, propose talks, express interests, etc. Super **simple**. The rest of the logistics of the event involved having coffee and lunch at the venue provided by [Geodan](https://www.geodan.nl/), and some beers afterward sponsored by [DAT.mobility](https://www.dat.nl/).

The list of ideas exposed at the event was large, but at the end, only three "classic" talks were delivered and after lunch, the group split into different discussions/Birds of a feather to cover the following themes:

- Serving vector tiles with PostGIS
- Postal codes (focused in Netherlands case)
- Cloud discussions
- Large data storage and partitioning

{{< twitter user="stvno" id="1194906030444044289" >}}

Then we grouped back to provide some outcomes/conclusions and we head towards the pub (and myself to the airport back to Valencia).

The talks were related to the BoF sessions and covered the following topics:

- **Tom van Tilburg** from Geodan introduced us to [Clickhouse database](https://clickhouse.yandex/), and how it can complement Postgres using its [Foreign Data Wrapper](https://github.com/adjust/clickhouse_fdw) to rely on this highly performant columnar storage for analyzing big volumes of data, basically to perform filtering and aggregation to send back to Postgres the results to be joined with master geometries and other indicators. The approach needs to craft carefully your queries to ensure you do all the processing in Clickhouse and return only the results. Anyway, I see more and more this database being mentioned as a good technological option to pay attention to.
- **Marco Slot** from Microsoft exposed how [Citus](https://github.com/citusdata/citus), an extension for enabling sharding for Postgres, can help scale the database to manage large datasets. It's not focused on anything geospatial in particular, but he was looking for use cases where Citus can help to achieve good results. In fact at this moment apparently Citus does not support PostGIS aggregates but you can track [this issue](https://github.com/citusdata/citus/issues/1016) for future developments.
- I presented the work that my colleague **Yuri** and I are doing to evolve the standard [OpenMapTiles](https://openmaptiles.org/) workflow from generating a static MBTiles asset to serve vector tiles into serving them directly from the database, leveraging the rather recent support for the MVT format just straight from PostGIS. You can find more details about the procedure on [this document](https://docs.google.com/document/d/1Q9qZjSqRN_3HNVgqL3OQKmEG1mh3quigc_wKo2EFwik/edit#) I used as a script for my talk and demos.

{{< twitter user="stvno" id="1194926960692080645">}}

As a conclusion, it was nice to attend this unconference, to catch up with some friends from the area, and learn how relevant it is becoming for the Postgres community to respond to big data use cases.
