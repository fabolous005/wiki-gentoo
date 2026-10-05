<!-- source: https://wiki.gentoo.org/wiki/LILO | group: Gentoo Wiki (Main) | wiki-title: LILO -->
---
title: LILO
url: https://wiki.gentoo.org/wiki/LILO
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-07-02"
fingerprint: d49abd1a5fa7fbfc
license: CC BY-SA 4.0
---

# LILO

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)



**LILO (LInux LOader)** is a simple boot loader to load Linux and other operating systems.

## Installation

LILO's installation is two-fold. One is the installation of the software itself on the system (but does not activate LILO), the second one is the installation (activation) of the LILO bootloader on the disk's MBR.

### USE flags


| [device-mapper](https://packages.gentoo.org/useflags/device-mapper) | Enable support for device-mapper from sys-fs/lvm2 | 
| [keytab](https://packages.gentoo.org/useflags/keytab) | Install keytab, keyboard remapping helper script | 
| [minimal](https://packages.gentoo.org/useflags/minimal) | Do not install optional bits (dolilo helper, docs, etc.) | 
| [pxeserial](https://packages.gentoo.org/useflags/pxeserial) | Avoid character echo on PXE serial console | 
| [static](https://packages.gentoo.org/useflags/static) | !!do not set this during bootstrap!! Causes binaries to be statically linked instead of dynamically | 

### Emerge

The software installation will only install the software on the file system, but will not install LILO in the MBR.

`root #``emerge --ask sys-boot/lilo`
### Installing LILO on the MBR

In order to install LILO on the MBR or update LILO, invoke lilo. However, before doing that, the /etc/lilo.conf file must be set up, which is covered in the [Configuration](https://wiki.gentoo.org/wiki/LILO#Configuration) section below.

`root #``lilo`
An example lilo.conf file is provided at /etc/lilo.conf.example. To start configuring LILO, copy over the example file.

`root #``cp /etc/lilo.conf.example /etc/lilo.conf`
Update the /etc/lilo.conf file accordingly.

### General configuration

First configure LILO to be deployed on the system. The `boot` parameter tells LILO where to install the LILO bootloader in. Usually, this is the block device that represents the first disk (the disk that the system will boot), such as /dev/sda. Be aware that the lilo.conf.example file still uses /dev/hda so make sure that references to /dev/hda are changed to /dev/sda.

**`/etc/lilo.conf`**

**Defining where to install LILO in**

Next, tell LILO what to boot as default (if the user does not select any other option at the boot menu). The name used here is the `label` value of the operating system blocks defined later in the file.

**`/etc/lilo.conf`**

**Booting the block labeled as Gentoo by default**

LILO will show the available options for a short while before continuing to boot the default selected operating system. How long it waits is defined by the `timeout` parameter and is measured in tenths of a second (so the value 10 is one second):

**`/etc/lilo.conf`**

**Setting a 5 second timeout before continuing to boot the default OS**

In case of slow loading time, one could use `compact` which will reduce the read operation of the sector from each individual sector into sector and its adjacent sectors. That will boost the booting time in slow machine. But caveat is that the file should be in contiguous sectors to have the effect. Partially continuous sectors are fine but if they are nowhere close to each other, the number of read operation would still stay the same. Hence, no effect.

**`/etc/lilo.conf`**

**Reduce the read operation for contiguous sectors**

### Configuring the Gentoo OS block

An example configuration block for Gentoo is shown below. It is given the "Gentoo" label to match the `default` parameter declared earlier.

**`/etc/lilo.conf`**

**Example Gentoo Linux configuration in lilo.conf**

This will boot the Linux kernel /boot/kernel-3.11.2-gentoo with root file system /dev/sda4.



### Adding kernel parameters

To add additional kernel parameters to the OS block, use the `append` parameter. For instance, to boot the Linux kernel silently (so it does not show any kernel messages unless critical):

**`/etc/lilo.conf`**

**Showing the use of the append parameter with the quiet option**

[systemd](https://wiki.gentoo.org/wiki/Systemd) users for instance would want to set `init=/usr/lib/systemd/systemd` so that the systemd init is used:

**`/etc/lilo.conf`**

**Using systemd with LILO**

As can be seen, additional kernel parameters are just appended to the same `append` parameter.

### Multiple block definitions

It is a good idea to keep old definitions available in case the new kernel doesn't boot successfully. This is accomplished by creating another block:

**`/etc/lilo.conf`**

**Defining a second OS block**

## Usage

### Updating LILO in the MBR

As mentioned earlier, lilo has to be executed in order to install LILO in the MBR. This step has to be repeated every time /etc/lilo.conf is modified or when the Linux kernel(s) that the /etc/lilo.conf file points to are updated!

`root #``lilo`
Running lilo too much doesn't hurt.

#### Dual boot Gentoo and FreeBSD

To dual boot Gentoo and FreeBSD, edit /etc/lilo.conf as follows:

**`/etc/lilo.conf`**

**Dual boot: Gentoo and FreeBSD**

Make sure to adapt the example configuration file to match the setup used.

## Removal

### Unmerge

Uninstall lilo, simply:

`root #``emerge --ask --depclean --verbose sys-boot/lilo`
## See also

- [GRUB](https://wiki.gentoo.org/wiki/GRUB) — a multiboot secondary [bootloader](https://wiki.gentoo.org/wiki/Bootloader) capable of loading kernels from a variety of [filesystems](https://wiki.gentoo.org/wiki/Filesystem) on most system architectures.
- [Forum](https://forums.gentoo.org/viewtopic-p-8639638.html?sid=318bb4687e49059e853cf8c08b8218c4) discussion about initramfs and lilo
