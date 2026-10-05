<!-- source: https://wiki.gentoo.org/wiki/Kernel/Removal | group: Gentoo Wiki (Main) | wiki-title: Kernel/Removal -->
---
title: Kernel/Removal
url: https://wiki.gentoo.org/wiki/Kernel/Removal
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-09-26"
fingerprint: "16c0183fca2bbd07"
license: CC BY-SA 4.0
---

# Kernel/Removal

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

This article describes the **removal of old [kernels](https://wiki.gentoo.org/wiki/Kernel)**.

## Removing kernel sources

After a new kernel is installed and if it works satisfactorily, the old kernel can be removed. To remove the old kernel sources, emerge's *--depclean* option (short form *-c*) can be used to remove all old or unused versions of a slotted package, e.g. for [sys-kernel/gentoo-sources](https://packages.gentoo.org/packages/sys-kernel/gentoo-sources).

`root #``emerge --ask --depclean gentoo-sources:xx.yy.zzz`
Be sure to verify that it is not removing the sources for the currently running kernel (See [kernel upgrade](https://wiki.gentoo.org/wiki/Kernel/Upgrade) article on how to upgrade.)

### Protecting kernel sources

If newer kernel sources has been merged and emerge --depclean is run before switching to the newer sources, the current sources will be removed. To stay with the current sources, this removal is not wanted, because the sources may be needed e.g. for updating external kernel modules. It's therefore good practice to add the specific kernel version to the world file to protect it from `--depclean` operations.

`root #``emerge --ask --noreplace gentoo-sources:xx.yy.zzz`
Alternatively, you can explicitly exclude the sources from `--depclean`:

`root #``emerge --depclean --exclude=sys-kernel/gentoo-sources`
This will leave all of your kernel source build directories alone during cleanup, which you can then clean up with tools like eclean-kernel, referenced below.

Another way to prevent removal of the kernel sources located in /usr/src is to add the path to the `UNINSTALL_IGNORE` variable in make.conf:

**`/etc/portage/make.conf`**

**Prevent kernel sources from being removed**

```
UNINSTALL_IGNORE="${UNINSTALL_IGNORE} /usr/src/linux-*"
```
This won't prevent removal of the package version, but will protect the files. Note that `/lib/modules/*` is the Portage default, and the new filename pattern is added to it.

## Removing kernel leftovers

### Using eclean-kernel

[app-admin/eclean-kernel](https://packages.gentoo.org/packages/app-admin/eclean-kernel) is a simple tool for old kernel cleanup/removal. It removes both built kernel files and build directories if they're no longer reference by any preserved kernel.

See eclean-kernel --help post-installation for usage instructions:

`user $``eclean-kernel --help````
usage: eclean-kernel [-h] [-V] [-A] [-l] [-p] [-b BOOTLOADER] [-L LAYOUT] [-r ROOT] [-a] [-d] [-n NUM] [-s SORT_ORDER]
                      [-D] [-M] [--no-bootloader-update] [--no-kernel-install] [-x EXCLUDE]
 
 Remove old kernel versions, keeping either N newest kernels (with -n) or only those which are referenced by a bootloader
 (with -a).
 
 optional arguments:
   -h, --help            show this help message and exit
   -V, --version         show program's version number and exit
 
 action control:
   -A, --ask             Ask before removing each kernel
   -l, --list-kernels    List kernel files and exit
   -p, --pretend         Print the list of kernels to be removed and exit
 
 system configuration:
   -b BOOTLOADER, --bootloader BOOTLOADER
                         Bootloader used (auto, lilo, grub2, grub, yaboot, symlinks)
   -L LAYOUT, --layout LAYOUT
                         Layout used (auto, blspec, std)
   -r ROOT, --root ROOT  Alternate filesystem root to use
 
 kernel selection:
   -a, --all             Remove all kernels unless used by bootloader
   -d, --destructive     Destructive mode: remove kernels even when referenced by bootloader
   -n NUM, --num NUM     Leave only newest NUM kernels (see also: --sort-order)
   -s SORT_ORDER, --sort-order SORT_ORDER
                         Kernel sort order (mtime, version); default: version
 
 misc options:
   -D, --debug           Enable debugging output
   -M, --no-mount        Disable (re-)mounting /boot if necessary
   --no-bootloader-update
                         Do not update bootloader configuration after removing kernels (if supported by the bootloader
   --no-kernel-install   Do not call kernel-install while removing kernels (if installed)
   -x EXCLUDE, --exclude EXCLUDE
                         Exclude kernel parts from being removed (comma-separated, supported parts: vmlinuz, systemmap,
                         config, initramfs, modules, build, misc, emptydir)
```
For example, to keep three newest kernels around:

`root #``eclean-kernel -n 3`
### Manual removal

[Portage](https://wiki.gentoo.org/wiki/Portage) however only removes the files it installed - the files generated during the kernel build and installation remain. They can be safely removed.

- When a kernel is built in the source directory, files generated during the build process remain, and are not removed by Portage:

- `root #``rm -r /usr/src/linux-X.Y.Z`

- During kernel setup, the kernel modules are copied to a sub directory of /lib/modules/:

- `root #``rm -r /lib/modules/X.Y.Z`

- The old files in /boot can also be removed:

- `root #``rm /boot/vmlinuz-X.Y.Z``root #``rm /boot/System.map-X.Y.Z``root #``rm /boot/config-X.Y.Z``root #``rm /boot/initramfs-X.Y.Z`

- Lastly, remove all leftover entries from the [bootloader's](https://wiki.gentoo.org/wiki/Bootloader) config file.

## Troubleshooting

### Can not clean old kernels with eclean-kernel

This may have triggered a known bug in eclean-kernel: [bug #946132](https://bugs.gentoo.org/show_bug.cgi?id=946132). Here is a possible scenario for reference: if your system initially used systemd-boot and later switched to grub2, but the residual file layout under /efi was not cleared, then this bug may be triggered. The solution is to clean up the /efi layout or specify layout `-L std`.
