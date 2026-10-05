<!-- source: https://wiki.gentoo.org/wiki/Raspberry_Pi4_64_Bit_Install | group: Gentoo Wiki (Main) | wiki-title: Raspberry Pi4 64 Bit Install -->
---
title: Raspberry Pi4 64 Bit Install
url: https://wiki.gentoo.org/wiki/Raspberry_Pi4_64_Bit_Install
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2025-12-19"
fingerprint: be11b85b0db69aeb
license: CC BY-SA 4.0
---

# Raspberry Pi4 64 Bit Install

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)



This wiki page explains how to install a 64bit Gentoo system on the Raspberry Pi 4.

## Introduction

The install procedure of Gentoo on the Pi 4 is a standard Gentoo installation similar to the one described in the [AMD64 Handbook](https://wiki.gentoo.org/wiki/Handbook:AMD64) but without a live medium: the storage (SD card or external SSD) that will be used by the Pi 4 needs to be setup on a separate Gentoo machine (whose platform is probably not **aarch64**)

### Not tested

- Analogue Video Output
- Analogue Audio Output
- Camera
- Screen

## Preparing the external storage

This section is obsoleted by the [Raspberry Pi Install Guide](https://wiki.gentoo.org/wiki/Raspberry_Pi_Install_Guide#Preparing_the_disks)
~~Minimally, the storage can be formatted with 2 partitions.~~


Assuming the target storage is mounted at /dev/sda, this can be written with:

~~`root #``fdisk /dev/sda`Welcome to fdisk (util-linux 2.38.1).
Changes will remain in memory only, until you decide to write them.
Be careful before using the write command.~~

To create the GPT label:

~~`Command (m for help)``g`Created a new GPT disklabel (GUID: E361F483-EDF0-5846-BDCE-5A92124726D9).~~

To create the first 1gb partition:

~~`Command (m for help)``n`Partition number (1-128, default 1): 
First sector (2048-124735454, default 2048):Last sector, +/-sectors or +/-size{K,M,G,T,P} (2048-124735454, default 124733439): +1G~~

To create the root partition, spanning the rest of the volume:

~~`Command (m for help)``n````
Partition number (2-128, default 2): 
First sector (2099200-124735454, default 2099200): 
Last sector, +/-sectors or +/-size{K,M,G,T,P} (2099200-124735454, default 124733439): 
Created a new partition 2 of type 'Linux filesystem' and of size 58.5 GiB.
```~~

The first partition's type can be set to **ESP** with:

~~`Command (m for help)``t`Partition number (1,2, default 2): 1
Partition type or alias (type L to list all): 1
Changed type of partition 'Linux filesystem' to 'EFI System'.~~

The staged partition table can be read with:

~~`Command (m for help)``p`Disk /dev/sdf: 59.48 GiB, 63864569856 bytes, 124735488 sectors
Disk model: UHSII uSD Reader
Units: sectors of 1 \* 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disklabel type: gpt
Disk identifier: E361F483-EDF0-5846-BDCE-5A92124726D9
Device       Start       End   Sectors  Size Type
/dev/sdf1     2048   2099199   2097152    1G EFI System
/dev/sdf2  2099200 124733439 122634240 58.5G Linux filesystem~~

If the partition table looks correct, it can be written with:

~~`Command (m for help)``w`The partition table has been altered.
Calling ioctl() to re-read partition table.
Syncing disks.~~

Once the partitions have been created, filesystems can be written with:

~~`root #``mkfs.fat /dev/sda1``root #``mkfs.ext4 /dev/sda2`~~


## Mounting the base filesystem

In this guide, /dev/sda2 will be mounted to /mnt/gentoo.

`root #``mount /dev/sda2 /mnt/gentoo`
## Installing the Gentoo base system

From the target root directory, the stage3 file can be extracted with:

`/mnt/gentoo #``tar xvf stage3-`**arm64**-<release timestamp>.tar.xz
### QEMU user chroot

If the Gentoo machine being used to setup the Pi's storage device is not **aarch64** (e.g. **amd64**), QEMU needs to be setup and configured to chroot into this environment:

Once configured, the user target can be copied with:

`user $``cp /usr/bin/qemu-`**aarch64** /mnt/gentoo/usr/bin
Then the chroot can be entered using [sys-apps/arch-chroot](https://packages.gentoo.org/packages/sys-apps/arch-chroot):

`root #``arch-chroot /mnt/gentoo`
## Configuring Portage

### COMMON\_FLAGS

Raspberry Pi 4B users should use the `-mcpu=cortex-a72` compiler in favor of `-march` and `-mtune` altogether. `march` and `mtune` behavior on **arm64** systems is different from **amd64**. [Source](https://community.arm.com/arm-community-blogs/b/tools-software-ides-blog/posts/compiler-flags-across-architectures-march-mtune-and-mcpu)

**`/etc/portage/make.conf`**

### VIDEO\_CARDS

The mesa drivers the Pi 4 needs to have full hardware acceleration are v3d and vc4

**`/etc/portage/make.conf`**

```
VIDEO_CARDS="v3d vc4"
```
It also needs specific firmware, loaded in the section ["BIOS" configuration](https://wiki.gentoo.org#.22BIOS.22_configuration:_.2Fboot.2Fconfig.txt)

## Raspberry Pi Firmware

The Raspberry Pi 4 second stage bootloader (`bootcode.bin`) is included on SPI flash and does not need to be added to the /boot (**ESP**) volume. [\[1\]](https://wiki.gentoo.org#cite_note-1)

The following files are not included on builtin device storage, and must be added to the **ESP**:

- `start4.elf`
- `fixup4.dat`

These files are part of [sys-boot/raspberrypi-firmware](https://packages.gentoo.org/packages/sys-boot/raspberrypi-firmware), and are automatically installed to /boot on emerge:

`root #``emerge --ask sys-boot/raspberrypi-firmware`
## "BIOS" configuration: /boot/config.txt

The following options may be useful:

- `disable_overscan=1` Disable video overscanning.
- `dtoverlay=vc4-kms-v3d` Enable GPU acceleration
- `kernel kernel8.img` Change the path to the kernel file. The default for the RPi4 is `kernel8.img`.
- `cmdline=cmdline.txt` Change the path to the `cmdline.txt` (kernel commandline) file.
- `initramfs init.gz followkernel` Change the path to the loaded initramfs, `followkernel` should be used as it loads the image immediately after the kernel in memory.
- `arm_boost=1` Enable 1.8GHz frequency boost instead of 1.5Ghz.
- `dtparam=audio=on` Enable audio output
- `dtparam=i2c_arm=on` Enable I2C.
- `dtparam=spi=on` Enable SPI

There is a new [alternative audio mode](https://www.raspberrypi.org/forums/viewtopic.php?f=29&t=269769&p=1636828#p1636828) that does not use the `audio=on` **dtparam**.

### Kernel command line: /boot/cmdline.txt

This file /boot/cmdline.txt contains the command line that is given to the kernel. One important information it needs to contain is where the root file system is.

**`/boot/cmdline.txt`**

or

**`/boot/cmdline.txt`**

## Kernel

The kernel can either be manually compiled and fully customized or the official binary kernel from Raspberry can be installed.

### Raspberry official binary kernel

Install [sys-kernel/raspberrypi-image](https://packages.gentoo.org/packages/sys-kernel/raspberrypi-image) and [sys-kernel/raspberrypi-sources](https://packages.gentoo.org/packages/sys-kernel/raspberrypi-sources)

`root #``emerge --ask sys-kernel/raspberrypi-image sys-kernel/raspberrypi-sources`
### Manual build

If building in a chroot:

`/usr/src/linux #``make bcm2711_defconfig`
To cross compile the kernel:

`root #``ARCH=arm64 CROSS_COMPILE=aarch64-unknown-linux-gnu- make bcm2711_defconfig`
Check the other kernel configuration settings given in [configure the kernel](https://wiki.gentoo.org/wiki/Raspberry_Pi_3_64_bit_Install#Configure_the_kernel).

#### Kernel config tweaks

##### Setting the default CPU governor

The default CPU governor (**CPU\_FREQ\_DEFAULT\_GOV**) is `powersave`. **This runs the CPU at 600MHz all the time**. CPU governors can be controlled in /proc or the default CPU governor can be changed to be `schedutil` or `ondemand`:

**Set the default CPU governor**

#### Building the kernel

To build in a chroot:

`/usr/src/linux #``make -j<threads>`
To cross compile:

`/usr/src/linux #``ARCH=arm64 CROSS_COMPILE=aarch64-unknown-linux-gnu-  make -j<threads>`
#### Installing the kernel

The kernel can be installed to /boot with:

`/usr/src/linux #``make dtbs_install`
Kernel modules can be installed with:

`/usr/src/linux #``make -j<threads> modules_install`
#### Installing Device Tree Files and Overlays

Compiled DTBs are stored in arch/arm64/boot/dts/. bcm2711-rpi-4-b.dtb is required for the RPI4:

`root #````
mkdir --parents /boot/overlays
```
`root #````
cp -vR arch/arm64/boot/dts/overlays/*.dtbo /boot/overlays
```
`root #``cp -v arch/arm64/boot/dts/broadcom/bcm2711-rpi-4-b.dtb /boot`
## Configuring the system

The AMD64 handbook can be followed starting [Configuring the system](https://wiki.gentoo.org/wiki/Handbook:AMD64/Installation/System) until the end, except for [Configuring the bootloader](https://wiki.gentoo.org/wiki/Handbook:AMD64/Installation/Bootloader) which is taken care of in [#Firmware / Boot partition](https://wiki.gentoo.org#Firmware_.2F_Boot_partition) section. Then, the storage can be safely unmounted and plugged in the Pi 4 and have it successfully booting and working as expected.

### Zram

SWAP space was intentionally not allocated to the SD card, because it can destroy SD cards without wear-leveling. To use compressed RAM for swap space, as well as /var/log:

**`/etc/conf.d/zram-init`**

Then zram-init can be added to the *boot* runlevel:

`root #``rc-update add zram-init boot`
### Removing unnecessary cron jobs

Unnecessary writing can kill SD cards, logging to memory or remotely and disabling unneeded periodic tasks can extend the life of a Raspberry Pi's root file system device.

#### Disabling the man-db daily cron job

By default, [sys-apps/man-db](https://packages.gentoo.org/packages/sys-apps/man-db) installs /etc/cron.daily/man-db which updates man cache files daily. This action is only necessary when new man pages are added, and can be removed by masking that file from being installed, and deleting it moving it to another runlevel.

##### Masking the cron job file

**`/etc/portage/make.conf`**

**Ensure portage does not reinstall that file to /etc/cron.daily**

##### Disabling the daily cron task

The cron task can be removed or moved to a less frequent cron job:

`root #``mv /etc/cron.daily/man-db /etc/cron.monthly/man-db`
or:

`root #``chmod -x /etc/cron.daily/man-db`
##### Manually updating the cache

The man page cache can be manually updated with:

`root #``mandb`
### Making the DUID static

By default, dhcpcd will generate a randomized DUID when it first runs. To change this to be derived from the interface MAC address, so it can be safely stored in RAM:

**`/etc/dhcpcd.conf`**

**Make DUID based on MAC**

## Misc

### WiFi

WiFi needs three firmware files in /lib/firmware/brcm/.
These are now provided by [sys-kernel/linux-firmware](https://packages.gentoo.org/packages/sys-kernel/linux-firmware)

~~brcmfmac43455-sdio.bin
brcmfmac43455-sdio.clm\_blob
brcmfmac43455-sdio.txt~~

Which is almost but not quite the same as the Pi3. The catch is in **brcmfmac43455-sdio.txt** where **grep boardflags3** produces different results for the Pi3 and Pi4 files.

~~The Pi4 version returns boardflags3=0x44200100
The Pi3 version returns boardflags3=0x48200100~~

With the wrong brcmfmac43455-sdio.txt file, bluetooth will work but not WiFi.

These files, including the Pi 4 version of above file, are found in [sys-firmware/raspberrypi-wifi-ucode](https://packages.gentoo.org/packages/sys-firmware/raspberrypi-wifi-ucode), to install it you'll also need to install [sys-kernel/linux-firmware](https://packages.gentoo.org/packages/sys-kernel/linux-firmware) with the [savedconfig](https://wiki.gentoo.org/wiki/Savedconfig) USE flag and remove the conflicting files from the saved configuration, and then re-emerge linux-firmware to apply the new de-conflicted saved configuration:

~~/lib/firmware/brcm/brcmfmac43430-sdio.bin
/lib/firmware/brcm/brcmfmac43430-sdio.txt
/lib/firmware/brcm/brcmfmac43436-sdio.bin
/lib/firmware/brcm/brcmfmac43436-sdio.clm\_blob
/lib/firmware/brcm/brcmfmac43436-sdio.txt
/lib/firmware/brcm/brcmfmac43455-sdio.bin
/lib/firmware/brcm/brcmfmac43455-sdio.clm\_blob
/lib/firmware/brcm/brcmfmac43455-sdio.txt
/lib/firmware/brcm/brcmfmac43456-sdio.bin
/lib/firmware/brcm/brcmfmac43456-sdio.clm\_blob
/lib/firmware/brcm/brcmfmac43456-sdio.txt~~

### EEProm updates

[dev-embedded/rpi-eeprom](https://packages.gentoo.org/packages/dev-embedded/rpi-eeprom) provides the eeprom files and the updater, as well as a service to check and apply the updates.

There are 3 release channels of firmware updates:

- critical - Default - rarely updated
- stable - Updated when new/advanced features have been successfully beta tested.
- beta - New or experimental features are tested here first.

To configure which release channel you'd like the updater service to follow, edit /etc/conf.d/rpi-eeprom-update.

### Power over Ethernet

The Pi3b PoE HAT will power the P4 and a USB SSD.

Fan control works.

### microSD trim/discard

The microSD interface supports the trim command:

`Pi4_~arm64 ~ #``fstrim -av`
/boot: 7.7 GiB (8250073088 bytes) trimmed on /dev/mmcblk0p1

If you have a suitable microSD card, consider adding fstrim to a weekly or monthly cron job.

### USB attached SSD

Users with USB3 attached SSDs may have noticed that trim is not supported. It is a feature of USB storage that trim is disabled by default.
[Trim can be enabled](https://wiki.gentoo.org/wiki/Discard_over_USB) with mixed success. Only USB3 is supported and the USB3 to SSD bridge device must be able to pass the trim command.

One other thing to note, the Raspberry pi currently only supports booting from usb mass storage devices (including external hard drives) that have a 512 byte logical sector size. You may need to reformat (with the drive manufacturer's utility) them to enable 512 byte emulation.

See also [Unreliable USB Attached SCSI](https://wiki.gentoo.org/wiki/Raspberry_Pi_Install_Guide#Unreliable_USB_Attached_SCSI).

### Cross-compiling

[Cross-compiling](https://wiki.gentoo.org/wiki/Crossdev) is by far the fastest way to create packages for aarch64. Afterwards, binhost can be set up to download the packages that you desire. It is possible to create the entire RPI4 by using cross-compile and creating it through a stage2. [Distcc](https://wiki.gentoo.org/wiki/Raspberry_Pi/Cross_building) is also another great method for compiling aarch64 code. Code can be offloaded onto the main computer. Distcc will have more difficulties in boostraping though, unlike Cross-compiling.

## See also

- [Raspberry Pi](https://wiki.gentoo.org/wiki/Raspberry_Pi) — a series of small single-board computers.

## Acknowledgements

NeddySeagoon for taking over maintenance and upstreaming many ebuilds from sakaki's overlay after sakaki's retirement ( [Neddy's fork](https://github.com/NeddySeagoon/genpi64-overlay) )

sakaki's topic on the [Raspberry Pi forums](http://www.raspberrypi.org/forums/viewtopic.php?p=1491136)

All the contributors to [issue 3032](https://github.com/raspberrypi/linux/issues/3032) on the Raspberry Pi GitHub repository.
