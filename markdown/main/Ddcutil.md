<!-- source: https://wiki.gentoo.org/wiki/Ddcutil | group: Gentoo Wiki (Main) | wiki-title: Ddcutil -->
---
title: ddcutil
url: https://wiki.gentoo.org/wiki/Ddcutil
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-03-05"
fingerprint: e60023f49d0c2d06
license: CC BY-SA 4.0
---

# ddcutil

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)



ddcutil is a Linux program for managing monitor settings, such as brightness, color levels, and input source. Generally speaking, any setting that can be changed by pressing buttons on the monitor can be modified by ddcutil.

## Installation

### USE flags


| [+dbus](https://packages.gentoo.org/useflags/+dbus) | Enable dbus support for anything that needs it (gpsd, gnomemeeting, etc) | 
| [X](https://packages.gentoo.org/useflags/X) | Add support for X11 | 
| [drm](https://packages.gentoo.org/useflags/drm) | Use x11-libs/libdrm for more verbose diagnostics. | 
| [systemd](https://packages.gentoo.org/useflags/systemd) | Enable use of systemd-specific libraries and features like socket activation or session tracking | 
| [usb-monitor](https://packages.gentoo.org/useflags/usb-monitor) | Adds support for monitors attached via USB. | 
| [user-permissions](https://packages.gentoo.org/useflags/user-permissions) | Adds a udev rules to allow non-root users in the i2c group to access the /dev/i2c-\* devices. If usb-monitor is selected, users will need to be added to the video group to access the USB monitor. Otherwise, only root will be able to use ddcutil. | 

### Emerge

`root #``emerge --ask app-misc/ddcutil`
### Configuration

#### I2C

Module [i2c\_dev](https://wiki.gentoo.org/wiki/I2C) should be loaded to allow ddcutil to work. The package should automatically enable this via /usr/lib/modules-load.d/ddcutil.conf. If not:

`root #``echo "i2c_dev" > /etc/modules-load.d/i2c_dev.conf``root #``modprobe i2c_dev`
#### Permissions

On recent versions (2.2.0 at time of writing, early 2026), /usr/lib/udev/rules.d/60-ddcutil-i2c.rules is installed when [user-permissions](https://packages.gentoo.org/useflags/user-permissions) [is activated. However, 2.2.2 and beyond (currently in testing) contain a fixed udev rule for some hardware, which the user can override locally with](https://wiki.gentoo.org/wiki/USE_flag)

**`/etc/udev/rules.d/60-ddcutil-i2c.rules`**

```
# fix from ddcutil-2.2.2: 0x03* (see https://github.com/rockowitz/ddcutil/issues/530)
SUBSYSTEM=="i2c-dev", KERNEL=="i2c-[0-9]*", ATTRS{class}=="0x03*", TAG+="uaccess"
SUBSYSTEM=="dri", KERNEL=="card[0-9]*", TAG+="uaccess"
```


##### Granting I2C Device Permissions on Versions Prior to 1.4.0

It is necessary is to add users who will use ddcutil to group `i2c`

`root #``groupadd --system i2c``root #``modprobe i2c_dev`
Create udev rule for giving group i2c RW permission on the /dev/i2c devices

`root #``cp -v /usr/share/ddcutil/data/45-ddcutil-i2c.rules /etc/udev/rules.d`
