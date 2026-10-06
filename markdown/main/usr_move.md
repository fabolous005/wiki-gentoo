<!-- source: https://wiki.gentoo.org/wiki//usr_move | group: Gentoo Wiki (Main) | wiki-title: /usr move -->
---
title: "/usr move"
url: https://wiki.gentoo.org/wiki//usr_move
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2025-12-19"
fingerprint: "87397afa3a24df02"
license: CC BY-SA 4.0
---

# /usr move

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**Archived article**

This article is **archived (obsolete)**. Contents are surely incorrect for current usage, and are intended for historical reference only. *Page archived as of **March 2nd, 2014**.*

TLDR:

**Do not use this article!**

The /usr move is a concept of moving various applications installed into the filesystem root to /usr prefix in order to unify the filesystem layout.

## Details

### Current filesystem layout

Right now, the filesystem layout involves arbitrarily splitting the installed applications and libraries between / and /usr prefixes. The [FHS](https://wiki.gentoo.org/wiki/Filesystem_Hierarchy_Standard) specifies that the / prefix should be used by 'essential' executables and libraries.

This definition is quite blurry. In Gentoo, we usually interpret 'essential' as necessary for the system to boot and mount filesystems (especially /usr). This also involves rescue from common problems like filesystem damage.

The current filesystem layout looks like the following:

/
├── **bin**
├── boot
├── dev
├── etc
├── home
├── **lib**
├── **lib32**
├── **lib64**
├── media
├── mnt
├── opt
├── proc
├── root
├── run
├── **sbin**
├── sys
├── tmp
├── usr
│   ├── **bin**
│   ├── games
│   ├── *include*
│   ├── **lib**
│   ├── **lib32**
│   ├── **lib64**
│   ├── **libexec**
│   ├── local
│   ├── portage
│   ├── **sbin**
│   ├── **share**
│   ├── src
│   └── \<chost>
└── var

You can notice two things:

1. / contains both 'global' directories (/boot, /home...) and 'prefix' directories (/bin, /lib...),
2. / does not contain all 'prefix' directories /usr has (there is no /share and no /libexec).

### Wanted filesystem layout

The idea of complete /usr move involves not using the / prefix for installed packages and installing everything to /usr instead. However, this is practically not possible due to heavy reliance on rootfs paths in compiled executables and standards.

With a complete /usr move, the layout would look like the following:

/
├── boot
├── dev
├── etc
├── home
├── media
├── mnt
├── opt
├── proc
├── root
├── run
├── sys
├── tmp
├── usr
│   ├── **bin**
│   ├── games
│   ├── *include*
│   ├── **lib**
│   ├── **lib32**
│   ├── **lib64**
│   ├── **libexec**
│   ├── local
│   ├── portage
│   ├── **sbin**
│   ├── **share**
│   ├── src
│   └── \<chost>
└── var

Now 'prefix' directories start appearing in /usr prefix and / contains just 'global' paths.

### Realistic semi-clear layout

Sadly, as many applications and libraries expect various files to exist in a fixed locations, we would have to keep them available there. For that reason, the old 'prefix' directories will exist with a number of compatibility symlinks including:

/
├── bin
│   ├── sh
│   └── (...)
├── lib
│   └── ld-linux.so.2
└── lib64
    └── ld-linux-x86-64.so.2

## Benefits

Currently, the read-only shared data for various programs are split between / subdirectories and /usr. This means, that the rootfs usually contains both read-only and writable data which separating requires using a number of additional, small mountpoints.

The concept is that after the split, all shared, read-only data would be stored in /usr, while other locations in rootfs will be used to perform their respective (mostly read/write) tasks.

Possible uses include:

1. sharing the whole system over NFS. Right now this requires sharing /bin, /lib\*, /sbin and /usr. After the move, only /usr would be necessary;
2. mounting /usr read-only while keeping / read/write. Right now, this keeps the **most important** system executables and libraries writable. Of course, a preferable alternative is to mount whole / read-only but it usually involves moving some files from /etc to other locations (like /etc/resolv.conf when using DHCP).

### Avoiding excessive use of rootfs

The requirements for system boot are raising, and along with that, more files are moved onto rootfs to satisfy the dependencies. This slowly introduces the following problems:

1. Increasing space consumption on rootfs. The more dependencies are moved there, the more space is consumed. At some point, users may be required to resize their rootfs in order to fit the new dependencies being introduced;
2. Increasing number of **statically linked executables**. As an alternative to moving libraries into rootfs, some packages are using static linkage for their executables. Disadvantages of static linkage are not the topic of this article but their include inability to handle library upgrades through the package manager and thus make it harder to fix security issues;
3. Necessity of introducing additional directories on rootfs or moving files elsewhere. Right now, rootfs does not specify any location for shared, read-only data (/share) so those files are either placed in /usr/share, /etc or /lib.

### Simplification of ebuilds

Right now, an ebuild wishing to install a library to rootfs needs to:

1. Adjust the package install prefix from the default one (/usr) to /,
2. If a package uses static libraries, manually move them from /lib\* to /usr/lib\*,
3. If a package uses static libraries, create shared library wrappers in /usr/lib\* to avoid linker preferring the shared ones,
4. If some non-core executables depend on libraries in /usr, manually move them to /usr/bin.

The ebuild maintainer basically has to ensure that the program will work without /usr mounted. This means that he needs to ensure that all the dependencies, possibly with all flag combinations, end up in rootfs.

Moving the application to /usr simply makes the maintenance easier.

## Prerequisites

### initramfs or other pre-mount (for system packages)

In order to perform the /usr move on packages required to boot the system, it is necessary that all users having separate /usr partition have an enabled and working [initramfs](https://wiki.gentoo.org/wiki/Initramfs). Such an initramfs would need to mount the /usr filesystem in order to execute the system init.

A possible alternative for an initramfs (which many people oppose just because they can) is creating a single, statically linked pre-init executable on rootfs and using it to mount /usr before executing the actual init.

### /usr remount in system RC

Considering that initramfs would mount /usr read-only (alike / is mounted now), [OpenRC](https://wiki.gentoo.org/wiki/OpenRC) or any other RC used in Gentoo would need to be able to perform fsck on mounted /usr and re-mount it read/write afterwards. In other words, /usr would need to be handled alike / is handled now.

## Testing overlays

The /usr move variant described in this article is being tested in the [usr-gentoo](https://wiki.gentoo.org/index.php?title=Usr-gentoo&action=edit&redlink=1) overlay.

## The process

### Modifying package install prefixes

The most important process in the /usr move is updating the packages to not use / prefix. In most cases, this simply involves removing the `--prefix` argument to configure calls since /usr is the default prefix used by *econf*.

Some packages install files to rootfs directories explicitly. Those packages may require actually modifying the build system or specifying the prefix some other way.

### Adding necessary symlinks

To maintain compatibility with various packages, a symlinks to various binaries or libraries may need to be created on rootfs. However, it is preferred that packages in question are checked for possibility of fixing first.

For example, if a script calls /bin/foo, it is preferred that it is fixed to just call *foo* and let the shell handle *${PATH}* resolution rather than creating an unnecessary symlink.

## The /usr move in other distributions

A [similar concept is being considered in Fedora](https://fedoraproject.org/wiki/Features/UsrMove) yet it assumes that the rootfs directories would become symlinks to /usr counterparts.
