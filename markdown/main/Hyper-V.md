<!-- source: https://wiki.gentoo.org/wiki/Hyper-V | group: Gentoo Wiki (Main) | wiki-title: Hyper-V -->
---
title: Hyper-V
url: https://wiki.gentoo.org/wiki/Hyper-V
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2024-12-20"
fingerprint: "7e138954d1823b86"
license: CC BY-SA 4.0
---

# Hyper-V

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

Hyper-V is a [hypervisor](https://en.wikipedia.org/wiki/Hypervisor) integrated in current versions of Microsoft Windows. This article covers the specifics of *running Gentoo as a* **guest***operating system* inside a Hyper-V [virtual machine](https://en.wikipedia.org/wiki/virtual_machine).

## Installation

Hyper-V support for Gentoo guests requires two important steps: kernel support and user-space graphic driver support.

### Kernel

#### Linux guest support

Below is a summary of the kernel features that need to be compiled into the kernel, or provided as kernel modules, to be able to correctly run Gentoo under Hyper-V. Feature names are subject to change, so be sure to search the kernel's menuconfig for features containing the string `HYPERV`.

**Enable basic Hyper-V guest support**

To have all necessary options appear, there is an initial dependency chain. "Linux guest support" and "ACPI" must be enabled first in order for "Microsoft Hyper-V client drivers" to appear. "Microsoft Hyper-V client drivers" is necessary for most, if not all, other Hyper-V options to be available.

#### Graphics

For X11 (graphical) support the `CONFIG_DRM_FBDEV_EMULATION` kernel option is required:

**Enable graphical support via fbdev**

### Emerge

If X server graphical support is desired through fbdev, be sure to adjust /etc/portage/package.use:

**`/etc/portage/package.use/00video`**

```
 VIDEO_CARDS: -* fbdev
```
Next (re)emerge xorg-drivers package:

`root #``emerge --ask --update --newuse --deep x11-base/xorg-drivers`
### Integration Services

Sources for the integration services can be found in the kernel source tree. Unfortunatly there is no support from Gentoo for the integration services at time of writing, so manual setup is required:

`root #````
cd /usr/src/linux/tools/hv/
```
`root #``make install`
Next adjust the helpers to fit your system. For documentation, see the comments inside the files.

`root #````
vim /usr/libexec/hypervkvpd/hv_get_dhcp_info
```
`root #````
vim /usr/libexec/hypervkvpd/hv_get_dns_info
```
`root #````
vim /usr/libexec/hypervkvpd/hv_set_ifconfig
```
Finally, make sure the three daemons are started on boot. Again, you will have to write the services yourself. This should however be pretty straightforward, as they do not require any configuration.

## Removal

Removing the Hyper-V support is as simple as disabling the related kernel options (reverse the steps in the [Kernel section](https://wiki.gentoo.org#Kernel) above) and removing the files installed in the [Integration Services section](https://wiki.gentoo.org#Integration_Services).

## See also

- [Virtualization](https://wiki.gentoo.org/wiki/Virtualization) — the concept and technique that permits running software in an environment separate from a computer operating system.
- [Xen](https://wiki.gentoo.org/wiki/Xen) — a native, or bare-metal, [hypervisor](https://en.wikipedia.org/wiki/Hypervisor) that allows multiple distinct virtual machines (referred to as domains) to share a single physical machine.
