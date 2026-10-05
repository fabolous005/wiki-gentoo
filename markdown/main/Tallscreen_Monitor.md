<!-- source: https://wiki.gentoo.org/wiki/Tallscreen_Monitor | group: Gentoo Wiki (Main) | wiki-title: Tallscreen Monitor -->
---
title: Tallscreen Monitor
url: https://wiki.gentoo.org/wiki/Tallscreen_Monitor
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2019-12-13"
fingerprint: "16d2d53e1a23f984"
license: CC BY-SA 4.0
---

# Tallscreen Monitor

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

Do vertically scrolling texts and images look lame on your widescreen monitor? Then rotate it!

## Frame buffer or modesetting rotation

Support for framebuffer rotation must be enabled in the kernel (this is not required for rotation using Xorg).

**Tallscreen related kernel modifications**

The `fbcon` kernel boot option is used to rotate the kernel frame buffer at boot time.

**`/usr/src/linux/Documentation/fb/fbcon.txt`**

If you wish to rotate your display to the right and are using GRUB-0, append the option `fbcon=rotate:1` to the `kernel` lines in /boot/grub/grub.cfg

To perform the same rotation with GRUB2, append `fbcon=rotate:1` to the `GRUB_CMDLINE_LINUX` variable in /etc/default/grub and execute the grub-mkconfig command:

`root #``grub-mkconfig -o /boot/grub2/grub.cfg`
The screen should be reoriented on the next boot, assuming the kernel has been compiled with rotation support.

## Xorg rotation

xrandr ([x11-apps/xrandr](https://packages.gentoo.org/packages/x11-apps/xrandr)) can rotate Xorg output at runtime, but the best practice is to rotate the display when Xorg is initialized and before anything is rendered.

First determine the name of you display output by running xrandr while Xorg is active. Look for a line like *HDMI1 connected...*.

Then add a `Rotate` option to the monitor section of 40-monitor.conf configuration file:

**`/etc/X11/xorg.conf.d/40-monitor.conf`**

**Tallscreen related Xorg modifications**

Xorg should now rotate the screen at X startup.

The author of this article uses tallscreen monitors whenever possible, but also uses a tiling window manager. Results may vary with other desktop environments.
