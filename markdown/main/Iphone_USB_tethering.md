<!-- source: https://wiki.gentoo.org/wiki/Iphone_USB_tethering | group: Gentoo Wiki (Main) | wiki-title: Iphone USB tethering -->
---
title: Iphone USB tethering
url: https://wiki.gentoo.org/wiki/Iphone_USB_tethering
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-07-22"
fingerprint: "2200e957db2c39c8"
license: CC BY-SA 4.0
---

# Iphone USB tethering

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

This page is inspired by [Android USB tethering](https://wiki.gentoo.org/wiki/Android_USB_tethering). It only documents the differences.

## Tested devices

- iPhone 4S, iOS5
- iPhone 5, iOS6
- iPhone 5S, iOS9
- iPhone 7, iOS 14.7.1
- iPhone 11 Pro Max, iOS 18.5
- iPhone 15 Pro, iOS 18.1.1

## Kernel

**Linux 4.4**

## Tools needed

You will also need [app-pda/usbmuxd](https://packages.gentoo.org/packages/app-pda/usbmuxd) and [app-pda/ifuse](https://packages.gentoo.org/packages/app-pda/ifuse) from the default portage tree.

`root #``emerge --ask app-pda/usbmuxd app-pda/ifuse`
## ipheth interface

If everything is installed successfully, plug in the iPhone with USB cable. You should see something like:

A new network interface **eth1** plugged by ipheth can be found, after running DHCP on it:

`root #``ip a show eth1````
4: eth1: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast state UNKNOWN qlen 1000
    link/ether 9e:20:7b:6b:94:bd brd ff:ff:ff:ff:ff:ff
    inet 172.20.10.2/28 brd 172.20.10.15 scope global eth1
    inet6 fe80::9c20:7bff:fe6b:94bd/64 scope link 
       valid_lft forever preferred_lft forever
```

If no output is observed from dmesg, try mounting the iphone via ifuse:

`root #``ifuse /mnt/`
The device must be unlocked and incoming connections must be allowed while ifuse is mounting. Ifuse will guide provide guidance though the process; follow its instructions and try mounting until it succeeds.

## udev trigger

The [hotplug feature of OpenRC](https://wiki.gentoo.org/wiki/OpenRC/Event_driven) can be used to set up the ipheth interface automatically.

[net-misc/netifrc](https://packages.gentoo.org/packages/net-misc/netifrc) ships /lib/udev/net.sh, which can be used to hotplug network interfaces. The main part of the script is

**`/lib/udev/net.sh`**

**net interface hotplug**

```
IFACE=$1
ACTION=$2
...
SCRIPT=/etc/init.d/net.$IFACE
...
IN_HOTPLUG=1 "${SCRIPT}" --quiet "${ACTION}"
```
and it can be called with a [udev rule](https://wiki.gentoo.org/wiki/Udev):

**`/lib/udev/rules.d/90-iphone-tether.rules`**

**net.sh trigger**

```
# udev rules for setting correct configuration and pairing on tethered iPhones
ATTR{idVendor}!="05ac", GOTO="ipheth_rules_end"
# Execute pairing program when appropriate
ACTION=="add", SUBSYSTEM=="net", ENV{ID_USB_DRIVER}=="ipheth", SYMLINK+="iphone", RUN+="ipheth-pair", RUN+="net.sh eth1 start"
SUBSYSTEM=="net", ACTION=="remove", ENV{ID_USB_DRIVER}=="ipheth", SYMLINK+="iphone", RUN+="net.sh %k stop"
LABEL="ipheth_rules_end"
```
Enable hotplug to eth1, the ipheth device:

**`/etc/rc.conf`**

**rc\_hotplug**

```
# rc_hotplug is a list of services that we allow to be hotplugged.
# By default we do not allow hotplugging.
# A hotplugged service is one started by a dynamic dev manager when a matching
# hardware device is found.
# This service is intrinsically included in the boot runlevel.
# To disable services, prefix with a !
# Example - rc_hotplug="net.wlan !net.*"
# This allows net.wlan and any service not matching net.* to be plugged.
# Example - rc_hotplug="*"
# This allows all services to be hotplugged
rc_hotplug="net.eth1"
```
add net.eth1:

`root #``ln -s /etc/init.d/net.{lo,eth1}` After plugging in the iPhone, we can see the service started:

`root #``rc-status`
...
Dynamic Runlevel: hotplugged
 net.eth1                        \[  started  \]
...
