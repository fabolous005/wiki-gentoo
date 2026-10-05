<!-- source: https://wiki.gentoo.org/wiki/Steam_Controller | group: Gentoo Wiki (Main) | wiki-title: Steam Controller -->
---
title: Steam Controller
url: https://wiki.gentoo.org/wiki/Steam_Controller
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-06-21"
fingerprint: d6d08d9d00123e49
license: CC BY-SA 4.0
---

# Steam Controller

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

The [Steam](https://wiki.gentoo.org/wiki/Steam) Controller (2015) is a game controller developed by [Valve](https://en.wikipedia.org/wiki/Valve_Corporation). It features two trackpads (in place of thumbsticks) with [haptic](https://en.wikipedia.org/wiki/Haptic_technology) feedback and sixteen buttons. The Steam controller (2015) is designed not only for games supporting traditional controllers, but also for games that support keyboard and mouse.

## Installation

### Kernel

The Steam Controller (2015) is fully supported by Linux via the Steam client, however it does require [USB](https://wiki.gentoo.org/wiki/USB) and user level driver  (`CONFIG_INPUT_UINPUT`) support.

**Enabling user level driver support (`CONFIG_INPUT`, `CONFIG_INPUT_MISC`, `CONFIG_INPUT_UINPUT`)**

To have kernel support for the Steam controller (2015) (which makes it possible to use the Steam controller (2015) when not running the Steam client), enable the driver support in the kernel (`CONFIG_HID_STEAM`). Steam Controller (2015) support was added in kernel version 4.3 and above.

**Enabling Steam Controller (2015) driver support (`CONFIG_HID_STEAM`)**

### Permissions

#### systemd

If Steam was installed manually and systemd/elogind ***are*** being used, create the following udev rules file:

**`/etc/udev/rules.d/99-steam-controller-perms.rules`**

The above udev rules file will grant access to the Steam Controller (2015) by automatically setting an [ACL](https://wiki.gentoo.org/wiki/Filesystem/Access_Control_List_Guide) entry for the logged-in user.

If Steam was installed from anyc's Steam [external repository](https://wiki.gentoo.org/wiki/Steam#External_repositories), a udev rules file that supports systemd/elogind should already be installed with the >=steam-launcher-1.0.0.51-r1 ebuild.

Next, reload the udev rules files and trigger a device event for the new rule:

`root #````
udevadm control --reload
```
`root #````
udevadm trigger
```
Once the udev rules files are reloaded, the user using the Steam Controller (2015) will need to log out/in for the correct permissions to be set.

#### Manual udev

If Steam was installed [manually](https://wiki.gentoo.org/wiki/Steam#Manual) or from an [external repository](https://wiki.gentoo.org/wiki/Steam#External_repositories), and [systemd](https://wiki.gentoo.org/wiki/Systemd)/[elogind](https://wiki.gentoo.org/wiki/Elogind) ***are not*** being used, create the following [udev](https://wiki.gentoo.org/wiki/Udev) rules file:

**`/etc/udev/rules.d/99-steam-controller-perms.rules`**

The above udev rules file will grant access to the Steam Controller (2015)for users in the input group:

`root #``gpasswd -a <user> input`
## Usage

### Pairing

`B`+`Steam` puts the controller into Bluetooth Low Energy mode, whereas `A`+`Steam` puts the controller into wireless mode (requires the WiFi dongle).[\[1\]](https://wiki.gentoo.org#cite_note-1)

Press the `Steam`+`Y` key to enter pairing mode. Once paired successfully, the Steam button will glow white.

See the [Bluetooth](https://wiki.gentoo.org/wiki/Bluetooth) article for more information on pairing.

## steam input

for full usage of both controllers steam input is required. in order to use steam input it must be enabled in the steam settings, you must also launch your software threw steam which works natively for steam games but you will have to add your other software as a non steam game. in order for steam input to properly work games must be launched as a x11 window or within a shared gamescope session for both the game and steam. running software inside gamescope is desirable as it allows for wayland functionality such as HDR and better fractional scaling. to achieve this you must either run steam in a separate tty via gamescope (same as the steam deck's "gaming mode") or while nested with a command like.

`user $``gamescope -f -W 3840 -H 2160 --hdr-enabled --hdr-debug-force-support -- steam --steamos`
## See also

- [Sony DualShock](https://wiki.gentoo.org/wiki/Sony_DualShock) — describes the use of Sony DualShock 3 / [Sixaxis](https://en.wikipedia.org/wiki/Sixaxis), DualShock 4, and [DualSense](https://en.wikipedia.org/wiki/DualShock#DualSense) [PlayStation](https://en.wikipedia.org/wiki/PlayStation) controllers via USB and Bluetooth.
