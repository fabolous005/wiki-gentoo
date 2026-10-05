<!-- source: https://wiki.gentoo.org/wiki/Ventoy | group: Gentoo Wiki (Main) | wiki-title: Ventoy -->
---
title: Ventoy
url: https://wiki.gentoo.org/wiki/Ventoy
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2025-04-20"
fingerprint: "70ade1db816cbae5"
license: CC BY-SA 4.0
---

# Ventoy

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**Ventoy** is is an open source tool to create bootable USB drives for ISO/WIM/IMG/VHD(x)/EFI files. It allows the user to store multiple images, amongst other files, inside a single bootable medium.

Ventoy will create two partitions (EFI and exFAT) on the selected device. The exFAT one will work as regular storage, and will be scanned when booting Ventoy in search of bootable images.

## Prerequisites

### Overlay

Ventoy is not on Gentoo's main repository, but a binary package is hosted on the [GURU](https://wiki.gentoo.org/wiki/GURU) overlay.

#### eselect

Using [eselect repository](https://wiki.gentoo.org/wiki/Eselect/Repository), run this command to add the overlay:

`root #``eselect repository enable guru`
And then synchronize the repository:

`root #``emaint -r guru sync`
## Installation

### Emerge

Once the [GURU](https://wiki.gentoo.org/wiki/GURU) overlay is enabled, sys-boot/ventoy-bin can be emerged:

`root #``emerge --ask sys-boot/ventoy-bin`
## Usage

### Command line

Ventoy can be used on the command line. ventoy should be invoked along with the device in which the user wants to install or update Ventoy on.

`user $````
ventoy
```
```
     Ventoy: 1.0.98  x86_64
     longpanda admin@ventoy.net
     
```
[https://www.ventoy.net](https://www.ventoy.net)
Usage:  Ventoy2Disk.sh CMD [ OPTION ] /dev/sdX

CMD:
  -i  install Ventoy to sdX (fails if disk already installed with Ventoy)
  -I  force install Ventoy to sdX (no matter if installed or not)
  -u  update Ventoy in sdX
  -l  list Ventoy information in sdX

OPTION: (optional)
  -r SIZE_MB  preserve some space at the bottom of the disk (only for install)
  -s/-S       enable/disable secure boot support (default is enabled)
  -g          use GPT partition style, default is MBR (only for install)
  -L          Label of the 1st exfat partition (default is Ventoy)

-n          try non-destructive installation (only for install)
#### Installing Ventoy on a device

The -i flag can be used to install Ventoy on a device:

`root #``ventoy -g -i /dev/sdX`
#### Updating a Ventoy device

A Ventoy device can be updated with the -u flag:

`root #``ventoy -u /dev/sdX`
### GUI

Ventoy comes with a graphical tool as well. You can launch it via:

`root #``ventoygui`
## See also

- [LiveUSB](https://wiki.gentoo.org/wiki/LiveUSB) — explains how to create a *Gentoo LiveUSB* or, in other words, how to emulate a **x86** or **amd64** Gentoo LiveCD using a USB drive.
