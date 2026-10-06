<!-- source: https://wiki.gentoo.org/wiki/GRUB/Advanced_storage | group: Gentoo Wiki (Main) | wiki-title: GRUB/Advanced storage -->
---
title: GRUB/Advanced storage
url: https://wiki.gentoo.org/wiki/GRUB/Advanced_storage
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2023-05-04"
fingerprint: "3cea411b288cb7a8"
license: CC BY-SA 4.0
---

# GRUB/Advanced storage

[GRUB](https://wiki.gentoo.org/wiki/GRUB)

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**outdated**. You can help the Gentoo community by verifying and

[updating this article](https://wiki.gentoo.org/index.php?title=GRUB/Advanced_storage&action=edit).

This section is based on converting a non-UEFI, MBR partition based system to boot from a GPT RAID enabled disk. It is currently incomplete, and partially edited from the previous version.

## Booting from LVM logical volumes

GRUB2 supports booting from an [LVM](https://wiki.gentoo.org/wiki/LVM) partition however the `device-mapper` USE flag must be set in order to activate this feature.

**`/etc/portage/package.use`**

**Enabling the device-mapper USE flag for GRUB2**

```
sys-boot/grub:2 device-mapper
```
If GRUB2 is currently installed, re-emerge it using the `--newuse` option:

`root #``emerge --ask --newuse sys-boot/grub:2`
Next tell GRUB2 to pre-load the `lvm` module:

**`/etc/default/grub`**

**Preloading LVM module**

```
GRUB_PRELOAD_MODULES=lvm
```
If you get an 'invalid root device' when you boot and are using an initramfs (especially the one made by genkernel) you need to pass `dolvm` (for both lvm1 and lvm2) to the kernel:

**`/etc/default/grub`**

**Adding default kernel parameters**

```
GRUB_CMDLINE_LINUX_DEFAULT="dolvm"
```
Finally (re)generate a GRUB2 grub.cfg file using the grub2-mkconfig command.

## Booting from RAID array

In this section, we assume a system has three hard drives. /dev/sda is the original MBR Windows Vista disk, untouched apart from installing GRUB2 onto it. /dev/sdb and /dev/sdc are configured identically.

/dev/sdb3 and /dev/sdc3 are raided together to make the / partition.

/dev/sdb4 and /dev/sdc4 are raided together to make the /home partition.

**`/boot/grub/grub.cfg`**

```
menuentry 'Gentoo GNU/Linux, with Linux x86_64-3.14.14-gentoo' --class gentoo --class gnu-linux --class gnu --class os $menuentry_id_option 'gnulinux-x86_64-3.14.14-gentoo-advanced-8bb0e52c-d524-4af8-ac08-f3c9238c6040' {
	load_video
	set gfxpayload=keep
	insmod gzio
	insmod part_msdos
	insmod part_gpt
	insmod diskfilter
	insmod mdraid1x
	insmod ext2
	set root='mduuid/660afb13150e817a0cdd36476d5b2c51'
	if [ x$feature_platform_search_hint = xy ]; then
	  search --no-floppy --fs-uuid --set=root --hint='mduuid/660afb13150e817a0cdd36476d5b2c51'  8bb0e52c-d524-4af8-ac08-f3c9238c6040
	else
	  search --no-floppy --fs-uuid --set=root 8bb0e52c-d524-4af8-ac08-f3c9238c6040
	fi
	echo	'Loading Linux x86_64-3.14.14-gentoo ...'
	linux	/boot/kernel-3.14.14-gentoo domdadm root=UUID=8bb0e52c-d524-4af8-ac08-f3c9238c6040 ro  
	echo	'Loading initial ramdisk ...'
	initrd	/boot/initramfs-genkernel-x86_64-3.14.14-gentoo
}
 
menuentry 'Gentoo GNU/Linux mirror 2' --class gentoo --class gnu-linux --class gnu --class os $menuentry_id_option 'gnulinux-simple-8bb0e52c-d524-4af8-ac08-f3c9238c6040' {
        load_video
        insmod gzio
        insmod part_msdos
        insmod part_gpt
	insmod mdraid1x
        insmod ext2
        set root=(mduuid/660afb13:150e817a:0cdd3647:6d5b2c51)
        if [ x$feature_platform_search_hint = xy ]; then
	          search --no-floppy --fs-uuid --set=root --hint-bios=hd1,msdos2 --hint-efi=hd1,msdos2 --hint-baremetal=ahci1,msdos2 --hint='hd1,msdos2'  8bb0e52c-d524-4af8-ac08-f3c9238c6040
          else
		  search --no-floppy --fs-uuid --set=root 660afb13:150e817a:0cdd3647:6d5b2c51
        fi
	echo    'Loading Linux 3.14.14-gentoo ...'
	linux   /boot/kernel-3.14.14-gentoo domdadm root=/dev/disk/by-id/md-uuid-660afb13:150e817a:0cdd3647:6d5b2c51 ro
	echo    'Loading initial ramdisk ...'
	initrd  /boot/initramfs-genkernel-x86_64-3.14.14-gentoo
}
```
`insmod part_gpt` (or `insmod part_msdos`) is needed otherwise GRUB2 will not be able to read the partition table.

`insmod mdraid1x` is required for RAID v1.1 or higher. If using RAID v0.9 or v1.0 the RAID module might not be needed (`mdraid09` for v0.9) because the RAID information is stored at the end of the partition. Any references to `insmod raid` are obsolete.

`insmod ext2` is required for ext partitions. For a other file systems substitute the appropriate module.

`set root='mduuid/660afb13150e817a0cdd36476d5b2c51'` tells GRUB2 what root device Linux will be using.

At this point, GRUB2 is ready to hand over control to Linux. Any errors up to this point (missing modules, missing boot partition, etc.) are problems with GRUB2 not the Linux kernel. Onward from this point, it is safe to narrow errors down to Linux.

Linux is unable to assemble RAID assemblies in the kernel. It must call out to user-space tools; if the root partition is a RAID set, an initramfs **must** be used.

Make sure the kernel is configured correctly. Especially make sure that `CONFIG_FHANDLE` variable is set (this is why genkernel must not be used - genkernel will reset the value of `CONFIG_FHANDLE` variable and reap havoc upon the RAID set).

Compile the kernel manually by running the following commands as root:

`root #````
cd /usr/src/linux
```
`root #````
make
```
`root #````
make modules_install
```
`root #````
make install
```
At this point it is probably a good idea to emerge udev - and make sure all warnings are fixed!

Use genkernel utility to generate an initramfs:

`root #``genkernel --mdadm --install initramfs`
The `linux` and `initrd` lines in /boot/grub/grub.cfg should now load the Linux kernel. Add the `domdadm` parameter to the Linux kernel command line in GRUB2. Without it the RAID set will not assemble and the Linux kernel will not find the root device.

The first boot option correctly assembles the root RAID set at boot time, and boots successfully. Any attempt to access the root device from user space will fail as if the RAID set does not exist. This breaks GRUB2, and making the mistake of trying to run grub2-install on top of these errors will break GRUB boot altogether. Make sure Rescue CD and a copy of the Gentoo handbook are easily available before attempting to use a RAID set on the root disk.

The second option drops into a GRUB2 shell. Running ls /dev/md\* will show that initrd has found the RAID set devices, and giving it the device, e.g. /dev/md127, will boot into a fully working system with userspace access to the boot device.

Put these into a /etc/grub.d script at the first available opportunity.

Further documentation on this subject, along with booting from encrypted physical volumes, is close to null.

Any further documentation from a user that has successfully booted a system from a RAID array welcome to complete add a RAID boot section to this article.

Booting from a RAID array is very similar to booting from a LVM Logical Volume aside from RAID specific terminology and syntax of RAID partitioned volume.

This should work with a simple software RAID setup. However, the author has no idea or what command, if any exit at the moment of writing, to use to assemble an array such as mdadm --assemble --scan /dev/md0 that an initramfs could take care of.

## Booting from LUKS

Tell GRUB2 to look for cryptodisks:

`root #``echo "GRUB_CRYPTODISK_ENABLE=y" >> /etc/default/grub`
(Re)generate the grub.cfg with the grub2-mkconfig utility.
