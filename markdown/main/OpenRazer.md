<!-- source: https://wiki.gentoo.org/wiki/OpenRazer | group: Gentoo Wiki (Main) | wiki-title: OpenRazer -->
---
title: OpenRazer
url: https://wiki.gentoo.org/wiki/OpenRazer
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-05-01"
fingerprint: db1bc4566bc905ae
license: CC BY-SA 4.0
---

# OpenRazer

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**OpenRazer** is a community-led effort to support [Razer](https://www.razer.com/) peripherals on Linux.

## Installation

### USE flags


| [+client](https://packages.gentoo.org/useflags/+client) | Build the OpenRazer daemon client | 
| [+daemon](https://packages.gentoo.org/useflags/+daemon) | Build the OpenRazer daemon service | 
| [+strip](https://packages.gentoo.org/useflags/+strip) | Allow symbol stripping to be performed by the ebuild for special files | 
| [dist-kernel](https://packages.gentoo.org/useflags/dist-kernel) | Enable subslot rebuilds on Distribution Kernel upgrades | 
| [modules-compress](https://packages.gentoo.org/useflags/modules-compress) | Install compressed kernel modules (if kernel config enables module compression) | 
| [modules-sign](https://packages.gentoo.org/useflags/modules-sign) | Cryptographically sign installed kernel modules (requires CONFIG\_MODULE\_SIG=y in the kernel) | 
| [test](https://packages.gentoo.org/useflags/test) | Enable dependencies and/or preparations necessary to run tests (usually controlled by FEATURES=test but can be toggled independently) | 

### Emerge

`root #``emerge --ask sys-apps/openrazer`
### Additional software

Correctly OpenRazer can act as a backed for following packages:

- [sys-apps/razercommander](https://packages.gentoo.org/packages/sys-apps/razercommander) - a GUI OpenRazer client application,
- [sys-apps/polychromatic](https://packages.gentoo.org/packages/sys-apps/polychromatic) - most feature-fill GUI configuration tool, providing also a CLI interface and a system tray icon,
- [sys-apps/razer-cli](https://packages.gentoo.org/packages/sys-apps/razer-cli) - most minimal, CLI-only OpenRazer client.

## Configuration

### Kernel modules

The kernel modules should be automatically loaded by Udev, but can also be loaded by using the "modules" system service.

### Service

#### systemd

Upstream provides a systemd user service which can be enabled by the users running systemd.

## Usages

OpenRazer provides a kernel module as well as a client and server implementations. Currently all OpenRazer clients rely on that server called "openrazer-daemon". So, to make all GUI/TUI clients work correctly, first the "openrazer-daemon" has to be started. This can be done by either adding it to the desktop startup scripts, xinitrc script or manually starting from the command-line:

`user $``systemctl --user enable --now openrazer-daemon.service`
## Removal

### Unmerge

`root #``emerge --ask --depclean --verbose sys-apps/openrazer`
## Troubleshooting

### Device not recognized / permission denied on sysfs

When diagnosing issues, the OpenRazer daemon log is the first place to look. It can be found at: \~/.local/share/openrazer/logs/razer.log

To view the log:

`user $``cat ~/.local/share/openrazer/logs/razer.log`
If the daemon log reports an error such as:

CRITICAL | Could not access /sys/.../device\_type, file is not owned by plugdev

This is caused by two separate issues that must both be resolved.

#### Add user to the plugdev group

The user running the daemon must be a member of the plugdev group. To add the current user:

`root #``gpasswd -a $USER plugdev`
#### Symlink the udev rules

OpenRazer installs its udev rules to /usr/lib/udev/rules.d/99-razer.rules, however they may not be applied correctly without a symlink into /etc/udev/rules.d/:

`root #``ln -s /usr/lib/udev/rules.d/99-razer.rules /etc/udev/rules.d/99-razer.rules`
After completing both steps, reboot the system. The device sysfs files should now be owned by root:plugdev and the daemon should initialize correctly.
