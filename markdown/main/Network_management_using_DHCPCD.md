<!-- source: https://wiki.gentoo.org/wiki/Network_management_using_DHCPCD | group: Gentoo Wiki (Main) | wiki-title: Network management using DHCPCD -->
---
title: Network management using DHCPCD
url: https://wiki.gentoo.org/wiki/Network_management_using_DHCPCD
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2025-07-27"
fingerprint: "9c59f159191889ef"
license: CC BY-SA 4.0
---

# Network management using DHCPCD

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

This article is based on a [posting in the Gentoo Forums](https://forums.gentoo.org/viewtopic-t-965190.html) and explains how to use dhcpcd for complete network stack management.

Of course, it's possible to use dhcpcd together with net.\* scripts. In fact, if dhcpcd is installed, it's used as the default DHCP client if `config_eth0="dhcp"` is set in /etc/conf.d/net. This article is specifically about using dhcpcd standalone.

## Setup

Make sure [dhcpcd](https://wiki.gentoo.org/wiki/Dhcpcd) is installed. Next, add it to the default runlevel and start the service.

If using [OpenRC](https://wiki.gentoo.org/wiki/OpenRC), do:

`root #````
rc-update add dhcpcd default
```
`root #````
rc-service dhcpcd start
```
Or, if using [systemd](https://wiki.gentoo.org/wiki/Systemd):

`root #````
systemctl enable dhcpcd
```
`root #````
systemctl start dhcpcd
```
This setup deviates from the default networking instructions in the Gentoo Handbook. It has the advantage that it provides automatic switching between wired and wireless network interfaces.

## Wireless

### wpa\_supplicant

[wpa\_supplicant](https://wiki.gentoo.org/wiki/Wpa_supplicant) can be used for authentication of wireless and wired network interfaces. Create the configuration file if it does not exist:

**`/etc/wpa_supplicant/wpa_supplicant.conf`**

```
# Allow users in the 'wheel' group to control wpa_supplicant
ctrl_interface=DIR=/var/run/wpa_supplicant GROUP=wheel
 
# Make this file writable for wpa_gui / wpa_cli
update_config=1
```
For authentication for the wired interface, add the configuration file mentioned in [Setup wired 802.1X](https://wiki.gentoo.org/wiki/Wpa_supplicant#Setup_wired_802.1X).

First follow the setup guide for [dhcpcd](https://wiki.gentoo.org/wiki/Network_management_using_DHCPCD#Setup).

Emerge wpa\_supplicant (Version >=2.6-r2 is needed in order to get the [CONFIG\_MATCH\_IFACE](https://forums.gentoo.org/viewtopic-t-1036958-start-4.html) option [added in April 2017](https://gitweb.gentoo.org/repo/gentoo.git/commit/net-wireless/wpa_supplicant?id=423af2686b225f76bb8ae52221690581a56a9625)):

`root #``emerge --ask net-wireless/wpa_supplicant`
#### Using [OpenRC](https://wiki.gentoo.org/wiki/OpenRC)

Complete its conf.d file with the `-M` option for the wireless network interface:

**`/etc/conf.d/wpa_supplicant`**

```
wpa_supplicant_args="-B -M -c /etc/wpa_supplicant/wpa_supplicant.conf"
```
In case authentication for the [wired interface](https://wiki.gentoo.org/wiki/Wpa_supplicant#Setup_wired_802.1X) is needed, this configuration file should look like:

**`/etc/conf.d/wpa_supplicant`**

```
wpa_supplicant_args="-ieth0 -Dwired -c/etc/wpa_supplicant/wpa_supplicant_wired.conf -B -M -c/etc/wpa_supplicant/wpa_supplicant.conf"
```
With the configuration done, run it as a service:

`root #``rc-update add wpa_supplicant default``root #``rc-service wpa_supplicant start`
#### Using [Systemd](https://wiki.gentoo.org/wiki/Systemd)

[Systemd](https://wiki.gentoo.org/wiki/Systemd) allows a simpler per-device setup without needing to create the above conf.d files. As explained under *wpa\_supplicant* item in the [Native services](https://wiki.gentoo.org/wiki/Systemd#Native_services) section, a service symlink such as `wpa_supplicant@wlan0.service` looks for a separate configuration file to manage the device `wlan0` in this case.

To configure a specific device this way, first copy or rename the /etc/wpa\_supplicant/wpa\_supplicant.conf file as /etc/wpa\_supplicant/wpa\_supplicant-DEVNAME.conf where `DEVNAME` should be the name of the device, such as `wlan0`.

Then, navigate to /etc/systemd/system/multi-user.target.wants and create the symlink:

`root #``ln -s /lib/systemd/system/wpa_supplicant@.service wpa_supplicant@DEVNAME.service`
where `DEVNAME` is *same* device name as in the conf file above.

Test the system:

`root #````
systemctl daemon-reload
```
`root #````
systemctl start wpa_supplicant@DEVNAME
```
`root #````
systemctl status wpa_supplicant@DEVNAME
```
### Using net-wireless/iwd

See the [iwd](https://wiki.gentoo.org/wiki/Iwd) article.

## Testing

Stop the dhcpcd service, then start dhcpcd with the -d (`--debug`) and -B (`--nobackground`) options enabled to see it starting the connection:

`root #````
rc-service dhcpcd stop
```
`root #````
dhcpcd -dB
```
dhcpcd-6.11.3 starting
dev: loaded udev
eth0: executing \`/lib/dhcpcd/dhcpcd-run-hooks' PREINIT
eth0: executing \`/lib/dhcpcd/dhcpcd-run-hooks' NOCARRIER
wlan0: executing \`/lib/dhcpcd/dhcpcd-run-hooks' PREINIT
wlan0: executing \`/lib/dhcpcd/dhcpcd-run-hooks' CARRIER
DUID xx:xx:xx:xx:xx:xx:xx:xx:xx:xx:xx:xx:xx:xx
wlan0: IAID XX:XX:XX:XX
wlan0: delaying IPv6 router solicitation for 0.9 seconds
wlan0: delaying IPv4 for 0.3 seconds
eth0: waiting for carrier
wlan0: reading lease \`/var/lib/dhcpcd/dhcpcd-wlan0-.lease'
wlan0: rebinding lease of 192.168.178.23
wlan0: sending REQUEST (xid 0x66820be2), next in 3.3 seconds
wlan0: acknowledged 192.168.178.23 from 192.168.178.1
wlan0: leased 192.168.178.23 for 864000 seconds
wlan0: renew in 432000 seconds, rebind in 756000 seconds
wlan0: writing lease \`/var/lib/dhcpcd/dhcpcd-wlan0-.lease'
wlan0: IP address 192.168.178.23/24 already exists
wlan0: changing route to 192.168.178.0/24
wlan0: changing default route via 192.168.178.1
wlan0: ARP announcing 192.168.178.23 (1 of 2), next in 2.0 seconds
wlan0: executing \`/lib/dhcpcd/dhcpcd-run-hooks' BOUND
wlan0: soliciting an IPv6 router
wlan0: sending Router Solicitation
wlan0: ARP announcing 192.168.178.23 (2 of 2)

## Static IP address

In case the network interface card should be configured with a [static IP address](https://wiki.gentoo.org/wiki/Static_routing), type it into the [graphical user interface](https://wiki.gentoo.org/wiki/Dhcpcd-ui#Usage). Without the graphical user interface, entries can also be manually added to /etc/dhcpcd.conf as described in [Dhcpcd](https://wiki.gentoo.org/wiki/Dhcpcd#Static_IP_addresses).

## Migration from Gentoo net.\* scripts

When migrating from [Gentoo's net.\* scripts](https://wiki.gentoo.org/wiki/Netifrc) it is essential to remove the net.\* scripts from the runlevels. To see what runlevels they are currently in:

`user $``rc-config list | grep 'wpa_supplicant\|dhcpcd\|net\.'`
net.wlp8s0                default
  dhcpcd                    default

Any match starting with `net.` must not have a runlevel assigned. In the above example, net.wlp8s0 needs to be removed:

`root #````
rc-update del net.wlp8s0
```
`root #``rc-service net.wlp8s0 stop`
## Graphical User interface

A [dhcpcd graphical user interface](https://wiki.gentoo.org/wiki/Dhcpcd-ui) is provided by [net-misc/dhcpcd-ui](https://packages.gentoo.org/packages/net-misc/dhcpcd-ui).

## See also

- [Dhcpcd](https://wiki.gentoo.org/wiki/Dhcpcd) — a popular DHCP client capable of handling both IPv4 and IPv6 configuration.
- [Dhcpcd-ui](https://wiki.gentoo.org/wiki/Dhcpcd-ui) — a Qt and GTK monitor and configuration graphical user interface for [dhcpcd](https://wiki.gentoo.org/wiki/Dhcpcd).
