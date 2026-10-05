<!-- source: https://wiki.gentoo.org/wiki/Sony_Vaio_SVE | group: Gentoo Wiki (Main) | wiki-title: Sony Vaio SVE -->
---
title: Sony Vaio SVE
url: https://wiki.gentoo.org/wiki/Sony_Vaio_SVE
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2022-05-18"
fingerprint: "1ec09e9a2fad7df7"
license: CC BY-SA 4.0
---

# Sony Vaio SVE

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

"Chroot: illegal instruction" for amd64. Architecture x86 is working excellent.

## BIOS

Winbond 25Q32CVSIG

Programming is not working.

## Backlight - screen brightness

echo 255 > /sys/class/backlight/radeon\_bl0/brightness

To set brightness at startup create udev rule to load brightness level:

**`/etc/udev/rules.d/91-backlight.rules`**

**udev event to load brightness at boot**

```
# loading brightness
ACTION=="add", SUBSYSTEM=="backlight", RUN+="/bin/sh -c 'echo 150 > /sys/class/backlight/radeon_bl0/brightness'"
```
## Shift keys now working problem

Solution is to bind any other key to Shift.

### Console

- get key number:

\# showkey

- create file personal.map

keycode 86 = Shift Shift Shift Shift

- load key assigment:

\# loadkeys personal.map

can be placed in ̃/.bashrc file

### Xorg

setxkbmap -option caps:shiftlock

or

$ xmodmap .xmodmap

where .xmodmap:

keycode 66 = Shift\_L

you can get 66 with

̩$ xev

## Tested and works

- Ethernet
- Wifi
- Touchpad
- Audio
- HDMI and HDMI Audio

## Problems

\- Function keys is not working
