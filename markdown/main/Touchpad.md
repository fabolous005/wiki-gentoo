<!-- source: https://wiki.gentoo.org/wiki/Touchpad | group: Gentoo Wiki (Main) | wiki-title: Touchpad -->
---
title: Touchpad
url: https://wiki.gentoo.org/wiki/Touchpad
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2023-04-19"
fingerprint: b52f327be9b7a98c
license: CC BY-SA 4.0
---

# Touchpad

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

This meta article acts as a hub of shared knowledge for FOSS touchpad configuration efforts. Improving the state of touchpads is a continual improvement effort in the Linux ecosystem. With support enabled in the kernel, configuration can be performed in a variety of locations. Modern desktop environments build off device drivers exposed from [libinput](https://wiki.gentoo.org/wiki/Libinput).

## Known components

| Name | Linux hardware | Description | 
|---|---|---|
| [Alps\_PS/2](https://wiki.gentoo.org/wiki/Alps_PS/2) | [Device 'ALPS AlpsPS/2 DualPoint TouchPad'](https://linux-hardware.org/index.php?id=ps/2:alps-0008-alpsps-2-dualpoint-touchpad) | CONFIG\_MOUSE\_PS2\_ALPS | 

## Available software

### Desktop environments

Desktop environments generally include their own controls for touchpads customization. These can include hot corners, scroll behavior, pinch to zoom, etc.

- [GNOME](https://wiki.gentoo.org/wiki/GNOME)
  - Graphically the Settings menu ([gnome-base/gnome-settings-daemon](https://packages.gentoo.org/packages/gnome-base/gnome-settings-daemon) - included with GNOME) and GNOME Tweaks ([gnome-extra/gnome-tweaks](https://packages.gentoo.org/packages/gnome-extra/gnome-tweaks)).
  - From the commandline via gsettings (available via [dev-libs/glib](https://packages.gentoo.org/packages/dev-libs/glib)).
- [KDE Plasma](https://wiki.gentoo.org/wiki/KDE_Plasma)
  - ?
- [Xfce](https://wiki.gentoo.org/wiki/Xfce)
- System settings
- [LXDE](https://wiki.gentoo.org/wiki/LXDE)
  - ?
- [Sway](https://wiki.gentoo.org/wiki/Sway)
- [i3](https://wiki.gentoo.org/wiki/I3)

### Other

- [Xorg](https://wiki.gentoo.org/wiki/Xorg) can be used to configure touchpad behavior directly in the [xorg.conf](https://wiki.gentoo.org/wiki/Xorg.conf) file. Various drivers, such as [Libinput](https://wiki.gentoo.org/wiki/Libinput) or [Synaptics](https://wiki.gentoo.org/wiki/Synaptics) can be used to drive the hardware via Xorg.

## Features

### Disable while typing

Disable while typing (DWT) support can be activated as a feature of the desktop environment or [directly via libinput driver](https://wayland.freedesktop.org/libinput/doc/latest/palm-detection.html#disable-while-typing). Each DE has a different settings path in order to activate the feature.[\[1\]](https://wiki.gentoo.org#cite_note-1)

## See also

- [libinput](https://wiki.gentoo.org/wiki/Libinput) — an input device driver for [Wayland compositors](https://wiki.gentoo.org/wiki/Wayland_Desktop_Landscape#Compositors) and [X.org](https://wiki.gentoo.org/wiki/Xorg) window system.

## External resources

- [https://linuxtouchpad.org/](https://linuxtouchpad.org/) - A project dedicated to making touchpads on Linux systems behave in a manner similar to touchpads on MacBooks.
