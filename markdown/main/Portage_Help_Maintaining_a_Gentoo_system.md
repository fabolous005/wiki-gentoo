<!-- source: https://wiki.gentoo.org/wiki/Portage/Help/Maintaining_a_Gentoo_system | group: Gentoo Wiki (Main) | wiki-title: Portage/Help/Maintaining a Gentoo system -->
---
title: Portage/Help/Maintaining a Gentoo system
url: https://wiki.gentoo.org/wiki/Portage/Help/Maintaining_a_Gentoo_system
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2025-04-21"
fingerprint: a239f37e8e078d86
license: CC BY-SA 4.0
---

# Portage/Help/Maintaining a Gentoo system

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

There are a few common issues that Gentoo users regularly seek assistance for on [#gentoo](ircs://irc.libera.chat/#gentoo) ([webchat](https://web.libera.chat/#gentoo)) and the [forums](https://forums.gentoo.org/).

## What's normal?

Upgrades are **not** supposed to be hard. Furthermore, they're not hard for most Gentoo users.

But this doesn't mean people can't experience issues. A fair amount of such cases arise because of bad habits, users lacking the required portage skills, or previously used quick and dirty solutions to problems. This often leads to people avoiding upgrades, and then the problem gets worse.

## Procedure

### World file hygiene

This is easily the most important point: the system's [world file](<https://wiki.gentoo.org/wiki/World_set_(Portage)>) at /var/lib/portage/world should only contain things the user **personally wants**.

It should not contain (non-exhaustive list):

1. libraries, e.g. \*-libs/\* unless the system explicitly needs them for development
2. packages that previously failed to emerge and required running an emerge command again (use emerge --oneshot ... when doing that!)
3. most of dev-python/\* unless there is an actual need.
4. anything the user personally doesn't want/use/need.

Erroneous entries in the world file **will** lead to blockers when e.g. a library becomes obsolete or replaced, leading to a user having to fix the problem when for most people, it'll be resolved automatically.

#### Example

[sys-fs/e2fsprogs](https://packages.gentoo.org/packages/sys-fs/e2fsprogs) is absorbing [sys-libs/e2fsprogs-libs](https://packages.gentoo.org/packages/sys-libs/e2fsprogs-libs) (the details aren't relevant here, but see [bug #806875](https://bugs.gentoo.org/show_bug.cgi?id=806875)).

If a user has sys-libs/e2fsprogs-libs in their world file, they are telling Portage **the system must have it**. And Portage will try very, *very* hard to honour this request, like so:

`root #``emerge -p -uvDU @world````
[...]
[blocks B      ] >=sys-fs/e2fsprogs-1.46.4-r51 (">=sys-fs/e2fsprogs-1.46.4-r51" is soft blocking sys-libs/e2fsprogs-libs-1.46.4-r1)
[blocks B      ] sys-libs/e2fsprogs-libs ("sys-libs/e2fsprogs-libs" is soft blocking sys-fs/e2fsprogs-1.46.5)
 * Error: The above package list contains packages which cannot be
 * installed at the same time on the same system.
  (sys-fs/e2fsprogs-1.46.5:0/0::gentoo, ebuild scheduled for merge) pulled in by
    >=sys-fs/e2fsprogs-1.27 required by (sys-block/parted-3.4:0/0::gentoo, installed) USE="debug nls readline -device-mapper -verify-sig" ABI_X86="(64)"
    sys-fs/e2fsprogs required by (app-arch/libarchive-3.5.2:0/13::gentoo, installed) USE="acl bzip2 e2fsprogs iconv lzma xattr zlib -blake2 -expat -lz4 -lzo -nettle -static-libs -zstd" ABI_X86="32 (64) (-x32)"
    sys-fs/e2fsprogs
    sys-fs/e2fsprogs required by @system
  (sys-libs/e2fsprogs-libs-1.46.4-r1:0/0::gentoo, installed) pulled in by
    sys-libs/e2fsprogs-libs required by @selected
    sys-libs/e2fsprogs-libs[abi_x86_32(-),abi_x86_64(-)] required by (net-fs/samba-4.15.3:0/0::gentoo, installed) USE="acl client cups pam regedit system-mitkrb5 winbind -addc -ads -ceph -cluster -debug (-dmapi) (-fam) -glusterfs -gpg -iprint -json -ldap -profiling-data -python -quota (-selinux) -snapper -spotlight -syslog (-system-heimdal) -systemd (-test) -zeroconf" ABI_X86="32 (64) (-x32)" PYTHON_SINGLE_TARGET="python3_9 -python3_10 -python3_8"
    >=sys-libs/e2fsprogs-libs-1.42.9[abi_x86_32(-),abi_x86_64(-)] required by (app-crypt/mit-krb5-1.19.2:0/0::gentoo, installed) USE="keyutils nls pkinit threads -doc -lmdb -openldap (-selinux) -test -xinetd" ABI_X86="32 (64) (-x32)" CPU_FLAGS_X86="-aes"
```
Note that the main reason it is pulling in e2fsprogs-libs here is: `sys-libs/e2fsprogs-libs required by @selected`. This means the package is in the world file! Portage is trying hard to hold onto it because the user told it to, by previously putting it there.

The solution is to deselect it, which means to remove it from the world file:

`root #``emerge --deselect sys-fs/e2fsprogs-libs`
Now Portage is no longer being instructed to keep `-libs` and should proceed to upgrade.

### Oneshotting

One way to cause issues, especially when updating, is by adding build dependencies to your world file. Basically telling Portage you wish this package always be there. Oneshotting will build the package but not add it to the world file.

Some examples:

A user tries installing [app-editors/pluma](https://packages.gentoo.org/packages/app-editors/pluma), but the dependency [x11-libs/cairo](https://packages.gentoo.org/packages/x11-libs/cairo) fails. After fixing the issue the user may think running emerge --ask x11-libs/cairo would be a good method to make sure the fix has solved the issue. This is when `--oneshot` comes into play as it will allow the user to preform the sanity check without clogging up their world file with unneeded packages.

A user wants to update their Portage mirrors, but don't plan on doing it more than once a year. In this case it is pointless keeping the package [app-portage/mirrorselect](https://packages.gentoo.org/packages/app-portage/mirrorselect) on the system permanently. Using oneshot would allow the user to use it once and then on the next depclean (explained below) it would be removed.

`root #``emerge --ask --verbose --oneshot app-portage/mirrorselect`
### Depclean regularly

Depcleaning (emerge --ask --depclean) after every world upgrade is essential. This removes unnecessary packages from the system and keeps the dependency graph clean.

Failure to do so may result in:

1. older versions of toolchain versions being used unnecessarily (e.g. [gcc](https://wiki.gentoo.org/wiki/Gcc), binutils)
2. packages misbehaving when they detect a package installed which has been removed from the tree
3. possibly confusing emerge output on upgrades
4. wasted disk space because orphaned packages are being kept installed

If Portage tries to remove packages that are still required, the depcleaning should be canceled and portage instructed to keep the packages:

`root #``emerge --ask --noreplace foo`
### Never unmerge anything

Consider emerge --unmerge **banned**. Removing packages without --depclean may result in removing something which *is* needed and will only lead to broken packages and a possibly bricked install.

If the system no longer needs a package, run:

`root #``emerge --ask --depclean foo`
Portage will ask the user if they wish to remove it **only if it is safe to do so** (nothing depends on it). It will bail out if it is unsafe.
