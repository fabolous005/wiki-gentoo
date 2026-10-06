<!-- source: https://wiki.gentoo.org/wiki//etc/portage/package.license | group: Gentoo Wiki (Main) | wiki-title: /etc/portage/package.license -->
---
title: "/etc/portage/package.license"
url: https://wiki.gentoo.org/wiki//etc/portage/package.license
hostname: gentoo.org
sitename: "/etc/portage/package.license"
date: "2026-07-13"
fingerprint: "6d693f6c0fabb1f9"
license: CC BY-SA 4.0
---

# /etc/portage/package.license

From Gentoo Wiki

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

The **/etc/portage/package.license** file contains definitions for per-package `ACCEPT_LICENSE` statements. Some packages will only be allowed to be installed by accepting its license. Licenses are stored in /var/db/repos/gentoo/licenses/. This file, like package.use, can also be a directory.

## Example

`root #``emerge -pv linux-firmware`
These are the packages that would be merged, in order:
 
Calculating dependencies... done!
Dependency resolution took 1.14 s.
 
!!! All ebuilds that could satisfy "linux-firmware" have been masked.
!!! One of the following masked packages is required to complete your request:
- sys-kernel/linux-firmware-99999999::gentoo (masked by: || ( ) linux-fw-redistributable license(s), missing keyword)
A copy of the 'linux-fw-redistributable' license is located at '/var/db/repos/gentoo/licenses/linux-fw-redistributable'.
 
- sys-kernel/linux-firmware-20230117::gentoo (masked by: || ( ) linux-fw-redistributable license(s))
- sys-kernel/linux-firmware-20221214::gentoo (masked by: || ( ) linux-fw-redistributable license(s))
- sys-kernel/linux-firmware-20221109::gentoo (masked by: || ( ) linux-fw-redistributable license(s))
- sys-kernel/linux-firmware-20221012-r1::gentoo (masked by: || ( ) linux-fw-redistributable license(s))
- sys-kernel/linux-firmware-20221012::gentoo (masked by: || ( ) linux-fw-redistributable license(s))
- sys-kernel/linux-firmware-20220913-r2::gentoo (masked by: || ( ) linux-fw-redistributable license(s))
- sys-kernel/linux-firmware-20220815::gentoo (masked by: || ( ) linux-fw-redistributable license(s))
- sys-kernel/linux-firmware-20220708::gentoo (masked by: || ( ) linux-fw-redistributable license(s))
- sys-kernel/linux-firmware-20220610::gentoo (masked by: || ( ) linux-fw-redistributable license(s))
- sys-kernel/linux-firmware-20220509::gentoo (masked by: || ( ) linux-fw-redistributable license(s))
- sys-kernel/linux-firmware-20220411::gentoo (masked by: || ( ) linux-fw-redistributable license(s))
- sys-kernel/linux-firmware-20220310::gentoo (masked by: || ( ) linux-fw-redistributable license(s))
- sys-kernel/linux-firmware-20220209::gentoo (masked by: || ( ) linux-fw-redistributable license(s))
 
For more information, see the MASKED PACKAGES section in the emerge
man page or refer to the Gentoo Handbook.

In this case, the linux-fw-redistributable license must be accepted. To do this, create:

FILE **`/etc/portage/package.license`****package.license linux-firmware example**

```
# Accepting the license for linux-firmware
sys-kernel/linux-firmware linux-fw-redistributable
# Accepting any license that permits redistribution
sys-kernel/linux-firmware @BINARY-REDISTRIBUTABLE
```
## Format and examples

- Comment lines begin with `#` (no inline comments).
- One `DEPEND` atom per line followed by additional  licenses or groups.

FILE **`/etc/portage/package.license`****package.license example**

```
# Accepting google-chrome license for www-client/google-chrome for version equal or greater than 42.0.2311.90_p1
>=www-client/google-chrome-42.0.2311.90_p1 google-chrome
# Accepting google-chrome license for any version of www-client/google-chrome
www-client/google-chrome google-chrome
# Accepting google-chrome license for every www-client package at any version
www-client/* google-chrome
# Accepting google-chrome license for every package at any version
*/* google-chrome
# Accepting every license for every package at any version expect EULA (See below.)
*/*  * -EULA
```
Setting `*/*  *` is not recommended as some EULA licenses need to be carefully read for the users protection.

## See also

- [Handbook:AMD64/Working/Portage#Licenses](https://wiki.gentoo.org/wiki/Handbook:AMD64/Working/Portage#Licenses)
- [License groups](https://wiki.gentoo.org/wiki/License_groups) — enable the system's package manager to allow or disallow the installation of certain categories of software based on license compatibility.
- [Project:Licenses](https://wiki.gentoo.org/wiki/Project:Licenses) — tries to make heads and tails of license terms
