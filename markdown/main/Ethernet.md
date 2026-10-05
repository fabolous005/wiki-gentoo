<!-- source: https://wiki.gentoo.org/wiki/Ethernet | group: Gentoo Wiki (Main) | wiki-title: Ethernet -->
---
title: Ethernet
url: https://wiki.gentoo.org/wiki/Ethernet
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-06-22"
fingerprint: dc126a5d0aeebbd3
license: CC BY-SA 4.0
---

# Ethernet

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

This article describes the setup of an Ethernet network device.

## Installation

### Hardware detection

To choose the right driver, first detect the used Ethernet controllers. Use [lspci](https://wiki.gentoo.org/wiki/Hardware_detection) for this task:

`root #``lspci | grep --color Ethernet`
### Kernel

The next step is to activate the kernel options appropriate to the Ethernet hardware in the machine. Open up a second terminal window in order to reference the output generated in the step above. The option to enable Ethernet drivers can be found in the following location:

**Enable (`CONFIG_ETHERNET`)**

Scan the drivers carefully. Compare the results of the lspci command above with the available drivers in the list. Enable the feature(s) that match the installed Ethernet device(s).

## Configuration

### Physical interface settings

List **eth0** physical interface settings using [sys-apps/ethtool](https://packages.gentoo.org/packages/sys-apps/ethtool):

`root #``ethtool eth0````
Settings for eth0:
        Supported ports: [ TP ]
        Supported link modes:   10baseT/Half 10baseT/Full
                                100baseT/Half 100baseT/Full
                                1000baseT/Full
        Supported pause frame use: No
        Supports auto-negotiation: Yes
        Advertised link modes:  10baseT/Half 10baseT/Full
                                100baseT/Half 100baseT/Full
                                1000baseT/Full
        Advertised pause frame use: No
        Advertised auto-negotiation: Yes
        Speed: 1000Mb/s
        Duplex: Full
        Port: Twisted Pair
        PHYAD: 0
        Transceiver: internal
        Auto-negotiation: on
        MDI-X: off (auto)
        Supports Wake-on: umbg
        Wake-on: g
        Current message level: 0x00000007 (7)
                               drv probe link
        Link detected: yes
```
- Or using mii-tool from [sys-apps/net-tools](https://packages.gentoo.org/packages/sys-apps/net-tools) package:

`root #``mii-tool eth0 -v`
eth0: negotiated 1000baseT-FD flow-control, link ok
  product info: vendor 23:58:00, model 42 rev 0
  basic mode:   autonegotiation enabled
  basic status: autonegotiation complete, link ok
  capabilities: 1000baseT-FD 100baseTx-FD 100baseTx-HD 10baseT-FD 10baseT-HD
  advertising:  1000baseT-FD 100baseTx-FD 100baseTx-HD 10baseT-FD 10baseT-HD flow-control
  link partner: 1000baseT-HD 1000baseT-FD 100baseTx-FD 100baseTx-HD 10baseT-FD 10baseT-HD

### Physical interface features (TCP offloading)

Sometimes NIC's have basic/partial TCP/IP stack implementation, and support offloading of certain networking routines/protocols to the NIC chip.

Example below shows network interface offloading features supported on device **eth0**:

`root #``ethtool --show-offload eth0````
Features for eth0:
rx-checksumming: off
tx-checksumming: on
        tx-checksum-ipv4: off [fixed]
        tx-checksum-ip-generic: on
        tx-checksum-ipv6: off [fixed]
        tx-checksum-fcoe-crc: off [fixed]
        tx-checksum-sctp: off [fixed]
scatter-gather: on
        tx-scatter-gather: on
        tx-scatter-gather-fraglist: off [fixed]
tcp-segmentation-offload: on
        tx-tcp-segmentation: on
        tx-tcp-ecn-segmentation: off [fixed]
        tx-tcp6-segmentation: off [fixed]
udp-fragmentation-offload: off [fixed]
generic-segmentation-offload: on
generic-receive-offload: on
large-receive-offload: off [fixed]
rx-vlan-offload: on
tx-vlan-offload: on [fixed]
ntuple-filters: off [fixed]
receive-hashing: off [fixed]
highdma: off [fixed]
rx-vlan-filter: on [fixed]
vlan-challenged: off [fixed]
tx-lockless: off [fixed]
netns-local: off [fixed]
tx-gso-robust: off [fixed]
tx-fcoe-segmentation: off [fixed]
fcoe-mtu: off [fixed]
tx-nocache-copy: on
loopback: off [fixed]
rx-fcs: off
rx-all: off
```
## See also

- [Network management](https://wiki.gentoo.org/wiki/Network_management) — describes possibilities for managing the network stack.
- [Power management/Ethernet](https://wiki.gentoo.org/wiki/Power_management/Ethernet) — describes the setup of [power management](https://wiki.gentoo.org/wiki/Power_management) of [Ethernet] devices.

## External resources

- [TCP offload engine](https://en.wikipedia.org/wiki/TCP_offload_engine) (Wikipedia)
- [Ethernet](https://wiki.archlinux.org/index.php/Network_configuration/Ethernet) (Arch Wiki)
