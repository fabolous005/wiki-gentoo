<!-- source: https://wiki.gentoo.org/wiki/ISCSI/Target | group: Gentoo Wiki (Main) | wiki-title: ISCSI/Target -->
---
title: ISCSI/Target
url: https://wiki.gentoo.org/wiki/ISCSI/Target
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-03-09"
fingerprint: f7a1147399167beb
license: CC BY-SA 4.0
---

# ISCSI/Target

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

An iSCSI Targets machines that offer storage via iSCSI to a network. An [Initiator](https://wiki.gentoo.org/wiki/ISCSI/Initiator) connects to a target to use the storage on the target.

## Installation

### Kernel

### Emerge

The configuration tool is called targetcli and is included in two packages: [sys-block/targetcli](https://packages.gentoo.org/packages/sys-block/targetcli) or [sys-block/targetcli-fb](https://packages.gentoo.org/packages/sys-block/targetcli-fb) (free-branch). The free-branch version is more up-to-date, so this page assumes that one.

`root #``emerge --ask sys-block/targetcli-fb`
The targetcli command from [sys-block/targetcli-fb](https://packages.gentoo.org/packages/sys-block/targetcli-fb) package requires dbus to be running, so on OpenRC systems ensure it is added to the default run level and started:

`root #````
rc-update add dbus default
```
`root #````
rc-service dbus start
```
