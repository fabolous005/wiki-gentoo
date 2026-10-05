<!-- source: https://wiki.gentoo.org/wiki/Comparison_of_init_systems | group: Gentoo Wiki (Main) | wiki-title: Comparison of init systems -->
---
title: Comparison of init systems
url: https://wiki.gentoo.org/wiki/Comparison_of_init_systems
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-08-14"
fingerprint: "1138301e9b93b3ec"
license: CC BY-SA 4.0
---

# Comparison of init systems

From Gentoo Wiki

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

This article compares and contrasts **[init systems](https://wiki.gentoo.org/wiki/Init_system)** for Unix(like) [OSs](https://en.wikipedia.org/wiki/Operating_system), irrespective of whether they are available for Gentoo or not. See the [init system](https://wiki.gentoo.org/wiki/Init_system) ([meta](https://wiki.gentoo.org/wiki/Meta_article)) article for init system software available in Gentoo.

## Init system comparison table

| Feature | Init system |  |  |  |  |  |  |  |  |  |  |  |  | 
|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
|  | [sysvinit](https://wiki.gentoo.org/wiki/Sysvinit) | [OpenRC](https://wiki.gentoo.org/wiki/OpenRC) | [systemd](https://wiki.gentoo.org/wiki/Systemd) | SMF | launchd | Epoch | finit | [runit](https://wiki.gentoo.org/wiki/Runit) | [s6](https://wiki.gentoo.org/wiki/S6) + [s6-rc](https://wiki.gentoo.org/wiki/S6-rc) | [66](https://docs.obarun.org/66/0.9.0.0/) | BSD rc.d | [dinit](https://wiki.gentoo.org/wiki/Dinit) |  | 
| Officially supported Gentoo init | partially (used by OpenRC) |  |  |  |  |  |  |  |  |  |  |  |  | 
| Package / Bug# | [sys-apps/sysvinit](https://packages.gentoo.org/packages/sys-apps/sysvinit) | [sys-apps/openrc](https://packages.gentoo.org/packages/sys-apps/openrc) | [sys-apps/systemd](https://packages.gentoo.org/packages/sys-apps/systemd) | - | - | [sys-apps/epoch](https://packages.gentoo.org/packages/sys-apps/epoch) | - | [sys-process/runit](https://packages.gentoo.org/packages/sys-process/runit) | [sys-apps/s6](https://packages.gentoo.org/packages/sys-apps/s6) + [sys-apps/s6-rc](https://packages.gentoo.org/packages/sys-apps/s6-rc) | - | - | [sys-apps/dinit::guru](https://github.com/gentoo-mirror/guru/tree/master/sys-apps/dinit) [sys-apps/dinit-services::guru](https://github.com/gentoo-mirror/guru/tree/master/sys-apps/dinit-services) |  | 
| Supported platforms | Linux / BSD | Linux + BSD | Linux | Solaris | Darwin | Linux | Linux | Linux / BSD / Darwin | Linux / BSD / Darwin | Linux | BSD | Linux / BSD / Darwin |  | 
| Main coding language | C | POSIX shell (+ C) | C | C | C | C | C | C | C | C | POSIX shell (+ C) | C++ |  | 
| Main dependencies | - | init (sysv or BSD) | [D-Bus](https://wiki.gentoo.org/wiki/D-Bus) | init(sysv?) | - | libc, /bin/sh | ? | - | s6, execline | libc, oblibs | rcorder | - |  | 
| Init script/service format | single config file | shell scripts | config files (ini) | XML (+ shell scripts) | plist | multiple or single .conf | multiple or single .conf | shell scripts | execline or shell scripts | config files (INI) + execline or shell scripts | shell scripts | config files |  | 
| Per-service configuration |  |  |  |  | ? |  | ? |  |  |  |  |  |  | 
| Running as a daemon |  |  |  |  |  |  |  |  | [sys-apps/s6-linux-init](https://packages.gentoo.org/packages/sys-apps/s6-linux-init)) |  |  |  |  | 
| Cross-service dependencies/events |  |  |  |  |  |  | ? |  |  |  |  |  |  | 
| Parallel service startup |  |  |  |  |  |  |  |  |  |  |  |  |  | 
| Keeping daemons alive |  |  |  |  |  |  |  |  |  |  |  |  |  | 
| Preferred service file supplier | n/a | Gentoo | upstream | Solaris | MacOS | n/a | n/a | Void Linux | Artix Linux | Obarun | NetBSD, FreeBSD, OpenBSD | Artix Linux, Chimera Linux |  | 
| License | GPL v2+ | 2-cl. BSD | LGPL v2.1+ | ? | Apache License 2.0 | Unlicense | MIT | BSD | ISC | ISC | BSD | Apache License 2.0 |  | 

## OpenRC compared to systemd

| Feature | [OpenRC](https://wiki.gentoo.org/wiki/OpenRC) | [systemd](https://wiki.gentoo.org/wiki/Systemd) | 
|---|---|---|
| [Filesystem](https://wiki.gentoo.org/wiki/Filesystem) mounting | One script per group (root, local, network, [swap](https://wiki.gentoo.org/wiki/Swap), etc.). | Two units per [mount](https://wiki.gentoo.org/wiki/Mount) point (fsck + mount), runtime-generated with dependencies. | 
| getty (terminal prompts) | Started through /etc/inittab or via agetty script | One unit per console, instantiated from template on-demand. | 
| Networking setup | Several options like [dhcpcd](https://wiki.gentoo.org/wiki/Dhcpcd)<sup>[\[1\]](https://wiki.gentoo.org#cite_note-1)</sup><sup>[\[2\]](https://wiki.gentoo.org#cite_note-2)</sup>, [netifrc](https://wiki.gentoo.org/wiki/Netifrc), [iwd](https://wiki.gentoo.org/wiki/Iwd), or [NetworkManager](https://wiki.gentoo.org/wiki/NetworkManager).<sup>[\[3\]](https://wiki.gentoo.org#cite_note-3)</sup> | Integrated ( [systemd-networkd](https://wiki.gentoo.org/wiki/Systemd#systemd-networkd)), or using any of the external options mentioned prior. | 
| [X11 Display Manager](https://wiki.gentoo.org/wiki/Display_manager) setup | Single service for all (required to auto-restart). | Separate Display Manager units. | 

## See also

- [Dinit](https://wiki.gentoo.org/wiki/Dinit) — service supervisor with dependency support which can also act as an [init system](https://wiki.gentoo.org/wiki/Init_system)
- [init system](https://wiki.gentoo.org/wiki/Init_system) — An **init system** is the first program, other than the kernel, to be run after a Linux distribution is booted
- [OpenRC](https://wiki.gentoo.org/wiki/OpenRC) — a dependency-based [init system](https://wiki.gentoo.org/wiki/Init_system) for Unix-like systems that maintains compatibility with the system-provided init system (see the [openrc-init sub-article](https://wiki.gentoo.org/wiki/OpenRC/openrc-init)).
- [Runit](https://wiki.gentoo.org/wiki/Runit) — lightweight process supervision suite, originally inspired by [daemontools](https://wiki.gentoo.org/wiki/Daemontools) that offers fast and reliable service management.
- [S6 and s6-rc-based init system](https://wiki.gentoo.org/wiki/S6_and_s6-rc-based_init_system) — an init system built using components from the [s6](https://wiki.gentoo.org/wiki/S6), [s6-rc](https://wiki.gentoo.org/wiki/S6-rc) and [s6-linux-init](https://wiki.gentoo.org/wiki/S6-linux-init) packages
- [systemd](https://wiki.gentoo.org/wiki/Systemd) — a modern SysV-style init and [rc](https://wiki.gentoo.org/wiki/Rc) replacement for Linux systems.
- [User:AdibSaad/66](https://wiki.gentoo.org/wiki/User:AdibSaad/66) — 66 + 66-rc guide. Warning: status of instructions unknown. \[Instructions no longer relevant as many breaking releases have since taken place; the overlay is no longer hosted. New overlay and guide will soon be written\]

## External resources

- [s6 - Forum thread](https://forums.gentoo.org/viewtopic-t-994548.html)
- [Forum thread](https://forums.gentoo.org/viewtopic.php?p=7663768)
- [openrc-init](https://github.com/OpenRC/openrc/commit/13ca79856e5836117e469c3edbcfd4bf47b6bab0)
- [GNU shepherd](https://www.gnu.org/software/shepherd/) - service manager for the GNU OS.
- [Finit](https://troglobit.com/projects/finit/) - Fast init for Linux systems.
- [66](https://docs.obarun.org/66/latest/) - Init and service manager.
- [66 vs others](https://docs.obarun.org/66/0.9.0.0/66-vs-other-init-systems.html) - Objective comparison between 66 and other init/supervision systems.
- [66 overlay](https://github.com/pramodvu1502/66-svmgr-gentoo-overlay) - Overlay for the 66 service management suite (service definitions coming soon) (The link to the repo will change soon...)
- [Dinit](https://davmac.org/projects/dinit/)
- "[Comparison of Dinit with other supervision / init systems](https://github.com/davmac314/dinit/blob/master/doc/COMPARISON)" - by the developer of Dinit
