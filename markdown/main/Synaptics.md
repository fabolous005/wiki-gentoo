<!-- source: https://wiki.gentoo.org/wiki/Synaptics | group: Gentoo Wiki (Main) | wiki-title: Synaptics -->
---
title: synaptics
url: https://wiki.gentoo.org/wiki/Synaptics
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2023-11-01"
fingerprint: bd04181b7da6bded
license: CC BY-SA 4.0
---

# synaptics

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**synaptics** is the open source input driver for Synaptics and [ALPS](https://wiki.gentoo.org/wiki/Alps_PS/2) touchpads.

## Installation

### Kernel

Activate the following kernel options:

### Driver

**`/etc/portage/make.conf`**

**Set`INPUT_DEVICES`**

```
INPUT_DEVICES="synaptics libinput"
```
After setting the `INPUT_DEVICES` variable remember to update the system using the following command so the changes take effect:

`root #``emerge --ask --changed-use --deep @world`


## Configuration

The driver has a lot options for tuning. See the [synaptics(5)](https://man.archlinux.org/man/synaptics.5.en) [man page for more information.](https://wiki.gentoo.org/wiki/Special:MyLanguage/man_page)

### Fixed configuration

Referring to [xorg.conf](https://wiki.gentoo.org/wiki/Xorg.conf) there should have a /etc/X11/xorg.conf.d directory on the system. If there is none create one:

`root #``mkdir /etc/X11/xorg.conf.d`
Configure file /etc/X11/xorg.conf.d/50-synaptics.conf as in the example below:

**`/etc/X11/xorg.conf.d/50-synaptics.conf`**

### Configuration at runtime

Enable the above option to be able to configure the driver also at runtime. Changes at runtime will be lost with the next start of the X server. Add changes to the above config file to persist desired settings.

Configure the driver with the program synclient. Some examples:

List all parameters:

`user $``synclient -l`
Cut the right side of the touch area to expand the vertical scroll area:

`user $``synclient RightEdge=5000`
Finding the right edge parameter:

`user $``synclient -m 50`
Disable the mouse click function:

`user $``synclient MaxTapTime=0`
Finally, dump the handpicked configuration to the 99-synaptics file pasting output of the following command inside the `InputClass` section:

`user $``synclient -l | sed -e '1d' -e 's/^ \+/Option\t"/g' -e 's/ \+= /"\t"/g' -e 's/$/"/g'`
Alternatively there is the [KDE](https://wiki.gentoo.org/wiki/KDE) systemsettings module [kde-misc/synaptiks](https://packages.gentoo.org/packages/kde-misc/synaptiks):

`root #``emerge --ask kde-misc/synaptiks`
## Troubleshooting

### Touchpad is not recognized

If the touchpad does not show in either lsusb nor lspci, that might be due to the PS/2 controller and how it is handled by the kernel<sup>[\[1\]](https://wiki.gentoo.org#cite_note-1)</sup>. One indication is if dmesg returns sometings along the lines of:

`user $``dmesg | grep i8042`
i8042: PNP: PS/2 appears to have AUX port disabled, if this is incorrect please boot with i8042.nopnp

That AUX port is where the touchpad is connected<sup>[\[2\]](https://wiki.gentoo.org#cite_note-2)</sup>.
Try adding the following to the kernel command line, e.g. in /etc/default/grub:

**`/etc/default/grub`**

Now, update your grub.cfg:

`root #``grub-mkconfig -o /boot/grub/grub.cfg`
If, after rebooting with these parameters, a generic `Logitech PS/2 mouse` input device is detected, the appropriate PS/2 extension driver may be necessary in the Kernel config:

After rebooting, the touchpad should be recognized correctly.

## See also

- [libinput](https://wiki.gentoo.org/wiki/Libinput) — an input device driver for [Wayland compositors](https://wiki.gentoo.org/wiki/Wayland_Desktop_Landscape#Compositors) and [X.org](https://wiki.gentoo.org/wiki/Xorg) window system.
- [Xorg/Using the numeric keyboard keys as mouse](https://wiki.gentoo.org/wiki/Xorg/Using_the_numeric_keyboard_keys_as_mouse) — XOrg comes with built-in mouse emulation using the keyboards numeric keypad.
