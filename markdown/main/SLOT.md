<!-- source: https://wiki.gentoo.org/wiki/SLOT | group: Gentoo Wiki (Main) | wiki-title: SLOT -->
---
title: SLOT
url: https://wiki.gentoo.org/wiki/SLOT
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2024-04-21"
fingerprint: "5f7d6f0f8730af8e"
license: CC BY-SA 4.0
---

# SLOT

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

The `SLOT`[ebuild](https://wiki.gentoo.org/wiki/Ebuild) variable specifies a package version's **slot**. Slots can allow multiple versions of a package to be installed and managed simultaneously by Portage.

## Version specifiers

Slotted versions are denoted by adding a colon (`:`) plus the slot name after the package version. For example, slot 3 of x11-libs/gtk+-3.24.39::gentoo can be specified using x11-libs/gtk+-3.24.39:3::gentoo.

## Behavior

Every package version has one slot. Versions in different slots can be installed simultaneously. For example, to install [sys-kernel/gentoo-kernel-6.6.21:**6.6.21**](https://packages.gentoo.org/packages/sys-kernel/gentoo-kernel) alongside [sys-kernel/gentoo-kernel-6.1.81:**6.1.81**](https://packages.gentoo.org/packages/sys-kernel/gentoo-kernel), the following command can be used:

`root #``emerge --ask gentoo-kernel:6.6.21 gentoo-kernel:6.1.81`
This installs [sys-kernel/gentoo-kernel-6.6.21](https://packages.gentoo.org/packages/sys-kernel/gentoo-kernel) into slot 6.6.21 and [sys-kernel/gentoo-kernel-6.1.81](https://packages.gentoo.org/packages/sys-kernel/gentoo-kernel) into slot 6.1.81:

`user $``eselect kernel list`
Available kernel symlink targets:
  \[1\]   linux-6.1.81-gentoo \*
  \[2\]   linux-6.6.21-gentoo

A package version can only be installed into its own slot. Thus, [sys-kernel/gentoo-kernel-6.1.81](https://packages.gentoo.org/packages/sys-kernel/gentoo-kernel) cannot be installed into slot 6.6.21, 6, or foo.

## How different packages use slotting

Most packages don't use slots.

As can be inferred from section [Behavior](https://wiki.gentoo.org#Behavior), [sys-kernel/gentoo-kernel](https://packages.gentoo.org/packages/sys-kernel/gentoo-kernel) uses a different slot for each version. Some packages, such as [sys-devel/gcc](https://packages.gentoo.org/packages/sys-devel/gcc), use different slots for different *major revisions* — [sys-devel/gcc-13.2.1\_p20240210:**13**](https://packages.gentoo.org/packages/sys-devel/gcc) can be installed alongside [sys-devel/gcc-14.0.1\_pre20240317:**14**](https://packages.gentoo.org/packages/sys-devel/gcc), but not alongside [sys-devel/gcc-13.2.1\_p20240113-r1:**13**](https://packages.gentoo.org/packages/sys-devel/gcc).

In general, if two package versions own a few of the same files, they cannot be installed simultaneously, so they are placed in the same slot. Packages such as [sys-kernel/gentoo-kernel](https://packages.gentoo.org/packages/sys-kernel/gentoo-kernel) and [sys-devel/gcc](https://packages.gentoo.org/packages/sys-devel/gcc) incorporate the slot into the installed filenames (e.g. /boot/kernel-6.8.6-gentoo-dist and /usr/x86\_64-pc-linux-gnu/gcc-bin/13/gcc), eliminating conflicts between slots.

## Listing package slots with eix

eix (from the [app-portage/eix](https://packages.gentoo.org/packages/app-portage/eix) package) can be used to list a single package's slotted versions.

Before performing any operations using eix, update its database:

`user $``eix-update`
List the available versions:

`user $````
eix -e dev-lang/lua
```
```
[I] dev-lang/lua
     Available versions:  
     (5.1)  5.1.5-r200
     (5.3)  5.3.6-r102
     (5.4)  5.4.6
       {+deprecated readline}
     Installed versions:  5.1.5-r200(5.1)(06:33:41 25/03/2024)(deprecated readline) 5.4.6(5.4)(04:58:26 25/03/2024)(deprecated readline)
     Homepage:            https://www.lua.org/
     Description:         A powerful light-weight programming language designed for extending applications</blockquote>
```
The slots are shown in parentheses (5.1, 5.3 and 5.4 in this case).

To prevent packages from being installed into certain slots, add them to [/etc/portage/package.mask](https://wiki.gentoo.org/wiki//etc/portage/package.mask).

## See also

## External resources

- [Slotting](https://devmanual.gentoo.org/general-concepts/slotting/index.html) — a detailed guide for ebuild developers.
- [Package and Slot Moves](https://devmanual.gentoo.org/ebuild-maintenance/package-moves/#changing-ebuild's-slot) — how slot moves should be done.
