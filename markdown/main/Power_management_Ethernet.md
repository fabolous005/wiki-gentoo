<!-- source: https://wiki.gentoo.org/wiki/Power_management/Ethernet | group: Gentoo Wiki (Main) | wiki-title: Power management/Ethernet -->
---
title: Power management/Ethernet
url: https://wiki.gentoo.org/wiki/Power_management/Ethernet
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-01-17"
fingerprint: aed17b69e8b1f88a
license: CC BY-SA 4.0
---

# Power management/Ethernet

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

This article describes the setup of [power management](https://wiki.gentoo.org/wiki/Power_management) of [Ethernet](https://wiki.gentoo.org/wiki/Ethernet) devices.

## Disable Wake-on-LAN

With [Wake-on-LAN](https://en.wikipedia.org/wiki/Wake-on-LAN) (WOL) the computer's network card will remain powered to monitor for a 'magic' packet that will instruct it to wake the computer. To save some power WOL can be disabled.

ethtool is needed in order to perform this action if it's not already installed:

`root #``emerge --ask sys-apps/ethtool`
The current WOL setting can be checked with:

`root #``ethtool eth0 | grep -i wake````
        Supports Wake-on: pumbg
        Wake-on: g
```
Disable WOL via:

`root #``ethtool -s eth0 wol d`
### BIOS

Motherboards with on-board Ethernet devices usually have an option to disable WOL (if it's supported at all) in the [BIOS](https://wiki.gentoo.org/wiki/BIOS).

### Permanent Change

In order for the ethernet device changes to persist across reboots, we can set up the commands that make the changes to run at boot; this can be done as an init service or a [udev](https://wiki.gentoo.org/wiki/Udev) rule.

#### Init service

##### OpenRC

[OpenRC](https://wiki.gentoo.org/wiki/OpenRC) users should add the following string to the /etc/conf.d/net configuration file to disable WOL for a specific network interface using ethtool:

**`/etc/conf.d/net`**

```
ethtool_change_eth0="wol d"
```
##### Systemd

[systemd](https://wiki.gentoo.org/wiki/Systemd) users should edit the /etc/systemd/network/50-wired.link file instead:

**`/etc/systemd/network/50-wired.link`**

#### Udev

Make the following udev rule file to automate power management:

**`/etc/udev/rules.d/10-my-ethernet-power.rules`**

## Throttle Gigabit-Ethernet

Gigabit-Ethernet uses more power than Fast-Ethernet. Users that do not need more bandwidth can setup the network's card connection to be Fast-Ethernet. First install the package [sys-apps/ethtool](https://packages.gentoo.org/packages/sys-apps/ethtool):

`root #``emerge --ask sys-apps/ethtool`
Now setup the interface bandwith to be 100 Mbit/s:

`root #``ethtool -s eth0 autoneg off speed 100`
If Open-RC is used the network bandwidth can be always set at boot by adding the above command to the `ethtool_change_eth0` variable in the /etc/conf.d/net configuration file:

**`/etc/conf.d/net`**

```
ethtool_change_eth0="autoneg off speed 100"
```
## See also

- [Ethernet](https://wiki.gentoo.org/wiki/Ethernet) — the setup of an Ethernet network device.
