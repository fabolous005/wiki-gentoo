<!-- source: https://wiki.gentoo.org/wiki/Logitech_G15 | group: Gentoo Wiki (Main) | wiki-title: Logitech G15 -->
---
title: Logitech G15
url: https://wiki.gentoo.org/wiki/Logitech_G15
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2019-03-18"
fingerprint: "94c6384119cd01a7"
license: CC BY-SA 4.0
---

# Logitech G15

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

This article is written for the Logitech G15 but it should work with other keyboards in the G-series produced by Logitech.

## Installation

### Kernel

g15daemon requires user level driver support.

### Portage

To get other applications to know about the G15 keyboard, add the following to /etc/portage/make.conf.

**`/etc/portage/make.conf`**

```
LCD_DEVICES="g15"
USE="g15 lcd"
```
### Emerge

Install [app-misc/g15daemon](https://packages.gentoo.org/packages/app-misc/g15daemon) for the multimedia keys and LCD display, and [app-misc/g15macro](https://packages.gentoo.org/packages/app-misc/g15macro) for the `M^` and `G^` keys.

`root #``emerge --ask app-misc/g15daemon app-misc/g15macro`
## Configuration

After installing g15daemon and g15macro, check the g15daemon configuration file before starting the service. Normally the default values should work.

**`/etc/g15daemon.conf`**

### Xorg

Optionally add the following to /etc/X11/[xorg.conf](https://wiki.gentoo.org/wiki/Xorg.conf):

**`/etc/X11/xorg.conf`**

```
Option         "XkbModel" "logitech_g15"
```
### Service

#### OpenRC

Configure the g15daemon service configuration file:

**`/etc/conf.d/g15daemon`**

```
# Key to switch the client-screens. Default is the MR key,
# Set to "yes" to use L1 key instead (black round key below the LCD, above the multimedia keys).
CLIENT_SWITCH_L1="no"
# Set to "yes" to switch off the lcd backlight when stopping g15daemon.
BACKLIGHT_OFF="no"
```
To start g15daemon:

`root #``rc-service g15daemon start`
To start g15daemon at boot:

`root #``rc-update add g15daemon default`
If everything worked correctly a clock should be visible on the G15 LCD display.

## Usage

### Multimedia keys

To have the new keys working in X11, create a specific xmodmap in your home directory or edit an existing one.

Create the xmodmap:

`user $``cp /usr/share/g15daemon/contrib/xmodmaprc ~/.Xmodmap`
Adding keycodes to an existing xmodmap:

`user $``cat /usr/share/g15daemon/contrib/xmodmaprc >> ~/.Xmodmap`
To get the new keys you might need to restart your session or run:

`user $``xmodmap ~/.Xmodmap`
To check if your keys have been mapped correctly you can test them with [x11-apps/xev](https://packages.gentoo.org/packages/x11-apps/xev).

### G & M keys

To get your `G^` and `M^` keys to work you need to start up g15macro, preferably you should make it start automatically with the rest of your desktop.

`user $``/usr/bin/g15macro &`
Your extra keys should now be working and you should be able to record macro keys by pushing the `MR` key.

## See also

[Sidewinderd](https://wiki.gentoo.org/wiki/Sidewinderd) — a user space daemon that enables special keys and macro recording for various Logitech and Microsoft gaming peripherals.
