<!-- source: https://wiki.gentoo.org/wiki/Banana_Pi | group: Gentoo Wiki (Main) | wiki-title: Banana Pi -->
---
title: Banana Pi
url: https://wiki.gentoo.org/wiki/Banana_Pi
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2024-04-12"
fingerprint: "70af3eca5c89eb60"
license: CC BY-SA 4.0
---

# Banana Pi

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

The Banana Pi embedded system is very similar to the [Raspberry Pi](https://wiki.gentoo.org/wiki/Raspberry_Pi).

## Lemakers Gentoo Image

Download the compressed image file from [http://www.lemaker.org/mirror](http://www.lemaker.org/mirror) and extract it.

`user $` `tar xfzv Gentoo_For_BananaPro_v1412.tgz`
Connect a compatible SD card, or a SATA 2.5" harddisc with your computer. Lets assume the device is named /dev/sdx. Write the image to the media with dd

`user $` `sudo dd if=Gentoo_For_BananaPro_v1412.img of=/dev/sdx bs=1M`
## Manual Gentoo installation on SDD

Boot a Gentoo system (for example the Lemaker image) from SD card. 
Follow to the installation procedure in the  [amd64 manual](https://wiki.gentoo.org/wiki/Handbook:AMD64) 
but download and use an ARMv7a stage3 file from [https://www.gentoo.org/downloads/](https://www.gentoo.org/downloads/) instead of the amd64 version.

### Build the Kernel for a Banana Pi

### Installation of U-Boot

See the instructions on this [Gentoo wiki page](https://wiki.gentoo.org/wiki/Banana_Pi_the_Gentoo_Way#U-Boot).

### Install WiFi

On the Banana Pi Pro load the following module

`user $` `sudo modprobe ap6210` ## See also

- [Distcc/Cross-Compiling](https://wiki.gentoo.org/wiki/Distcc/Cross-Compiling) — shows the reader how to set up distcc for cross-compiling across different processor architectures.
- [Banana Pi the Gentoo Way](https://wiki.gentoo.org/wiki/Banana_Pi_the_Gentoo_Way) — provides details on how to install Gentoo with a user compiled kernel and a Gentoo stage 3 on the Banana Pi
