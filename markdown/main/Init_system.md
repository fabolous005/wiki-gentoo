<!-- source: https://wiki.gentoo.org/wiki/Init_system | group: Gentoo Wiki (Main) | wiki-title: Init system -->
---
title: Init system
url: https://wiki.gentoo.org/wiki/Init_system
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-07-22"
fingerprint: bb185b3e238fafa9
license: CC BY-SA 4.0
---

# Init system

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

An **init system** is the first program, other than the kernel, to be run after a Linux distribution is booted.

Due to the flexibility of Gentoo, several init systems are available. Be aware, however, that even if another init system has been installed and set up, quite often [OpenRC](https://wiki.gentoo.org/wiki/OpenRC) is still required for other purposes.

## Available software

| Name | Package | Description | 
|---|---|---|
| [Dinit](https://wiki.gentoo.org/wiki/Dinit) | [sys-apps/dinit-services::guru](https://github.com/gentoo-mirror/guru/tree/master/sys-apps/dinit-services) | service supervisor with dependency support which can also act as an [init system] | 
| [openrc-init](https://wiki.gentoo.org/wiki/OpenRC/openrc-init) | [sys-apps/openrc](https://packages.gentoo.org/packages/sys-apps/openrc) | An init system (replacement for /sbin/init) included with OpenRC since version 0.25.0. | 
| [runit](https://wiki.gentoo.org/wiki/Runit) | [sys-process/runit](https://packages.gentoo.org/packages/sys-process/runit) | lightweight process supervision suite, originally inspired by [daemontools](https://wiki.gentoo.org/wiki/Daemontools) that offers fast and reliable service management. | 
| [s6 + s6-rc](https://wiki.gentoo.org/wiki/S6_and_s6-rc-based_init_system) |  | an init system built using components from the [s6](https://wiki.gentoo.org/wiki/S6), [s6-rc](https://wiki.gentoo.org/wiki/S6-rc) and [s6-linux-init](https://wiki.gentoo.org/wiki/S6-linux-init) packages | 
| [sysvinit](https://wiki.gentoo.org/wiki/Sysvinit) | [sys-apps/sysvinit](https://packages.gentoo.org/packages/sys-apps/sysvinit) | a collection of System V-style init programs originally written by Miquel van Smoorenburg | 
| [systemd](https://wiki.gentoo.org/wiki/Systemd) | [sys-apps/systemd](https://packages.gentoo.org/packages/sys-apps/systemd) | a modern SysV-style init and [rc](https://wiki.gentoo.org/wiki/Rc) replacement for Linux systems. | 

| [sys-apps/s6](https://packages.gentoo.org/packages/sys-apps/s6) | 
| [sys-apps/s6-rc](https://packages.gentoo.org/packages/sys-apps/s6-rc) | 

## See also

- [Comparison of init systems](https://wiki.gentoo.org/wiki/Comparison_of_init_systems) — compares and contrasts **[init systems]** for Unix(like) [OSs](https://en.wikipedia.org/wiki/Operating_system)
