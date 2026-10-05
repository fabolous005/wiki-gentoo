<!-- source: https://wiki.gentoo.org/wiki/Sony_Vaio_Pro_SVP-1x21 | group: Gentoo Wiki (Main) | wiki-title: Sony Vaio Pro SVP-1x21 -->
---
title: Sony Vaio Pro SVP-1x21
url: https://wiki.gentoo.org/wiki/Sony_Vaio_Pro_SVP-1x21
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2024-04-28"
fingerprint: "167b1f5fd8e33b47"
license: CC BY-SA 4.0
---

# Sony Vaio Pro SVP-1x21

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)


How to setup Gentoo Linux on the Sony Vaio Pro Haswell based ultrabooks.

## Preparation

It's recommended you back up your disk so that you can keep the windows recovery partition in-case you ever need it.

Since most live usb sticks do not support the WiFi card (Intel(R) Dual Band Wireless N 7260) of these laptops, you will likely be without Internet while setting up the laptop until you can compile a kernel which contains the support (3.11.x or newer). The easiest way to bootstrap the laptop into a WiFi capable kernel is to use USB tethering with an internet capable phone. Alternatively if you do not own a tethering capable phone, you can download the following (or newer) packages from a [Gentoo Mirror](http://www.gentoo.org/main/en/mirrors2.xml). Or, if you have a working Gentoo machine already, simply copying them from that machine's /usr/portage/distfiles. Be sure also to get the latest firmware for the Intel 7260 WiFi card [\[1\]](http://intellinuxwireless.org).

`root #````
 cd /usr/portage/distfiles
```
`root #````
 cp autogen* bc* busybox* cpio* dmraid* efibootmgr* fuse* gcc-*-specs* gcc-*-patches* gcc-*-piepatches* gcc-* gcc-*-uclibc-patches* genkernel* genpatches-*.base* genpatches-*.extras* gnupg* grub* guile* hwids* libnl* linux* LVM2* mdadm* open-iscsi* pciutils* unifont* unionfs-fuse* wireless_tools* wpa_supplicant* /mnt/usbkey
```
These are all the source packages required to install efibootmgr, gentoo-sources, grub, wireless-tools, and wpa\_supplicant. With all this in hand you should be able to install Gentoo, build the newest kernel with support for the Intel 7260, and all the wireless tools necessary to connect to a wireless access point.

You will also need an appropriate stage3 tarball and the portage-latest tarball, so get those from your favourite mirror: [Gentoo Mirrors](http://www.gentoo.org/main/en/mirrors2.xml).

Another alternative is preparing your own live usb stick that has the necessary support for the Intel 7260.

## Kernel configuration

It's highly recommended you start from a kernel seed from [Pappy's Kernel Seeds](http://kernel-seeds.org/). Follow the guide for [Working with Kernel Seeds](http://kernel-seeds.org/working.html). The guide will take you through all the important steps of preparing your kernel. In the following sections, recommended configurations for **most** the Vaio Pro's hardware are included, as well as tips for getting some things working correctly. For those remaining pieces of hardware not listed, just follow the Working with Kernel Seeds guide.

### CPU

### SATA

Works with the generic ahci driver.

### Network

Be sure to place the appropriate firmware retrieved from [\[2\]](http://intellinuxwireless.org) in /lib/firmware or create an ebuild to do so.

### I2C support

### Webcam

### Graphics

### Sound

Using ALSA alone doesn't produce very good sound. It's recommended that you use [PulseAudio](https://wiki.gentoo.org/wiki/PulseAudio) with this laptop. Using plain alsa produces with this laptop produces poor sound quality at this time.

### SD card reader

Since kernel version 3.11.0, the SD card reader in the Sony Vaio Pro 13 (Realtek Semiconductor Co., Ltd. RTS5209 PCI Express Card Reader) does not work unless an SD card is inserted before booting. This unfortunately renders the SD Card reader mostly useless. Other than this major bug, the card reader works fine with the following config, and with the use of the rts\_pstor package.

`root #``emerge --ask rts_pstor`
