<!-- source: https://wiki.gentoo.org/wiki/Seatd | group: Gentoo Wiki (Main) | wiki-title: Seatd -->
---
title: Seatd
url: https://wiki.gentoo.org/wiki/Seatd
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2024-10-22"
fingerprint: ae67302ca99228ae
license: CC BY-SA 4.0
---

# Seatd

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**Seatd** is a minimal seat management daemon, and a universal seat management library. Seat management takes care of mediating access to shared devices (graphics, input), without requiring applications like [Wayland compositors](https://wiki.gentoo.org/wiki/Wayland_compositor) being granted root privileges.

## Installation

### USE flags


| [builtin](https://packages.gentoo.org/useflags/builtin) | Enable embedded server in libseat | 
| [elogind](https://packages.gentoo.org/useflags/elogind) | Enable session tracking via sys-auth/elogind | 
| [selinux](https://packages.gentoo.org/useflags/selinux) | !!internal use only!! Security Enhanced Linux support, this must be set by the selinux profile or breakage will occur | 
| [server](https://packages.gentoo.org/useflags/server) | Enable standalone seatd server, replacement to (e)logind | 
| [systemd](https://packages.gentoo.org/useflags/systemd) | Enable use of systemd-specific libraries and features like socket activation or session tracking | 

### Emerge

`root #``emerge --ask sys-auth/seatd`
### Service

#### OpenRC

To use the seatd service, the `builtin` and `server` USE flags must be enabled. In addition, users of seatd must be part of the `video` and `seat` groups:

`root #``gpasswd -a larry video``root #``gpasswd -a larry seat`
Add the seatd daemon to the default runlevel so that seat management is provided on system startup:

`root #``rc-update add seatd default`
Start the seatd daemon now:

`root #``rc-service seatd start`
## External resources

- [seatd(1)](https://man.archlinux.org/man/seatd.1.en)- [seatd-launch(1)](https://man.archlinux.org/man/seatd-launch.1.en)
