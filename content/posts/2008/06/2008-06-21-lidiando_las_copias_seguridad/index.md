---
title: "Lidiando con las copias de seguridad"
date: "2008-06-21"
categories: 
  - "other"
---

Llevaba ya tiempo teniendo problemas con el equipo desobremesa de casa. Al principio pensé que era un problema de mi [Debian](http://www.debian.org/index.es.html)(sacrílego de mí) y pensé en renovar el sistema poniendo una reciénsaluda [Ubuntu8.04](http://www.ubuntu.com/products/WhatIsUbuntu/desktopedition). Para poder hacer ese cambio debía primero hacer un _backup_de todos nuestros datos, películas, música, etc etc y de paso dejar ese_backup_ por si acaso. Así que lo primero fuecomprar un disco duro externo de 750GB (109 euros en el [Saturn](http://www.saturn.es) deMassalfassar, Valencia).

El disco, con la idea de separar todo el multimedia de losverdaderos datos personales lo particioné de la siguiente forma: 150GBen ext2 y 600GB en FAT32. De esta forma tenía unapartición que seguro no iba a poder estropear al conectar eldisco en otro sitio que no fuera un linux.

Hecha la copia inicial y el cambio de sistema operativo meplanteé cómo montar un sistema de _backups_ que almenos me asegurara que los datos importantes que tenemos en el equipono se pierden. Así llegué a mirar un montón de artículos sobre [unison](http://www.cis.upenn.edu/%7Ebcpierce/unison/)y especialmente [rsync](http://samba.anu.edu.au/rsync/).Me quedé con éste último porque me pareció el más potente y versátil.

Al final lo que estaba mal no era Debian, sino el equipo yhemos tenido que adquirir uno nuevo pero gracias a los _backups_y demás no he tenido que sufrir mucho para dejarle a [Aida](http://aidaivars.wordpress.com/) laUbuntu tal y como la tenía con el equipo anterior, con todas lasconfiguraciones respetadas.

¿Cómo hacemos las copias de seguridad? Pues he escrito unpequeño guión que básicamente copia al disco duro externo todos loscambios y borra del mismo todo lo que se haya borrado en el equipo. Esdecir, no hay copias incrementales sino un espejo exacto de lo que hayen el ordenador.

Por otro lado hay que asegurarse que el disco siempre se montaen la misma carpeta. El soporte de linux para montar discos ([udev](http://www.kernel.org/pub/linux/utils/kernel/hotplug/udev.html))monta el dispositivo en una carpeta con el nombre del disco en lacarpeta /media.Así, si el disco tiene nombre la carpeta siempre será la misma en micaso /media/backups.

Además, otra de las cosas realmente buenas de rsync es quepermite incluir un listado de exclusiones, por lo que realmente no sehace backup de cosas como los archivos temporales del firefox, laspapeleras de reciclaje, etc etc. Se pueden escribir comodines y así porejemplo evitar que se hagan copias de seguridad de cierto tipo deficheros, carpetas que son comunes, etc.

No hacemos las copias de forma automatizada porque el disco engeneral lo tenemos apagado. Lo que me queda por hacer es que este _backup_se lance de forma automática al enchufarlo, lo cual es sencilloutilizando reglas de udev.

En definitiva, he conseguido un sistema que realiza copias deseguridad de nuestras carpetas home (y de paso hago backup de /etc porsi acaso) de forma sencilla, prácticamente desasistida y guardando logsde lo que hace cada _script_.

Script de backup en /home/sync/backup\_homes.sh

#!/bin/bash

sincro(){    $ECHO "----------SINCRO() START---------------"

    # make sure we're running as root    if (( \`$ID -u\` != 0 ));     then { $ECHO "Sorry, must be root.  Exiting..."; exit; }     fi

    # make sure the mount folder is available    if \[ ! -d $MOUNT\_FOLDER \];     then $ECHO "Sorry, backup disk not mounted. Exiting..."; exit;     fi

    # make sure the origin folder is available    if \[ -z $1  \];     then $ECHO "Sorry, not parameter passed. Exiting..."; exit;     fi

    if \[ ! -d $1 \];     then     $ECHO "Sorry, the parameter is not a valid directory. Exiting..."; exit     else     $ECHO "Starting rsyncing of the $1 into $MOUNT\_FOLDER (excluding $EXCLUDES)..."

    # do the rsync    $RSYNC        -v -a --delete-after --delete-excluded        --exclude-from=$EXCLUDES  --ignore-errors        $1 $MOUNT\_FOLDER;        fi    $ECHO "----------SINCRO() END---------------"}

# Remove any system pathunset PATH

# ------------- system commands used by this script ---ID=/usr/bin/id;ECHO=/bin/echo;MOUNT=/bin/mount;RM=/bin/rm;MV=/bin/mv;CP=/bin/cp;TOUCH=/bin/touch;MKDIR=/bin/mkdirPWD=/bin/pwdDATE=/bin/dateTAR=/bin/tarRSYNC=/usr/bin/rsync;

# ------------- file locations ---------------------MOUNT\_FOLDER=/media/backups/backup\_pc;SYNC\_HOME=/home/syncEXCLUDES=$SYNC\_HOME/backup\_exclude;LOG\_FILE=\`$DATE +%g%m%d-%H%M%S\`"-synchro.log"LOG\_PATH=/tmp/$LOG\_FILE

# ------------- synchro folderssincro /etc > $LOG\_PATH;sincro /home/jorge >> $LOG\_PATH;sincro /home/aida >> $LOG\_PATH;sincro /home/sync >> $LOG\_PATH;

# ------------- compress log file$TAR czf $SYNC\_HOME/$LOG\_FILE.tar.gz $LOG\_PATH

Fichero de exclusiones: /home/sync/backup\_exclude

/\*\*/.local/share/Trash/\*\*/.thumbnails/\*\*/.gvfs/\*\*/.mozilla/firefox/\*.default/Cache/\*\*/.mozilla/firefox/\*.default/OfflineCache/\*\*/media/\*\*/tmp\*.mp4\*·mp3
