<!-- source: https://wiki.gentoo.org/wiki/Freefall | group: Gentoo Wiki (Main) | wiki-title: Freefall -->
---
title: Freefall
url: https://wiki.gentoo.org/wiki/Freefall
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2018-07-03"
fingerprint: "170e346ae175898f"
license: CC BY-SA 4.0
---

# Freefall

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**Resources**

**freefall** is a simple daemon providing HDD shock protection for HP laptops supporting the feature officially called "HP Mobile Data Protection System 3D" or "HP 3D DriveGuard".

## Installation

### Kernel

You need to activate the following kernel option either as built-in or as module.

```
Device drivers --->
    [*] X86 Platform Specific Device Drivers  --->
        <*> HP laptop accelerometer
```
### Emerge

The freefall daemon and init script can be found in [sys-apps/linux-misc-apps](https://packages.gentoo.org/packages/sys-apps/linux-misc-apps):

`root #``emerge --ask sys-apps/linux-misc-apps`
## Configuration

### Files

Set the HDD in the /etc/conf.d/freefall configuration file.

### Services

#### OpenRC

Start freefall daemon:

`root #``/etc/init.d/freefall start`
To start freefall at boot time, add it to your boot runlevel:

`root #``rc-update add freefall boot`
## Testing

After reboot with new kernel, check if hp\_accel driver was initialized correctly:

`user $``dmesg | grep hp_accel`
### Testing shock protection

Find the HDD's unload\_heads file, it will be somewhere in the /sys directory:

`user $``find /sys -name unload_heads`
Go to its directory and run:

`user $``watch --difference=permanent cat unload_heads`
Lift the laptop into free space and simulate free fall while holding it firmly. About a 10 cm drop should be enough.

If the disk protection works then the HDD should make "click" sound, see one of your laptop's LEDs flashing and the watch value's background should permanently turn black.
