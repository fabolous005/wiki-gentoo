<!-- source: https://wiki.gentoo.org/wiki/Sysfs/network_devices | group: Gentoo Wiki (Main) | wiki-title: Sysfs/network devices -->
---
title: Sysfs/network devices
url: https://wiki.gentoo.org/wiki/Sysfs/network_devices
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2020-07-03"
fingerprint: "5fcbab8c981339f8"
license: CC BY-SA 4.0
---

# Sysfs/network devices

From Gentoo Wiki

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

Get the device name by listing the /sys/class/net directory contents using ls -al or the tree command (provided by the [app-text/tree](https://packages.gentoo.org/packages/app-text/tree) package):

`user $``tree /sys/class/net`
/sys/class/net/
├── enp2s14 -> ../../devices/pci0000:00/0000:00:1e.0/0000:02:0e.0/net/enp2s14
├── lo -> ../../devices/virtual/net/lo
├── sit0 -> ../../devices/virtual/net/sit0
└── wlp8s0 -> ../../devices/pci0000:00/0000:00:1c.0/0000:08:00.0/net/wlp8s0
