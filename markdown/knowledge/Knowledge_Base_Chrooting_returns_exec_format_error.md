<!-- source: https://wiki.gentoo.org/wiki/Knowledge_Base:Chrooting_returns_exec_format_error | group: Gentoo Knowledge | wiki-title: Knowledge_Base:Chrooting_returns_exec_format_error -->
---
title: Knowledge Base:Chrooting returns exec format error
url: https://wiki.gentoo.org/wiki/Knowledge_Base:Chrooting_returns_exec_format_error
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2020-07-19"
fingerprint: "78bf7e0f2ca6c994"
license: CC BY-SA 4.0
---

# Knowledge Base:Chrooting returns exec format error

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

## Synopsis

During the installation of Gentoo Linux, attempting to chroot into the new environment breaks with the following error:

`root #``chroot /mnt/gentoo /bin/bash`
chroot: failed to run command \`/bin/bash': Exec format error

## Environment

This article applies to Gentoo Linux installations on an x86\_64 platform (**amd64** architecture).

## Analysis

The error *Exec format error* means that the binary being executed is made for a different architecture than the environment currently booted. It usually occurs when the system has been booted on a 32-bit system when a 64-bit environment is trying to load.

## Resolution

Reboot the live environment and choose the correct architecture (most LiveCDs support a 64-bit kernel as well as a 32-bit option, although it is not booted by default). Look for entries labeled **gentoo64** or **linux64** if trying to boot a 64-bit system.

## See also

- [Chroot](https://wiki.gentoo.org/wiki/Chroot) — a Unix system utility used to change the apparent root directory to create a new environment logically separate from the main system's root directory.
