<!-- source: https://wiki.gentoo.org/wiki/Hardkernel_ODROID-M1S | group: Gentoo Wiki (Main) | wiki-title: Hardkernel ODROID-M1S -->
---
title: Hardkernel ODROID-M1S
url: https://wiki.gentoo.org/wiki/Hardkernel_ODROID-M1S
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2024-04-12"
fingerprint: "91843b7a97eb2fc4"
license: CC BY-SA 4.0
---

# Hardkernel ODROID-M1S

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

This article is a supplementary guide for the nuances of installing Gentoo on the M1S system.  This guide assumes the user can boot the system via a drive that is not the installation destination(SD card or onboard MMC). It will begin with assuming the user has done nothing to the XU4 and assuming that the M1S has internet access to it.  The guide will walk through steps that are outside the standard Gentoo handbook, and will refer back to the handbook for standard installation sections.  This guide will cover, getting a boot-able image usable for installation,  minor U-boot configuration, and lastly kernel requirements to make a boot-able system.  This guide is based off the [ODROID-XU4](https://wiki.gentoo.org/wiki/Hardkernel_ODROID-XU4) and [ODROID-N2](https://wiki.gentoo.org/wiki/Hardkernel_ODROID-N2) Wiki articles.

## Prerequisites

- Computer running linux or unix based OS
- Access to read/write to an SD-Card from this computer

## Booting installation media

### Getting an image

The official documentation is here and it points to images the user can download. [Odroid M1S OS images](https://wiki.odroid.com/odroid-m1s/os_images/os_images) Although this link uses an older image, for the purpose of this document the new version of Ubuntu 22 for the m1s was used.  Available from here [Ubuntu M1S images](http://ppa.linuxfactory.or.kr/images/raw/arm64/jammy/)  At the time of this writing this is the image that was used.

### Micro SD-Card preparation Option 1

The boot process of the M1s is described in the [ODROID-M1S wiki](https://wiki.odroid.com/odroid-m1s/board_support/boot_sequence) First the boot software loads U-Boot, which as to be a specific location, which then loads the actual kernel from the boot partition.  Boot order is the SD card first then the eMMC

### OTG mmc writing

This has not been tested during the writing of this document, but according to the odroid wiki you can use OTG and their SDK to write to the eMMC drive.

### Place image on SD card

Once the image file has been downloaded, write it to the SD card. Please be careful of your system and drive lettering. Many people also use something like etcher to write images for windows or other operating systems.

`root #``xz -dc ubuntu-22.04-server-odroidm1s-20231114.img.xz | dd of=/dev/sdY status=progress`
### Boot and installation

Place the SD card in the system and power the system on. A USB keyboard and HDMI compatible monitor or a Serial adapter compatible with the interface on the board will be required if access to the system is needed(EG: network is not working). SSH into the system using username odroid and password is the same.

## Gentoo installation

Installation from this point the user can follow the standard handbook. Come back here when the kernel is ready to be configured.

## Kernel building

The user can build the kernel manually or with genkernel. Below is a list of options that will need to be enabled. At the time of this writing only the 6.1.x kernel was able to boot and operate properly. 6.6.x and 5.10 were attempted, but did not work.

Edit kernel options:

## Making initramfs

For this build Dracut is used for building the initramfs. This is done for the user when running make install in the kernel source tree. Before doing the dracut configuration needs to be modified. You will also need u-boot-tools installed.

### Install packages

`root #``emerge --ask sys-kernel/dracut dev-embedded/u-boot-tools`
### Modify Dracut configuraiton

Then create the needed configuration file, this is needed to make it include the video card driver.

`root #``mkdir -p /etc/dracut.conf.d/``root #``echo 'add_dracutmodules+=" drm "' > /etc/dracut.conf.d/odroid.conf`
Edit the system dracut DRM script, the section that needs to be modified look like this. It is near the top of the file.

**`/usr/lib/dracut/modules.d/50drm/module-setup.sh`**

The rockchip and panfrost directories were added to this list.

## Kernel install and initramfs install

Lastly from the kernel source tree run the modules\_install and install. This will execute dracut and place the kernel and initramfs in the /boot directory.

`root #``make modules_install``root #``make install`
The make install script also handles formatting the kernel and initramfs properly for boot with uboot. There is no need to use the mkImage script from uboot.

## Boot loader configuration

The boot file system now should just have your kernel along with the supporting files. The version of uboot currently with the M1S uses a boot.scr file to handle its configuration. We will create a boot.txt file to use as the source to build the .scr file. We will also require a DTB file from the boot medium for the system.

### Kernel Device tree DTS and DTB

First we will get the Device tree files. As of this writing the kernel source does not include the needed DTS file for the M1S hardware, thus we will use the one from the install medium. Create a new session into the running installation source OS(ubuntu). Go to the /boot directory and copy the dtb-6.1.0-odroid-arm64 or which ever one is available to your boot partition.

`root #``cp /boot/dtb-6.1.0-odroid-arm64 /mnt/gentoo/boot`
Back inside the gentoo install, we will use the dtc "compiler" that comes with the kernel to "re-compile" the file.

`root #``/usr/src/linux/scripts/dtc/dtc -I dtb -O dts -o /boot/source.dts /boot/dtb-6.1.0-odroid-arm64`
Then re-compile

`root #``/usr/src/linux/scripts/dtc/dtc -I dts -O dtb -o /boot/dtb-6.1.74-gentoo-arm64 /boot/source.dts`
Cleanup the dtb file copied from the installation media

`root #``rm /boot/dtb-6.1.0-odroid-arm64`
### Creating boot.scr

Now we create a boot.txt file

**`/boot/boot.txt`**


In this file the user will need to modify their kernel arguments and kernel version string.
Kernel version is set on the line that looks like this    **setenv fk\_kvers "6.1.74-gentoo-arm64"**
Kernel boot line arguments are built using lines like this **setenv bootargs " ${bootargs} root=YOUR\_ROOT"**  Some lines are configured for the user like Console etc.   The user is required to put the correct root string for their system.(this is the kernel cmdline)

Once modifications have been made the file must be "compiled".

`root #``mkimage -A arm64 -T script -O linux -n 'boot script' -C none -d /boot/boot.txt /boot/boot.scr`
This will properly format the boot.scr file for uboot.

## Finalizing

From here it is back to the handbook to complete the installation.
