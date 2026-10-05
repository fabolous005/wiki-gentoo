<!-- source: https://wiki.gentoo.org/wiki/Pololu_Maestro_Servo_Controller | group: Gentoo Wiki (Main) | wiki-title: Pololu Maestro Servo Controller -->
---
title: Pololu Maestro Servo Controller
url: https://wiki.gentoo.org/wiki/Pololu_Maestro_Servo_Controller
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2016-11-28"
fingerprint: "4e3f9a88609e33d8"
license: CC BY-SA 4.0
---

# Pololu Maestro Servo Controller

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

This document describes how to install Pololu Maestro Servo Controller board software on Gentoo.

## Install needed packages

Install [dev-lang/mono](https://packages.gentoo.org/packages/dev-lang/mono) and [dev-libs/libusb](https://packages.gentoo.org/packages/dev-libs/libusb):

`root #``emerge dev-lang/mono dev-libs/libusb`
## Download software

Download Maestro Control Center [\[1\]](http://www.pololu.com/file/download/maestro-linux-100507.tar.gz?file_id=0J315) and 
USB Software Development Kit  [\[2\]](https://www.pololu.com/file/download/pololu-usb-sdk-140604.zip?file_id=0J765).

It is necessary to run MaestroControlCenter and UscCmd programs with "mono"

`user $``mono MaestroControlCenter``user $``mono UscCmd`
Read README.txt files there for more information how to use and compile software.

## Troubleshooting

Create link to libusb-1.0.so.0.1.0 if Maestro Control Center or other program got error "Library not found: libusb-1.0"

`root #``cd /lib``root #``ln -s libusb-1.0.so.0.1.0 libusb-1.0.so`
Edit Makefile if USB Software Development Kit couldn't build with error "make: gmcs: Command not found".

**`Makefile`**

**USB Software Development Kit**

```
 line: 
CSC:=gmcs 
to 
CSC:=mcs
```
