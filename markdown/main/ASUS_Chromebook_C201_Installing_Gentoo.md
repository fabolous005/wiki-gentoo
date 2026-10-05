<!-- source: https://wiki.gentoo.org/wiki/ASUS_Chromebook_C201/Installing_Gentoo | group: Gentoo Wiki (Main) | wiki-title: ASUS Chromebook C201/Installing Gentoo -->
---
title: ASUS Chromebook C201/Installing Gentoo
url: https://wiki.gentoo.org/wiki/ASUS_Chromebook_C201/Installing_Gentoo
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2024-04-29"
fingerprint: "15871f3f84cf7f74"
license: CC BY-SA 4.0
---

# ASUS Chromebook C201/Installing Gentoo

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

This guide is about installing Gentoo on an [ASUS Chromebook C201](https://wiki.gentoo.org/wiki/ASUS_Chromebook_C201).

## Additional hardware requirements

- USB ethernet adapter

## Obtaining installation media

Consult the guide on [creating bootable media for depthcharge based devices](https://wiki.gentoo.org/wiki/Creating_bootable_media_for_depthcharge_based_devices) for instructions on how to manually create the installation media.

## Preparing the device

To be able to boot from external media like USB drives, the Asus Chromebook C201 first needs to be switched into developer mode<sup>[\[1\]](https://wiki.gentoo.org#cite_note-1)</sup>.

This can be achieved by pressing `Esc`+`Refresh`+`Power` when the device is switched off to enter the recovery mode screen.

Pressing `Ctrl`+`D` and then following subsequent on-screen instructions enables the developer mode.

Finally one of the verified boot parameters needs to be modified: Boot the device and enter Chrome OS' crosh shell, e.g. by opening Chromium Browser and hitting `Ctrl`+`Alt`+`T`.
Enable booting from external media:

`root #````
crossystem dev_boot_usb=1
```
## Booting the installation media

Power on and when the boot screen is displayed press `Ctrl`+`U` to boot from the installation media.
Log in (in case the installation media was manually created following the instructions referred to above the username is ‘root’ and the password is ‘gentoo’).

## Configuring the installation media

Begin with [configuring the network](https://wiki.gentoo.org/wiki/Handbook:AMD64/Installation/Networking).

Once a network connection is established make sure Portage is set up:

`root #````
emerge-webrsync
```
Now install required tools ([sys-block/parted](https://packages.gentoo.org/packages/sys-block/parted) and [sys-boot/vboot-utils](https://packages.gentoo.org/packages/sys-boot/vboot-utils)):

`root #````
emerge --ask sys-block/parted sys-boot/vboot-utils
```
## Creating a backup of the eMMC

`root #````
dd if=/dev/mmcblkX of=/PATH/TO/ARBITRARY_BACKUP_LOCATION/c201backup.img
```
## Preparing the disks

Cf. [Preparing the disks](https://wiki.gentoo.org/wiki/Handbook:AMD64/Installation/Disks).

Recommended partition layout and size - a GUID Partition Table (GPT) is mandatory:

| /dev/mmcblkXp1 | kernel partition | 64MiB | 
| /dev/mmcblkXp2 | root partition | available space | 

`root #````
parted /dev/mmcblkX mklabel gpt
```
`root #````
parted -a optimal /dev/mmcblkX unit mib mkpart Kernel 1 65
```
`root #````
parted -a optimal /dev/mmcblkX unit mib mkpart root 65 100%
```
Finish the preparation of the partitions by creating a filesystem on the root partition. Replace ROOTFS\_TYPE with any suitable filesystem type, e.g. ext4 or btrfs.

`root #````
mkfs.ROOTFS_TYPE /dev/mmcblkXp2
```
Depthcharge, the chromebooks’ bootloader, requires some specific parameters to be set. These signal the bootloader the presence of a valid kernel partition:

`root #````
cgpt add -i 1 -t kernel -l Kernel -S 1 -T 5 -P 15 /dev/mmcblkX
```
Mount the root partition:

`root #````
mkdir /mnt/gentoo
```
`root #````
mount /dev/mmcblkXp2 /mnt/gentoo
```
## Installing the Gentoo installation files

Choose an ARMv7a|HardFP stage3 from the main website's [download section](https://www.gentoo.org/downloads/#other-arches) or consider going for an armv7a\_hardfp-musl stage3 from the [Hardened musl](https://wiki.gentoo.org/wiki/Project:Hardened_musl#Goals) project.
Follow [Installing the Gentoo installation files](https://wiki.gentoo.org/wiki/Handbook:AMD64/Installation/Stage) from the Handbook.

## Installing the Gentoo base system

Skip "Mounting the boot partition", apart from that follow the Handbook for [Installing the Gentoo base system](https://wiki.gentoo.org/wiki/Handbook:AMD64/Installation/Base).

## Configuring the Linux kernel

Install the tools required for deploying the kernel ([dev-embedded/u-boot-tools](https://packages.gentoo.org/packages/dev-embedded/u-boot-tools), [sys-apps/dtc](https://packages.gentoo.org/packages/sys-apps/dtc) and [sys-boot/vboot-utils](https://packages.gentoo.org/packages/sys-boot/vboot-utils)):

`root #````
emerge --ask dev-embedded/u-boot-tools sys-apps/dtc sys-boot/vboot-utils
```
Configure [sys-kernel/gentoo-sources](https://packages.gentoo.org/packages/sys-kernel/gentoo-sources) as usual, cf. [Configuring the Linux kernel](https://wiki.gentoo.org/wiki/Handbook:AMD64/Installation/Kernel). Alternatively refer to the [RockMyy repository](https://github.com/Miouyouyou/RockMyy) to include Mali GPU drivers.

### Building the kernel and device tree binaries

`user $````
make -j5 zImage dtbs modules
```
From the kernel build directory, copy the zImage and the target device's device tree binary to the desired working directory. Replace DTBINARY with the filename of the target device's device tree binary, e.g. rk3288-veyron-speedy.dtb for the Asus Chromebook C201 (which is based on Rockchip's RK3288 SoC and a board with the codename "Veyron Speedy").

`user $````
cp -a arch/arm/boot/zImage /PATH/TO/ARBITRARY_WORKING_DIRECTORY
```
`user $````
cp -a arch/arm/boot/dts/DTBINARY /PATH/TO/ARBITRARY_WORKING_DIRECTORY
```
### Optional: Creating a custom initramfs

Follow instructions from the [Custom Initramfs](https://wiki.gentoo.org/wiki/Custom_Initramfs) article.
Embed the initramfs into the kernel. Alternatively create it as a separate file (cf. [Custom Initramfs - Creating a separate file](https://wiki.gentoo.org/wiki/Custom_Initramfs#Creating_a_separate_file)):

`user $````
find /PATH/TO/INITRAMFS/ -print0 | cpio --null --create --verbose --format=newc | gzip --best > /PATH/TO/ARBITRARY_WORKING_DIRECTORY/initrd.img
```
### Creating the FIT image

Change to the directory where the kernel, the device tree binary and (optionally) the initramfs are located:

`user $````
cd /PATH/TO/ARBITRARY_WORKING_DIRECTORY
```
Create the configuration file ("gentoo.its") for the Flattened Image Tree (FIT) with the following content<sup>[\[2\]](https://wiki.gentoo.org#cite_note-2)</sup>. Again replace DTBINARY with the filename of the target device's device tree binary, e.g. rk3288-veyron-speedy.dtb for the Asus Chromebook C201.

**`gentoo.its`**

```
;
/ {
    description = "Linux kernel image with one or more FDT blobs";
    #address-cells = <1>;
    images {
        kernel@1{
            description = "vmlinuz";
            data = /incbin/("zImage");
            type = "kernel_noload";
            arch = "arm";
            os = "linux";
            compression = "none";
            hash@1{
                algo = "sha1";
            };
        };
        fdt@1{
            description = "dtb";
            data = /incbin/("DTBINARY");
            type = "flat_dt";
            arch = "arm";
            compression = "none";
            hash@1{
                algo = "sha1";
            };
        };
        ramdisk@1{
            description = "initrd.img";
            data = /incbin/("initrd.img");
            type = "ramdisk";
            arch = "arm";
            os = "linux";
            compression = "none";
            hash@1{
                algo = "sha1";
            };
        };
    };
    configurations {
        default = "conf@1";
        conf@1{
            kernel = "kernel@1";
            fdt = "fdt@1";
            ramdisk = "ramdisk@1";
        };
    };
};
```
Pack the FIT image:

`user $````
sync
```
`user $````
mkimage -f gentoo.its gentoo.itb
```
### Preparing verified boot

Create a file ("kernel.flags") for the CMDLINE parameters. Replace ROOTFS\_TYPE with the root partition's filesystem type, e.g. ext4 or btrfs.

**`kernel.flags`**

```
console=tty1 root=/dev/mmcblkXp2 rootfstype=ROOTFS_TYPE rootwait
```
Sign and pack the kernel:

`user $````
sync
```
`user $````
futility --debug vbutil_kernel --arch arm --version 1 --keyblock /usr/share/vboot/devkeys/kernel.keyblock --signprivate /usr/share/vboot/devkeys/kernel_data_key.vbprivk --bootloader kernel.flags --config kernel.flags --vmlinuz gentoo.itb --pack vmlinuz.signed
```
### Installing the kernel

Install the modules, keeping them small to save some space:

`root #````
make INSTALL_MOD_STRIP=1 modules_install
```
Install the kernel image to the kernel partition:

`root #````
sync && dd if=vmlinuz.signed of=/dev/mmcblkXp1
```
## Configuring the system

Consult the Handbook again for [Configuring the system](https://wiki.gentoo.org/wiki/Handbook:AMD64/Installation/System).

## Installing system tools

Stick with the Handbook: [Installing system tools](https://wiki.gentoo.org/wiki/Handbook:AMD64/Installation/Tools)

## Finalizing

[Finalize](https://wiki.gentoo.org/wiki/Handbook:AMD64/Installation/Finalizing) the new Gentoo installation according to the handbook. To use wifi remember to install [net-wireless/wpa\_supplicant](https://packages.gentoo.org/packages/net-wireless/wpa_supplicant):

`root #````
emerge –ask net-wireless/wpa-supplicant
```
Also keep in mind that the built-in wifi requires [proprietary firmware](https://wiki.gentoo.org/wiki/ASUS_Chromebook_C201#Built-in_wifi).

## External resources

- PDF: Additional information on FIT images: Joel A Fernandes, [Flattened Image Trees: A powerful kernel image format (PDF)](https://elinux.org/images/f/f4/Elc2013_Fernandes.pdf), [Embedded Linux Wiki](https://elinux.org/Main_Page), February 21, 2013. Retrieved on February 25th, 2019
