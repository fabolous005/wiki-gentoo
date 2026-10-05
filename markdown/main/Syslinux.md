<!-- source: https://wiki.gentoo.org/wiki/Syslinux | group: Gentoo Wiki (Main) | wiki-title: Syslinux -->
---
title: Syslinux
url: https://wiki.gentoo.org/wiki/Syslinux
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-02-08"
fingerprint: "958ddf1ba5a6ade1"
license: CC BY-SA 4.0
---

# Syslinux

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**Syslinux** is a package that contains a family of [bootloaders](https://wiki.gentoo.org/wiki/Bootloader). The package includes SYSLINUX (FAT filesystem bootloader), EXTLINUX ([ext2/3/4](https://wiki.gentoo.org/wiki/Ext4), [btrfs](https://wiki.gentoo.org/wiki/Btrfs) and [xfs](https://wiki.gentoo.org/wiki/XFS) filesystem bootloader), PXELINUX (Network PXE bootloader) and ISOLINUX (ISO-9660) for CD/DVD bootloading.

Support for EFI was added in version 6.00.[\[1\]](https://wiki.gentoo.org#cite_note-1)

## Installation

The installation of the Syslinux package will provide the software on the system but not install or activate any of the various bootloaders contained in the package.

### USE flags


### Emerge

`root #``emerge --ask sys-boot/syslinux`
## Setup on BIOS systems

The rest of this section will presume the boot partition is located at /dev/sda1 (which is a common location). If this is not the case be sure to make adjustments as needed.

To use EXTLINUX be sure to install the proper *boot sector*, then install the boot loader into the partition. If these steps are omitted EXTLINUX will not be operational. This type of a boot sector setup is not needed for the SYSLINUX, PXELINUX and ISOLINUX installations.

### Boot sector

First, install the boot sector provided by Syslinux. Use extra care with this command; if `count=1` is not given it will overwrite the entire disk rather than just the first 440 bytes:

`root #``dd bs=440 conv=notrunc count=1 if=/usr/share/syslinux/mbr.bin of=/dev/sda`
Mark the boot partition as active. A `*` will appear in the "Boot" column:

`root #``fdisk /dev/sda`
Command (m for help): a
Partition number (1-3): 1
Command (m for help): p
 ...
   Device Boot      Start         End      Blocks   Id  System
/dev/sda1   \*        2048      133119       65536   83  Linux
/dev/sda2          133120     4327423     2097152   82  Linux swap / Solaris
/dev/sda3         4327424  1953525167   974598872   83  Linux
  
Command (m for help): w

First, install the boot sector provided by Syslinux. Use special care with this command; if `count=1` is not given it will overwrite the entire disk rather than just the first 440 bytes:

`root #``dd bs=440 conv=notrunc count=1 if=/usr/share/syslinux/gptmbr.bin of=/dev/sda`
Next, run [sys-apps/gptfdisk](https://packages.gentoo.org/packages/sys-apps/gptfdisk)'s gdisk utility and enable the *legacy BIOS bootable* attribute on the partition where /boot/extlinux is stored.

`root #``gdisk /dev/sda`
Command (? for help): x
 
Expert command (? for help): a
Partition number (1-3): 1
Known attributes are:
0: system partition
1: hide from EFI
2: legacy BIOS bootable
60: read-only
62: hidden
63: do not automount
 
Attribute value is 0000000000000000. Set fields are:
  No fields set
 
Toggle which attribute field (0-63, 64 or \<Enter> to exit): 2
Have enabled the 'legacy BIOS bootable' attribute.
Attribute value is 0000000000000004. Set fields are:
2 (legacy BIOS bootable)
 
Toggle which attribute field (0-63, 64 or \<Enter> to exit): 
 
Expert command (? for help): w

### Boot loader installation

#### EXTLINUX

Use the extlinux command to install the necessary files to /boot/extlinux

`root #````
mkdir /boot/extlinux
```
`root #````
extlinux --install /boot/extlinux
```
`root #````
ln -snf . /boot/boot
```
The Syslinux package contains various modules to enable additional features. Starting with Syslinux version 5, some modules depends on others, so it is a good idea to copy most basic modules regardless of the use case. See [Checking dynamic links](https://wiki.gentoo.org/wiki/Syslinux#Checking_dynamic_links) to verify whether all dependencies are installed.

`root #````
cd /usr/share/syslinux
```
`root #``cp menu.c32 memdisk libcom32.c32 libutil.c32 /boot/extlinux`
#### ISOLINUX

To install ISOLINUX, start off with a base directory in which all the files that need to reside on the CD or DVD are situated. In the base directory, create a subdirectory called isolinux and copy the isolinux.bin file from the Syslinux package into the isolinux folder:

`root #````
mkdir isolinux
```
`root #``cp /usr/share/syslinux/isolinux.bin isolinux/`
Create the isolinux.cfg file according to the instructions mentioned below.

Next, create two more directories kernel and images in the base directory:

`root #``mkdir kernel images`
Copy the memdisk binary into the kernel directory:

`root #``cp /usr/share/syslinux/memdisk kernel/`
When the configuration has been made, the following mkisofs command can be used to create the final ISO image (remember to substitute `${BASEDIR}` with the same base directory used in the previous commands):

`root #``mkisofs -o output.iso -b isolinux/isolinux.bin -c isolinux/boot.cat -no-emul-boot -boot-load-size 4 -boot-info-table ${BASEDIR}` The file boot.cat will be automatically created.

#### PXELINUX

With PXELINUX is possible to netboot using images that are shared through a TFTP server. This article will assume there is a TFTP server installed, and its TFTP root directory is located at /var/lib/tftpboot With this setup, copy the PXELINUX loader to the TFTP boot directory and create a configuration directory:

`root #````
cp /usr/share/syslinux/pxelinux.0 /var/lib/tftpboot/pxelinux.0
```
`root #``mkdir /var/lib/tftpboot/pxelinux.cfg`
The config directory can be used to store different configurations for the netbooted clients. When a client tries to boot, the MAC address or the IP address is used to determine the appropriate client config file. First it tries to look for the MAC address, followed by a try on the hexadecimal representation of the client IP. After that a character is stripped from the end of this hexadecimal representation until no more characters are left, or until a configuration file is located. If none of the tries match, the *default* config file is used.

The hexadecimal representation of an IP can be found by using the gethostip command:

`user $``gethostip -x 192.168.0.50`
C0A80032

An example config file matching sequence occurs as follows:

#### SYSLINUX

Use the syslinux command to install the SYSLINUX bootloader on the (FAT) file system:

`root #````
syslinux --install /dev/sda1
```
### EFI system partition

Create a partition of type `EF` (MBR) or `EF00` (GPT) with a FAT32 file system. It is also possible to use an existing [EFI system partition (ESP)](https://wiki.gentoo.org/wiki/EFI_System_Partition) if one is present. It is advisable to mount this partition at /boot/efi/.

### Boot loader install

In the EFI system partition, create a directory for the Syslinux files. This directory will also contain the configuration files that will be created later.

`root #``mkdir -p /boot/efi/EFI/syslinux`
Copy the syslinux.efi and ldlinux.e64 files along with the other desired .c32 files from /usr/share/syslinux/efi64/ to the new syslinux directory. For example:

`root #````
cd /usr/share/syslinux/efi64
```
`root #``cp syslinux.efi ldlinux.e64 menu.c32 libcom32.c32 libutil.c32 /boot/efi/EFI/syslinux`
If the system has not been booted in EFI mode, then the Syslinux files need to be copied to the /boot/efi/EFI/Boot directory instead and syslinux.efi needs to be renamed to bootx64.efi. If this is the case then skip the next section concerning the [efibootmgr](https://wiki.gentoo.org/wiki/Efibootmgr) utility.

### Making Syslinux known to EFI

Ensure that `CONFIG_EFI_VARS` is enabled in the kernel. If it was built as a module, ensure that it is loaded into memory. This action can be done using the modprobe utility. After the modules have been loaded create a new boot entry using efibootmgr, adjusting `/dev/sda` to the disk containing `/boot`:

`root #````
modprobe efivars
```
`root #``efibootmgr -c -d /dev/sda -l '\EFI\syslinux\syslinux.efi' -L SYSLINUX -p 1`
efibootmgr will automatically adjust the EFI boot order to put the most recently created entry at the top of the list. If that is undesired, change the boot order with the `-o` option.

## Configuration

The configuration file for Syslinux is called syslinux.cfg. For compatibility with existing installs, the following legacy configuration file names are still supported:

- EXTLINUX: extlinux.conf
- ISOLINUX: isolinux.cfg

The configuration *format*, however, is the same. The configuration file must be present in the directory where Syslinux is installed.

### Simple configuration

This will provide a simple "boot:" prompt, similar to the one in Gentoo's Minimal LiveCD:

**`syslinux.cfg`**

### Menu configuration

The following configuration provides a simple text menu example. This is done via the `vesamenu` module. In some cases where `vesa` is not compatible, the simpler menu module will work. Copy the VESA menu module into the boot filesystem or EFI system partition. BIOS systems should use the following example:

`root #``cp /usr/share/syslinux/vesamenu.c32 /boot/extlinux/`
EFI systems should use:

`root #``cp /usr/share/syslinux/efi64/vesamenu.c32 /boot/efi/EFI/syslinux/`
**`/boot/extlinux/extlinux.conf`**

### Passing kernel parameters

Unless the kernel parameters are hard-coded and the initramfs is built-in into the kernel image, these may need to be passed on to the kernel through the boot loader. To do so, use `APPEND` and/or the `INITRD` parameter:

**`syslinux.cfg`**

**Adding kernel parameters**

Since syslinux-5.00, the .c32 modules use dynamic linking. To verify if the dependencies has been installed enter the Syslinux directory and use the ldd command:

`root #``LD_LIBRARY_PATH=. ldd *.c32`
If any .c32 modules are missing be sure to copy them to the directory.

## See also

- [GRUB Legacy](https://wiki.gentoo.org/wiki/GRUB_Legacy) — also known as **GRUB Legacy** or GRUB version 1, was previously recommended by the [Handbook](https://wiki.gentoo.org/wiki/Handbook) as Gentoo's default [bootloader](https://wiki.gentoo.org/wiki/Bootloader) on **x86** and **amd64** architectures.
- [GRUB](https://wiki.gentoo.org/wiki/GRUB) — a multiboot secondary [bootloader](https://wiki.gentoo.org/wiki/Bootloader) capable of loading kernels from a variety of [filesystems](https://wiki.gentoo.org/wiki/Filesystem) on most system architectures.
