<!-- source: https://wiki.gentoo.org/wiki/OpenCT | group: Gentoo Wiki (Main) | wiki-title: OpenCT -->
---
title: OpenCT
url: https://wiki.gentoo.org/wiki/OpenCT
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2019-06-24"
fingerprint: a610d95aad87faec
license: CC BY-SA 4.0
---

# OpenCT

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**CTAPI** is a German standard for PC to smartcard reader communication, which is implemented by OpenCT. The international standard is in contrast [PC/SC](https://wiki.gentoo.org/wiki/PCSC-Lite).

## Installation

### Kernel

You have to enable kernel support depending on how your cardreader is connected:

- For USB cardreader see the [USB](https://wiki.gentoo.org/wiki/USB) article.
- For PC-Card cardreader see the [PC-Card](https://wiki.gentoo.org/wiki/PC-Card) article.
- For serial cardreader enable serial support.

### USE flags

Portage knows the global `openct` USE flag for enabling support for OpenCT in other packages. Enabling this USE flag will pull in [dev-libs/openct](https://packages.gentoo.org/packages/dev-libs/openct) automatically:

**`/etc/portage/make.conf`**

Other USE flags of openct include:


| [debug](https://packages.gentoo.org/useflags/debug) | Add debug output to the driver library for pcsc-lite. | 
| [doc](https://packages.gentoo.org/useflags/doc) | Add extra documentation (API, Javadoc, etc). It is recommended to enable per package instead of globally | 
| [pcsc-lite](https://packages.gentoo.org/useflags/pcsc-lite) | Build a driver library for sys-apps/pcsc-lite, providing PC/SC API access to devices supported by OpenCT. | 
| [selinux](https://packages.gentoo.org/useflags/selinux) | !!internal use only!! Security Enhanced Linux support, this must be set by the selinux profile or breakage will occur | 
| [usb](https://packages.gentoo.org/useflags/usb) | Add USB support to applications that have optional USB support (e.g. cups) | 

### Emerge

After setting this you want to update your system so the changes take effect:

`root #``emerge --ask --changed-use --deep @world`
## Configuration

### Permissions

Add your user to the *openct* group to be able to access the cardreader:

`root #``gpasswd -a larry openct`
### Services

#### OpenRC

To tart OpenCT:

`root #``/etc/init.d/openct start`
To start OpenCT at boot time, add it the default runlevel:

`root #``rc-update add openct default`
## Usage

### Listing readers

List all detected cardreaders:

`user $``openct-tool list`
If there is a detected cardreader, insert a smartcard. Test the access by checking the [ATR](https://en.wikipedia.org/wiki/Answer_to_reset):

`user $``openct-tool atr`
Detected CCID Compatible
Card present, status changed
ATR: 3B 75 94 00 00 62 02 02 03 01
