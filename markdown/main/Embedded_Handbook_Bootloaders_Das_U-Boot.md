<!-- source: https://wiki.gentoo.org/wiki/Embedded_Handbook/Bootloaders/Das_U-Boot | group: Gentoo Wiki (Main) | wiki-title: Embedded Handbook/Bootloaders/Das U-Boot -->
---
title: Embedded Handbook/Bootloaders/Das U-Boot
url: https://wiki.gentoo.org/wiki/Embedded_Handbook/Bootloaders/Das_U-Boot
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2025-12-17"
fingerprint: "3703d21805b5bde3"
license: CC BY-SA 4.0
---

# Embedded Handbook/Bootloaders/Das U-Boot

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)



**Das U-Boot** (subtitled *the Universal Boot Loader* and often shortened to **U-Boot**)<sup>[\[1\]](https://wiki.gentoo.org#cite_note-uboot_wp-1)</sup> is an open-source boot loader used in embedded devices to initialize the hardware and load the device's [kernel](https://wiki.gentoo.org/wiki/Kernel). It is available for a number of computer architectures, including [PowerPC](https://wiki.gentoo.org/wiki/PowerPC), [ARM](https://wiki.gentoo.org/wiki/ARM), [MIPS](https://wiki.gentoo.org/wiki/MIPS), AVR32, x86, 68k, Nios II, and MicroBlaze.[\[1\]](https://wiki.gentoo.org#cite_note-uboot_wp-1)

U-Boot is more than a simple boot loader, providing an interactive command-line interface or shell. This allows users and developers to load and boot a kernel from a variety of sources, including flash memory, SD (TF) cards, SATA drives, or over a network using TFTP. Its scripting capabilities and memory manipulation commands make it a valuable tool for debugging and system development.

## U-Boot Mind-set

Aside from a few odd/legacy boards, most of the current device support in u-boot should follow the Linux kernel devicetree and u-boot driver models, albeit with each vendor on their own upgrade cycle. That said, well-maintained SoC families should have a device tree file and corresponding defconfig, but more importantly, most current board configs should have at least CONFIG\_DISTRO\_DEFAULTS and basic EFI support enabled. As of v2022.10 there were 355 defconfig files with CONFIG\_DISTRO\_DEFAULTS enabled.

To effectively use u-boot on a given device (board), the following information is generally required:

- any additional source repositories (eg, TFA)
- which build artifacts are needed (eg, flash-image.bin)
- how and where are these artifacts installed (eg, dd to MMC device, SPI flash via u-boot command, or external flash tool)
- debug UART connector and device names (eg, debug header or micro-USB/FTDI)

Board examples:

Each board is different and may or may not have an accessible debug header or on-board USB-UART. Check vendor docs and wikis, u-boot docs and search engines. Most boards with RPI-compatible header can use the UART pins near one end of the 40-pin header. For serial console to work, inittab needs the correct device name, baud rate, and term type.

- esspressobin (**ttyMV0**) - micro-USB connector on the "front" side of the board is debug UART
- beaglebone (old: **ttyO0** | new: **ttyS0**) - 6-pin debug UART header behind the USB-A host connector
- orange-pi variants (**ttyS0**) - 3-pin debug UART header, often near ethernet or USB host connector
- rockchip variants (**ttyS2 @ 1500000**) - 3 pins for UART2 on RPI header (the end near the USB connector)

Note for the latter two examples above, the device name may end in a different number depending on how many UART ports are actually there and enabled.

## Gentoo Specific Instructions

There is no package for the **Das U-boot** source code. Follow the instructions in the [documentation](http://www.denx.de/wiki/U-Boot/Documentation) for obtaining the source from git or an archived release.

There is a package for the Das U-boot utility tools (such as **mkimage**), see [u-boot-tools](https://wiki.gentoo.org/wiki/U-boot-tools).

It is most likely the build will be cross-compiled. See [Crossdev](https://wiki.gentoo.org/wiki/Crossdev).

## Building U-boot for an ARMv7 Target

Building the bootloader for a supported armv7 FOSS device such as beaglebone is fairly simple and straightforward, and generally only requires the u-boot sources.

Build target cross-compiler:

`root #``crossdev -t armv7a-unknown-linux-gnueabihf`
Download u-boot:

`user $``git clone -b v2023.04` [https://github.com/u-boot/u-boot](https://github.com/u-boot/u-boot)`user $``cd u-boot/`
Find the configuration for your device; if there is no exact match in the existing defconfigs, there may be a close enough match (eg, in the case of Allwinner fruity-pi or Rockchip 3328 boards).

Configure and build u-boot:

`user $``make ARCH=arm CROSS_COMPILE=armv7a-unknown-linux-gnueabihf- distclean``user $``make ARCH=arm CROSS_COMPILE=armv7a-unknown-linux-gnueabihf- <your_device>_defconfig``user $``make ARCH=arm CROSS_COMPILE=armv7a-unknown-linux-gnueabihf-`
For most FOSS developmment boards, the resulting u-boot images are intended for booting SDCard/EMMC, or possibly installing in SPI flash.

For example, the udoo IMX6quad u-boot binfiles are:

1. SPL
2. u-boot.img

Use the **dd** command to install them in the space before the first partition. Note this will destroy all data on the card!

Erase partition table/labels on microSD card:

`root #``dd if=/dev/zero of=${DISK} bs=1M count=10`
Install bootloader bins:

`root #``dd if=./SPL of=${DISK} seek=1 bs=1k``root #``dd if=./u-boot.img of=${DISK} seek=69 bs=1k`
where usually DISK=/dev/sdX

## Building U-boot for an ARM64 Target

Building the bootloader for at least a few supported arm64 FOSS devices such as rock-pi-4 is still simple-ish, and only requires 2 cross-compilers and 2 source trees (and possibly some blobs). The example rk3399 is a fully supported platform in both TFA and u-boot.

- check board vendor docs and repos for bootloader forks and additional bootloader repos
- check u-boot docs and source for board support
- check TFA docs and source for platform support and possible build recipes

Build target cross-compilers:

`root #``crossdev -t arm-none-eabi``root #``crossdev -t aarch64-unknown-linux-gnu``user $``export M0_CROSS_COMPILE=arm-none-eabi-`
Download TFA:

`user $``git clone` [https://github.com/ARM-software/arm-trusted-firmware](https://github.com/ARM-software/arm-trusted-firmware)`user $``cd arm-trusted-firmware/`
Configure and Build:

`user $``make CROSS_COMPILE=aarch64-unknown-linux-gnu- realclean``user $``make CROSS_COMPILE=aarch64-unknown-linux-gnu- PLAT=rk3399`
Export BL31.elf:

`user $```export BL31=`pwd`/build/rk3399/release/bl31/bl31.elf``
Download u-boot:

`user $``git clone -b v2023.04` [https://github.com/u-boot/u-boot](https://github.com/u-boot/u-boot)`user $``cd u-boot/`
Configure and Build:

`user $``make ARCH=arm CROSS_COMPILE=aarch64-unknown-linux-gnueabihf- distclean``user $``make ARCH=arm CROSS_COMPILE=aarch64-unknown-linux-gnueabihf- rock-pi-4-rk3399_defconfig``user $``make ARCH=arm CROSS_COMPILE=aarch64-unknown-linux-gnueabihf-`
Here the output is one or more u-boot blobs for installing on SDCard/EMMC but for other boards using SPI flash images (eg, espressobin), the output might come from TFA, as u-boot is just one of the input source trees for Marvell Armada. For modern rockchip devices and relatively recent u-boot/TFA, the build output is the combined SPL/TPL and dtb flash file `u-boot-rockchip.bin`, where most of the Internet still refers to the following dtb and loader files shown below.  Using the single flash file with `seek=64` should be equivalent to the steps shown below.

The (older) rock-pi-4 u-boot binfiles are:

1. idbloader.img
2. u-boot.itb

Use the **dd** command to install them in the space before the first partition. Note this should not destroy all data on the card IFF the first partition starts at 16MB (32768 sectors for 512 byte sectors).

Erase partition table/labels on microSD card:

`root #``dd if=/dev/zero of=${DISK} bs=1M count=20`
Install bootloader bins:

`root #``dd if=./idbloader.img of=${DISK} seek=64` `root #``dd if=./u-boot.itb of=${DISK} seek=16384`
where usually DISK=/dev/sdX

## Gentoo U-boot Examples

- [ESPRESSOBin](https://wiki.gentoo.org/wiki/ESPRESSOBin) arm64 SPI flash
- [Udoo](https://wiki.gentoo.org/wiki/Udoo) armv7 SDCard (old)
- [PINE64\_ROCKPro64/Installing\_U-Boot](https://wiki.gentoo.org/wiki/PINE64_ROCKPro64/Installing_U-Boot) arm64 SDCard

## External References

## Booting a new distro kernel with U-Boot

In addition to new boards, U-boot development has also been focused on moving existing SoC families to newer APIs and driver models, including syncing U-boot with kernel devicetree files periodically and pushing towards more agnostic boot flows and standardized distribution support. A recent U-boot version should support booting several different kernel image formats from a variety of media.

Depending on which u-boot bits are configured/used, several types of files can be booted:

- raw binaries
- FIT images
- various kernel images
- legacy U-Boot images
- UEFI binaries

which can then be used with multiple boot methods:

- legacy boot.scr
- extlinux configuration (ala syslinux)
- EFI boot

*However*, the extlinux boot method **does not** use the bootefi command, and *only* the bootefi command can boot a (grub) EFI binary. The upshot is extlinux will fail to boot the new signed distribution kernels ([sys-kernel/gentoo-kernel-bin](https://packages.gentoo.org/packages/sys-kernel/gentoo-kernel-bin) version 6.5.x and up) but u-boot can still be used to boot the EFI grub binary (using the minimum GPT partition layout, eg, a 512 MB ESP and the rest in ext4 rootfs).

#### Prerequisites

The following boot flow was tested on espressobin v5 and both nanopi-r5c (rk3568) and rk3328 libre board using the appropriate u-boot builds for each board.

1. stage3/4 arm64 on USB stick, SSD, or SDCard
2. make a small dracut configuration to match your boot requirements
3. check u-boot environment/defconfig for distro\_bootcmd or bootflow, upgrade/rebuild u-boot if needed
4. emerge [sys-kernel/gentoo-kernel-bin](https://packages.gentoo.org/packages/sys-kernel/gentoo-kernel-bin)
5. emerge [sys-boot/grub](https://packages.gentoo.org/packages/sys-boot/grub) with efi support and [devicetree patches](https://github.com/VCTLabs/embedded-overlay)
6. install grub with the removable flag, eg: `grub-install --target=arm64-efi --efi-directory=/boot/efi --removable`
7. edit `/etc/default/grub` and set GRUB\_DEFAULT\_DTB to the appropriate dtb, eg `marvell/armada-3720-espressobin.dtb`
8. add `efi=noruntime` to GRUB\_CMDLINE\_LINUX
9. run `grub-mkconfig -o /boot/grub/grub.cfg`

According to both the TFA and U-boot docs, the trusted firmware approach should also work on at least some armv7 devices, eg, rk3288.

The following display shows the tested version(s) of the espressobin bootloader components, where almost everything is the latest/stable vendor and TFA versions, and u-boot was left at 2022.10:

With kernel, initramfs, and updated grub.cfg in place, try booting your device; note this should work on whatever the current u-boot support is configured for, eg, MMC, USB, SATA, NVME, PXE, etc.

If it doesn't boot to the grub menu, check the following:

1. grub install used the removable flag
2. u-boot env shows appropriate bootcmd value
3. verify EFI options in u-boot `.config` file
4. append `efi=noruntime` to the kernel commandline

### Fallback extlinux.conf method

*Note that this method only works with unsigned/locally built kernel images.*

The basic requirement for the *distro\_bootcmd* to run is the extlinux.conf file; this assumes the kernel and dtbs were installed using the default kernel install paths. A basic example would look something like the following:

**`extlinux.conf`**

```
LABEL Gentoo arm64
        KERNEL ../vmlinuz-5.10.14-aarch64-x0
        APPEND console=ttyS0,115200 root=/dev/mmcblk0p1 rw rootfstype=ext4 rootwait net.ifnames=0
        FDTDIR ../dtbs/5.10.14-aarch64-x0/
```
Create a file similar to the above under `/boot/extlinux`. Be sure to use tabs for indenting, and make sure to **use your console and root devices, along with your kernel version** in the config file you create:

`user $````
cd /mnt/gentoo/boot
```
`user $````
sudo mkdir extlinux
```
`user $````
sudo nano extlinux/extlinux.conf
```
`user $``cd -`


1. ↑ <sup>[1.0](https://wiki.gentoo.org#cite_ref-uboot_wp_1-0)</sup> <sup>[1.1](https://wiki.gentoo.org#cite_ref-uboot_wp_1-1)</sup> [Wikipedia U-Boot article](https://en.wikipedia.org/wiki/Das_U-Boot). denx.de. Retrieved 2025-08-23.
