<!-- source: https://wiki.gentoo.org/wiki/OctoPrint | group: Gentoo Wiki (Main) | wiki-title: OctoPrint -->
---
title: OctoPrint
url: https://wiki.gentoo.org/wiki/OctoPrint
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2023-08-27"
fingerprint: b6aa8241fdc727e4
license: CC BY-SA 4.0
---

# OctoPrint

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

OctoPrint is an open source 3D printer web interface written in Python. It provides real-time video streaming during prints, remote printer control, an integrated GCODE visualizer, temperature monitoring, and many other features. A large community-maintained plugin ecosystem is available as well to extend its functionality.[\[1\]](https://wiki.gentoo.org#cite_note-1)

### Printer support

OctoPrint supports a large number of printers out of the box. A listing of printers known to work/not work is available at this post.[\[2\]](https://wiki.gentoo.org#cite_note-2)

## Installation

Previous OctoPrint ebuild work can be seen at these links.[\[3\]](https://wiki.gentoo.org#cite_note-3)[\[4\]](https://wiki.gentoo.org#cite_note-4)[\[5\]](https://wiki.gentoo.org#cite_note-5)[\[6\]](https://wiki.gentoo.org#cite_note-6)[\[7\]](https://wiki.gentoo.org#cite_note-7)

The installation places files in the following locations:

| Path | Description | 
|---|---|
| /var/lib/octoprint | The home folder for the OctoPrint User, containing configuration, log and stored files | 
| /etc/init.d/octoprint | RC-Service Script for starting and stopping OctoPrint | 
| /etc/conf.d/octoprint | This is where the OctoPrint configuration file should go but currently does not (See the home folder) | 
| /usr/bin/octoprint | This is the main Python script that is called by the service. If one calls it, a .OctoPrint folder will be created in the home directory. | 
| /usr/libs/python3\_11/site-packages | The main installation of OctoPrint Code - one Python version | 

### Users/groups

The OctoPrint manual recommends creating an OctoPrint user and allowing access to the Serial and Terminal groups. Gentoo provides the tty group for terminal access but this is not really required by OctoPrint. As there is no Serial group, the uucp group is specified in the ebuild.

### Serial

Note that one must enable USB/Serial communication in the Kernel for these printers to work. 3D Printers communicate with the computer via a serial interface. The [Arduino](https://wiki.gentoo.org/wiki/Arduino#Prepare_the_toolchain) section describes how to set this up. There is a nice entry in the [Gentoo Forums](https://forums.gentoo.org/viewtopic-t-907860-start-0.html) that explained this setup as well. The Arduino website further discusses serial connection [issues](https://playground.arduino.cc/Linux/All).

### Cura

Cura is a 3D slicing program which seems to have gone through three major architecture changes roughly coinciding with versions 0.15.x and 2.1.x. OctoPrint 12.1 does not support the 2.1.x implementation and the ebuild locks in the 0.15.6 version.

When OctoPrint calls Cura it might fail with one of the following error codes. This occurred with the 2.1.x version and OctoPrint 12.5.

| Error Code | Error Description | 
|---|---|
| 1 | Cura executed but did not produce a G-Code file. The logs indicate a call arguments issue. | 
| 6 | This call to Cura appears to be mostly meta information about the STL file. | 

#### Plugin

The Cura Plugin is enabled by default but one must specify the location of the Cura Binary. There are two created by [media-gfx/cura](https://packages.gentoo.org/packages/media-gfx/cura) - one in /user/bin/cura (user interface), and one in /user/bin/CuraEngine (the slicing engine). The latter path must be provided in the OctoPrint Cura Plugin settings.

The CuraEngine requires a Printer Profile generated from Cura itself. This is done by navigating to Cura → Preferences → Profiles → Profile → Export and saving the resulting file which is then loaded directly into the OctoPrint interface under Settings → CuraEngine → Import → Profile.

## Service

Once installed, one may enable an OctoPrint service via OpenRC commands.

## Login

When one initially opens the first page in OctoPrint they are asked to create an admin account. Choose a secure password if it is desired to be able to remotely access the printer.

## See also

- [3D Printing](https://wiki.gentoo.org/wiki/3D_Printing) — an overview of 3D printing with Gentoo
- [Project:3dprint](https://wiki.gentoo.org/wiki/Project:3dprint)

## External resources

- [Gentoo 3D Printer Project](https://packages.gentoo.org/maintainer/3dprint@gentoo.org) - Currently available packages
- [https://community.octoprint.org/](https://community.octoprint.org/) - OctoPrint forum
