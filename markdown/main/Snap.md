<!-- source: https://wiki.gentoo.org/wiki/Snap | group: Gentoo Wiki (Main) | wiki-title: Snap -->
---
title: Snap
url: https://wiki.gentoo.org/wiki/Snap
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2025-09-25"
fingerprint: cb53952e49d72914
license: CC BY-SA 4.0
---

# Snap

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**[Snap](https://snapcraft.io/)** is a software management and deployment system that enables distribution-independent software deployment. The packages are called 'snaps' and one tool for using them is '[snapd](https://packages.gentoo.org/packages/app-containers/snapd)'. Canonical, the developer of Snap, manages the [Snap Store](https://snapcraft.io/store) service through which snaps are deployed.
snapd is a REST API daemon for managing snap packages. Users can interact with it using the snap client, which is part of the same package.

## Installation

### USE flags


| [+forced-devmode](https://packages.gentoo.org/useflags/+forced-devmode) | Automatically disable application confinement if feature detection fails. | 
| [apparmor](https://packages.gentoo.org/useflags/apparmor) | Enable support for the AppArmor application security system | 
| [gtk](https://packages.gentoo.org/useflags/gtk) | Add support for x11-libs/gtk+ (The GIMP Toolkit) | 
| [kde](https://packages.gentoo.org/useflags/kde) | Add support for software made by KDE, a free software community | 
| [systemd](https://packages.gentoo.org/useflags/systemd) | Enable use of systemd-specific libraries and features like socket activation or session tracking | 

### Emerge

`root #``emerge --ask app-containers/snapd`
## Basic usage

To install an application, e.g. Thunderbird, run:

`root #``snap install thunderbird`
To remove an application, e.g. Thunderbird, run:

`root #``snap remove thunderbird`
To run the application use created .desktop file or run:

`user $``snap run thunderbird`
To update installed applications and runtimes:

`root #``snap refresh`
To create the snap directory manually, run:

`root #``mkdir -p /var/lib/snapd/snaps`
## Systemd Users

To enable and start the Snap daemon after emerging, run:

`root #``systemctl enable --now snapd.socket`
## Graphical management

The Snap Store can be installed via snap

`root #``snap install snap-store`
## Troubleshooting

Filesystem uses lzo compression Erro.
Enable [use flags](https://wiki.gentoo.org/wiki/Package.use) "lzo" on [sys-fs/squashfs-tools](https://packages.gentoo.org/packages/sys-fs/squashfs-tools)

## Support

If you have any further questions, the [Snapcraft forum](https://forum.snapcraft.io/) is a great place to go.

## See also

- [Docker](https://wiki.gentoo.org/wiki/Docker) — a [container](<https://en.wikipedia.org/wiki/Container_(virtualization)>)-based [virtualization](https://wiki.gentoo.org/wiki/Virtualization) system
- [LXD](https://wiki.gentoo.org/wiki/LXD) — a system container manager
- [systemd/systemd-nspawn](https://wiki.gentoo.org/wiki/Systemd/systemd-nspawn) — a lightweight, loosely [chroot](https://wiki.gentoo.org/wiki/Chroot)-like, OS-level [OCI container](https://opencontainers.org/) environment native to [systemd](https://wiki.gentoo.org/wiki/Systemd).
- [Flatpak](https://wiki.gentoo.org/wiki/Flatpak) — a package management framework aiming to provide support for sandboxed, distro-agnostic binary packages for Linux desktop applications.
- [Security Handbook/Linux Security Modules/AppArmor](https://wiki.gentoo.org/wiki/Security_Handbook/Linux_Security_Modules/AppArmor)
