<!-- source: https://wiki.gentoo.org/wiki/Mosh | group: Gentoo Wiki (Main) | wiki-title: Mosh -->
---
title: Mosh
url: https://wiki.gentoo.org/wiki/Mosh
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2022-12-10"
fingerprint: "9fbbe1d8fd017af0"
license: CC BY-SA 4.0
---

# Mosh

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**Mosh** is a SSH client server that is aware of connectivity problems of the original SSH implementation. Mosh can migrate physical connections and IP addresses while staying connected. Mosh depends on [SSH](https://wiki.gentoo.org/wiki/SSH).

## Installation

### USE flags


| [+client](https://packages.gentoo.org/useflags/+client) | Build network client | 
| [+hardened](https://packages.gentoo.org/useflags/+hardened) | Activate default security enhancements for toolchain (gcc, glibc, binutils) | 
| [+server](https://packages.gentoo.org/useflags/+server) | Build network server | 
| [+utempter](https://packages.gentoo.org/useflags/+utempter) | Include libutempter support | 
| [examples](https://packages.gentoo.org/useflags/examples) | Include example scripts | 
| [nettle](https://packages.gentoo.org/useflags/nettle) | Use dev-libs/nettle for some cryptographic functions instead of dev-libs/openssl. With Nettle, some of mosh's own code is used for OCB. | 
| [syslog](https://packages.gentoo.org/useflags/syslog) | Enable support for syslog | 
| [ufw](https://packages.gentoo.org/useflags/ufw) | Install net-firewall/ufw rule set | 

### Emerge

Install [net-misc/mosh](https://packages.gentoo.org/packages/net-misc/mosh):

`root #``emerge --ask net-misc/mosh`
## Configuration

Mosh requires [UTF-8](https://wiki.gentoo.org/wiki/UTF-8) [locales](https://wiki.gentoo.org/wiki/Localization/Guide#Locale_system) to be set in order to run. To check, run:

`user $``locale -a`
In case it does not return the [UTF-8](https://wiki.gentoo.org/wiki/UTF-8) locale see [UTF-8#Setting up UTF-8 with Gentoo Linux](https://wiki.gentoo.org/wiki/UTF-8#Setting_up_UTF-8_with_Gentoo_Linux).

### Firewall

Each mosh client requires a free and accessible UDP port between 60000 and 61000 on the server to function.

With [Ufw](https://wiki.gentoo.org/wiki/Ufw), allow these ports with:

`root #``ufw allow 60000:61000/udp`
## Usage

### Connecting

Once the remote host has SSH running, mosh installed, and the UFT8 locale set connection is possible:

`user $``mosh user@remote-host.com`
## See also

- [ssh](https://wiki.gentoo.org/wiki/Ssh) — the ubiquitous tool for logging into and working on remote machines securely.
