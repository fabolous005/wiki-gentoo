<!-- source: https://wiki.gentoo.org/wiki/Udisks | group: Gentoo Wiki (Main) | wiki-title: Udisks -->
---
title: udisks
url: https://wiki.gentoo.org/wiki/Udisks
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2023-12-18"
fingerprint: "260070438bd7f19d"
license: CC BY-SA 4.0
---

# udisks

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)


**udisks** is a [D-Bus](https://wiki.gentoo.org/wiki/D-Bus) daemon offering storage related services. Provided services include:

- Enumeration of storage devices, much like [udev](https://wiki.gentoo.org/wiki/Udev) but with more details.
- [Mounting](https://wiki.gentoo.org/wiki/Mount) of filesystems.
- [Partition](https://wiki.gentoo.org/wiki/Partition) of storage devices.
- [Monitor](https://wiki.gentoo.org/wiki/Smartmontools) of storage devices.
- [Configuration](https://wiki.gentoo.org/wiki/Hdparm) of storage devices.

## Installation

### Prerequisites

Udisks uses [D-Bus](https://wiki.gentoo.org/wiki/D-Bus) and [polkit](https://wiki.gentoo.org/wiki/Polkit), so set them up first.

### Kernel

Activate the following kernel options (depending on the slot, see "USE flags" below):

In order for udisks to work with the `lvm` USE flag, udisks will require the following LVM kernel parameters:

**linux-4.9 Enabling LVM**

### USE flags


| [+daemon](https://packages.gentoo.org/useflags/+daemon) | Build the system daemon, not just the client tool | 
| [+introspection](https://packages.gentoo.org/useflags/+introspection) | Add support for GObject based introspection | 
| [acl](https://packages.gentoo.org/useflags/acl) | Add support for Access Control Lists | 
| [debug](https://packages.gentoo.org/useflags/debug) | Enable extra debug codepaths, like asserts and extra output. If you want to get meaningful backtraces see https://wiki.gentoo.org/wiki/Project:Quality\_Assurance/Backtraces | 
| [elogind](https://packages.gentoo.org/useflags/elogind) | Enable session tracking via sys-auth/elogind | 
| [lvm](https://packages.gentoo.org/useflags/lvm) | Add support for Logical Volume Management via sys-fs/lvm2 | 
| [nls](https://packages.gentoo.org/useflags/nls) | Add Native Language Support (using gettext - GNU locale utilities) | 
| [selinux](https://packages.gentoo.org/useflags/selinux) | !!internal use only!! Security Enhanced Linux support, this must be set by the selinux profile or breakage will occur | 
| [smart](https://packages.gentoo.org/useflags/smart) | Support SMART via sys-libs/libblockdev | 
| [systemd](https://packages.gentoo.org/useflags/systemd) | Support sys-apps/systemd's logind | 

Portage knows the global USE flag `udisks` for enabling support for udisks in other packages. Enabling this USE flag will pull in [sys-fs/udisks](https://packages.gentoo.org/packages/sys-fs/udisks) automatically (default for *desktop* [profiles](<https://wiki.gentoo.org/wiki/Profile_(Portage)>)):

**`/etc/portage/make.conf`**

**Enabling udisks globally**

```
USE="udisks"
```
### Emerge

Once the global USE flag is set, upgrade system in order for udisks to be pulled in:

`root #``emerge --ask --changed-use --deep @world`
### Additional software

- [sys-apps/gnome-disk-utility](https://packages.gentoo.org/packages/sys-apps/gnome-disk-utility) - [GNOME](https://wiki.gentoo.org/wiki/GNOME) program to partition, configure and monitor storage devices.
- [sys-fs/udiskie](https://packages.gentoo.org/packages/sys-fs/udiskie) - Automatic mount of drives. (Udisks2 is automatically detected from 0.6.3 on.)

## Configuration

Udisks uses [polkit](https://wiki.gentoo.org/wiki/Polkit) to handle permissions.

### Groups

Make sure each user is the plugdev group. Here the example user [larry](https://wiki.gentoo.org/wiki/User:Larry) is used.

`foo $``groups`
wheel audio users plugdev larry

If plugdev is not among the available user groups (common users, who use a window manager instead of a desktop environment), add user to the plugdev group.

`root #``usermod -a -G plugdev foo`
### Rules

Most desktop environments will set the required rules automatically.
Less sophisticated window managers need some polkit rules as described in
[https://github.com/coldfix/udiskie/wiki/Permissions](https://github.com/coldfix/udiskie/wiki/Permissions).

Make sure that the owner is polkit:

`root #``chown polkitd:root /etc/polkit-1/rules.d/50-udiskie.rules`
There is no need to wrap the window manager around a dbus/elogind session:

**`~/.xinitrc`**

## Usage

Udisks can be controlled with the udisksctl command:

- udisksctl help - Shows a help list with general commands
- udisksctl info - Shows information about an object
- udisksctl dump - Shows information about all objects
- udisksctl status - Shows high-level status
- udisksctl monitor - Monitor changes to objects
- udisksctl mount - Mount a filesystem
- udisksctl unmount - Unmount a filesystem
- udisksctl unlock - Unlock an encrypted device
- udisksctl lock - Lock an encrypted device
- udisksctl loop-setup - Set-up a loop device
- udisksctl loop-delete - Delete a loop device
- udisksctl power-off - Safely power off a drive
- udisksctl smart-simulate - Set SMART data for a drive

A more detailed explanation of an existing udisksctl command can be achieved by using `udisksctl COMMAND --help`.

### Python scripting

The following code is an example of listening to udisk on the system D-Bus for device events. We stick with udisk1 and glib style event loops in order for the code to also be able to run on a RedHat or Centos system. Otherwise use the udisk2 monitor API to just look at mount events.

The relevant udisk documentation URL for 1.0.5 (Rhel6 is at 1.0.1) is [here](http://udisks.freedesktop.org/docs/1.0.5/Device.html)

**`udisk_listener.py`**

```
import os
import dbus
from dbus.mainloop.glib import DBusGMainLoop
import gobject
uid = None
def device_added(device):
    print("---added---")
    device_dump(device)
def device_removed(device):
    print("---removed---")
    print(str(device))
    device_dump(device)
    pass
def device_changed(device):
    print("---changed---")
    device_dump(device)
    pass
def device_dump(device):
    device_obj = system_bus.get_object("org.freedesktop.UDisks", device)
    device_props = dbus.Interface(device_obj, dbus.PROPERTIES_IFACE)
    #
    #  beware.... anything after this may or may not be defined depending on the event and state of the drive. 
    #  Attempts to get a prop that is no longer set will generate a dbus.connection:Exception
    #
    try:
        print("DeviceFile:" + device_props.Get('org.freedesktop.UDisks.Device',"DeviceFile"))
    except:
        print("DeviceFile is unset")
        
    try:
        print("NativePath: " + device_props.Get('org.freedesktop.UDisks.Device',"NativePath"))
    except:
        print("NativePath: is unset")
    
    try:
        is_mounted = device_props.Get('org.freedesktop.UDisks.Device', "DeviceIsMounted")
        if is_mounted:
            mounted_uid = device_props.Get('org.freedesktop.UDisks.Device', "DeviceMountedByUid")
            if mounted_uid == uid:
                print("mounted by me")
            else:
                print("mounted by " + str(mounted_uid))
            mountpaths =  device_props.Get('org.freedesktop.UDisks.Device', "DeviceMountPaths")
            for test in mountpaths:
                print("paths: " + test)
        else:
            print("unmounted")
    except:
        print("DeviceIsMounted is unset")
    
    try:    
        is_media_available = device_props.Get('org.freedesktop.UDisks.Device', "DeviceIsMediaAvailable")
        if is_media_available:
            print("media available")
        else:
            print("media not available")
    except:
        print("DeviceIsMediaAvailable is not set")
    try:        
        is_partition_table = device_props.Get('org.freedesktop.UDisks.Device', "DeviceIsPartitionTable")
        if is_partition_table:
            print("device is partition table")
    except:
        print("DeviceIsPartitionTable is not set")
    try:        
        is_partition = device_props.Get('org.freedesktop.UDisks.Device', "DeviceIsPartition")
        if is_partition:
            print("device is partition")
    except:
        print("DeviceIsPartition is not set")
        
    try:
        is_removeable = device_props.Get('org.freedesktop.UDisks.Device', "DeviceIsRemovable")
        if is_removeable:
            print("device is removable")
        else:
            print("device is not removable")
    except:
        print("DeviceIsRemovable is not set")
if __name__ == '__main__':
    uid = os.getuid()
    DBusGMainLoop(set_as_default=True)
    system_bus = dbus.SystemBus()
    udisk_proxy = system_bus.get_object("org.freedesktop.UDisks", "/org/freedesktop/UDisks")
    udisk_iface = dbus.Interface(udisk_proxy, "org.freedesktop.UDisks")
    udisk_iface.connect_to_signal('DeviceAdded', device_added)
    udisk_iface.connect_to_signal('DeviceRemoved', device_removed)
    udisk_iface.connect_to_signal('DeviceChanged', device_changed)
    
    loop = gobject.MainLoop()
    loop.run()
```
#### USB drive example

The following example is from a Gentoo system when an 8GB USB drive containing Kubuntu 14.04 installer is plugged into a USB port. It was created by dd'ing the ISO file directly to the thumb drive. fdisk shows that the 1gb or so of good stuff ended up in /dev/sdf2 while /dev/sdf1 is the free space left over. Note that the first event is the "whole drive" or partition table getting looked at.

This happens when the device notifier pops up and we have KDE open the drive in the dolphin file manager.

All this happens when we do a "safely remove" in dolphin. Notice that some of the properties are already unset and we are catching exceptions in device\_dump during examination

Then we get one last removal signal when we yank the thumb drive

#### CD/DVD media example

The use of a DVD or CDROM in an internal drive will cause only changes events to signal. Here's an example for a mount followed by an unmount of a Gentoo autobuild iso in a Centos6 VM's virtual CDROM. Notice that there are two change signals per action. The insertion shows the first event with media available and then a second event on the mount where the DeviceMountedByUid value properly matched up with our UID. Presumably the two for the removal are the unmount followed by an eject, but media shows as not being available on both so the two are indistinguishable.

\-

## Troubleshooting

- [Gentoo bugtracker: known bugs](https://bugs.gentoo.org/buglist.cgi?quicksearch=udisks&order=bug_id%20DESC)
- [Freedesktop.org bugtracker: known bugs](https://bugs.freedesktop.org/buglist.cgi?bug_status=UNCONFIRMED&bug_status=NEW&bug_status=CONFIRMED&bug_status=ASSIGNED&bug_status=REOPENED&bug_status=NEEDINFO&bug_status=PLEASETEST&bug_status=IN_PROGRESS&product=udisks&component=&order=bug_id%20DESC)
- udisks communicates over D-Bus, so also see the [D-Bus "Troubleshooting" section](https://wiki.gentoo.org/wiki/D-Bus#Troubleshooting).

## See also

- [D-Bus](https://wiki.gentoo.org/wiki/D-Bus) — an interprocess communication (IPC) system for software applications.
