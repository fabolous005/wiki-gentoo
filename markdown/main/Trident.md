<!-- source: https://wiki.gentoo.org/wiki/Trident | group: Gentoo Wiki (Main) | wiki-title: Trident -->
---
title: trident
url: https://wiki.gentoo.org/wiki/Trident
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2023-04-05"
fingerprint: "244fb659d89ea115"
license: CC BY-SA 4.0
---

# trident

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)


**trident** is the open source graphics drivers for [Trident](https://en.wikipedia.org/wiki/Trident_Microsystems) graphics cards.

## Installation

### Kernel

You need to activate the following kernel options:

### Firmware

Unknown IRQ microcode needed for Trident graphics support at this time, since the Kernel driver (tridentfb) should support most functions for Trident cards.

[x11-drivers/xf86-video-trident](https://packages.gentoo.org/packages/x11-drivers/xf86-video-trident) is an old video card and old driver. It has no corresponding KMS (Kernel ModeSetting), at least in recent (3.12.21) kernels.
Although in some distros (AFAIK Fedora 21) such drivers were removed, to my experience it works without KMS. At least with =x11-base/xorg-server-1.15.0.

Make sure firmware for your model (check available ones in /lib/firmware/trident) is included in kernel:

**Including trident firmware**

Below is a list of the firmware files needed for each family of cards:

No known firmware files are needed for any particular Trident card at this time.

### Driver

**`/etc/portage/make.conf`**

**Set`VIDEO_CARDS` to trident**

```
VIDEO_CARDS="... trident ..."
```
After setting or altering `VIDEO_CARDS` values remember to update the system using the following command so the changes take effect:

`root #``emerge --ask --changed-use --deep @world`
## Configuration

### Permissions

If the [`acl`](https://packages.gentoo.org/useflags/acl) USE flag is enabled globally and [`elogind`](https://packages.gentoo.org/useflags/elogind) is being used (default for desktop profiles) permissions to video cards will be handled automatically. It is possible to check the permissions using getfacl:

`user $``getfacl /dev/dri/card0 | grep larry``user:`**larry**:rw-
A broader solution is to add the user(s) needing access the video card to the video group:

`root #``gpasswd -a larry video`
Note that users will be able to run X without permission to the DRI subsystem, but hardware acceleration will be disabled.

### xorg.conf

The [X server](https://wiki.gentoo.org/wiki/X_server) is designed to work out-of-the-box, with no need to manually edit X.Org's configuration files. It should detect and configure devices such as displays, keyboards, and mice.

However, the main configuration file of the X server is the [xorg.conf](https://wiki.gentoo.org/wiki/Xorg.conf).

You can force the X server to use desired driver with:

**`/etc/X11/xorg.conf.d/trident.conf`**

**Explicit trident driver section**

```
Section "Device"
   Identifier  "trident"
   Driver      "trident"
EndSection
```
To my experience quoted config don't works. Last error message in Xorg.log is:

\[   218.176\] (II) Loading sub module "xaa"
\[   218.176\] (II) LoadModule: "xaa"
\[   218.177\] (WW) Warning, couldn't open module xaa
\[   218.178\] (II) UnloadModule: "xaa"
\[   218.178\] (II) Unloading xaa
\[   218.178\] (EE) TRIDENT: Failed to load module "xaa" (module does not exist, 0)

Adding to trident.conf the option:

**`/etc/X11/xorg.conf.d/trident.conf`**

```
Option		"AccelMethod" "EXA"
```
makes xorg server operable.

### Framebuffer (GRUB or LILO)

## Documentation

Full Documentation can be found under /usr/src/linux/Documentation/fb/tridentfb.txt.
