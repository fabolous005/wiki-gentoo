<!-- source: https://wiki.gentoo.org/wiki/Raspberry_Pi/Mainline_Kernel | group: Gentoo Wiki (Main) | wiki-title: Raspberry Pi/Mainline Kernel -->
---
title: Raspberry Pi/Mainline Kernel
url: https://wiki.gentoo.org/wiki/Raspberry_Pi/Mainline_Kernel
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2025-12-17"
fingerprint: "3953819f46df9927"
license: CC BY-SA 4.0
---

# Raspberry Pi/Mainline Kernel

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

This guide is intended as a supplement to [Install Guide](https://wiki.gentoo.org/wiki/Raspberry_Pi/Quick_Install_Guide) and an alternative to [Raspberry Pi Kernel](https://wiki.gentoo.org/wiki/Raspberry_Pi/Kernel_Compilation).

### Build tools

`root #``emerge --ask sys-apps/dtc`
### Fetch sources

`root #``emerge --ask sys-kernel/vanilla-sources`
For the rest of this example, I will be assuming vanilla-sources-4.14.21.

`root #````
cd /usr/src/linux-4.14.21
```
### Configure the kernel

#### Raspberry Pi 2

`root #````
make multi_v7_defconfig
```
Note: multi\_v7\_defconfig will enable more than you need, but it is currently the most appropriate included defconfig to choose from.

#### Raspberry Pi 3 (32 bit)

`root #````
make multi_v7_defconfig
```
Note: multi\_v7\_defconfig will enable more than you need, but it is currently the most appropriate included defconfig to choose from.

### Build the kernel

`root #````
make
```
`root #````
make zinstall modules_install dtbs_install
```
### Configuring bootloader

Contrary to popular belief, you don't need u-boot to boot the mainline kernel, just populate the appropriate required files:

#### Raspberry Pi 2

**`/boot/config.txt`**

**`/boot/cmdline.txt`**

You can specify the root as a device or via PARTUUID, just ensure it's accurate for your system.

#### Raspberry Pi 3 (32 bit)

**`/boot/config.txt`**

**`/boot/cmdline.txt`**

You can specify the root as a device or via PARTUUID, just ensure it's accurate for your system.

### Installing firmware

Finally, until [rpi-open-firmware](https://github.com/christinaa/rpi-open-firmware) is ready, you'll need to copy the following binary blobs from [raspberrypi-firmware](https://github.com/raspberrypi/firmware/tree/master/boot) to your /boot partition.

- bootcode.bin
- start.elf
- fixup\_\*.dat
- start\_\*.bin
