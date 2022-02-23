---
title: "First release of gvSIG mobile"
date: "2008-03-20"
categories: 
  - "gvsig"
---

[![gvSIG](images/Logo-gvSIG_150_14.gif)](http://www.gvsig.org)I'mreally happyto post about the first release of gvSIG mobile. This is the port (moreor less) of the well known [gvSIG](http://www.gvsig.org)software to mobile devices. My coworkers Miguel (CTO of [Prodevelop](http://www.prodevelop.es)) andJavi (a really smart developer) [presented](http://www.foss4g2007.org/presentations/view.php?abstract_id=124)that project at [FOSS4G2007](http://www.foss4g2007.org)but at this time gvSIG mobile was a inner project of Prodevelop. Nowthis project has won the contest to get the contract with the [Valencian Gobernment](http://www.cit.gva.es),so we are now "official" and public.

The project will be carried out mainly by Prodevelop but alsoby the University of Valencia (the [RoboticsInsitute](http://robotica.uv.es/castellano/home.html)) and [IVER](http://www.iver.es/)always with the coordination and management of the Valencian Gobernmentas usual on the rest of gvSIG projects.

[![Miguel showing gvSIG Mobile](images/1478491410_057f287d3a.jpg)](http://www.flickr.com/photos/xurxosanz/1478491410/ "Miguel showing gvSIG Mobile por XuRxO, en Flickr")

There are [binariesand sources](http://www.gvsig.gva.es/index.php?id=descarga0&L=2%2Findex.php%3Fid%3D1&K=1&L=2) of this first release (we are translating theuser manual in English, sorry). The project has just started, so thisfirstrelease is only a demonstration and a _proof of concept_about developing Java software for that kind of devices and the veryspecific Javaprofile called [ConnectedDevice Configuration](http://java.sun.com/products/cdc/). We are now just refactoring theinternal architecture of the software to provide better modularity andextensibility like gvSIG desktop has. Furthermore, we want to havegvSIG mobile not only in Windows Mobile devices but also in otherdevices like Linux gadgets, [Andoid](http://code.google.com/android/), [Symbian](http://www.symbian.com/),... whoknows.

With that release you can:

- View shapefiles, ecw raster files, georeferenced png andgif files
- Connect to WMS services
- Connect to your GPS and view your position and the typicalGPS quality parameters
- Log waypoints and tracks in GPX format
- Measure distances and areas
- Get info form shapefile attribute tables and WMS services
- Really simplified symbology for vector data

At this time gvSIG mobile works fine with the IBM J9 VirtualMachine (non free software) but we are starting to work with the [phoneME](https://phoneme.dev.java.net/)project, the first Sun's Java Virtual Machine for mobile devices and wehave got really positive experiences.

If you want to see it in action, there are several [Youtube](http://youtube.com/user/gvsig)demos. See the FOSS4G2007 [presentation](http://www.foss4g2007.org/presentations/view.php?abstract_id=124)to get more details of the project, if you are a Windows Mobile userI'm sure you'll enjoy it ;)

If you have any question, bug, feature request or whateverabout the project, please send us a mail through the gvSIG [Internationalmailing list](http://runas.cap.gva.es/mailman/listinfo/gvsig_internacional).
