<!-- source: https://wiki.gentoo.org/wiki/Raspberry_Pi/Kernel_Compilation | group: Gentoo Wiki (Main) | wiki-title: Raspberry Pi/Kernel Compilation -->
---
title: Raspberry Pi/Kernel Compilation
url: https://wiki.gentoo.org/wiki/Raspberry_Pi/Kernel_Compilation
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2025-12-20"
fingerprint: "5793313e90ea3f27"
license: CC BY-SA 4.0
---

# Raspberry Pi/Kernel Compilation

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

The [Raspberry Pi](https://wiki.gentoo.org/wiki/Raspberry_Pi) cannot run a vanilla Linux kernel. A patched version of the kernel is maintained by the Raspberry Pi Foundation and is available from their [GitHub page](https://github.com/raspberrypi).

## Prerequisites

To compile a kernel, [dev-vcs/git](https://packages.gentoo.org/packages/dev-vcs/git) is required to download the source code when not using [sys-kernel/raspberrypi-sources](https://packages.gentoo.org/packages/sys-kernel/raspberrypi-sources) and also (optional) [genkernel](https://wiki.gentoo.org/wiki/Genkernel) to manage the build process.

`root #``emerge --ask dev-vcs/git sys-kernel/genkernel`
## Get the kernel source

`root #``emerge --ask sys-kernel/raspberrypi-sources`
or manually:

`root #````
cd /opt
```
`root #````
git clone --depth 1 https://github.com/raspberrypi/linux.git
```
`root #````
ln -s /opt/linux /usr/src/linux
```
## Compile and install the kernel with genkernel

genkernel can build a Linux kernel with support for many different features. Follow one of the examples below that has the required features.

### Default kernel

In this example, the configuration options from the running kernel are used to compile the new kernel.

`root #``genkernel --kernel-config=/proc/config.gz kernel`
After the kernel has compiled, it will be installed into the /boot folder.

### Kernel with initramfs

This example will run menuconfig before compiling the kernel to enable any extra modules needed.
Using a kernel with an [initramfs](https://wiki.gentoo.org/wiki/Initramfs) allows loading modules, decrypt partitions and other more complex task that maybe require early in the boot process.

`root #``genkernel --kernname=rpi --menuconfig all`
To support initramfs, the following options need to be enabled in menuconfig:

After the kernel has compiled, it and the initramfs be installed into the /boot folder, add it to bootloader (skip to **[Adding new kernel to the bootloader](https://wiki.gentoo.org#Adding_new_kernel_to_the_bootloader)**)

## Compile and install the kernel without genkernel

The first time configuring the kernel sources, create a default .config file (for Raspberry Pi2 use bcm2709\_defconfig):

`root #````
cd /usr/src/linux
```
`root #````
make bcm2835_defconfig
```
After that, modify this default configuration (a good idea is to add .config support):

`root #````
cd /usr/src/linux
```
`root #````
make menuconfig
```
An example config can be found at [https://github.com/modulix/raspggen/blob/master/kernel.conf](https://github.com/modulix/raspggen/blob/master/kernel.conf)

Then, try to compile/install it:

`root #````
cd /usr/src/linux
```
`root #````
mount /boot
```
`root #````
mkdir /boot/overlays
```
`root #````
make -j4 Image modules dtbs
```
`root #````
make modules_install dtbs_install
```
for 32-bit kernels:

`root #````
gzip -9cf arch/arm/boot/Image > /boot/kernel7.img 
```
for 64-bit kernels:

`root #````
gzip -9cf arch/arm64/boot/Image > /boot/kernel8.img 
```

For now, to have [WiFi](https://wiki.gentoo.org/wiki/Wifi) work, download firmware:

`root #````
wget https://github.com/RPi-Distro/firmware-nonfree/blob/master/brcm80211/brcm/brcmfmac43430-sdio.bin -O /lib/firmware/brcm/brcmfmac43430-sdio.bin
```
`root #````
wget https://github.com/RPi-Distro/firmware-nonfree/blob/master/brcm80211/brcm/brcmfmac43430-sdio.txt -O /lib/firmware/brcm/brcmfmac43430-sdio.txt
```
## Adding new kernel to the bootloader

By default, the Raspberry Pi looks for a kernel in /boot/kernel.img. This is changed in the configuration file /boot/config.txt to load the new kernel.

**`/boot/config.txt`**

**Example config.txt**

If using an initramfs, add that to the config.txt as well:

**`/boot/config.txt`**

**Example config.txt with an initramfs**

Now, the Raspberry Pi can be rebooted and should make use of the new kernel. If for some reason the new kernel does not load or gives errors, the kernel entry in the /boot/config.txt can be removed. Then, on the next reboot, the default kernel.img will be loaded.

## Detailed step-by-step guide

Upon encountering problems building or deploying the kernel, try following the [detailed kernel building guide](http://visualkernel.com/tutorials/raspberry/buildkernel/) for clues on resolving the problems. Additionally The Raspberry Pi foundation provides these [build guides](https://www.raspberrypi.org/documentation/linux/kernel/building.md) to assist in Kernel compilation.
