<!-- source: https://wiki.gentoo.org/wiki/Haguichi | group: Gentoo Wiki (Main) | wiki-title: Haguichi -->
---
title: Haguichi
url: https://wiki.gentoo.org/wiki/Haguichi
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2025-01-01"
fingerprint: b4c8055c8f4e89d6
license: CC BY-SA 4.0
---

# Haguichi

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

Haguichi is a graphical front-end to [Hamachi](https://wiki.gentoo.org/wiki/Hamachi), a VPN tunneling engine.

## Installation

## Kernel configuration

The kernel must support tunneling.

**Enabling`CONFIG_TUN` in the kernel**

### Emerge

First unmask and emerge Hamachi, the VPN engine:

`root #``echo "net-vpn/logmein-hamachi ~amd64" >> /etc/portage/package.accept_keywords``root #``emerge --ask net-vpn/logmein-hamachi`
Haguichi requires mono and gtk-sharp. Emerge them in one fell swoop:

`root #``emerge --ask dev-lang/mono dev-dotnet/gtk-sharp dev-dotnet/notify-sharp dev-dotnet/gconf-sharp dev-dotnet/ndesk-dbus dev-dotnet/ndesk-dbus-glib`
Then nab the Haguichi tarball:

`user $``wget http://launchpad.net/haguichi/1.0/1.0.20/+download/haguichi-1.0.20-clr4.0.tar.gz`
Untar the file, and go into the resulting directory:

`user $``tar -xf haguichi*.tar.gz && cd haguichi*`
Then build and install the package:

`user $``./configure --prefix=/usr && make && su -c 'make install'`
## See also

- [Hamachi](https://wiki.gentoo.org/wiki/Hamachi) — a cross platform VPN tunneling engine.
