<!-- source: https://wiki.gentoo.org/wiki/Knowledge_Base:Freeing_disk_space | group: Gentoo Knowledge | wiki-title: Knowledge_Base:Freeing_disk_space -->
---
title: Knowledge Base:Freeing disk space
url: https://wiki.gentoo.org/wiki/Knowledge_Base:Freeing_disk_space
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2024-10-01"
fingerprint: "9d6ba87b04e42f01"
license: CC BY-SA 4.0
---

# Knowledge Base:Freeing disk space

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

Although disk space is relatively cheap as of writing this article, it may not be so easy or even possible to expand storage on mobile, embedded or other devices, so freeing useless disk space is often important.

This article introduces tools that help to remove unnecessary system files and optimize the filesystem in order to free disk space.

## Analysis

### System files

#### Package manager

Over time large (or a large number of) unnecessary files accumulate in certain directories on the system. This generally occurs from system upgrades. The following table provides description of file paths to consider for cleanup.;

| Path | Explanation | 
|---|---|
| /var/tmp/portage/ | If the installation of a package with a big source tree (kernel sources) is interrupted, the sources are not deleted automatically. | 
| /var/cache/distfiles/ | Source code archives and distribution files for older versions of programs are not automatically removed when a new version is emerged. | 
| /var/cache/binpkgs/ | As with distribution files, binary packages are not automatically removed. | 

