<!-- source: https://wiki.gentoo.org/wiki/Tinc | group: Gentoo Wiki (Main) | wiki-title: Tinc -->
---
title: Tinc
url: https://wiki.gentoo.org/wiki/Tinc
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2022-05-26"
fingerprint: fc027b7f1bbd338b
license: CC BY-SA 4.0
---

# Tinc

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**tinc** is a versatile VPN which can work in a P2P configuration as well as more traditional topologies. It can be used to create a private mesh network without needing to configure individual connections between each nodes, as long as a path exists between them.

## Installation

### Emerge

`root #``emerge --ask net-vpn/tinc`
## Configuration

### Basics

All steps must be repeated per-machine unless otherwise noted. box1 is used as a placeholder.

First, choose a VPN/network name. As an example, *larrynet* is used here:

`root #``mkdir -p /etc/tinc/larrynet/hosts`
All configuration will be done within /etc/tinc/larrynet.

Create the main tinc config file at /etc/tinc/larrynet/tinc.conf:

**`/etc/tinc/larrynet/tinc.conf`**

Generate a key for the host (choose the default save locations):

`root #``tincd -n larrynet -K 4096`
There should now be a:

- *private* key (do not share this with any other person or machine!) at /etc/tinc/larrynet/rsa\_key.priv, and
- *public* key at /etc/tinc/larrynet/hosts/box1. This file will later need to be shared across each machine in the network.

The next step is to configure the network which may need to be adapted per desired configuration.

### Network configuration

On each host, some basics must be set. box2 must be configured to know about box1's location and details:

**`/etc/tinc/larrynet/hosts/box1`**

Create hooks for tinc to bring up and shutdown the network:

**`/etc/tinc/larrynet/tinc-up`**

**`/etc/tinc/larrynet/tinc-down`**

And make them executable:

`root #``chmod +x /etc/tinc/larrynet/tinc-up /etc/tinc/larrynet/tinc-down`
### Summary

For **each** machine, follow these steps:

1\. Create /etc/tinc/larrynet/tinc.conf with the hostname as above.

2\. Create a /etc/tinc/larrynet/hosts/$hostname file as above *for each host in the network*, i.e. every machine must have a hosts file for every other machine.

## Automatic startup

### OpenRC

Edit /etc/conf.d/tinc.networks to add the network name:

**`/etc/conf.d/tinc.networks`**

Start up the network:

`root #``/etc/init.d/tincd start`
Start it on boot:

`root #``rc-update add tincd default`
### systemd

`root #``systemctl enable --now tincd@larrynet`
