<!-- source: https://wiki.gentoo.org/wiki/Systemd/systemd-networkd | group: Gentoo Wiki (Main) | wiki-title: Systemd/systemd-networkd -->
---
title: Systemd/systemd-networkd
url: https://wiki.gentoo.org/wiki/Systemd/systemd-networkd
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-02-17"
fingerprint: b74d6379bdb3b3c1
license: CC BY-SA 4.0
---

# Systemd/systemd-networkd

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

systemd-networkd provides configuration of wired network interfaces. It is disabled by default on Gentoo systems, but can be quickly enabled as necessary enable connectivity.

To configure systemd-networkd, create a \*.network file under the /etc/systemd/network directory. Files are written in INI style syntax, and usually given a name starting with two digits which determines the order they are loaded. See the [systemd.network(5)](https://man.archlinux.org/man/systemd.network.5.en) [man page for reference.](https://wiki.gentoo.org/wiki/Special:MyLanguage/man_page)

A \*.network file contains a `[Match]` section, which specifies which interfaces the file will be applied to, and further sections, especially `[Network]` which describe the configuration to applied. As configuration files are loaded in alphabetical order, the first file matching an interface is used. As such, configuration files with less specific `[Match]` criteria would usually be given names with higher numbers than those with more specific criteria.

Interfaces can be matched on many properties, including the interface name, its MAC address, or device path.

A configuration which configures all Ethernet interfaces (which are typically given names starting with `en`) to use DHCP is:

**`/etc/systemd/network/50-dhcp.network`**

```
[Match]
Name=en*
 
[Network]
DHCP=yes
```
Wired connection with static IP address (uses [CIDR](https://en.wikipedia.org/wiki/CIDR) notation for subnetting):

**`/etc/systemd/network/50-static.network`**

```
[Match]
Name=enp1s0
 
[Network]
Address=192.168.1.10/24
Gateway=192.168.1.1
DNS=192.168.1.1
```
A simple DHCP configuration for the wireless card wlan0 is given below:

**`/etc/systemd/network/50-wireless.network`**

```
[Match]
Name=wlan0
 
[Network]
DHCP=yes
IgnoreCarrierLoss=3s
```
Connecting to a wireless access point can be achieved using [Iwd](https://wiki.gentoo.org/wiki/Iwd) or [Wpa supplicant](https://wiki.gentoo.org/wiki/Wpa_supplicant).

Note that systemd-networkd does not update the resolv.conf file or provide DNS resolution by default. See the [systemd/systemd-resolved](https://wiki.gentoo.org/wiki/Systemd/systemd-resolved) article for details on how to setup domain name resolution (DNS) on systemd systems.

To enable and start the service now:

`root #``systemctl enable --now systemd-networkd.service`
A networkctl user space utility exists to query and control the networking subsystem for systems running systemd-networkd.

`user $``networkctl --help````
networkctl [OPTIONS...] COMMAND
Query and control the networking subsystem.
Commands:
  list [PATTERN...]      List links
  status [PATTERN...]    Show link status
  lldp [PATTERN...]      Show LLDP neighbors
  label                  Show current address label entries in the kernel
  delete DEVICES...      Delete virtual netdevs
  up DEVICES...          Bring devices up
  down DEVICES...        Bring devices down
  renew DEVICES...       Renew dynamic configurations
  forcerenew DEVICES...  Trigger DHCP reconfiguration of all connected clients
  reconfigure DEVICES... Reconfigure interfaces
  reload                 Reload .network and .netdev files
Options:
  -h --help              Show this help
     --version           Show package version
     --no-pager          Do not pipe output into a pager
     --no-legend         Do not show the headers and footers
  -a --all               Show status for all links
  -s --stats             Show detailed link statistics
  -l --full              Do not ellipsize output
  -n --lines=INTEGER     Number of journal entries to show
     --json=pretty|short|off
                         Generate JSON output
See the networkctl(1) man page for details.
```
## Troubleshooting

### Hangs

Users may encounter a situation where the built-in systemd-networkd-wait-online service may try to confirm network connectivity for all links *it is aware of* and which are *managed* by systemd-networkd, but there aren't any configured by the system administrator. This will result in the default two-minute timeout to occur, preventing any depending service from accessing the network. The following examples of problems may follow:


The list of affected services can be obtained via

`root #``systemctl list-dependencies systemd-networkd-wait-online.service --all --reverse`
Several approaches can be used to fix any such problem.

If a specific network interface is known to be required for network connectivity (e.g., `eth0`), run

`root #````
systemctl disable systemd-networkd-wait-online.service
```
`root #````
systemctl enable systemd-networkd-wait-online@eth0.service
```
Alternatively, create an override for the service via

`root #``systemctl edit systemd-networkd-wait-online.service`
and add the lines

**`/etc/systemd/system/systemd-networkd-wait-online.service.d/override.conf`**

```
[Match]
[Service]
ExecStart=
ExecStart=/usr/lib/systemd/systemd-networkd-wait-online --interface=eth0
```
where the first `ExecStart=` clears any existing entries, and eth0 should be replaced with name of the interface that is primary/relevant for the behavior that is desired. One could also write

**`/etc/systemd/system/systemd-networkd-wait-online.service.d/override.conf`**

```
[Match]
[Service]
ExecStart=
ExecStart=/usr/lib/systemd/systemd-networkd-wait-online --interface=any
```
to accept connectivity with any interface, or

**`/etc/systemd/system/systemd-networkd-wait-online.service.d/override.conf`**

```
[Match]
[Service]
ExecStart=
ExecStart=/usr/lib/systemd/systemd-networkd-wait-online --ignore=eth0
```
to ignore a particular one.[\[1\]](https://wiki.gentoo.org#cite_note-1)

### NetworkManager

systemd-networkd and [NetworkManager](https://wiki.gentoo.org/wiki/NetworkManager) are generally incompatible and should not be run together. They both attempt to manage the same network resources, leading to potential conflicts and unpredictable behavior.

To disable the NetworkManager services, run

`root #````
systemctl disable NetworkManager.service
```
`root #````
systemctl disable NetworkManager-wait-online.service
```
- [systemd/systemd-resolved](https://wiki.gentoo.org/wiki/Systemd/systemd-resolved) — a address name resolution (DNS) daemon which can be used in conjunction with systemd-networkd.
- [systemd](https://wiki.gentoo.org/wiki/Systemd) — a modern SysV-style init and [rc](https://wiki.gentoo.org/wiki/Rc) replacement for Linux systems.
- [Network management](https://wiki.gentoo.org/wiki/Network_management) — describes possibilities for managing the network stack.
