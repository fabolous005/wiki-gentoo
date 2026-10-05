<!-- source: https://wiki.gentoo.org/wiki/Canon_CAPT_Printer | group: Gentoo Wiki (Main) | wiki-title: Canon CAPT Printer -->
---
title: Canon CAPT Printer
url: https://wiki.gentoo.org/wiki/Canon_CAPT_Printer
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2022-08-15"
fingerprint: ae27375315a7b9ca
license: CC BY-SA 4.0
---

# Canon CAPT Printer

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

This article is **deprecated (obsolete)**. Contents are <u>no longer relevant</u>, and are intended for historical reference only!

TLDR:

**Do not use this article!**

There are some open source community efforts here [https://github.com/agalakhov/captdriver/](https://github.com/agalakhov/captdriver/) and here [https://github.com/ra1nst0rm3d/captdriver](https://github.com/ra1nst0rm3d/captdriver).


This page explains setup and configuration of Canon CAPT Printers.

## Introduction

The **Canon i-Sensys** series of laser printers are supported under CUPS by Canon's proprietary Canon Advanced Printing Technology (CAPT) backend.

Supported models are: LBP9100C, LBP7210Cdn, LBP7200C series, LBP7200Cdn (network mode), LBP7018C, LBP7010C, LBP6310dn, LBP6300, LBP6300n, LBP6200, LBP6020, LBP6018, LBP6000, LBP5300, LBP5100, LBP5050 series, LBP5000, LBP3500, LBP3310, LBP3300, LBP3250, LBP3210, LBP3200, LBP3150, LBP3108, LBP3100, LBP3050, LBP3018, LBP3010, LBP3000, LBP2900, LBP-1210, LBP-1120, LBP-810

## Prerequisites

### CUPS

Make sure [net-print/cups](https://packages.gentoo.org/packages/net-print/cups) is installed:

`root #``emerge --ask net-print/cups`
The cups service must be started before the driver(s) can be installed.

#### OpenRC

If OpenRC is used, then the following command can be issued to start the *cups* service.

`root #``/etc/init.d/cupsd start`
To have it started automatically at boot time, use rc-update:

`root #``rc-update add cupsd default`
#### systemd

To start CUPS immediately, issue the following command:

`root #``systemctl start cups`
To start it automatically at boot:

`root #``systemctl enable cups`
## Installing the drivers

Drivers are located on Gentoo's Bugzilla [bug #130612](https://bugs.gentoo.org/show_bug.cgi?id=130612)

Ebuilds for latest version 2.60 are in github repo: [https://github.com/homoludens/canoncapt-euilds](https://github.com/homoludens/canoncapt-euilds)

For using it you would need to create custom repository explained on page [Creating an ebuild repository](https://wiki.gentoo.org/wiki/Creating_an_ebuild_repository).

Download archive [net-print-cndrvcups.tar.gz](https://github.com/homoludens/canoncapt-euilds/archive/master.zip) and unpack it in created overlay.
It contains two ebuilds: net-print/cndrvcups-capt and net-print/cndrvcups-common.

If your custom repository location is /usr/local/portage you should have directories:

/usr/local/portage/net-print/cndrvcups-common

/usr/local/portage/net-print/cndrvcups-capt

In that case you will need to issue this commands to be able to emerge ebuilds:

`root #````
 pushd /usr/local/portage/net-print/cndrvcups-common
```
`root #````
 pkgdev manifest
```
`root #````
 popd
```
`root #````
 pushd /usr/local/portage/net-print/cndrvcups-capt
```
`root #````
 pkgdev manifest
```
`root #````
 popd
```
After that you will need to emerge them (net-print/cndrvcups-capt depends on net-print/cndrvcups-common):

`root #``emerge --ask net-print/cndrvcups-capt`
If you are using USB printer you will need usblp kernel module which is by default blacklisted by net-print/cups, easiest way around this is recompiling cups with USE="-usb". usbplp module is creating /dev/usb/lp0 device used for communicating with printer.

Currently only use flag is USE="-cngplp", disabled by default and is creating some GUI that is not really interesting and needed. And creates multiple issues with compiling this drivers.

## Configuration

### CUPS

There are two options for setting device-uri with lpadmin command using *ccp://localhost:59687* and *ccp:/var/ccpd/fifo0*.

Make sure cupsd is started:

`root #``/etc/init.d/cupsd status`
if it is not, start it first:

`root #``/etc/init.d/cupsd start`
#### Option 1: Using ccp://localhost:59687

First the printer needs to be registered with CUPS using lpadmin:

`root #``lpadmin -p` *printer_model* -m *printer_driver_file* -v ccp://localhost:59687 -E
The printer driver file can be found in /usr/share/cups/model. For the printer model, use the *ShortNickName* found inside the PPD, or take it directly from the file name, for example CNCUPS**LBP6310**CAPTK.ppd

Alternatively you could check the table provided on the [Ubuntu help page](https://help.ubuntu.com/community/CanonCaptDrv190), which matches each supported printer with its corresponding PPD.

For example, for the Canon LBP6310dn (UK model) you would enter

`root #` `lpadmin -p LBP6310 -m CNCUPSLBP6310CAPTK.ppd -v ccp://localhost:59687 -E`
#### Option 2: Using ccp:/var/ccpd/fifo0

If previous option is not working for you you can try:

If the status monitor works but printing does not, make sure that /var/ccpd/fifo0 actually exists:

`user $``ls -l /var/ccpd`
When missing, it can be added manually:

`root #````
mkdir /var/ccpd
```
`root #````
mkfifo /var/ccpd/fifo0
```
`root #````
chown -R lp:lp /var/ccpd
```
`root #` `lpadmin -p LBP6310 -m CNCUPSLBP6310CAPTK.ppd -v ccp:/var/ccpd/fifo0 -E`
### CAPT

Next you'll need to register the printer with the CAPT driver itself via ccpdadmin:

#### USB printers

`root #` `ccpdadmin -p` *printer_model* -o *usb_port*
e.g.

`root #``ccpdadmin -p LBP6310 -o /dev/usb/lp0`
#### network printers

`root #` `ccpdadmin -p` *printer_model* -o net:*ip_address*
e.g.

`root #``ccpdadmin -p LBP6310 -o net:192.168.1.100`


### Starting service

After this you will need to start ccpd service:

`root #` `/etc/init.d/ccpd start`
And add it do default init.

### Status monitor

The driver includes a status monitor GUI which can be launched with

`user $``captstatusui -P` *printer_model*
e.g.

`user $``captstatusui -P LBP6310`
If you only want the status monitor to pop up when a problem occurs, simply append the -e switch:

`user $``captstatusui -P LBP6310 -e`
You may want to add this to your startup folder/script

## Troubleshooting

### 100% CPU

This is known to happen. *ccpd* service must be started **after** *cupsd* and *udevd*.

### module usblp

If status monitor reports error "Check the DevicePath of /etc/ccpd.conf", then you probably don't have usblp kernel module. Run the following command:

lsmod | grep usblp

If it outputs nothing, load the module and restart ccpd:

`root #````
modprobe usblp
```
`root #````
ls -l /dev/usb/lp0
```
`root #````
/etc/init.d/ccpd stop
```
`root #````
/etc/init.d/ccpd start
```
usblp has been deprecated by CUPS and might not be automatically loaded when you connect your USB printer.

### Device URI: ccp://localhost:59687 VS ccp:/var/ccpd/fifo0

On some systems setting printer with ccp://localhost:59687 does not work, in that case try Option 2 ccp:/var/ccpd/fifo0.

See Section for configuring CUPS.

### CUPS

Use cups web interface to detect problem [https://localhost:631](https://localhost:631) it can help with different issues. Since Canon's this drivers are notorious for not providing *any* debug information.

### 64-bit systems

Proprietary part of this module is complied for 32-bit systems. This can cause problems mostly related to libc6 and libpopt0 libraries on which it depends. You can check that with:

`user $``ldd /usr/bin/captfilter`
Additionally linked ebuilds are only tested for x86.

(I hope someone with amd64 will be able to provide support or more info, until then take a look at [https://forums.gentoo.org/viewtopic-t-1017844-highlight-capt.html](https://forums.gentoo.org/viewtopic-t-1017844-highlight-capt.html) - it has useful information about amd64)

Until someone helps with amd64, his should be helpful and should be done before emerging driver:

`root #``emerge --ask x11-libs/pangox-compat``root #``echo "dev-libs/popt abi_x86_32" >>/etc/portage/package.use``root #``emerge --ask dev-libs/popt``root #``echo "dev-libs/libxml2 abi_x86_32" >>/etc/portage/package.use``root #``emerge --ask dev-libs/libxml2`
Last command also pulls [sys-libs/zlib](https://packages.gentoo.org/packages/sys-libs/zlib) with *abi\_x86\_32* use flag.

### Helpful Gentoo forum topics

[https://forums.gentoo.org/viewtopic-t-1017844-highlight-capt.html](https://forums.gentoo.org/viewtopic-t-1017844-highlight-capt.html) -- this one is useful for **amd64**

### Bugs

Expect them. And please report bugs/problems you have with ebuilds to [bug #130612](https://bugs.gentoo.org/show_bug.cgi?id=130612)

## Alternatives

While searching for solutions of your problems with some of this printers you will get to this pages, eventually:

[Driverless printing](https://wiki.gentoo.org/wiki/Driverless_printing).  To be tested/verified by someone owning such printer

Those were really nice open source drivers, that were actually working at the time, but only had one release. It would be nice if someone would take over those projects and keep them alive. Until then, we will have to use official ones.
