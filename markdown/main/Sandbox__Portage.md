<!-- source: https://wiki.gentoo.org/wiki/Sandbox_(Portage) | group: Gentoo Wiki (Main) | wiki-title: Sandbox (Portage) -->
---
title: Sandbox (Portage)
url: https://wiki.gentoo.org/wiki/Sandbox_(Portage)
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2023-03-12"
fingerprint: cccfa1ade272f85
license: CC BY-SA 4.0
---

# Sandbox (Portage)

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

Sandbox is a library (and helper utility) to run programs in a "sandboxed" environment, i.e. to restrict a process's access to system ressources. This is used as a QA measure to try and prevent applications from modifying files they should not.

In Gentoo, it is used to build applications as root, making sure that the build system does not do anything harmful outside of its build directory - such as install files to the live root file system or modify config files on the fly.

For people who are familiar with the Debian "fakeroot" project or the RPM based "InstallWatch", sandbox is in the same vein of these projects.

## How sandboxing works

When using the sandbox, some environment variables are set to provide some configuration to sandbox, then the LD\_PRELOAD variable is set.

When the ELF loader runs, it will now load the sandbox library first. Whenever applications make a library call that is wrapped by sandbox, the arguments will be checked and any access that is not permitted is logged and an error is returned. Any access that is permitted is of course forwarded along to the real C library.

Static ELFs and setuid/setgid programs require [another method](https://gitweb.gentoo.org/proj/sandbox.git/tree/README.md).

## Installation

All Gentoo installations come with Sandbox. Like all data, there is a possibility Sandbox can become corrupted or even uninstalled, which is *very* bad. If this is the case there *are* ways Sandbox can be recovered.

### Emerge

`root #``emerge --ask --oneshot sys-apps/sandbox`
## Configuration

### Files

There are multiple files used to configure Sandbox.

See [/etc/sandbox.conf](https://wiki.gentoo.org/wiki//etc/sandbox.conf) and /etc/sandbox.d/\* configuration files for more information.