Gentoo includes the [eclean](https://wiki.gentoo.org/wiki/Eclean) utility as part of the [app-portage/gentoolkit](https://packages.gentoo.org/packages/app-portage/gentoolkit) package to help clean up no longer relevant packages and distfiles.

##### Flatpak

Systems with Flatpak may have lingering dependencies after uninstalls. To remove the files taking space:

`user $``flatpak uninstall --unused`
#### Kernel sources

| Path | Explanation | 
|---|---|
| /lib/modules/${old\_kernel} | The module files installed after kernel compilation are not tracked by the package manager an thus are not deleted after being unmerged. | 
| /usr/src/linux-${old\_kernel} | As with module files, kernel object files are not removed by the package manager. | 

Kernel source files distributed through the packages manager will be automatically cleaned up after the kernel sources package has been unmerged or depcleaned. Kernel sources that have been installed (emerged) *and* compiled create object (.o) files and binaries that Gentoo's package manager does *not* clean up because these files have been created post-package install.

There is a utility available called eclean-kernel to help find and clean up compiled kernels:

`root #``emerge --ask app-admin/eclean-kernel`
#### Locales

| Path | Explanation | 
|---|---|
| /usr/share/locale | Many locales from installed packages accumulate here. | 

These files must be manually removed. Be careful to not remove desired locales. Portage can skip copying files by first masking every locale, then unmasking the desired locales.

**`/etc/portage/make.conf`**

### User files

#### Cache directories

Over time a large number of unnecessary files accumulate in certain directories as various applications are used:

| Path | Description | 
|---|---|
| \~/.cache/ | Cache folder used by browsers (and a few other applications). | 
| \~/.thumbnails/ | Folder to keep generated thumbnails. | 

#### Personal files

Several graphical tools exist that help to visualize occupied space on the filesystem tree which may help to identify directories and files that take up too much space:

| Utility | Package | Desktop environment | 
|---|---|---|
| filelight | [kde-apps/filelight](https://packages.gentoo.org/packages/kde-apps/filelight) | KDE Plasma | 
| kdirstat | [kde-misc/kdirstat](https://packages.gentoo.org/packages/kde-misc/kdirstat) | KDE Plasma | 
| qdirstat | [sys-apps/qdirstat](https://packages.gentoo.org/packages/sys-apps/qdirstat) | Qt | 
| baobab | [sys-apps/baobab](https://packages.gentoo.org/packages/sys-apps/baobab) | GNOME | 
| xdiskusage | [x11-misc/xdiskusage](https://packages.gentoo.org/packages/x11-misc/xdiskusage) | Plain X | 
| ncdu ncdu-bin | [sys-fs/ncdu](https://packages.gentoo.org/packages/sys-fs/ncdu) [sys-fs/ncdu-bin](https://packages.gentoo.org/packages/sys-fs/ncdu-bin) | Ncurses (terminal) | 

Select an appropriate utility from the table above and install it. Once installed use the utility to locate large or unneeded personal files and directories.

### Filesystem reserved blocks percentage

On the ext\* family of filesystems, 5% of all blocks are reserved for the privileged user and group by default when created. This provides a safety measure in the case of very low disk space, so privileged processes won't run out of disk space. However, on filesystems with hundreds of gigabytes, 5% is a lot more than would be typically needed in such a situation (on a 300 GB filesystem, that would be about 15 GB). Such reserved space would be even less useful on filesystems serving only as storage, e.g. /home/.

### Filesystem fragmentation

Although most filesystems use strategies to prevent file fragmentation, some files get fragmented over time. A fragmented file may occupy more blocks than would be needed if the file was stored in a contiguous way. Also, as free space becomes fragmented, the possibility of files becoming fragmented increases.

## Resolution

The process or cleaning up these directories can be automated to a certain extent using these tools:

| Utility | Package | Description | 
|---|---|---|
| [eclean](https://wiki.gentoo.org/wiki/Eclean) | [app-portage/gentoolkit](https://packages.gentoo.org/packages/app-portage/gentoolkit) | Removes files in `[DISTDIR](https://wiki.gentoo.org/wiki/DISTDIR)` and `[PKGDIR](https://wiki.gentoo.org/wiki/PKGDIR)` intelligently, preserving files needed for rebuilding or repairing the system. | 
| eclean-kernel | [app-admin/eclean-kernel](https://packages.gentoo.org/packages/app-admin/eclean-kernel) | Removes unused kernels and their files, preserving the most recent kernels. (Active development happens on version 2. Unmask `=app-admin/eclean-kernel-1.99.4` for this, but beware of bugs and run it in pretend mode first.) | 

### User files

#### Cache directories

Remove files from \~/.cache directories as needed to free space. This will most likely remove some of the customization in local applications such as file and web browsers, office applications, etc. Be aware of these changes before proceeding.

#### Personal files

Move or remove any large or unneeded files to free disk space. Moving files to external drives or network storage locations can be helpful if the files need to be preserved.

### Reducing the reserved blocks percentage

The reserved blocks percentage on a ext\* filesystem can be reduced using the tune2fs tool from the [sys-fs/e2fsprogs](https://packages.gentoo.org/packages/sys-fs/e2fsprogs) package.

This would reduce the percentage of reserved blocks on the filesystem to 1% on the partition represented by the block device /dev/sdXY:

`root #``tune2fs -m 1 /dev/sdXY`
### Defragmenting the filesystem

This approach usually doesn't save much disk space, but might prevent further file fragmentation which would use up more disk space in the long run.

Several tools exist, some are filesystem specific (which usually provide better performance), some not:

| Utility | Package | Comment | 
|---|---|---|
| shake | [sys-fs/shake](https://packages.gentoo.org/packages/sys-fs/shake) | Defragments any filesystem in userspace by rewriting files to force contiguous storage. | 
| e4defrag | [sys-fs/e2fsprogs](https://packages.gentoo.org/packages/sys-fs/e2fsprogs) | Made for the ext4 filesystem. | 
| reiserfs-defrag | [sys-fs/reiserfs-defrag](https://packages.gentoo.org/packages/sys-fs/reiserfs-defrag) | Make for ReiserFS filesystems. | 

## See also

- ["Is it safe to delete these files?"](https://wiki.gentoo.org/wiki/FAQ#Source_tarballs_are_collecting_in_.2Fvar.2Fcache.2Fdistfiles.2F._Is_it_safe_to_delete_these_files.3F) - The Gentoo FAQ.

## External resources

- [Running out of disk space](https://forums.gentoo.org/viewtopic-t-30547.html) on the Gentoo Forums.
