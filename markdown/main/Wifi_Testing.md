<!-- source: https://wiki.gentoo.org/wiki/Wifi/Testing | group: Gentoo Wiki (Main) | wiki-title: Wifi/Testing -->
---
title: Wifi/Testing
url: https://wiki.gentoo.org/wiki/Wifi/Testing
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2025-10-01"
fingerprint: "35cbfc59a0139bb1"
license: CC BY-SA 4.0
---

# Wifi/Testing

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

## Testing

After a reboot with the new kernel or after loading the modules, the device can be checked for availability by using following methods:

- Using the [/sys file system](https://wiki.gentoo.org/wiki/Wifi/Testing#.2Fsys_file_system)
- Using the [ip command](https://wiki.gentoo.org/wiki/Wifi/Testing#ip_command)
- Using the [ifconfig command](https://wiki.gentoo.org/wiki/Wifi/Testing#ifconfig_command)
- Using the [iw command](https://wiki.gentoo.org/wiki/Wifi/Testing#iw_command)

Get the device name by listing the /sys/class/net directory contents using ls -al or the tree command (provided by the [app-text/tree](https://packages.gentoo.org/packages/app-text/tree) package):

`user $``tree /sys/class/net`
/sys/class/net/
├── enp2s14 -> ../../devices/pci0000:00/0000:00:1e.0/0000:02:0e.0/net/enp2s14
├── lo -> ../../devices/virtual/net/lo
├── sit0 -> ../../devices/virtual/net/sit0
└── wlp8s0 -> ../../devices/pci0000:00/0000:00:1c.0/0000:08:00.0/net/wlp8s0

To obtain the device name and verify that the wireless card is detected, execute the following [ip command](https://wiki.gentoo.org/wiki/Iproute2):

`user $``ip addr`
3: wlan0:   ...

A network card can be activated as follows:

`root #``ip link set wlan0 up`
The ifconfig command is provided through the [sys-apps/net-tools](https://packages.gentoo.org/packages/sys-apps/net-tools) package. Use ifconfig -a to list all detected network cards, even those that are not enabled/active yet:

`user $``ifconfig -a`
wlan0     ...

A network card can be activated as follows:

`root #``ifconfig -v wlan0 up`
SIOCSIFFLAGS: Operation not possible due to RF-kill
WARNING: at least one error occurred. (-1)

In this example, enabling the wireless card failed as a radio frequency kill state is set (usually to reduce power consumption and not connect by accident to a wireless network).

If the wireless network card driver supports the nl80211 stack, then the iw command as offered by the [net-wireless/iw](https://packages.gentoo.org/packages/net-wireless/iw) package can show the detected wireless cards:

`root #``iw dev`
phy#0
	Interface wlan0
		ifindex 4
		type managed
