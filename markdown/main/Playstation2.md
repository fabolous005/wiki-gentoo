<!-- source: https://wiki.gentoo.org/wiki/Playstation2 | group: Gentoo Wiki (Main) | wiki-title: Playstation2 -->
---
title: Playstation2
url: https://wiki.gentoo.org/wiki/Playstation2
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-06-14"
fingerprint: "9afd1e4eefc5a702"
license: CC BY-SA 4.0
---

# Playstation2

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

This guide is intended for those that wish to build their own version of Gentoo using MUSL for the Sony Playstation 2, for those looking at just downloading the file to play with then check the instructions at [https://github.com/frno7/gentoo-mipsr5900el/wiki/Gentoo-live-USB-for-the-PlayStation-2](https://github.com/frno7/gentoo-mipsr5900el/wiki/Gentoo-live-USB-for-the-PlayStation-2) where I have provided a mirror which will be used a LiveCD to install Gentoo directly to your PS2 hard drive once support is added to the kernel.

## Installing Gentoo on the PlayStation 2

### Crossdev

emerge crossdev to allow building of MIPS environment:

`root #``emerge --ask crossdev`
As the PS2 only has 32MB of RAM the use of MUSL is recommended but for this to work a [patch](https://github.com/frno7/gentoo-mipsr5900el/blob/main/patches/musl/r5900-ll-sc.patch) must be manually added until it's added to the main tree [bug #851759](https://bugs.gentoo.org/show_bug.cgi?id=851759).

`root #``mkdir -p /etc/portage/patches/cross-mipsr5900el-unknown-linux-musl/musl/``root #``wget` [https://raw.githubusercontent.com/frno7/gentoo-mipsr5900el/main/patches/musl/r5900-ll-sc.patch](https://raw.githubusercontent.com/frno7/gentoo-mipsr5900el/main/patches/musl/r5900-ll-sc.patch)`root #``mv r5900-ll-sc.patch /etc/portage/patches/cross-mipsr5900el-unknown-linux-musl/musl/`
Now to create the cross compiler:

`root #``crossdev -s4 -t mipsr5900el-unknown-linux-musl`
If no errors were reported, move on to the next section.

### Building the Environment

#### Set the system profile

The PS2 is 32bit [little endian](https://en.wikipedia.org/wiki/Endianness) CPU and currently only the [o32](https://en.wikipedia.org/wiki/MIPS_architecture#Calling_conventions) ABI is supported however the profile for MUSL is needed:

`root #``PORTAGE_CONFIGROOT=/usr/mipsr5900el-unknown-linux-musl  eselect profile list`
\[1\]   default/linux/mips/17.0/o32 (exp)
 \[2\]   default/linux/mips/17.0/o32/systemd/merged-usr (exp)
 \[3\]   default/linux/mips/17.0/n32 (exp)
 \[4\]   default/linux/mips/17.0/n32/systemd/merged-usr (exp)
 \[5\]   default/linux/mips/17.0/n64 (exp)
 \[6\]   default/linux/mips/17.0/n64/systemd/merged-usr (exp)
 \[7\]   default/linux/mips/17.0/multilib/o32 (exp)
 \[8\]   default/linux/mips/17.0/multilib/n32 (exp)
 \[9\]   default/linux/mips/17.0/multilib/n32/systemd/merged-usr (exp)
 \[10\]  default/linux/mips/17.0/multilib/n64 (exp)
 \[11\]  default/linux/mips/17.0/mipsel/o32 (exp)
 \[12\]  default/linux/mips/17.0/mipsel/o32/systemd/merged-usr (exp)
 \[13\]  default/linux/mips/17.0/mipsel/n32 (exp)
 \[14\]  default/linux/mips/17.0/mipsel/n32/systemd/merged-usr (exp)
 \[15\]  default/linux/mips/17.0/mipsel/n64 (exp)
 \[16\]  default/linux/mips/17.0/mipsel/n64/systemd/merged-usr (exp)
 \[17\]  default/linux/mips/17.0/mipsel/multilib/o32 (exp)
 \[18\]  default/linux/mips/17.0/mipsel/multilib/n32 (exp)
 \[19\]  default/linux/mips/17.0/mipsel/multilib/n32/systemd/merged-usr (exp)
 \[20\]  default/linux/mips/17.0/mipsel/multilib/n64 (exp)
 \[21\]  default/linux/mips/17.0/mipsel/multilib/n64/systemd/merged-usr (exp)
 \[22\]  default/linux/mips/23.0/mipsel/o32\_sf (stable)
 \[23\]  default/linux/mips/23.0/mipsel/o32\_sf/systemd (stable)
 \[24\]  default/linux/mips/23.0/mipsel/o32 (stable)
 \[25\]  default/linux/mips/23.0/mipsel/o32/systemd (stable)
 \[26\]  default/linux/mips/23.0/mipsel/n32 (stable)
 \[27\]  default/linux/mips/23.0/mipsel/n32/systemd (stable)
 \[28\]  default/linux/mips/23.0/mipsel/n64 (stable)
 \[29\]  default/linux/mips/23.0/mipsel/n64/systemd (stable)
 \[30\]  default/linux/mips/23.0/mipsel/multilib/n32 (dev)
 \[31\]  default/linux/mips/23.0/mipsel/multilib/n32/systemd (dev)
 \[32\]  default/linux/mips/23.0/mipsel/multilib/n64 (dev)
 \[33\]  default/linux/mips/23.0/mipsel/multilib/n64/systemd (dev)
 \[34\]  default/linux/mips/23.0/o32\_sf (stable)
 \[35\]  default/linux/mips/23.0/o32\_sf/systemd (stable)
 \[36\]  default/linux/mips/23.0/o32 (stable)
 \[37\]  default/linux/mips/23.0/o32/systemd (stable)
 \[38\]  default/linux/mips/23.0/n32 (stable)
 \[39\]  default/linux/mips/23.0/n32/systemd (stable)
 \[40\]  default/linux/mips/23.0/n64 (stable)
 \[41\]  default/linux/mips/23.0/n64/systemd (stable)
 \[42\]  default/linux/mips/23.0/multilib/n32 (dev)
 \[43\]  default/linux/mips/23.0/multilib/n32/systemd (dev)
 \[44\]  default/linux/mips/23.0/multilib/n64 (dev)
 \[45\]  default/linux/mips/23.0/multilib/n64/systemd (dev)
 \[46\]  default/linux/mips/23.0/split-usr/mipsel/o32\_sf (dev)
 \[47\]  default/linux/mips/23.0/split-usr/mipsel/o32 (dev)
 \[48\]  default/linux/mips/23.0/split-usr/mipsel/n32 (dev)
 \[49\]  default/linux/mips/23.0/split-usr/mipsel/n64 (stable)
 \[50\]  default/linux/mips/23.0/split-usr/mipsel/multilib/n32 (dev)
 \[51\]  default/linux/mips/23.0/split-usr/mipsel/multilib/n64 (dev)
 \[52\]  default/linux/mips/23.0/split-usr/o32\_sf (dev)
 \[53\]  default/linux/mips/23.0/split-usr/o32 (dev)
 \[54\]  default/linux/mips/23.0/split-usr/n32 (dev)
 \[55\]  default/linux/mips/23.0/split-usr/n64 (stable)
 \[56\]  default/linux/mips/23.0/split-usr/multilib/n32 (dev)
 \[57\]  default/linux/mips/23.0/split-usr/multilib/n64 (dev)
 \[58\]  default/linux/mips/17.0/o32/musl (exp)
 \[59\]  default/linux/mips/17.0/n64/musl (exp)
 \[60\]  default/linux/mips/17.0/mipsel/o32/musl (exp)
 \[61\]  default/linux/mips/17.0/mipsel/n64/musl (exp)
 \[62\]  default/linux/mips/23.0/mipsel/o32/musl (dev)
 \[63\]  default/linux/mips/23.0/mipsel/n64/musl (exp)
 \[64\]  default/linux/mips/23.0/o32/musl (dev)
 \[65\]  default/linux/mips/23.0/n64/musl (exp)
 \[66\]  default/linux/mips/23.0/split-usr/mipsel/o32/musl (exp)
 \[67\]  default/linux/mips/23.0/split-usr/mipsel/n64/musl (exp)
 \[68\]  default/linux/mips/23.0/split-usr/o32/musl (exp)

\[69\] default/linux/mips/23.0/split-usr/n64/musl (exp)

`root #``PORTAGE_CONFIGROOT=/usr/mipsr5900el-unknown-linux-musl  eselect profile set default/linux/mips/23.0/mipsel/o32/musl`
#### Common Flags

As already mentioned the PS2 has very little RAM so using the -Os optimisation is recommended as is targetting for the PS2's CPU the r5900.

**`/usr/mipsr5900el-unknown-linux-musl/etc/portage/make.conf`**

#### USEFLAGS and Workarounds

To stop errors the following USEFLAGS need to be set:

`root #``mkdir /usr/mipsr5900el-unknown-linux-musl/etc/portage/package.use`
**`/usr/mipsr5900el-unknown-linux-musl/etc/portage/package.use/system`**

**`/usr/mipsr5900el-unknown-linux-musl/etc/portage/profile/package.provided`**

And disable LTO for perl:

`root #``mkdir /usr/mipsr5900el-unknown-linux-musl/etc/portage/package.env`
**`/usr/mipsr5900el-unknown-linux-musl/etc/portage/package.env/perl`**

`root #``mkdir /usr/mipsr5900el-unknown-linux-musl/etc/portage/env`
**`/usr/mipsr5900el-unknown-linux-musl/etc/portage/env/no-lto.conf`**

#### Patches

Again due to [bug #851759](https://bugs.gentoo.org/show_bug.cgi?id=851759) the required patch for MUSL to build is needed:

`root #``mkdir -p /usr/mipsr5900el-unknown-linux-musl/etc/portage/patches/sys-libs/musl``root #``cd /usr/mipsr5900el-unknown-linux-musl/etc/portage/patches/sys-libs/musl`
#### Emerge the system

`root #``USE="-openmp" mipsr5900el-unknown-linux-musl-emerge -va1 @system`
Next, emerge dhcpcd for networking once the PS2 has booted.

`root #``emerge --ask net-misc/dhcpcd`
#### Tarball creation

Create a tar.xz of the system files created ready for use later on.

`root #``cd /usr/mipsr5900el-unknown-linux-musl/``root #``tar -cvf /home/USER/PS2-Gentoo-MUSL.tar.xz .`
Change the \`/home/USER\` part to where every is a suitable location to store this tarball.

### PS2 Kernel

Make sure git is installed on the system before moving on:

`root #``emerge --ask dev-vcs/git`
#### initramfs

Using the premade [initramfs](https://wiki.gentoo.org/wiki/Initramfs) files as building our own doesn't achieve anything special, select either initramfs-ntsc-ps2 or initramfs-pal-ps2 depending on the region of your PS2 (currently only pal).

Choose a suitable location to unpack these files, then begin to edit the initramfs/init file ready to load Gentoo on boot.

`root #``unzip initramfs*.zip``root #``tar xvf initramfs-*.tar.gz``root #``nano initramfs/init`
**`initramfs/init`**

Replace the existing contents with the information above. This script will load all the required modules then switch the root to boot Gentoo from the USB drive which will be setup later in the guide.

#### Building the Kernel

Checkout the source from git using the follow:

To avoid issues with your host machine later on, set the environmental flags on each command to build the kernel with the INITRAMFS created in the last step.

`root #``ARCH=mips CROSS_COMPILE=mipsr5900el-unknown-linux-musl- INSTALL_MOD_PATH=../initramfs INSTALL_MOD_STRIP=1 make ps2_defconfig``root #``ARCH=mips CROSS_COMPILE=mipsr5900el-unknown-linux-musl- INSTALL_MOD_PATH=../initramfs INSTALL_MOD_STRIP=1 make olddefconfig``root #``ARCH=mips CROSS_COMPILE=mipsr5900el-unknown-linux-musl- INSTALL_MOD_PATH=../initramfs INSTALL_MOD_STRIP=1 make vmlinux``root #``ARCH=mips CROSS_COMPILE=mipsr5900el-unknown-linux-musl- INSTALL_MOD_PATH=../initramfs INSTALL_MOD_STRIP=1 make modules``root #``ARCH=mips CROSS_COMPILE=mipsr5900el-unknown-linux-musl- INSTALL_MOD_PATH=../initramfs INSTALL_MOD_STRIP=1 make modules_install``root #``rm ../initramfs/lib/modules/*/{build,modules.*,source}``root #``ARCH=mips CROSS_COMPILE=mipsr5900el-unknown-linux-musl- INSTALL_MOD_PATH=../initramfs INSTALL_MOD_STRIP=1 make vmlinuz`
If no errors occurred then a file called vmlinuz was created which needs to be renamed to boot on the PS2:

`root #``mv vmlinuz PS2Linux.ELF`
### Preparing the USB Drive

Create two partitions on a USB drive, with one being a 256MB w95 partition and the rest being a Linux partition.

`root #````
fdisk -l /dev/sdc
Device     Boot  Start      End  Sectors  Size Id Type
```
Disk /dev/sdc: 14.91 GiB, 16005464064 bytes, 31260672 sectors

Disk model: Cruzer Glide

Units: sectors of 1 * 512 = 512 bytes

Sector size (logical/physical): 512 bytes / 512 bytes

I/O size (minimum/optimal): 512 bytes / 512 bytes

Disklabel type: dos

Disk identifier: 0x9579130d


/dev/sdc1         2048   526335   524288  256M  b W95 FAT32

/dev/sdc2       526336 31260671 30734336 14.7G 83 Linux
Now create the following filesystems on these partitions noting the labels used as these are important to making the INITRAMFS work correctly that was setup earlier.

`root #``emerge --ask sys-fs/dosfstools``root #``mkfs.vfat -n PS2BOOT /dev/sdc1``root #``mkfs.ext4 -L PS2Root /dev/sdc2`
#### Copying Kernel

Mount fat32 partition to copy over the kernel created earlier that ulaunch/wlaunch will be able to read on the PS2,

`root #``mkdir /mnt/usb``root #``mount /dev/sdc1 /mnt/usb``root #``cp PS2Linux.ELF /mnt/usb/``root #``umount /mnt/usb`
#### Unpack the Gentoo Tarball

Mount the ext4 partition to unpack the tarball from ealier,

`root #``mount /dev/sdc2 /mnt/usb``root #``cd /mnt/usb``root #``tar xvf /home/USER/PS2-Gentoo-MUSL.tar.xz`
#### Clean Up

Tidy up some unneeded files and other quality of life fixes:

`root #``rm -r /mnt/usb/cache``root #``mkdir dev home media opt root boot mnt sys`
Edit make.conf to sane defaults in case later a binhost is added:

**`/mnt/usb/etc/portage/make.conf`**
