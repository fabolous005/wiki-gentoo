<!-- source: https://wiki.gentoo.org/wiki/Proxychains | group: Gentoo Wiki (Main) | wiki-title: Proxychains -->
---
title: proxychains
url: https://wiki.gentoo.org/wiki/Proxychains
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2023-04-25"
fingerprint: ffe3954b73c168a7
license: CC BY-SA 4.0
---

# proxychains

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)



proxychains force any tcp connections to flow through a proxy (or proxy chain). Tool used to secure internet connections.

## Installation

[net-misc/proxychains](https://packages.gentoo.org/packages/net-misc/proxychains) does not have USE flags right now.

`root #``emerge --ask net-misc/proxychains`
## DNS leakage

Proxy chains has "proxy\_dns" option in /etc/proxychains.conf to prevent "DNS leaks", but this options will work only if application support "Proxy DNS when using socks5", like Firefox has.

To test if application leaks DNS you can use [Tcpdump](https://wiki.gentoo.org/wiki/Tcpdump) tool.

To block all DNS request for user ff (simple sandbox for Firefox) in [nftables](https://wiki.gentoo.org/wiki/Nftables) use command:

`root #``nft add rule filter output meta skuid ff ip daddr != { 127.0.0.1/8, 224.0.0.0/8 } drop`
To prevent leakage [net-proxy/dnsproxy](https://packages.gentoo.org/packages/net-proxy/dnsproxy) can be used on remote SSH server with following commands.

At local machine:

`user $``ssh -L 6667:0.0.0:6667 root@remove_ssh_ip`
At server:

`root #``socat tcp4-listen:6667,reuseaddr,fork UDP:127.0.0.1:53000`
At local machine:

`root #``socat udp-listen:53,reuseaddr,fork TCP:127.0.0.1:6667 &``root #``echo "nameserver 127.0.0.1" > /etc/resolv.conf``root #``chattr +i /etc/resolv.conf`
Check dnsproxy with command:

`root #``dig @127.0.0.1 -p 53 gentoo.org`
