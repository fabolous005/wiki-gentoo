<!-- source: https://wiki.gentoo.org/wiki/OpenRC_notebook_roaming_How-To | group: Gentoo Wiki (Main) | wiki-title: OpenRC notebook roaming How-To -->
---
title: OpenRC notebook roaming How-To
url: https://wiki.gentoo.org/wiki/OpenRC_notebook_roaming_How-To
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2022-03-01"
fingerprint: b6838a7b8f28abc0
license: CC BY-SA 4.0
---

# OpenRC notebook roaming How-To

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

- migrate "pm-utils" instructions to use [elogind](https://wiki.gentoo.org/wiki/Elogind) instead, see [https://gitweb.gentoo.org/repo/gentoo.git/commit/profiles/package.mask?id=315d70c92b40d8bb1af7402aab9f0e6bd53f5a0e](https://gitweb.gentoo.org/repo/gentoo.git/commit/profiles/package.mask?id=315d70c92b40d8bb1af7402aab9f0e6bd53f5a0e)

This article shows how to set up OpenRC to act as a convenient, transparent, and flexible networking client which supports roaming as an alternative to [other roaming solutions](https://wiki.gentoo.org/wiki/Network_management) often used on notebooks.

## Requirements

The currently stable version of these packages are all that is needed (no \*kit software required):

- [OpenRC](https://wiki.gentoo.org/wiki/OpenRC) ([sys-apps/openrc](https://packages.gentoo.org/packages/sys-apps/openrc)) - management of services
  - have the netifrc USE flag enabled if you want the good old net.\* style network management
- [wpa\_supplicant](https://wiki.gentoo.org/wiki/Wpa_supplicant) — an app for [Wi-Fi](https://wiki.gentoo.org/wiki/Wi-Fi) authentication([net-wireless/wpa\_supplicant](https://packages.gentoo.org/packages/net-wireless/wpa_supplicant))
  - enable the qt5 USE flag if you want to have a GUI for the Wi-FI client
- [sys-apps/ifplugd](https://packages.gentoo.org/packages/sys-apps/ifplugd) - for hotplugging of ethernet interfaces
  - [sys-apps/netplug](https://packages.gentoo.org/packages/sys-apps/netplug) can be used as an alternative, but development has been discontinued
- [dhcpcd](https://wiki.gentoo.org/wiki/Dhcpcd) — a popular DHCP client capable of handling both IPv4 and IPv6 configuration. ([net-misc/dhcpcd](https://packages.gentoo.org/packages/net-misc/dhcpcd))
  - other compatible DHCP clients exist (dhclient provided by [net-misc/dhcp](https://packages.gentoo.org/packages/net-misc/dhcp) or udhcp provided by [sys-apps/busybox](https://packages.gentoo.org/packages/sys-apps/busybox)), but dhcpcd is the recommended solution as it works well with often reconfigured wireless interfaces (so the wireless interface doesn't need hotplugging)

## Configuration and setup

Before you start configuring the interfaces, make sure that they work (meaning you have the required drivers installed) and that you know their names. A convenient way is to make a [Udev rule to give them human-readable names](https://wiki.gentoo.org/wiki/Udev#NIC_assigned_eth0.2C_but_is_moved_to_eth1) like ethernet0, wireless0.

### OpenRC service management

For each interface an init script must be created for OpenRC to manage that interface by making a symlink to net.lo

`root #``ln -s /etc/init.d/net.lo /etc/init.d/net.${INTERFACE_NAME}` where **${INTERFACE\_NAME}** is the name of the interface, e.g. eth0, wlan0, ethernet0, wireless0

If simple roaming is required, any network interface will satisfy the virtual **net** service. Therefore it is recommended to set rc\_depend\_strict="NO" in /etc/rc.conf.

**`/etc/rc.conf`**

```
rc_depend_strict="NO"
```
The services responsible for managing the interfaces must be added to one or more runlevel. This makes it possible to have network profiles by having most services in the default runlevel and then adding the interfaces to [stacked runlevels](https://wiki.gentoo.org/wiki/OpenRC/Stacked_runlevel) and having different configuration files in /etc/conf.d/ for each runlevel as /etc/conf.d/net.${RUNELEVEL}

The options and syntax of /etc/conf.d/net is described in the [Gentoo Handbook](https://wiki.gentoo.org/wiki/Handbook:X86/Networking/Introduction) which shows how to set up more complicated networks (this how-to assumes that all connections are initiated via DHCP)

#### Suspend to RAM example

With most services in the default runlevel and network services in a runlevel "network", which is [stacked](https://wiki.gentoo.org/wiki/OpenRC/Stacked_runlevel) on top of the default runlevel, a PM hook for [sys-power/pm-utils](https://packages.gentoo.org/packages/sys-power/pm-utils) can be crated which transparently stops network services before suspending, thus preventing timeouts and other problems.

`root #````
mkdir /etc/runlevels/network/
```
`root #````
rc-update --stack add default network   # include init scripts form the default runlevel in the network runelvel
```
`root #````
rc-update del net.ethernet0 default  # remove networking scripts from the default runlevel
```
`root #````
rc-update add net.ethernet0 network  # and add them to the network runlevel
```
`root #````
rc-update del net.wireless0 default
```
`root #````
rc-update add net.wireless0 network
```
`root #````
rc-update del netmount default  # remove dependent networking services from the default runlevel
```
`root #````
rc-update add netmount network  # which would start the networking scripts because of "need net" stanza
```
**`/etc/pm/sleep.d/00network`**

**PM hook example**

```
#!/bin/bash
# properly stop and start the network
if [ -r "${PM_FUNCTIONS}" ]; then
    . "${PM_FUNCTIONS}" 
elif [ -r "${FUNCTIONS}" ]; then
    . "${FUNCTIONS}"
else
# pm-utils version is too old, or something else is wrong
    exit $NA
fi
case "$1" in
   	hibernate|suspend)
		exec rc default
		;;
	thaw|resume)
		exec rc network
		;;
	*) exit $NA
		;;
esac
```
The hook must be executable:

`root #``chmod a+x /etc/pm/sleep.d/00network`
To boot into the network runlevel rather than the default one, /etc/inittab must be changed as explained in [the Handbook](https://wiki.gentoo.org/wiki/Complete_Handbook/Configuring_the_boot_process#Init)

**`/etc/inittab`**

The bootloader can also support multiple options for different setups:

**`/boot/grub/grub.conf`**

**Grub v1 example**

#### Exclusive network interface example

It may be desirable to have always only one networking interface configured. This can be done by the postup or postdown functions defined in /etc/conf.d/net[\[1\]](https://wiki.gentoo.org#cite_note-handbook-functions-1)

**`/etc/conf.d/net`**

```
() {
     rc-service net.ethernet0 stop
}
postup_ethernet0() {
     rc-service net.wireless0 stop
}
postdown_wireless0() {
     rc-service net.ethernet0 start
}
postdown_ethernet0() {
     rc-service net.wireless0 start
}
```
#### Network specific log-in procedure example

The postup or postup\_${ESSID} (this function would be triggered only after successful connection to the given ${ESSID}) functions defined in /etc/conf.d/net<sup>[\[1\]](https://wiki.gentoo.org#cite_note-handbook-functions-1)</sup> can be used to submit data to e.g. a web log-in form.

**`/etc/conf.d/net`**

```
() {
     wget --save-cookies cookies.txt \
                        --post-data 'user=foo&password=bar' \
                        http://server.com/auth.php
}
```
### WiFi management

See [wpa\_supplicant](https://wiki.gentoo.org/wiki/Wpa_supplicant).

### Ethernet management

OpenRC automatically hotplugs all interfaces if configured in /etc/rc.conf:

**`/etc/rc.conf`**

```
rc_hotplug="*"
```
so the respective services will be started if that interface becomes available and ifplugd will be automatically started for every ethernet interface to ensure that the device gets configured when a carrier is acquired (e.g. a cable is plugged in). ifplugd will emit beeps on state change which can be disabled in /etc/conf.d/net

**`/etc/conf.d/net`**

**disabling ifplugd beeping for interface ethernet0**

```
ifplugd_ethernet0="--no-beep"
```
