<!-- source: https://wiki.gentoo.org/wiki/PINE64_ROCKPro64/Installing_Gentoo | group: Gentoo Wiki (Main) | wiki-title: PINE64 ROCKPro64/Installing Gentoo -->
---
title: PINE64 ROCKPro64/Installing Gentoo
url: https://wiki.gentoo.org/wiki/PINE64_ROCKPro64/Installing_Gentoo
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2024-04-06"
fingerprint: "9281ff1f8923cbcc"
license: CC BY-SA 4.0
---

# PINE64 ROCKPro64/Installing Gentoo

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

The basic installation for the RockPro64 is VERY similar to the amd64 guide, so much so that only the differences will be documented.

NOTE: Although other processes may be viable, this document only covers the procedure I follow and test with!

The main amd64 installation guide is [Handbook:AMD64/Installation/Media](https://wiki.gentoo.org/wiki/Handbook:AMD64/Installation/Media) - Readers are encouraged to review it and become familiar before starting the RockPro64 installation; be sure to pay close attention to the required changes outlined in the sections below.

## Preparing the system

It is highly recommend to attach a serial console until the device is fully configured! It is recommended to use the [Das U-Boot](https://wiki.gentoo.org/wiki/Das_U-Boot) bootloader to boot the kernel.

From new, the board does not have any bootloader installed in the onboard SPI Flash memory. At power up, the boot ROM will look for an image (e.g. bootloader), first in the onboard SPI flash, then optional eMMC module and finally the SD-Card. If no image with the correct header in the correct locations are found, a special OTG mode (USB) is entered. Initinally it is possible to put Das U-boot on the SD-Card and boot from the SD-Card to the avoid the complexity of having to flash the onboard SPI flash memory. The documentation for attaching a serial console and for writing the U-boot firmware to the SPI device can be found on the upstream wiki: [https://wiki.pine64.org/wiki/ROCKPro64](https://wiki.pine64.org/wiki/ROCKPro64)

## Preparing the SDHC card with the prebuilt aarch64 boot image

Follow the general guidelines of the "Obtain the media" section of the amd64 guide. The difference is that an aarch64 ARM image is required. These can be found at: [https://www.gentoo.org/downloads/](https://www.gentoo.org/downloads/) or one of the many Gentoo mirrors. Make sure to download the arm64 minimal installation CD image and follow the amd64 guide to write it to a SDHC card.

Once this is done, you will be able to boot into the install image and the real fun begins...

Please note: When booting from the installation CD image, the screen will remain blank for about a minute before you get the login prompt.

If you have a serial console, the system will appear to hang for almost 45 seconds after "random:fast init done"... just be patient; this is normal.

See the following serial console output:

[    1.197865] Console: switching to colour frame buffer device 240x75
[    1.222074] fb0: EFI VGA frame buffer device
[    1.223296] SMCCC: SOC_ID: ARCH_SOC_ID not implemented, skipping ....
[    1.224599] hid: raw HID events driver (C) Jiri Kosina
[    1.225352] usbcore: registered new interface driver usbhid
[    1.225940] usbhid: USB HID core driver
[    1.229333] drop_monitor: Initializing network drop monitor service
[    1.230238] Initializing XFRM netlink socket
[    1.231303] NET: Registered PF_INET6 protocol family
[    3.757120] random: fast init done
[   46.635061] Freeing initrd memory: 97700K

**The disk MUST partitioned with an [EFI partition](https://wiki.gentoo.org/wiki/EFI_System_Partition) as outlined in the amd64 guide. The author of this page has only tested with GPT, not MBR partitioning.**

It is recommended an 8 GiB swap partition is defined.

... then  follow the [amd64 installation guide](https://wiki.gentoo.org/wiki/Handbook:AMD64) for several pages, with the only (obvious) difference being the selection of an arm64 stage3 tarball instead of the amd64 variant.

## After installing the stage 3 tarball

To optimize the compiler flags for a RockPro64 system (at the expense of POSSIBLY being able to move the boot disk to a different arm system...), add the following edits to the `CFLAGS` variable in Portage's /etc/portage/make.conf file:

**`/etc/portage/make.conf`**

**RockPro64 specifics**

```
CFLAGS="-O2 -pipe -march=armv8-a+crc+crypto -mtune=cortex-a72.cortex-a53 -mfix-cortex-a53-835769 -mfix-cortex-a53-843419"
MAKEOPTS="-j6"
```
## Configuring the bootloader

NOTE: I have only tested using GRUB2 - if you want to try LILO and figure it out, please update this article!

You MUST install the standalone version of GRUB, which is in fact an EFI module. This is why you MUST have partitioned the disk with an [EFI system partition](https://wiki.gentoo.org/wiki/ESP)...

I install on to an external SATA SSD, so that is what is outlined in the steps below. Once running on the SSD, I have used the same method to configure SDHC cards successfully, the only difference being the Linux device names and EFI device names used.

**`/boot/grub/grub.conf`**

**GRUB configuration file**

Notes on the GRUB configuration file above:

- The serial port is configured - note the default baud rate and the fact that it is S2, not S0
- On the "set root" line, the disk numbering is kind of wonky. The SDHC card is enumerated as hd1 on my system, even though it has boot priority
  - Note: if in doubt about your EFI device names or you guess incorrectly, you can type "c" during the 5 second GRUB delay to drop into the command line. The ls command can then be used to determine device naming.
- On the linux line, change to "root=/dev/mmcblk1p3" if you are configuring a SDHC card
- You need to change the kernel and initramfs version numbers to match your environment

### Installing a standalone version of GRUB

NOTE: I create a /boot/efi directory on my disk, and I mount the EFI partition there:

`root #````
mount /dev/sda1 /boot/efi
```
`root #``mkdir -p /boot/efi/EFI/BOOT`
The command to install the standalone GRUB image is

`root #``grub-mkstandalone -O arm64-efi -o /boot/efi/EFI/BOOT/BOOTAA64.EFI "boot/grub/grub.cfg=/root/grub.cfg"`
NOTE: my grub.cfg file is in the /root directory, change the path in the "boot/grub/grub.cfg=" to reflect where the grub.cfg file was created.

You may now shutdown, remove the install CD image, and boot your new Gentoo installation.

## Big Endian Support

If you don't know what this means, you don't need it...

I have successfully created a big endian port. I am documenting the procedure to run big endian mode and submitting the patches over the next few weeks.

A Stage 3 tarball and bootable SDHC images for big endian can be found here:

**NOTE: video output is currently not available for the big endian port - a serial console is required.**

After all the sources are cleaned up and published, I will fix the rockchip video driver, which is currently not endian clean...

The endian wars have ended. Little endian won. But writing code (specifically code that interacts with hardware) that is endian neutral is a "Good Thing", so I wanted a big endian system to test on. The RockPro64 is an affordable system that supports big endian operation AND has a PCIe x4 slot! Hurray!

## See also

- [Embedded Handbook](https://wiki.gentoo.org/wiki/Embedded_Handbook) — a collection of community maintained documents providing a consolidation of embedded and SoC knowledge for Gentoo..
