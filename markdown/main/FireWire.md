<!-- source: https://wiki.gentoo.org/wiki/FireWire | group: Gentoo Wiki (Main) | wiki-title: FireWire -->
---
title: FireWire
url: https://wiki.gentoo.org/wiki/FireWire
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2018-12-08"
fingerprint: "9e95a958a3829986"
license: CC BY-SA 4.0
---

# FireWire

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

This article describes the setup of FireWire (IEEE 1394, i.Link) controllers.

## Installation

### Kernel configuration

For FireWire support, the following kernel options need to be activated:

**Add FireWire driver support**

Select additional drivers as needed. For example, a FireWire hard drive may need the following options enabled:

**Add additional driver support**

### USE flags

Portage knows the global USE flag `ieee1394` for enabling support for FireWire in other packages. Enabling this USE flag will pull in [sys-libs/libraw1394](https://packages.gentoo.org/packages/sys-libs/libraw1394) automatically:

**`/etc/portage/make.conf`**

```
USE="... ieee1394 ..."
```

### Emerge

After setting the USE flag above update the system so the changes take effect:

`root #``emerge --ask --changed-use --deep @world`
