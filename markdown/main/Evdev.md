<!-- source: https://wiki.gentoo.org/wiki/Evdev | group: Gentoo Wiki (Main) | wiki-title: Evdev -->
---
title: evdev
url: https://wiki.gentoo.org/wiki/Evdev
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2019-01-09"
fingerprint: "7e419c58119f3904"
license: CC BY-SA 4.0
---

# evdev

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**evdev** is

- The open source input driver ([x11-drivers/xf86-input-evdev](https://packages.gentoo.org/packages/x11-drivers/xf86-input-evdev)) for many input devices like keyboards, mice, joysticks and more.
- The short name of the Linux kernel's event interface (CONFIG\_INPUT\_EVDEV), needed for [libinput](https://wiki.gentoo.org/wiki/Libinput#Kernel).
- The [`input_devices_evdev`](https://packages.gentoo.org/useflags/input_devices_evdev) USE flag.

## Installation

### Kernel

You need [USB](https://wiki.gentoo.org/wiki/USB) support, if you have an USB input device. Also you need to activate the following kernel options:

**PS/2 keyboard/mouse support**

**USB input device support**

Some USB mice (e.g. Logitech G5 and Razer Naga 2014) additionally need the following option:

**Improved transaction support**

### Driver

**`/etc/portage/make.conf`**

**Set`INPUT_DEVICES`**

```
INPUT_DEVICES="evdev"
```
After setting the `INPUT_DEVICES` variable remember to update the system using the following command so the changes take effect:

`root #``emerge --ask --changed-use --deep @world`


## Configuration

### Keyboard layout

To set the default layout copy the file 10-evdev.conf to /etc/X11/xorg.conf.d and edit the keyboard section, e.g. for a German layout:

`root #``cp /usr/share/X11/xorg.conf.d/10-evdev.conf /etc/X11/xorg.conf.d/`
**`/etc/X11/xorg.conf.d/10-evdev.conf`**

```
Section "InputClass"
        Identifier "evdev keyboard catchall"
        ...
        Driver "evdev"
        Option "xkb_layout" "de"
EndSection
```
For more info please read the [Configuring the keyboard](https://wiki.gentoo.org/wiki/Xorg/Guide#Configuring_the_keyboard).

## See also

- [Libinput](https://wiki.gentoo.org/wiki/Libinput) — an input device driver for [Wayland compositors](https://wiki.gentoo.org/wiki/Wayland_Desktop_Landscape#Compositors) and [X.org](https://wiki.gentoo.org/wiki/Xorg) window system.
