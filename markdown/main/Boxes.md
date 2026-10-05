<!-- source: https://wiki.gentoo.org/wiki/Boxes | group: Gentoo Wiki (Main) | wiki-title: Boxes -->
---
title: Boxes
url: https://wiki.gentoo.org/wiki/Boxes
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-04-24"
fingerprint: a23c52f8efa3c374
license: CC BY-SA 4.0
---

# Boxes

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**Boxes** is an application from the [GNOME](https://wiki.gentoo.org/wiki/GNOME) project that allows creating and accessing [virtual machines](https://wiki.gentoo.org/wiki/Virtualization), running locally or remotely. It also allows to connect to the display of a remote computer.

Boxes leverages [QEMU](https://wiki.gentoo.org/wiki/QEMU) with  [KVM](https://wiki.gentoo.org/wiki/KVM), [libvirt](https://wiki.gentoo.org/wiki/Libvirt), [SPICE](https://wiki.gentoo.org/wiki/Remote_desktop#SPICE), and [VNC](https://wiki.gentoo.org/wiki/Remote_desktop#VNC_.28RFB_protocol.29).

## Installation

### USE flags


### Emerge

To use the file sharing feature of Boxes, [net-misc/spice-gtk](https://packages.gentoo.org/packages/net-misc/spice-gtk) must be emerged with the webdav USE flag enabled:

**`/etc/portage/package.use/gnome-boxes`**

Install gnome-boxes:

`root #``emerge --ask gnome-extra/gnome-boxes`
## Configuration

### User permissions

To create and run virtual machines using Boxes as a normal user, the user permissions and service for [libvirt](https://wiki.gentoo.org/wiki/Libvirt) must be set up [as shown in this article](https://wiki.gentoo.org/wiki/Libvirt#User_permissions).

Additionally, the user must be added to the kvm group:

`root #``usermod -a -G kvm <user>`
## See also

- [Gnome](https://wiki.gentoo.org/wiki/Gnome) — a feature-rich desktop environment provided by the [GNOME project](https://www.gnome.org).
- [Virtualization](https://wiki.gentoo.org/wiki/Virtualization) — the concept and technique that permits running software in an environment separate from a computer operating system.
