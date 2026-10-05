<!-- source: https://wiki.gentoo.org/wiki/Trust_Flex_tablet | group: Gentoo Wiki (Main) | wiki-title: Trust Flex tablet -->
---
title: Trust Flex tablet
url: https://wiki.gentoo.org/wiki/Trust_Flex_tablet
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2023-08-16"
fingerprint: "8589677f0f3e2d9b"
license: CC BY-SA 4.0
---

# Trust Flex tablet

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

This is a guide on how to configure and install the Trust Flex graphics tablet using the Wizardpen driver provided by a portage overlay. This guide may also be modified to work with other non-Wacom graphics tablets compatible with the Wizardpen driver however this article is only about the Trust Flex graphics tablet.

Some of these steps may not be necessary however they are all of the steps which the original author of this article took in order to get the tablet working.

## Kernel options

In order for the Trust Flex graphics tablet to work, the following options specific to the Trust Flex tablet need to be selected during the kernel configuration:

**Example output after searching for WALTOP**

After this the kernel must be recompiled and booted into.

## Modifying xorg.conf

Next it is important to add a section in /etc/X11/xorg.conf that defines the tablet as an input device. This can be accomplished by editing or creating (if it does not exist yet) /etc/X11/xorg.conf to contain the following:

**`/etc/X11/xorg.conf`**

```
Section "InputDevice"
   Identifier  "tablet"
   Driver  "wizardpen"
   Option  "Device"  "/dev/tablet"
EndSection
```
## Adding udev rules

As udev is the device manager and handles all user space actions when adding or removing devices, udev rules for the tablet must be created so that it functions properly. The following file must be created and edited as shown:

**`/etc/udev/rules.d/67-xorg-wizardpen.rules`**



## Compiling the Wizardpen driver

The Wizardpen driver necessary for the functioning of this and other Wizardpen compatible tablets will be installed from the *wavilen* portage overlay. After installing and configuring eselect repository, the overlay can be installed using the following commands:

`root #``eselect repository enable wavilen`
Then sync the repos:

`root #``emerge --sync`
and finally compile the wizardpen driver itself:

`root #``emerge -av wizardpen`
Finally reboot and plug in your Trust Flex graphics tablet. The tablet should now be functioning in software that supports it such as GIMP and mypaint.
