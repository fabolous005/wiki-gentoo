<!-- source: https://wiki.gentoo.org/wiki/MIPS/ERLite-3 | group: Gentoo Wiki (Main) | wiki-title: MIPS/ERLite-3 -->
---
title: MIPS/ERLite-3
url: https://wiki.gentoo.org/wiki/MIPS/ERLite-3
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2024-04-10"
fingerprint: "844ab35255853183"
license: CC BY-SA 4.0
---

# MIPS/ERLite-3

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

The **EdgeRouter Lite (ERLite-3)** is an MIPS64 router (MIPS64r2, Cavium Octeon) with 512 MB of RAM, which uses a (removable) USB pendrive for storage. This document describes how to run Gentoo on this device.

![](https://wiki.gentoo.org/images/thumb/8/8f/Edgerouter-original-package.jpg/300px-Edgerouter-original-package.jpg)

![](https://wiki.gentoo.org/images/thumb/0/07/Edgerouter-nocover.jpg/300px-Edgerouter-nocover.jpg)

## What's is provided

### GPL Sources

Ubiquiti provides a GPL archive containing the sources of all open-source software that runs on this router. The GPL archive can be found here [\[1\]](http://www.ubnt.com/download#EdgeRouter:Lite)

### The Processor

system type             : UBNT\_E100 (CN5020p1.1-500-SCP)
processor               : 0
cpu model               : Cavium Octeon+ V0.1
BogoMIPS                : 1000.00
wait instruction        : yes
microsecond timers      : yes
tlb\_entries             : 64
extra interrupt vector  : yes
hardware watchpoint     : yes, count: 2, address/irw mask: \[0x0ffc, 0x0ffb\]
isa                     : mips1 mips2 mips3 mips4 mips5 mips64r2
ASEs implemented        :
shadow register sets    : 1
kscratch registers      : 0
core                    : 0
VCED exceptions         : not available
VCEI exceptions         : not available

processor               : 1
cpu model               : Cavium Octeon+ V0.1
BogoMIPS                : 1000.00
wait instruction        : yes
microsecond timers      : yes
tlb\_entries             : 64
extra interrupt vector  : yes
hardware watchpoint     : yes, count: 2, address/irw mask: \[0x0ffc, 0x0ffb\]
isa                     : mips1 mips2 mips3 mips4 mips5 mips64r2
ASEs implemented        :
shadow register sets    : 1
kscratch registers      : 0
core                    : 1
VCED exceptions         : not available
VCEI exceptions         : not available

### The USB Flash Drive

The details of the USB flash drive that comes with this board are the following:

#### Partitioning

The USB flash drive on the board has two partitions. A small FAT32 one for the Linux Kernel image and a big ext3 one containing the root filesystem in a squashfs file. The squashfs file is in the GPL archive as well.

### The Linux Kernel

The Linux kernel that comes with the board is a modified 2.6.32.13 one. If you ever want to rebuild it yourself from the GPL archive, you need a mips64-octeon-linux-gnu toolchain that comes with the Cavium SDK. Typical distro toolchains (mips64-unknonwn-\*) will fail to compile these kernel sources with errors like these:

`root #``make ARCH=mips  CROSS_COMPILE=mips64-unknown-linux-gnu-`
Error: Opcode not supported on this processor: octeon (mips64r2) \`saa $6,($7)
Error: Opcode not supported on this processor: octeon (mips64r2) \`saa $9,($7)
Error: Opcode not supported on this processor: octeon (mips64r2) \`saa $3,($7)

### The RootFS

This router comes with Debian 6.0.6

**`/etc/debian_version`**

## Preparing the Gentoo MIPS64 RootFS

### Getting the MIPS64 stage3 tarball

The ERLite-3 uses a 64-bit MIPS64r2 Big-Endian Cavium Octeon processor. So the stage3 we want for this board is a mip64-\* one. For this guide, we will pick a glibc based one.

`root #````
mkdir /mnt/erlite-3/
```
`root #``wget` [http://distfiles.gentoo.org/experimental/mips/stages/stage3-mips64_multilib-20130715.tar.bz2](http://distfiles.gentoo.org/experimental/mips/stages/stage3-mips64_multilib-20130715.tar.bz2) 
`root #````
tar xvjf stage3-mips64_multilib-20130715.tar.bz2  -C /mnt/erlite-3
```
`root #````
rm stage3-mips64_multilib-20130715.tar.bz2 
```
### Getting the latest portage snapshot

`root #````
tar xjvpf portage-latest.tar.bz2 -C /mnt/erlite-3/usr
```
`root #````
rm portage-latest.tar.bz2
```
### Configure /etc/fstab

If you are going to use NFSroot to boot your ERLite-3 board, you need to edit the /dev/ROOT entry in your fstab like this

**`/mnt/erlite-3/etc/fstab`**

### Reset root password

In order to be able to login after the first boot, you need to reset the root password. For this edit the /mnt/erlite-3/etc/shadow file and remove the '\*' from the second column.

## Building the toolchain

The are many different ways to build a mips64 toolchain. We will use the Gentoo [sys-devel/crossdev](https://packages.gentoo.org/packages/sys-devel/crossdev) script to build one.

`root #``emerge --ask crossdev`
Create a cross toolchain for MIPS64 (big-endian system):

`root #``crossdev -t mips64-unknown-linux-gnu`
Make sure you read this [crossdev info](https://wiki.gentoo.org/wiki/Embedded_Handbook/General/Creating_a_cross-compiler) and this [Embedded handbook](https://wiki.gentoo.org/wiki/Embedded_Handbook) to understand how to use crossdev in your day-to-day development.

## The MIPS64 Linux Kernel

### Getting the Kernel

We will use the Linux Kernel from the Linus' git repo for now, but there might be other repos more suitable for this board out there. If you find one, please edit this guide as appropriate.

`root #````
mkdir /mnt/erlite-3-kernel ; cd /mnt/erlite-3-kernel
```
`root #````
git clone --depth 1 git://git.kernel.org/pub/scm/linux/kernel/git/torvalds/linux.git
```
`root #````
cd linux
```
### Building the Kernel

`root #````
ARCH=mips CROSS_COMPILE=mips64-unknown-linux-gnu- make cavium_octeon_defconfig
```
`root #````
ARCH=mips CROSS_COMPILE=mips64-unknown-linux-gnu- make
```
`root #````
ARCH=mips CROSS_COMPILE=mips64-unknown-linux-gnu- make modules_install INSTALL_MOD_PATH=/mnt/erlite-3/
```
## Booting Up

### NFS Boot

#### Install and Configure the TFTP Server

The TFTP server will be used to load the new kernel image on the board.
For this, you need the [net-ftp/tftp-hpa](https://packages.gentoo.org/packages/net-ftp/tftp-hpa) package.

`root #``emerge --ask tftp-hpa`
Edit the /etc/conf.d/in.tftpd file as appropriate:

**`/etc/conf.d/in.tftpd`**

And start the service (add it to the default runlevel if you wish)

`root #````
rc-update add in.tftpd default (optional)
```
`root #``/etc/init.d/in.tftpd start`
#### Install and Configure the NFS Server

The NFS Server will export the ([previously extracted and prepared](https://wiki.gentoo.org#Preparing_the_Gentoo_RootFS)) stage3 (/mnt/erlite-3) to the ERLite-3 board.

In order to use your PC as an NFS server, you need the following kernel options to be enabled:

You also need to build and configure the [net-fs/nfs-utils](https://packages.gentoo.org/packages/net-fs/nfs-utils) as follows:

`root #``emerge --ask nfs-utils`
Then edit the /etc/exportfs as follows:

**`/etc/exports`**

`root #````
rc-update add nfs default (optional)
```
`root #``/etc/init.d/nfs start`
#### Prepare U-Boot for NFS boot

U-boot is pre-configured as follows:

For NFS boot, we need to set the following variables:

ipaddr : IP for the ERLite-3 board (e.g. 192.168.1.2)
serverip : IP for the PC acting as tftp server (e.g. 192.168.1.3)
bootcmd : We need to override the existing 'bootcmd' command with one suitable for tftp boot

For this, use the following commands in the u-boot command line (Press Ctrl-C to interrupt the boot process)

setenv ipaddr 192.168.1.2
setenv serverip 192.168.1.3
setenv bootcmd 'tftpboot $loadaddr vmlinux; bootoctlinux $loadaddr coremask=0x3 ip=192.168.1.2::192.168.1.254:255.255.255.0:edge:eth0:off root=/dev/nfs nfsroot=192.168.1.3:/mnt/erlite-3/,tcp,vers=3

And now save the new configuration

saveenv

Now, it is time to reset the router and boot into your shiny new Gentoo MIPS64 rootfs.

### USB Boot

Successful boot from USB drive is confirmed on vanilla kernel 3.11\_rc4, same is true fo Kernel 3.17. A kernel configuration file is available at [https://github.com/xypron/kernel-edgerouter](https://github.com/xypron/kernel-edgerouter).

Console

Related options in kernel config

`user $``pinkbyte@octeon ~ $ zcat /proc/config.gz | grep OCTEON`
CONFIG\_CAVIUM\_OCTEON\_SOC=y
# CONFIG\_CAVIUM\_OCTEON\_2ND\_KERNEL is not set
CONFIG\_CAVIUM\_OCTEON\_CVMSEG\_SIZE=2
CONFIG\_CAVIUM\_OCTEON\_LOCK\_L2=y
CONFIG\_CAVIUM\_OCTEON\_LOCK\_L2\_TLB=y
CONFIG\_CAVIUM\_OCTEON\_LOCK\_L2\_EXCEPTION=y
CONFIG\_CAVIUM\_OCTEON\_LOCK\_L2\_LOW\_LEVEL\_INTERRUPT=y
CONFIG\_CAVIUM\_OCTEON\_LOCK\_L2\_INTERRUPT=y
CONFIG\_CAVIUM\_OCTEON\_LOCK\_L2\_MEMCPY=y
# CONFIG\_OCTEON\_ILM is not set
CONFIG\_CPU\_CAVIUM\_OCTEON=y
CONFIG\_SYS\_HAS\_CPU\_CAVIUM\_OCTEON=y
CONFIG\_PATA\_OCTEON\_CF=y
CONFIG\_OCTEON\_MGMT\_ETHERNET=y
CONFIG\_MDIO\_OCTEON=y
CONFIG\_HW\_RANDOM\_OCTEON=y
CONFIG\_I2C\_OCTEON=y
CONFIG\_SPI\_OCTEON=y
CONFIG\_OCTEON\_WDT=y
CONFIG\_USB\_OCTEON\_EHCI=y
CONFIG\_USB\_OCTEON\_OHCI=y
CONFIG\_USB\_OCTEON2\_COMMON=y
CONFIG\_EDAC\_OCTEON\_PC=y
CONFIG\_EDAC\_OCTEON\_L2C=y
CONFIG\_EDAC\_OCTEON\_LMC=y
CONFIG\_EDAC\_OCTEON\_PCI=y
CONFIG\_OCTEON\_ETHERNET=y
CONFIG\_OCTEON\_USB=y

Kernel command line parameters can be adjusted through U-boot.
