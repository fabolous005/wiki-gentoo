<!-- source: https://wiki.gentoo.org/wiki/IPSet | group: Gentoo Wiki (Main) | wiki-title: IPSet -->
---
title: IPSet
url: https://wiki.gentoo.org/wiki/IPSet
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2025-05-09"
fingerprint: "75b12c5fadff21e5"
license: CC BY-SA 4.0
---

# IPSet

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

*IPSet is used to set up, maintain and inspect so called IP sets in the Linux kernel. Depending on  the  type of  the  set,  an IP set may store IP(v4/v6) addresses, (TCP/UDP) port numbers, IP and MAC address pairs, IP address and port number pairs, etc.* - Wikipedia

IPSet is a tool for [Iptables](https://wiki.gentoo.org/wiki/Iptables), successor of IPpool. It is an administration tool for IP sets which can be added to IPTables rules to filter out networks.

## Installation

### Prerequisites

You will need to configure your kernel to support ipset.

### Kernel

For example, if ipset support is compiled as a module:

**Kernel settings for ipset**

then select the desired ipset types.

### USE flags


| [+strip](https://packages.gentoo.org/useflags/+strip) | Allow symbol stripping to be performed by the ebuild for special files | 
| [dist-kernel](https://packages.gentoo.org/useflags/dist-kernel) | Enable subslot rebuilds on Distribution Kernel upgrades | 
| [modules](https://packages.gentoo.org/useflags/modules) | Build the kernel modules | 
| [modules-compress](https://packages.gentoo.org/useflags/modules-compress) | Install compressed kernel modules (if kernel config enables module compression) | 
| [modules-sign](https://packages.gentoo.org/useflags/modules-sign) | Cryptographically sign installed kernel modules (requires CONFIG\_MODULE\_SIG=y in the kernel) | 

### Emerge

Install IPSet:

`root #``emerge --ask net-firewall/ipset`
## Filtering

The simple following script can be used to filter IP addresses based on a file that have to be retrieved on the internet, and then create or update iptables firewall rules:

**`~/scripts/ips.sh`**

**Example IPSet script**

```
#!/bin/sh
opt="hash:net --hashsize 64"
datadir=/var/lib/ipset
target=https://feeds.dshield.org/block.txt
set=IPBlock
wget -qNO - $target > $datadir/$set || exit
tmp=${set}-tmp
ipset create $tmp $opt
networks="$(grep -E '^[0-9]' $datadir/block.txt | sed -rne 's/(^([0-9]{1,3}\.){3}[0-9]{1,3}).*$/\1/p')"
for i in $networks; do
    ipset add $tmp ${i}/24
done
ipset create -exist $set $opt &&
ipset swap $tmp $set &&
ipset destroy $tmp &&
echo "IPSet: $set updated"
unset -v i networks opt set tmp
```
The above script is just a simple way to retrieve different or various IPSet table and make use of an up to date filtering.

The script creates a new table and swap and destroys a previous set if one exists. For a more refined script see the following examples:

Save the rules to a file and start IPSet init service:

`root #``/etc/init.d/ipset save``root #``/etc/init.d/ipset start``root #``rc-update add ipset boot`
The previous network filtering can be added to iptables with the following command:

`root #``iptables -I INPUT -m set --match-set IPBlock src,dst -j Drop`
## See also

- [Iptables](https://wiki.gentoo.org/wiki/Iptables) — a program used to configure and manage the kernel's netfilter modules.
