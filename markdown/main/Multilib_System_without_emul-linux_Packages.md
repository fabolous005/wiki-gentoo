<!-- source: https://wiki.gentoo.org/wiki/Multilib_System_without_emul-linux_Packages | group: Gentoo Wiki (Main) | wiki-title: Multilib System without emul-linux Packages -->
---
title: Multilib System without emul-linux Packages
url: https://wiki.gentoo.org/wiki/Multilib_System_without_emul-linux_Packages
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2021-11-12"
fingerprint: "23b1636e8007b74b"
license: CC BY-SA 4.0
---

# Multilib System without emul-linux Packages

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

This article is **deprecated (obsolete)**. Contents are <u>no longer relevant</u>, and are intended for historical reference only!

TLDR:

**Do not use this article!**

## Why this article is outdated

Read the [news item](https://gentoo.org/support/news-items/2015-03-28-true-multilib.html)

## Where to find updated information

Read the [Project page](https://wiki.gentoo.org/wiki/Project:Multilib/Concepts)

## Introduction

For 64bit systems it is sometimes necessary to execute 32bit binaries, e.g. if the application is only available as 32bit binary like Opera, Steam, Windows applications and games via wine, ... To achieve this, Gentoo has a multilib profile which offers some emul-linux packages that bundle some 32bit libraries. So if an application like Opera, Steam or wine gets installed, then it pulls the emul-linux packages it needs to run. This approach works but has at least two drawbacks:

- The bundled libraries are several months old (based on the stable libraries in Gentoo). Owners of relatively modern graphic cards (1-2 years old) who want to play a game once in a while (without proprietary drivers) need the latest available drivers and libraries because versions older than a month may render even old games completely unplayable. If the performance is excellent with multimedia and desktop applications but very low with wine and steam (native or not), then the problem lies with the emul-linux packages.
- Prepackaged software compiled by other people is not the Gentoo way where the user wants to set his USE-flags and compiler flags etc. by himself.

But the end of emul-linux is coming: Several months ago Gentoo packages introduced a new flag (abi\_x86\_32) which will also create 32bit libraries of every package where this flag has been activated. So the times where a user e.g. had a fast 64bit mesa-10.2 library for his RadeonSI card and a 32bit emul-linux-opengl bottleneck with mesa-10.0 are over: If everything is set up properly, emerge mesa will install the same version of mesa in 64bit and 32bit on the system; same USE-flags, same everything.

This document will show how to setup a Gentoo \~amd64 system for this new way of dealing with 32bit libraries. A stable amd64 system may not work this way but if the new feature is completely stable, it will be available to all users eventually.

## Overview

The following steps are needed to setup the system for abi\_x86\_32:

1. Unmerge all packages from Overlays that replace packages from the base system (like X11 packages from x11-overlay) and all packages that depend on emul-linux packages, because they will interfere with the transition process.
2. Note which emul-linux packages are installed, remove and mask them
3. Set the abi\_x86\_32 flag for those packages which were part of the previously uninstalled emul-linux packages. The flag may be set globally but it makes no sense to create additional 32bit binaries for really everything (a 32bit copy of your desktop and GUI applications is probably very useless)
4. Recompile everything that changed (which may need several hours)

## Detailed Guide

For this guide you need eix and gentoolkit installed unless you know how to achieve the same things without their functionality. The guide follows the steps from the overview above.

### Dealing with Overlays

Removing overlay packages depends on what you have installed so it cannot be handled here. If dependency problems or USE-flag problems occur later, you will see which package caused it.

### Remove emul-linux and all dependencies

To find out which packages depend on emul-linux packages, you can use the following shell command:

`root #``for EMUL in $(eix -I --only-names emul-linux); do equery depends $EMUL; done`
Note which packages were printed, then unmerge them. Gcc might be seen among those packages because of its multilib flag and, of course, do NOT unmerge GCC.

Execute

`root #``eix -I --only-names emul-linux`
to know which emul-packages you had installed, open [http://dev.gentoo.org/\~pacho/emul.html](http://dev.gentoo.org/~pacho/emul.html) and open the linked site listing the contents of the emul-packages.

Now unmerge all emul-packages and mask them by adding them to /etc/portage/package.mask

**`/etc/portage/package.mask`**

**Masking all emul-linux packages**

### Apply the abi\_x86\_32 flag

First, unmask abi\_x86\_32 flag by putting the following in /etc/portage/profile/use.mask. If the file does not exist, create it.

**`/etc/portage/profile/use.mask`**

**Unmasking abi\_x86\_32 flag**

Then, open /etc/portage/package.use, append all package names from the site to it and append to every package name the flag *abi\_x86\_32*, like so:

**`/etc/portage/package.use`**

**Setting abi\_x86\_32**

It does not matter if you need or want all these packages, just set the flag in case it gets emerged. I also added

**`/etc/portage/package.use`**

**Setting abi\_x86\_32 for wine**

because my main motivation was to play games via wine and mostly every commercial Windows game comes as 32bit binary.

### Update the System

Emerge everything that changed:

`root #``emerge --keep-going --update --newuse --deep --with-bdeps=y world`
. It is imperative to emerge with dependency and newuse checks.

If emerge complains about missing useflags for some packages (it probably will!), add them to the /etc/portage/package.use file like you did for the emul-linux packages. In my case I needed to add

**`/etc/portage/package.use`**

**Setting additional abi\_x86\_32 flags - EXAMPLE**

but the exact list will most likely depend on your USE-flags and will vary!

After all has been rebuilt, it is a good idea to restart the system or at least X. Then emerge all applications that needed emul-linux packages before. If something still depends on these packages now, fill out a bug.

### GNU Compiler Collection special case

The above will succeed until recompiling [sys-devel/gcc](https://packages.gentoo.org/packages/sys-devel/gcc) is required and reached. This may happen depending on sys-devel/gcc\[$USE\] USE flags because the toolchain herd did not yet resolve [bug #511832](https://bugs.gentoo.org/show_bug.cgi?id=511832) for quite some time now. The associated patch may be an overkill but the fix seems almost too easy. So follow the next sub section for a workaround.

### Dead END

After following the previous steps one will get in a dead end with various packages depending on emulation packages not yet converted to the newer multilib variant. So, to not waste such an effort after so much trouble to get emul cruft out of the system, a workaround is to create */etc/portage/profile*, if not already created, and add the emulation packages pulled in by those remaining dependencies, like GNU Compiler Collection case, in *package.provided*. At this point, one should be fine to go for the time being. Until those problematic packages get converted to the new multilib standard, this workaround will remain a safe net avoiding the numerous issues related to having emulation packages when using \~arch or simply for performance related issues.
