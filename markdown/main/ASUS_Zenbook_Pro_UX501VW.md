<!-- source: https://wiki.gentoo.org/wiki/ASUS_Zenbook_Pro_UX501VW | group: Gentoo Wiki (Main) | wiki-title: ASUS Zenbook Pro UX501VW -->
---
title: ASUS Zenbook Pro UX501VW
url: https://wiki.gentoo.org/wiki/ASUS_Zenbook_Pro_UX501VW
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2024-12-22"
fingerprint: "3e41c954d9963be4"
license: CC BY-SA 4.0
---

# ASUS Zenbook Pro UX501VW

From Gentoo Wiki

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**Resources**

## Installation

### Kernel

KERNEL **Linux 4.10 touchpad and touchscreen support**

```
Device Drivers --->
 Input device support --->
  [*] Mice --->
   <*> ELAN I2C Touchpad support
    [*] Enable I2C support
    [*] Enable SMbus support
  [*] Touchscreens --->
   <*> USB Touchscreen Driver
    [*] (All devices selected)
 
 I2C support --->
  [*] I2C support
  I2C Hardware Bus support --->
   <*> (I selected all Intel options)
   <*> Synopsys DesignWare Platform
   <*> Synopsys DesignWare PCI
   [*] Intel Baytrail I2C semaphore support
```
KERNEL **Linux 4.10 wifi support**

```
Device Drivers --->
 [*] Network device support --->
  [*] Wireless LAN --->
   [*] Intel devices
    <*/M> Intel Wireless WiFi Next Gen AGN - Wireless-N/Advanced-N/Ultimate-N (iwlwifi)
     <*/M> Intel Wireless WiFi MVM Firmware support
```
## Configuration

### Xorg

For proper input, video, and high DPI support on the 3840x2160 screen:

FILE **`/etc/portage/package.use/00input`**

```
*/* INPUT_DEVICES: libinput
```
FILE **`/etc/portage/package.use/00video`**

```
*/* VIDEO_CARDS: -* intel i965 nouveau
```
Create a xorg configuration file to set the monitor size in millimeters (these values were quickly and crudely calculated; you may want to measure your own):

FILE **`/etc/X11/xorg.conf.d/90-hidpi.conf`**

```
Section "Monitor"
	Identifier	"<default monitor>"
	DisplaySize	342 193
EndSection
```
If not using GNOME or some other software which manages Xresources and DPI for you, you will need to specify the DPI:

FILE **`~/.Xresources`**

```
Xft.dpi: 240
```
And you will need this line in someplace like \~/.xinitrc or \~/.xsession (depending on your configuration) in order to apply the Xresources file:

FILE **`~/.xinitrc`**

```
xrdb -merge ~/.Xresources
```
Other applications may handle high DPI weirdly. The [Arch wiki page on High DPI](https://wiki.archlinux.org/index.php/HiDPI) has many helpful hints.
