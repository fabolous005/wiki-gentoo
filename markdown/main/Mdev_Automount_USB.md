<!-- source: https://wiki.gentoo.org/wiki/Mdev/Automount_USB | group: Gentoo Wiki (Main) | wiki-title: Mdev/Automount USB -->
---
title: Mdev/Automount USB
url: https://wiki.gentoo.org/wiki/Mdev/Automount_USB
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2021-02-02"
fingerprint: "9cc3ce166bac8bea"
license: CC BY-SA 4.0
---

# Mdev/Automount USB

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

This article describes how to implement automounting of USB devices on a machine using mdev as the device manager. There are many links on the web describing how to implement automounting on embedded devices, running [busybox](https://wiki.gentoo.org/wiki/Busybox) with mdev. They all seem to assume only one user, namely root. It's more of a challenge to allow regular users to automount and unmount, without compromising overall system security. Unless otherwise specified, all steps below are to be done by root. This example is done on Gentoo Linux, but it is hopefully portable to any Linux distro.

## Constraints (or "why do it this way?")

1. Why use pmount and pumount?
  1. pmount supports mounting FAT-based filesystems with desired permissions and assigns user root and group plugdev.
  2. pmount optionally supports assigning a user-selected name to the mounted directory, similar to "writing udev rules". That functionality has not yet been implemented in this work, but using pmount leaves the door open to doing so later.
  3. pumount (pmount's version of "umount") is restricted to acting in the /media directory. This is a security feature, in that it won't unmount devices whose directories are not in the /media directory.
2. Why does automounting use /media?
  - Because pmount is currently hard-coded to automount in /media. It would require a change to the source code of pmount to use a different directory.
3. Why are scripts launched with `#!/bin/busybox ash` rather than `#!/bin/bash`?
  - If a machine is running mdev as the device manager, it's guaranteed to have busybox present. Although this is currently implemented on Gentoo Linux, it is hopefully portable to other Linux distros, and embedded systems. There may be lightweight distros that do not have bash loaded.
4. Why is the main automount script under /lib/mdev?
  - Because that's where mdev already puts some scripts under Gentoo. Keeping mdev-related scripts in the same area makes things easier to keep track of.

## Preliminary Setup

- This page assumes a Linux distro using busybox's mdev device manager as described in [the mdev page](https://wiki.gentoo.org/wiki/Mdev).
- Create the directory /media
- Regular user accounts that need to access FAT-formatted USB keys must be added to group "plugdev".
- Install [sys-apps/pmount](https://packages.gentoo.org/packages/sys-apps/pmount) and [app-admin/sudo](https://packages.gentoo.org/packages/app-admin/sudo), if they have not been installed yet.
- In /etc/sudoers.d/ create a file 001 (if it doesn't exist). To the file add a line like:

**`/etc/sudoers.d/001`**

Replace "USERID" and "HOSTNAME" with the actual regular userid and the actual hostname. If you have 2 or more users that need to unmount USB devices in /media, add a separate line for each one.

## Scripts for use for unmounting

Create a file /usr/local/bin/ux containing the following lines:

**`/usr/local/bin/ux`**

```
#!/bin/busybox ash
 pumount ${1}
```
Then, **as local user** for each user that needs to unmount USB devices that have been automounted, create an executable script "um" in the user's \~/bin directory containing the lines:

**`~/bin/um`**

```
#!/bin/busybox ash
 sudo /usr/local/bin/ux ${1}
```
## Theoretical background

For more information on mdev scripts, see [Busybox's mdev primer](http://git.busybox.net/busybox/plain/docs/mdev.txt). For the purposes of this page, here are the critical details:

- On a machine running mdev, the pseudo-file /proc/sys/kernel/hotplug should contain a string with the full path and filename of the hotplug handler. On Gentoo linux this is /sbin/mdev
- When the system boots up, mdev is invoked as `mdev -s` to populate /dev. This invocation supplies a very minimal environment, only the device name. When a USB device is inserted or removed, there can be one or more events. The insertion/removal events have additional environmental variables available. Testing for these variables allows a script to determine whether the event is device-population at bootup, or insertion/removal later on.
- The file /etc/mdev.conf is a list of patterns that is consulted by mdev. If a pattern is matched, certain attributes can be set for the device (user:group, permissions, symlinks). But most important for our purposes, commands can be executed. This is how auto(un)mounting is accomplished.

## Config files and scripts

- Create directory /lib/mdev/autousb/ amd executable file /lib/mdev/autousb/automount
  - mkdir /lib/mdev/autousb
  - touch /lib/mdev/autousb/automount
  - chmod 744 /lib/mdev/autousb/automount
- copy the contents of the automount script, listed at [/automount](https://wiki.gentoo.org/wiki/Mdev/Automount_USB/automount) into your file /lib/mdev/autousb/automount
- In file /etc/mdev.conf make the following change. Comment out the line

sd\[a-z\].\* root:disk 660 \*/lib/mdev/usbdisk\_link

and replace it with the line

sd\[a-z\].\* root:disk 660 \*/lib/mdev/autousb/automount

**`/etc/mdev.conf`**

## Usage notes

- Rebooting is not required. Automounting should now work.
- When a USB device with recognizable partition(s) is inserted, a corresponding subdirectory is created in /media/, e.g. /media/sdc1/, and mounted with filesystem type "auto"
- If script files and sudo permissions have been set up as described above, a local user can unmount /media/sdc1/ with the command `um sdc1`
- The subdirectory /media/sdc1/ will not be deleted until the USB device is physically removed
- Note that all unmounted subdirectories in /media/ will be removed anytime an external USB device is removed. Do not manually create create subdirectories in /media
