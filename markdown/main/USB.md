<!-- source: https://wiki.gentoo.org/wiki/USB | group: Gentoo Wiki (Main) | wiki-title: USB -->
---
title: USB
url: https://wiki.gentoo.org/wiki/USB
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-02-03"
fingerprint: a600527ebcbab9ab
license: CC BY-SA 4.0
---

# USB

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

This article describes the setup of **USB** (Universal Serial Bus) controllers.

## Installation

### Hardware detection

To choose the right driver, first detect the used USB controllers. The [lspci](https://wiki.gentoo.org/wiki/Hardware_detection) utility works nicely for this task:

`root #``lspci | grep --color -i usb`
### Kernel

See the kernel section of the [USB guide](https://wiki.gentoo.org/wiki/USB/Guide#Config_options_for_the_kernel) and the USB host controllers section of the [Kernel configuration guide](https://wiki.gentoo.org/wiki/Kernel/Gentoo_Kernel_Configuration_Guide#USB_host_controllers).

### Portage

Portage knows the [`usb` USE flag](https://packages.gentoo.org/useflags/usb). Some packages include or exclude support for USB based on this flag. As with all [USE flags](https://wiki.gentoo.org/wiki/USE_flag), can be set as a value of the `USE` variable in [/etc/portage/make.conf](https://wiki.gentoo.org/wiki//etc/portage/make.conf#USE) or in [/etc/portage/package.use](https://wiki.gentoo.org/wiki//etc/portage/package.use).

### Emerge

Install the [sys-apps/usbutils](https://packages.gentoo.org/packages/sys-apps/usbutils) package, if it is not already installed by adding the [`usb` USE flag](https://packages.gentoo.org/useflags/usb) and re-running emerge with `--changed-use`:

`root #``emerge --ask sys-apps/usbutils`
## External resources

- [Universal Serial Bus Device Class Definition for Audio Devices](https://www.usb.org/sites/default/files/Audio2_with_Errata_and_ECN_through_Apr_2_2025.pdf) \[PDF, 2025-04-02\]
