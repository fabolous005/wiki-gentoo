<!-- source: https://wiki.gentoo.org/wiki/Bubblewrap | group: Gentoo Wiki (Main) | wiki-title: Bubblewrap -->
---
title: Bubblewrap
url: https://wiki.gentoo.org/wiki/Bubblewrap
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2024-04-11"
fingerprint: "8d305d3e51eb7400"
license: CC BY-SA 4.0
---

# Bubblewrap

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**Bubblewrap** is a low-level unprivileged sandboxing tool used by [Flatpak](https://wiki.gentoo.org/wiki/Flatpak). Bubblewrap makes extensive use of user namespaces in the Linux kernel to allow unprivileged users to sandbox programs.

## Installation

### USE flags


| [debug](https://packages.gentoo.org/useflags/debug) | Enable extra debug codepaths, like asserts and extra output. If you want to get meaningful backtraces see https://wiki.gentoo.org/wiki/Project:Quality\_Assurance/Backtraces | 
| [selinux](https://packages.gentoo.org/useflags/selinux) | !!internal use only!! Security Enhanced Linux support, this must be set by the selinux profile or breakage will occur | 
| [suid](https://packages.gentoo.org/useflags/suid) | Enable setuid root program(s) | 
| [test](https://packages.gentoo.org/useflags/test) | Enable dependencies and/or preparations necessary to run tests (usually controlled by FEATURES=test but can be toggled independently) | 

The `suid` USE flag can be used to support using bubblewrap without user namespaces by setting suid on the `bwrap` binary.

### Emerge

`root #``emerge --ask sys-apps/bubblewrap`
### Kernel

User namespaces can be enabled in the kernel so that `suid` is not required on the `bwrap` binary:

**Enabling user namespaces**

## Troubleshooting

## Possible obstacles

### User namespaces not available in the current kernel

Make sure user namespaces are enabled in the kernel or enable the `suid` USE flag. `CONFIG_USER_NS=y`
