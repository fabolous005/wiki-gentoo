<!-- source: https://wiki.gentoo.org/wiki/USB_to_serial_adapter | group: Gentoo Wiki (Main) | wiki-title: USB to serial adapter -->
---
title: USB to serial adapter
url: https://wiki.gentoo.org/wiki/USB_to_serial_adapter
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2022-02-20"
fingerprint: "5f1b2016d99339c4"
license: CC BY-SA 4.0
---

# USB to serial adapter

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

This article details the process of enabling support for USB to serial (RS-232, RS-422, RS-485 etc) adapters in the Linux kernel.

Most modern, user facing hardware devices do not come with serial ports accessible from the exterior casing of the device. This becomes an issue when attempting to configure or troubleshoot certain devices that implement a console port with serial communication for data input/output. Most modern hardware devices *do* include at least one USB port that is accessible from the device casing.

The Linux kernel has support for USB to serial adapters. These devices can be also known as USB serial converters. Support can be enabled in the kernel, udev will configure the device, and an engineer can obtain success communicating with serial input/output via the console port.

## Installation

### Kernel

Enable support for USB to serial by selecting the following symbols in the [kernel's configuration](https://wiki.gentoo.org/wiki/Kernel/Configuration): `CONFIG_USB_SERIAL`, `CONFIG_USB_SERIAL_CONSOLE`, `CONFIG_USB_SERIAL_GENERIC`, and `CONFIG_USB_SERIAL_SIMPLE`:

**USB to serial support**

From this point, specific drivers for each hardware device can be enabled as necessary. To reduce kernel size it is wise to build support for additional adapters as modules.

After the above options have been enabled do not forget to recompile the kernel, update the bootloader, and re-execute the new kernel. Instructions for how to do these steps can be found in related articles.

### Additional software

Useful software for accessing the serial interface:



## See also

- [Kernel](https://wiki.gentoo.org/wiki/Kernel) — a central part of the Gentoo [operating system (OS)](https://en.wikipedia.org/wiki/operating_system)
- [Kernel/Configuration](https://wiki.gentoo.org/wiki/Kernel/Configuration) — describes the manual configuration and setup of the [Linux kernel](https://wiki.gentoo.org/wiki/Kernel).
