<!-- source: https://wiki.gentoo.org/wiki/Icu | group: Gentoo Wiki (Main) | wiki-title: Icu -->
---
title: icu
url: https://wiki.gentoo.org/wiki/Icu
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2024-01-11"
fingerprint: ea00414e8e4194ca
license: CC BY-SA 4.0
---

# icu

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)


**icu** is the "International Components for Unicode", a library used by other programs requiring [Unicode](https://wiki.gentoo.org/wiki/Unicode) support.

## Installation

### USE flags


| [debug](https://packages.gentoo.org/useflags/debug) | Enable extra debug codepaths, like asserts and extra output. If you want to get meaningful backtraces see https://wiki.gentoo.org/wiki/Project:Quality\_Assurance/Backtraces | 
| [doc](https://packages.gentoo.org/useflags/doc) | Add extra documentation (API, Javadoc, etc). It is recommended to enable per package instead of globally | 
| [examples](https://packages.gentoo.org/useflags/examples) | Install examples, usually source code | 
| [static-libs](https://packages.gentoo.org/useflags/static-libs) | Build static versions of dynamic libraries as well | 
| [test](https://packages.gentoo.org/useflags/test) | Enable dependencies and/or preparations necessary to run tests (usually controlled by FEATURES=test but can be toggled independently) | 
| [verify-sig](https://packages.gentoo.org/useflags/verify-sig) | Verify upstream signatures on distfiles | 

### Emerge

The [dev-libs/icu](https://packages.gentoo.org/packages/dev-libs/icu) package usually doesn't need to be installed manually; it should be pulled in as-required when installing other software that uses it.

`root #``emerge --ask dev-libs/icu`
## Upgrading

When a new version of [dev-libs/icu](https://packages.gentoo.org/packages/dev-libs/icu) becomes available, trying to update one's system via **emerge** can result in copious output along the lines of:

```
WARNING: One or more updates/rebuilds have been skipped due to a dependency conflict:
dev-libs/icu:0
 (dev-libs/icu-74.1:0/74.1::gentoo, ebuild scheduled for merge) USE="-debug -doc -examples -static-libs -test -verify-sig" ABI_X86="32 (64) (-x32)" conflicts with
   dev-libs/icu:0/73.1= required by (dev-libs/xerces-c-3.2.4-r2:0/0::gentoo, installed) USE="iconv icu -curl -doc -examples -static-libs -test -threads" ABI_X86="(64)" CPU_FLAGS_X86="sse2"
               ^^^^^^^^
   dev-libs/icu:0/73.1= required by (x11-libs/vte-0.74.2:2.91/2.91::gentoo, installed) USE="crypt icu introspection vala -debug -gtk-doc -systemd -vanilla" ABI_X86="(64)"
               ^^^^^^^^
   dev-libs/icu:0/73.1=[abi_x86_64(-)] required by (net-fs/samba-4.18.8:0/0::gentoo, installed) USE="acl client cups pam regedit system-mitkrb5 zeroconf -addc -ads -ceph -cluster -debug (-fam) -glusterfs -gpg -iprint -json -ldap -llvm-libunwind -profiling-data -python -quota (-selinux) -snapper -spotlight -syslog (-system-heimdal) -systemd (-test) -unwind -winbind" ABI_X86="(64) -32 (-x32)" CPU_FLAGS_X86="aes" PYTHON_SINGLE_TARGET="python3_11 -python3_10"
               ^^^^^^^^
```
The simplest way to upgrade is to directly ask **emerge** to install the new version. For example, if the new version is 74.1:

`root #``emerge --ask --ignore-world dev-libs/icu:0/74.1`
Once installation is completed, rebuild the relevant libraries:

`root #``emerge @preserved-rebuild`
However, note that if any installed packages require an older version of [dev-libs/icu](https://packages.gentoo.org/packages/dev-libs/icu), **emerge** will want to downgrade the package to that version; in that case, those packages will need updates before the latest version of [dev-libs/icu](https://packages.gentoo.org/packages/dev-libs/icu) can be used on the system.
