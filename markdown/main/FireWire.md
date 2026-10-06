<!-- source: https://wiki.gentoo.org/wiki/FireWire | group: Gentoo Wiki (Main) | wiki-title: FireWire -->
---
title: FireWire
url: https://wiki.gentoo.org/wiki/FireWire
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2018-12-08"
fingerprint: "9f878958838299c5"
license: CC BY-SA 4.0
---

# FireWire

From Gentoo Wiki

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**Resources**

This article describes the setup of FireWire (IEEE 1394, i.Link) controllers.

## Installation

### Kernel configuration

For FireWire support, the following kernel options need to be activated:

KERNEL **Add FireWire driver support**

```
Device Drivers --->
    IEEE 1394 (FireWire) support  --->
        <*> FireWire driver stack
        <*>   OHCI-1394 controllers
```
Select additional drivers as needed. For example, a FireWire hard drive may need the following options enabled:

KERNEL **Add additional driver support**

```
Device Drivers --->
    SCSI device support --->
        <*> SCSI device support
        <*> SCSI disk support
    IEEE 1394 (FireWire) support  --->
        <*>   Storage devices (SBP-2 protocol)
```
### USE flags

Portage knows the global USE flag `ieee1394` for enabling support for FireWire in other packages. Enabling this USE flag will pull in [sys-libs/libraw1394](https://packages.gentoo.org/packages/sys-libs/libraw1394) automatically:

FILE **`/etc/portage/make.conf`**

```
USE="... ieee1394 ..."
```

### USE flags for
            [sys-libs/libraw1394](https://packages.gentoo.org/packages/sys-libs/libraw1394)
            
            Library that provides direct access to the IEEE 1394 bus

| [valgrind](https://packages.gentoo.org/useflags/valgrind) | Enable annotations for accuracy. May slow down runtime slightly. Safe to use even if not currently using dev-debug/valgrind | 

### Emerge

After setting the USE flag above update the system so the changes take effect:

`root #``emerge --ask --changed-use --deep @world`
