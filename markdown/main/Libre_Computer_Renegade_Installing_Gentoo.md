<!-- source: https://wiki.gentoo.org/wiki/Libre_Computer_Renegade/Installing_Gentoo | group: Gentoo Wiki (Main) | wiki-title: Libre Computer Renegade/Installing Gentoo -->
---
title: Libre Computer Renegade/Installing Gentoo
url: https://wiki.gentoo.org/wiki/Libre_Computer_Renegade/Installing_Gentoo
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-05-03"
fingerprint: "19951d1f15ef8f62"
license: CC BY-SA 4.0
---

# Libre Computer Renegade/Installing Gentoo

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

This guide is about installing Gentoo on the eMMC of a [Libre Computer Renegade](https://wiki.gentoo.org/wiki/Libre_Computer_Renegade) alias ROC-RK3328-CC single board computer. It assumes that the current platform is a Gentoo Linux one. The majority of steps however can be performed from any distribution, the Gentoo-specific way of doing things (such as installing software) just needs to be translated to the other distribution's practices.

## Additional hardware requirements

- USB-A male to male cable
- USB drive (at least 2GB but not bigger than the eMMC)

## Installing required tools

Install [sys-apps/dtc](https://packages.gentoo.org/packages/sys-apps/dtc), [sys-apps/gptfdisk](https://packages.gentoo.org/packages/sys-apps/gptfdisk) and [sys-block/parted](https://packages.gentoo.org/packages/sys-block/parted):

`root #````
emerge –ask sys-apps/dtc sys-apps/gptfdisk sys-block/parted
```
## Preparing the disks

This is the recommended partition layout:

|  | Purpose <sup>[\[1\]](https://wiki.gentoo.org#cite_note-1)</sup> | Start (Bytes) | End (Bytes) | Size (Bytes) | Partition Offset | 
|---|---|---|---|---|---|
| /dev/sdX1 | preloader (miniloader or U-Boot SPL) | 32768 | 4128767 | 4096000 | 0x40 | 
| /dev/sdX2 | U-Boot | 8388608 | 12582911 | 4194304 | 0x4000 | 
| /dev/sdX3 | trusted OS ( [ATF](https://developer.trustedfirmware.org/project/profile/1/)) | 12582912 | 16777215 | 4194304 | 0x6000 | 
| /dev/sdX4 | boot partition | 16777216 | 134217727 | 117440512 | 0x8000 | 
| /dev/sdX5 | root partition | 134217728 | arbitrary | depends | 0x40000 | 

Create the partitions on the installation media:

`root #````
parted /dev/sdX mklabel gpt
```
`root #````
parted -a optimal /dev/sdX unit b mkpart loader1 32768 4128767
```
`root #````
parted -a optimal /dev/sdX unit b mkpart loader2 8388608 12582911
```
`root #````
parted -a optimal /dev/sdX unit b mkpart atf 12582912 16777215
```
`root #````
parted -a optimal /dev/sdX unit b mkpart boot 16777216 134217727
```
`root #````
parted -a optimal /dev/sdX unit b mkpart rootfs 134217728 100%
```
`root #````
parted /dev/sdX set 4 boot on
```
Due to upstream providing only an old and buggy version of u-boot the rootfs partition's GUID must be set to B921B045-1DF0-41C3-AF44-4C6F280D3FAE:

`root #````
gdisk /dev/sdX <<EOF
```
\> x
> c
> 5
> B921B045-1DF0-41C3-AF44-4C6F280D3FAE
> w
> y
> EOF

The boot partition must contain a FAT16 filesystem. Create the appropriate file systems and mount the partitions:

`root #````
mkfs.fat -F 16 /dev/sdX4
```
`root #````
mkfs.ext4 /dev/sdX5
```
`root #````
mkdir /mnt/{newboot,newroot}
```
`root #````
mount /dev/sdX4 /mnt/newboot
```
`root #````
mount /dev/sdX5 /mnt/newroot
```
## Creating a cross-compiler

Create an ebuild repository for the cross toolchain by following the [section in the Crossdev article](https://wiki.gentoo.org/wiki/Crossdev#Crossdev_overlay).

Then install [sys-devel/crossdev](https://packages.gentoo.org/packages/sys-devel/crossdev) and create the cross toolchain. Replace TUPLE with the [system tuple](https://wiki.gentoo.org/wiki/Embedded_Handbook/Tuples) fitting the target architecture, i.e. aarch64-linux-gnu for the ARMv8-A based Libre Computer Renegade.

`root #````
emerge --ask sys-devel/crossdev
```
`root #````
crossdev --stable --target TUPLE
```

Finally prepare cross-compiling the kernel.

Replace TARGETARCH with the target device's architecture, arm64.

`user $````
export ARCH=TARGETARCH
```
Replace TUPLE again with the system tuple fitting the target architecture, aarch64-linux-gnu. Mind the trailing "-".

`user $````
export CROSS_COMPILE=TUPLE-
```
## Configuring the Linux kernel

If all necessary drivers are built into the kernel (i.e. no loadable modules are needed to boot the target device) there's no need to have an initramfs, which keeps things simple. Especially if the host system's architecture differs from the target device's architecture creating an initramfs would at least involve cross-compiling busybox. With that in mind configure [sys-kernel/gentoo-sources](https://packages.gentoo.org/packages/sys-kernel/gentoo-sources) as usual, cf. [Configuring the Linux kernel](https://wiki.gentoo.org/wiki/Handbook:AMD64/Installation/Kernel). As a reference a basic [example config](https://notabug.org/hashashini/example-kernel-configs-le/raw/master/roc-rk3328-cc-linux-5.2.config) might be consulted.

Build the kernel and device tree binaries:

`user $````
make Image dtbs
```
## Installing the kernel

From the kernel build directory, copy the Image and the target device's device tree binary to the desired working directory. Replace DTBINARY with the filename of the target device's device tree binary, rk3328-roc-cc.dtb.

`root #````
cp -a arch/arm/boot/Image /mnt/newboot/
```
`root #````
cp -a arch/arm/boot/dts/rockchip/DTBINARY /mnt/newboot/
```
Create directory "extlinux" on the boot partition:

`root #````
mkdir /mnt/newboot/extlinux
```
In this directory create the file extlinux.conf.

**`extlinux.conf`**

Unmount the boot partition:

`root #````
umount /mnt/newboot
```
## Creating a backup of the boot partition

For example in case someday a kernel upgrade goes wrong, backup the new boot partition.

`root #````
dd if=/dev/sdX4 of=/PATH/TO/ARBITRARY_BACKUP_LOCATION/rk3328-roc-cc-boot.img
```
## Installing the Gentoo base system

As arm64 is not yet officially supported, get a stage3 tarball from Gentoo's [experimental repository](https://gentoo.osuosl.org/experimental/arm64/) and extract it to /mnt/newroot/:

`root #````
tar xvpf /PATH/TO/STAGE3-TARBALL -C /mnt/newroot/
```
Switch the new gentoo installation to unstable and adjust make.conf to utilize all four cores of the CPU. Replace TARGETARCH with the target device's architecture, arm64.

`root #````
echo "ACCEPT_KEYWORDS=\"~TARGETARCH\"" >> /mnt/newroot/etc/portage/make.conf
```
`root #````
echo "MAKEOPTS=\"-j4\"" >> /mnt/newroot/etc/portage/make.conf
```
Provide a minimal fstab (cf. [Handbook](https://wiki.gentoo.org/wiki/Handbook:AMD64/Installation/System#Filesystem_information)):

`root #````
echo "/dev/mmcblk0p5 / ext4 defaults 0 1" > /mnt/newroot/etc/fstab
```
`root #````
echo "/dev/mmcblk0p4 /boot vfat defaults 0 0" >> /mnt/newroot/etc/fstab
```
Set the new installation’s root password to “gentoo”, cf. [Setting a default root password](https://wiki.gentoo.org/wiki/Setting_a_default_root_password):

`root #````
sed -i 's/root\:\*/root\:\$6\$I9Q9AyTL\$Z76H7wD8mT9JAyrp\/vaYyFwyA5wRVN0tze8pvM\.MqScC7BBm2PU7pLL0h5nSxueqUpYAlZTox4Ag2Dp5vchjJ0/' /mnt/newroot/etc/shadow
```
Finish this step by unmounting the bootable media’s rootfs:

`root #````
umount /mnt/newroot
```
## Installing the firmware

Download Firefly's Linux SDK<sup>[\[2\]](https://wiki.gentoo.org#cite_note-2)</sup>:

Assemble the firmware files idbloader.img, uboot.img and trust.img and the DDR init binary rk3328\_loader\_ddr786\_v1.06.243.bin which is needed for flashing to the eMMC later on:

`user $````
./build/mk-uboot.sh roc-rk3328-cc
```
Finish the remaining partitions by copying the firmware files to the USB drive.

`root #````
dd if=out/u-boot/idbloader.img of=/dev/sdX1
```
`root #````
dd if=out/u-boot/uboot.img of=/dev/sdX2
```
`root #````
dd if=out/u-boot/trust.img of=/dev/sdX3
```
## Flashing to eMMC

Download and install rkdeveloptool<sup>[\[3\]](https://wiki.gentoo.org#cite_note-3)</sup>:

`user $````
cd rkdeveloptool
```
`user $````
autoreconf -i
```
`user $````
./configure
```
`user $````
make
```
`root #````
make install
```
Force the Libre Computer Renegade into Maskrom Mode<sup>[\[4\]](https://wiki.gentoo.org#cite_note-4)</sup>:

- Pull all the USB cables (including micro USB cable and male to male USB cable) out of the board to keep the board powered off and if applicable pull out the SD card
- Use a male to male USB cable to connect the host system and the USB OTG port (lower one of double-decker, white) of the target device, for pictures see the [ROC-RK3328-CC Manual](https://roc-rk3328-cc.readthedocs.io/en/latest/flash_emmc.html#maskrom-mode)
- Connect the eMMC CLK and GND pads with metal tweezers while plugging in the micro USB cable to power on the board. Wait about 1 second before breaking the connection of the two pads


Finally flash the eMMC with the image of the USB drive.

`root #````
rkdeveloptool db      out/u-boot/rk3328_loader_ddr786_v1.06.243.bin
```
`root #````
rkdeveloptool wl 0x0  /dev/sdX
```
`root #````
rkdeveloptool rd
```
## Finalizing the installation

Boot the Libre Computer Renegade and change the root password:

`root #````
passwd
```
Consult the Handbook for [configuring the network](https://wiki.gentoo.org/wiki/Handbook:AMD64/Installation/Networking), [configuring the system](https://wiki.gentoo.org/wiki/Handbook:AMD64/Installation/System), [installing system tools](https://wiki.gentoo.org/wiki/Handbook:AMD64/Installation/Tools) and [finalizing](https://wiki.gentoo.org/wiki/Handbook:AMD64/Installation/Finalizing) the new Gentoo installation.

## References

1. [↑](https://wiki.gentoo.org#cite_ref-1) [Default storage map](http://opensource.rock-chips.com/wiki_Partitions), [Rockchip wiki](http://opensource.rock-chips.com/wiki_Main_Page). Retrieved on Jun 1st, 2019
2. [↑](https://wiki.gentoo.org#cite_ref-2) Firefly Team, [Compiling linux firmware](https://roc-rk3328-cc.readthedocs.io/en/latest/linux_compile_firmware.html), [ROC-RK3328-CC Manual](https://roc-rk3328-cc.readthedocs.io/en/latest). Retrieved on May 31st, 2019
3. [↑](https://wiki.gentoo.org#cite_ref-3) Firefly Team, [Flashing to the eMMC - rkdeveloptool](https://roc-rk3328-cc.readthedocs.io/en/latest/flash_emmc.html#rkdeveloptool), [ROC-RK3328-CC Manual](https://roc-rk3328-cc.readthedocs.io/en/latest). Retrieved on Jun 1st, 2019
4. [↑](https://wiki.gentoo.org#cite_ref-4) Firefly Team, [Flashing to the eMMC - Maskrom Mode](https://roc-rk3328-cc.readthedocs.io/en/latest/flash_emmc.html#maskrom-mode), [ROC-RK3328-CC Manual](https://roc-rk3328-cc.readthedocs.io/en/latest). Retrieved on Jun 1st, 2019
