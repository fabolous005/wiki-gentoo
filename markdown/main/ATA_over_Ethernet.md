<!-- source: https://wiki.gentoo.org/wiki/ATA_over_Ethernet | group: Gentoo Wiki (Main) | wiki-title: ATA over Ethernet -->
---
title: ATA over Ethernet
url: https://wiki.gentoo.org/wiki/ATA_over_Ethernet
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2022-04-25"
fingerprint: afe1ba494dbcff89
license: CC BY-SA 4.0
---

# ATA over Ethernet

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

ATA over Ethernet (AoE) is a network protocol developed by the Brantley Coile Company, designed for simple, high-performance access of block storage devices over Ethernet networks. It is used to build storage area networks (SANs) with low-cost, standard technologies.

AoE runs on layer 2 Ethernet. AoE does not use layer 3 IPv4 or IPv6; it cannot be accessed over the Internet or other IP networks. In this regard it is more comparable to FCoE (Fibre Channel over Ethernet) than iSCSI.

## Installation

### Kernel

**Enable`CONFIG_ATA_OVER_ETH` in the kernel**

```
Device Drivers  --->
  [*] Block devices  --->
    <M> ATA over Ethernet support
```
### Server

Install the vblade package:

`root #``emerge -av sys-block/vblade`
Create a 100GB sparse file which will be the AoE target:

`root #``dd if=/dev/zero bs=1MiB count=0 seek=100000 of=/mnt/storage01/aoe-target-0-0.bin`
Edit the vblade config file:

**`/etc/conf.d/vblade`**

**Configure an AoE target**

```
# If you intent to run only one vblade, you should edit config_vblade0
# SYNTAX: SHELF SLOT NETIF SOURCE
config_vblade0="0 0 eth0 /mnt/storage01/aoe-target-0-0.bin"
# SHELF is a numeric value >= 0
# SLOT is a numeric value 0 <= X <= 15
# NETIF is a network interface name
# SOURCE is a file or block device
# For additional vblades, run:
# ln -s /etc/init.d/vblade.vblade0 /etc/init.d/vblade.$NAME
# and define config_$NAME in this file.
# Note that the combination of SHELF:SLOT:NETIF should be unique for your
# network.
# Some additional examples
# config_vblade1="0 1 eth0 /root/test2.img"
# config_foobar="0 1 eth1 /dev/md0"
```
Start the vblade service:

`root #``/etc/init.d/vblade.vblade0 start`
### Client

Install the aoetools package:

`root #``emerge -av sys-block/aoetools`
Use aoe-stat to view the aoe target and the above example should be located at /dev/etherd/e0.0

## See also

- [ISCSI](https://wiki.gentoo.org/wiki/ISCSI) — an IP-based network standard and a [Storage Area Network](https://en.wikipedia.org/wiki/Storage_area_network) (SAN) protocol.
