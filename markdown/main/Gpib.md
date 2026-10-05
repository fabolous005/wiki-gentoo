<!-- source: https://wiki.gentoo.org/wiki/Gpib | group: Gentoo Wiki (Main) | wiki-title: Gpib -->
---
title: Gpib
url: https://wiki.gentoo.org/wiki/Gpib
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2022-07-04"
fingerprint: f2221c708f9d99f9
license: CC BY-SA 4.0
---

# Gpib

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

###### Linux Gpib Howto in Gentoo

linux-gpib with python flags is using python 2.7

##### Installing linux-gpib

First set use flags in /etc/portage/package.use:

`user $``sudo vim /etc/portage/package.use`
Add line for python bindings and Documentation:

**`/etc/portage/package.use`**

Other possible Flags *pcmcia static debug guile perl php tcl firmware*.
If you want to use *pcmia* you need to have a 2.7 kernel. But *pcmia*  is normally just used in old computers so it doesn't matter.

Install it with:

`root #``emerge --ask sci-libs/linux-gpib`
Add your user to gpib group

`root #``gpasswd -a user gpib`
##### Configuring linux-gpib

To configure Linux GPIB you need first to create a config file at /etc/gpib.conf:

`root #``vim /etc/gpib.conf`
Just an example which is for NI USB- HS Adapter is:



**`/etc/gpib.conf`**


You can lookup the board\_type at [Linux-gpib-Website](http://linux-gpib.sourceforge.net/doc_html/x259.html).


There is an example config file with more parameters on [GPIB-Linux Website](http://linux-gpib.sourceforge.net/doc_html/r23.html).

After you wrote your own config file, you have to plug in your GPIB Device and either first chgrp the gpib Devices in /dev/ and make them write and readable:

`root #````
chgrp gpib /dev/gpib*
```
`root #````
chmod g+wr /dev/gpib*
```
or do all things as root.

After doing that you should be able to call with root

`root #``/usr/sbin/gpib_config -m 0`
0 is the device you plugged in.

##### Example Python

Be carefull linux-gpib cames with python2.7 bindings so first start python2.7

`user $``python2.7`
This example is tested with a HP8720D

To import gpib:

`>>>``import gpib`
To get a device of a board named in /etc/gpib.conf joe:

`>>>``con=gpib.dev(0,16)`
0 is the GPIB BOARD and 16 the Adress of the Listener

To write a command:

`>>>``gpib.write(con,'STAR2400MHZ;')`
or more universal

`>>>``gpib.write(con,'*IDN?')`
To read the result:

`>>>``gpib.read(con,1000)`
1000 is the Number of byte that are read
