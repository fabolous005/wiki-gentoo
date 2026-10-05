<!-- source: https://wiki.gentoo.org/wiki/Initramfs | group: Gentoo Wiki (Main) | wiki-title: Initramfs -->
---
title: initramfs
url: https://wiki.gentoo.org/wiki/Initramfs
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2025-10-17"
fingerprint: "73290cbab797af3d"
license: CC BY-SA 4.0
---

# initramfs

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)


An **initramfs** (**init**ial **ram f**ile **s**ystem) is used to prepare Linux systems during boot before the **init** process starts.

The initramfs usually takes care of mounting important file systems (by loading the proper kernel modules and drivers) such as /usr or /var, preparing the /dev file structure, etc. Users who use an encrypted file system will also have the initramfs ask them for the passphrase before it can mount the file systems. When the file systems are mounted, control is passed on to **init** which then takes care of further starting all necessary services and booting up the remainder of the system.

## See also

- [Initramfs - make your own](https://wiki.gentoo.org/wiki/Initramfs_-_make_your_own) — build an initramfs which does not contain kernel modules.
- [Custom Initramfs](https://wiki.gentoo.org/wiki/Custom_Initramfs) — the successor of *initrd*. It provides early userspace which can do things the kernel can't easily do by itself during the boot process.
- [Dracut](https://wiki.gentoo.org/wiki/Dracut) — an [initramfs] infrastructure and aims to have as little as possible hard-coded into the initramfs.
- [Early Userspace Mounting](https://wiki.gentoo.org/wiki/Early_Userspace_Mounting) — how to build a custom minimal [initramfs] that checks the /usr filesystem and pre-mounts /usr.
- [Initramfs/Guide](https://wiki.gentoo.org/wiki/Initramfs/Guide) — covers the concepts of the initramfs as well as how to properly create and manage initramfs instances.
- [Handbook:AMD64/Installation/Kernel#Optional: Building an initramfs](https://wiki.gentoo.org/wiki/Handbook:AMD64/Installation/Kernel#Optional:_Building_an_initramfs)
- [Old Fashioned Gentoo Install#Making the initrd](https://wiki.gentoo.org/wiki/Old_Fashioned_Gentoo_Install#Making_the_initrd)
