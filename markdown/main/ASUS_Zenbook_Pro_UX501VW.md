<!-- source: https://wiki.gentoo.org/wiki/ASUS_Zenbook_Pro_UX501VW | group: Gentoo Wiki (Main) | wiki-title: ASUS Zenbook Pro UX501VW -->
---
title: ASUS Zenbook Pro UX501VW
url: https://wiki.gentoo.org/wiki/ASUS_Zenbook_Pro_UX501VW
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2024-12-22"
fingerprint: bc5b4d5ad597bb8c
license: CC BY-SA 4.0
---

# ASUS Zenbook Pro UX501VW

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

## Installation

### Kernel

**Linux 4.10 touchpad and touchscreen support**

**Linux 4.10 wifi support**

## Configuration

### Xorg

For proper input, video, and high DPI support on the 3840x2160 screen:

**`/etc/portage/package.use/00input`**

```
 INPUT_DEVICES: libinput
```
**`/etc/portage/package.use/00video`**

```
 VIDEO_CARDS: -* intel i965 nouveau
```
Create a xorg configuration file to set the monitor size in millimeters (these values were quickly and crudely calculated; you may want to measure your own):

**`/etc/X11/xorg.conf.d/90-hidpi.conf`**

If not using GNOME or some other software which manages Xresources and DPI for you, you will need to specify the DPI:

**`~/.Xresources`**

And you will need this line in someplace like \~/.xinitrc or \~/.xsession (depending on your configuration) in order to apply the Xresources file:

**`~/.xinitrc`**

Other applications may handle high DPI weirdly. The [Arch wiki page on High DPI](https://wiki.archlinux.org/index.php/HiDPI) has many helpful hints.
