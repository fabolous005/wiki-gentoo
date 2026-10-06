<!-- source: https://wiki.gentoo.org/wiki/Gentoo_for_Network_Admins | group: Gentoo Wiki (Main) | wiki-title: Gentoo for Network Admins -->
---
title: Gentoo for Network Admins
url: https://wiki.gentoo.org/wiki/Gentoo_for_Network_Admins
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-08-30"
fingerprint: b4a51be499b568c7
license: CC BY-SA 4.0
---

# Gentoo for Network Admins

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

This article hosts a guide for forging Gentoo into a fully-fledged, network-debugging Swiss army knife. This guide might be useful for:

- NOC engineers
- network admins
- site reliability engineers
- devops/netops engineers

## Useful tools

### HTTP(S) debugging

`root #``emerge --ask net-misc/curl`
simple HTTPS verification:

`user $``curl -Is` [https://www.example.com](https://www.example.com)
HTTP/2 200
date: Mon, 08 Jan 2024 09:53:11 GMT
server: Apache/2.4
last-modified: Sat, 06 Jan 2024 05:43:21 GMT
vary: Accept-Encoding
x-frame-options: SAMEORIGIN
content-type: text/html
content-language: en
age: 66
etag: W/"1968a-15ag718a56dfa-gzip"
accept-ranges: bytes
content-length: 123574

### DNS debugging

[net-dns/bind](https://packages.gentoo.org/packages/net-dns/bind) contains most of the DNS debugging tools such as nslookup, dig, and host.

`root #``emerge --ask net-dns/bind`
### SSL/TLS/PKI troubleshooting

The s\_client, ocsp, x509 commands and others are included in the [dev-libs/openssl](https://packages.gentoo.org/packages/dev-libs/openssl) package.

`root #``emerge --ask dev-libs/openssl`
### Port knocking/scanning

Most popular command for netcat is nc -zv \<host> \<port>

`root #``emerge --ask net-analyzer/openbsd-netcat`
Check which ports are open, which services are running with [nmap](https://wiki.gentoo.org/wiki/Nmap).

`root #``emerge --ask net-analyzer/nmap`
### Traffic analyzers

Listen on an interface and show src/dest traffic and speed:

`root #``emerge --ask net-analyzer/iftop`
Inspect incoming/outgoing packets:

`root #``emerge --ask net-analyzer/tcpdump`
Inspect incoming/outgoing packets when [net-analyzer/wireshark](https://packages.gentoo.org/packages/net-analyzer/wireshark) is available, using tshark:

`user $``tshark -i eth0`
Automate network testing when [net-analyzer/wireshark](https://packages.gentoo.org/packages/net-analyzer/wireshark) is available, using sharkd using python scripts. For unix and TCP sockets. Starting a spawnd instance:

`user $``spawnd -`
Hello in child.

### Network bandwidth measurement

[iperf](https://wiki.gentoo.org/wiki/Iperf) has many use cases. It can for example stress test a network by running

`user $``iperf -c qa2`
### IP troubleshooting (L3)

MyTraceroute does a traceroute by probing with ICMP packets:

`root #``emerge --ask net-analyzer/mtr`
In case ICMP is blocked by some firewall on the LAN, try tcptraceroute:

`root #``emerge --ask net-analyzer/tcptraceroute`
lft Layer four traceroute, traceroute using TCP:

`root #``emerge --ask net-analyzer/lft`
### L2 troubleshooting

Directly connected neighbor detection, capabilities, connected port etc:

`root #``emerge --ask net-misc/lldpd`
### L1 troubleshooting

Link detection, WOL support, link modes et. al.:

`root #``emerge --ask sys-apps/ethtool`
### Others

**xclip**

[x11-misc/xclip](https://packages.gentoo.org/packages/x11-misc/xclip) can be used to copy logs, file contents, etc. without leaving the terminal. For example:

`user $``cat /var/log/emerge.log | xclip -sel clip`
**openssl**

Use openssl for random generation instead of a dedicated tool or script:

`user $``openssl rand -hex 16`
71cb1117861f4a8cf08489b9f8cd6b73

[net-misc/telnet-bsd](https://packages.gentoo.org/packages/net-misc/telnet-bsd) - a working telnet client is a useful tool for troubleshooting networks.

## See also

- [Recommended tools](https://wiki.gentoo.org/wiki/Recommended_tools) — lists system-administration related tools recommended for use in a **[shell](https://wiki.gentoo.org/wiki/Shell) environment** ([terminal/console](https://wiki.gentoo.org/wiki/Terminal_emulator))
