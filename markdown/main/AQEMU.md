<!-- source: https://wiki.gentoo.org/wiki/AQEMU | group: Gentoo Wiki (Main) | wiki-title: AQEMU -->
---
title: AQEMU
url: https://wiki.gentoo.org/wiki/AQEMU
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2025-08-27"
fingerprint: d6c3b90f07db8970
license: CC BY-SA 4.0
---

# AQEMU

From Gentoo Wiki

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

AQEMU is a user-friendly GUI front-end to the [QEMU](https://wiki.gentoo.org/wiki/QEMU) and [KVM](https://wiki.gentoo.org/wiki/KVM) emulators. The AQEMU front-end is written using the Qt5 framework.

## Installation

### USE flags

Cannot load package information. Is the atom *app-emulation/aqemu* correct?

### Emerge

Install aqemu:

`root #``emerge --ask app-emulation/aqemu`
## Configuration

Configuration for AQEMU is a breeze. Simply follow the setup wizard to create a virtual hard drive, then set up a virtual machine.

### Enabling VM graphical output

In version 0.8.2-r2 or lower, there is no setting to enable the graphical output from the virtual machines. To have the machine display output to a GTK window an additional option currently not "supported" in AQEMU is needed. Presuming a virtual machine has already been created, follow these instructions:

1. Start AQEMU.
2. Click the *Advanced* tab.
3. Under *Additional QEME/KVM Arguments* enter `-display gtk` in the textbox then click *Apply*.
4. Start the virtual machine. Output should be displayed in a GTK window.

## See also

- [QEMU](https://wiki.gentoo.org/wiki/QEMU) — a generic, open-source hardware emulator and virtualization suite.
