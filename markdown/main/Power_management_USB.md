<!-- source: https://wiki.gentoo.org/wiki/Power_management/USB | group: Gentoo Wiki (Main) | wiki-title: Power management/USB -->
---
title: Power management/USB
url: https://wiki.gentoo.org/wiki/Power_management/USB
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-01-24"
fingerprint: fc663853d3b1b3a8
license: CC BY-SA 4.0
---

# Power management/USB

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

This article describes the Linux's ability to power off USB devices and to let USB devices request to wake them up again.

It is important to note many optical mice do not support power saving. Once they lose power they cannot detect motion and cannot power back on when motion is invoked. With this being stated, it is possible to use specific driver calls or the sysfs /sys/bus/usb/devices/usb\*/power/control file to enable or disable auto-suspend for individual USB peripherals.

## Configuration

### Kernel

Device Drivers --->
  \[\*\] USB support ---> [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_USB\_SUPPORT\</code> to find this item.
    \<M> Support for Host-side USB [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_USB\</code> to find this item.
    (2) Default autosuspend delay [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_USB\_AUTOSUSPEND\_DELAY\</code> to find this item.

### Set Default USB Autosuspend Delay Value

#### Via Kernel

The default USB autosuspend delay value can be set in the kernel at compile time. The kernel `CONFIG_` option, `CONFIG_USB_AUTOSUSPEND_DELAY`, can be set to `2`.

#### Via Command Line

The default USB autosuspend delay value can be set in the kernel command line. This implementation uses GRUB as an example:

Add `usbcore.autosuspend=2` to the `GRUB_CMDLINE_LINUX_DEFAULT` variable inside the config file found in /etc/default/grub. Regenerate the grub.cfg config via the grub2-mkconfig command and reboot in order for the changes to take effect.

`root #``grub2-mkconfig -o /path/to/grub.cfg`
#### Via Modprobe

The default USB autosuspend delay value can be set as an option when the USB module loads.

**`/etc/modprobe.d/usb.conf`**

### Udev

Make the following [udev](https://wiki.gentoo.org/wiki/Udev) rule file to automate power management:

**`/etc/udev/rules.d/10-my-usb-power.rules`**

## Usage

### Set autosuspend

We can set an autosuspend time for a specific USB device using a local.d service. There are many ways to do this besides using a local.d service, but the method is the same: write a value to the file, /sys/bus/usb/devices/\*-\*/power/control. The value can be a number (representing the number of seconds to delay the suspension) or "`auto`" (to use the default value set in the kernel).

Run lsusb to list all USB devices with their bus and device numbers:

`root #``lsusb`
Bus 007 Device 001: ID 1d6b:0001 Linux Foundation 1.1 root hub
Bus 002 Device 001: ID 1d6b:0002 Linux Foundation 2.0 root hub
Bus 006 Device 001: ID 1d6b:0001 Linux Foundation 1.1 root hub
Bus 005 Device 001: ID 1d6b:0001 Linux Foundation 1.1 root hub
Bus 001 Device 001: ID 1d6b:0002 Linux Foundation 2.0 root hub
Bus 004 Device 002: ID 05af:1012 Jing-Mold Enterprise Co., Ltd 
Bus 004 Device 001: ID 1d6b:0001 Linux Foundation 1.1 root hub
Bus 003 Device 002: ID 09da:9090 A4Tech Co., Ltd. XL-730K / XL-750BK / XL-755BK Mice
Bus 003 Device 001: ID 1d6b:0001 Linux Foundation 1.1 root hub

As an example, we want a USB device (such as the **A4Tech** mouse listed above) to autosuspend after being in the idle state for 45 minutes.

Don't forget to make the local.d file executable:

`root #``chmod +x /etc/local.d/mouse_auto_sleep.start`
If we reboot, the **A4Tech** mouse should autosuspend after being in the idle state for 45 minutes.

Read /usr/src/linux/Documentation/usb/power-management.txt for more information on power management in Linux.

## See also

- [USB](https://wiki.gentoo.org/wiki/USB) — the setup of **USB** (Universal Serial Bus) controllers

## External resources

- [https://github.com/mvp/uhubctl](https://github.com/mvp/uhubctl) - USB hub per-port power control
