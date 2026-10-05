<!-- source: https://wiki.gentoo.org/wiki//etc/hosts | group: Gentoo Wiki (Main) | wiki-title: /etc/hosts -->
---
title: "/etc/hosts"
url: https://wiki.gentoo.org/wiki//etc/hosts
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2024-07-04"
fingerprint: f4cbe313ac69ffd0
license: CC BY-SA 4.0
---

# /etc/hosts

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

The **/etc/hosts** file is a file associating host names with IP addresses. It can be used to manually specify the IP address of, for example, a named device on a LAN, without having to set up a DNS server:

**`/etc/hosts`**

It will then be possible to do things like `ssh user@larry`, rather than `ssh user@192.168.1.100`.

The /etc/hosts file will only be consulted if the `files` is specified for the `hosts` entry in [nsswitch.conf(5)](https://man.archlinux.org/man/nsswitch.conf.5.en)[, e.g.:](https://wiki.gentoo.org/wiki/Special:MyLanguage/man_page)

**`/etc/nsswitch.conf`**

As DNS is not involved, tools like [host(1)](https://man.archlinux.org/man/host.1.en) [and](https://wiki.gentoo.org/wiki/Special:MyLanguage/man_page) [dig(1)](https://man.archlinux.org/man/dig.1.en) [cannot be used to test whether host name lookup is working; instead, one should use](https://wiki.gentoo.org/wiki/Special:MyLanguage/man_page) [getent(1)](https://man.archlinux.org/man/getent.1.en)[, e.g.:](https://wiki.gentoo.org/wiki/Special:MyLanguage/man_page)

`user $``getent hosts larry`
/etc/hosts can be used to do DNS-level blocking of problematic hosts and domains by adding blacklists to it, such as [the oisd.nl blacklists](https://oisd.nl/downloads) and [the Ultimate Hosts Blacklist](https://github.com/Ultimate-Hosts-Blacklist/Ultimate.Hosts.Blacklist).

To reference hosts and devices on a LAN by name, without having to manually maintain entries in /etc/hosts, set up [zero-configuration networking](https://en.wikipedia.org/wiki/Zero-configuration_networking) (zeroconf) using [Avahi](https://wiki.gentoo.org/wiki/Avahi).
