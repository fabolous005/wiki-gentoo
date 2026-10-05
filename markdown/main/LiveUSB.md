<!-- source: https://wiki.gentoo.org/wiki/LiveUSB | group: Gentoo Wiki (Main) | wiki-title: LiveUSB -->
---
title: LiveUSB
url: https://wiki.gentoo.org/wiki/LiveUSB
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-06-13"
fingerprint: "948ffa5bcde3abdd"
license: CC BY-SA 4.0
---

# LiveUSB

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

This article explains how to create a *Gentoo LiveUSB* or, in other words, how to emulate a **x86** or **amd64** Gentoo LiveCD using a USB drive. This is particularly useful for installing Gentoo on a modern laptop with no CD-ROM drive.

Although the instructions found in this document aim at emulating a Gentoo LiveCD using a USB drive, they should work for any arbitrary block device as long as the device names are adjusted accordingly.

Many other methods available on different operating systems should work for creating bootable LiveUSB drives for Gentoo installation.

## Prerequisites

In order to boot a LiveUSB, the following will be needed:

- Bootable USB drive with enough space to write the image to. When using the Gentoo Minimal Installation CD, at least 1GB.
- A computer which supports booting from USB. While common now, some early systems with USB did not.

Access to the following is needed for creating a LiveUSB:

- A computer able to run the [dd](https://wiki.gentoo.org/wiki/Dd) command, which includes Linux and macOS
- A computer running Microsoft Windows with the appropriate software (see the [Windows section](https://wiki.gentoo.org/wiki/LiveUSB#Creating_bootable_LiveUSB_drives_under_Windows) below)

### Choosing an installation media for Gentoo

The architecture appropriate [Gentoo Minimal Installation CD](https://www.gentoo.org/downloads/) iso can be downloaded and used to install from a [command line interface](https://wiki.gentoo.org/wiki/Terminal_emulator), as a "light" installation option. There is also a [Gentoo LiveGUI USB Image](https://www.gentoo.org/downloads/) which can provide a more ergonomic option (e.g. open the handbook in another window and copy paste commands with a middle mouse click to a terminal emulator, use a GUI [IRC](https://wiki.gentoo.org/wiki/IRC) client for [support](https://wiki.gentoo.org/wiki/Support), etc.).

When downloading, adjust the architecture (**x86**, **amd64**, **arm**, **sparc**, etc.) portion of the URL to match the system's CPU. Gentoo mirrors are [here](https://www.gentoo.org/downloads/mirrors/).

The Gentoo bootable images are by no means the only thing that can be used to install Gentoo - almost any modern LiveCD should work. Use whatever feels the most comfortable.

### Convert the ISO image to hybrid mode

Most modern LiveCD's, like Gentoo are already in hybrid mode. If the LiveUSB does not boot, then it may be that the image will have to be converted to hybrid mode. Hybrid mode means image will enable the ISO to boot from both a CD-ROM device *or* a USB drive.

Convert the ISO with the following command:

`root #``isohybrid filename.iso`
The isohybrid command comes as part of the [sys-boot/syslinux](https://packages.gentoo.org/packages/sys-boot/syslinux) package.

## Using dd to write the ISO image to a USB drive

When using the Gentoo Installation CD ISOs, it is sufficient to directly copy the ISO contents onto the USB device. The [dd](https://wiki.gentoo.org/wiki/Dd) command can be used to accomplish this. For instance, assuming the USB device is at /dev/sdc:

`root #``dd if=/path/to/image.iso of=/dev/sdc bs=8192k status=progress; sync`
The command will exit without any errors when it completes the transfer successfully. Depending on the size of the ISO image and the speed of the USB device, this process could take some time. Be patient!

Once complete, the USB drive should be bootable.

On Windows, the dd command is also available through various projects, such as [Cygwin](https://www.cygwin.com), [GNUWin32](http://gnuwin32.sourceforge.net/), or [Chrysocome](http://www.chrysocome.net/dd).

### Writable partition

An extra partition can be created in the wasted space on large USB sticks, and be used for anything.

Add a new partition using [fdisk](https://wiki.gentoo.org/wiki/Fdisk), keeping the *iso9660 signature*. Stick with fdisk's default suggestions.

`root #``fdisk --wipe never /dev/sdc`
After booting into the live environment, the newly created partition will first be available to the root account, and ready to be formatted. To become root:

`user $``sudo su -`
The remainder of this document explains how to setup a vfat partition that is writable using the [GRUB](https://wiki.gentoo.org/wiki/GRUB) bootloader.

## Creating bootable LiveUSB drives from Linux systems

### Manually preparing a LiveUSB drive

This method works at least with current Gentoo [GRUB](https://wiki.gentoo.org/wiki/GRUB)-based images.

#### Preparing the USB drive

##### Partitioning the drive

Create a MBR partition table with a FAT16 partition on the USB drive, and mark it bootable using fdisk. In a combined approach, this is fine for booting BIOS and UEFI machines. An example partitioning scheme can be seen below:

`root #``fdisk -l /dev/sdc`
Disk /dev/sdc: 2063 MB, 2063597056 bytes
255 heads, 63 sectors/track, 250 cylinders
Units = cylinders of 16065 \* 512 = 8225280 bytes
  
   Device Boot      Start         End      Blocks   Id  System
/dev/sdc1   \*           1         250     2008124+   6  FAT16

On large drives, a second partition could be created for the remaining space, and formatted when needed later. A Gentoo LiveGUI (or another large) image needs this second partition right away, to circumvent the FAT file size limit. The first partition may be smaller in that case.

`root #``fdisk -l /dev/sdc`
Disk /dev/sdc: 14.92 GiB, 16025387008 bytes, 31299584 sectors
Disk model: USB Flash Disk  
Units: sectors of 1 \* 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disklabel type: dos
Device     Boot   Start      End  Sectors  Size Id Type
/dev/sdc1  \*       2048  1050623  1048576  512M  6 FAT16
/dev/sdc2       1050624 31299583 30248960 14.4G 83 Linux

##### Creating the filesystem

Create a FAT16 filesystem on the USB drive using mkfs.fat:

`root #``emerge --ask sys-fs/dosfstools``root #``mkfs.fat -n LiveUSB -F 16 /dev/sdc1`
mkfs.fat 4.2 (2021-01-31)
mkfs.fat: Warning: lowercase labels might not work properly on some systems

GRUB and the kernel will use "LiveUSB" as root label. (The warning can be ignored under Linux.)

And additionally for the LiveGUI workaround, choose a different label and any filesystem type supported by the image, that can handle files larger than 4GB.

`root #``mkfs.ext4 -L LiveDATA /dev/sdc2`
mke2fs 1.47.4 (6-Mar-2025)
Creating filesystem with 3781120 4k blocks and 946560 inodes
(...)

The kernel will use "LiveDATA" as root label.

#### Copying the files

##### Mounting the Gentoo boot image

Download a Gentoo boot image for the system's architecture from a the main site's [download page](https://www.gentoo.org/downloads/) and mount the ISO image on /mnt/cdrom as shown below:

`root #````
mkdir -p /mnt/cdrom
```
`root #````
mount -o loop,ro -t iso9660 /path/to/isofile.iso /mnt/cdrom
```
Adjust the `/path/to/isofile.iso` as necessary to the location of the downloaded boot image ISO.

##### Mounting the LiveUSB

Mount the newly formatted USB drive on /mnt/usb as shown below:

`root #````
mkdir -p /mnt/usb
```
`root #````
mount -t vfat /dev/sdc1 /mnt/usb
```
Additionally in case of the LiveGUI image:

`root #````
mkdir -p /mnt/data
```
`root #````
mount -t ext4 /dev/sdc2 /mnt/data
```
##### Copying the files

Copy the files from the boot image to the LiveUSB.

Non-LiveGUI:

`root #````
cp -r /mnt/cdrom/* /mnt/usb
```
LiveGUI:

`root #````
shopt -s extglob
```
`root #````
cp -r /mnt/cdrom/!(image.squashfs) /mnt/usb
```
`root #````
cp /mnt/cdrom/image.squashfs /mnt/data
```
Unmount the ISO image:

`root #``umount /mnt/cdrom`
##### Modify the bootloader configuration

Change all root labels to "LiveUSB", so the partition can be found (4x in this example). For the LiveGUI workaround, remember to change CDLABEL to "LiveDATA" instead!

**`/mnt/usb/boot/grub/grub.cfg`**

#### Installing a bootloader

##### Installing GRUB

Finally install the grub bootloader on the USB drive. Pick one or more targets supported by the image.

64 bit UEFI:

`root #``grub-install --force --removable --no-floppy --target=x86_64-efi --boot-directory=/mnt/usb/boot --efi-directory=/mnt/usb/efi`
32 bit UEFI:

`root #``grub-install --force --removable --no-floppy --target=i386-efi --boot-directory=/mnt/usb/boot --efi-directory=/mnt/usb/efi`
BIOS:

`root #``grub-install --force --no-floppy --target=i386-pc --boot-directory=/mnt/usb/boot /dev/sdc`
##### Unmounting the drive

`root #````
umount /mnt/usb
```
`root #````
umount /mnt/data # LiveGUI
```
#### Writable partition

After booting into the live environment, the USB drive will be mounted read-only. To make it writable:

`user $``sudo su - # To become root``root #``mount -o remount,rw /run/initramfs/live`
## Creating bootable LiveUSB drives under Windows

### Rufus

Rufus is a free and open source project created to write images to USB drives, with a variety of operating systems. It tends to be faster than the Universal USB installer (see in the next section).

Rufus can be downloaded from the project's [homepage](https://rufus.ie/en/).

Rufus is easy to use and should be mostly self explanatory. For more information, see Rufus' [FAQ page](https://github.com/pbatard/rufus/wiki/FAQ).

### Universal USB Installer

Universal USB installer is one of the oldest Linux-capable LiveUSB creators for Windows systems. It supports most Linux distributions, and has a simple, helpful wizard for selecting the Linux distribution. For Gentoo Minimal Installation CDs, however, select the Try Unlisted Linux ISO which is at the very bottom of the list. Select the proper USB drive to format and extract Linux to, and click Create.

Universal USB installer can be downloaded from its [homepage](https://www.pendrivelinux.com/universal-usb-installer-easy-as-1-2-3/).

## Booting

Insert the USB drive and turn on the computer, make sure the BIOS (or EFI firmware) has been set to boot from USB. If all goes well, a standard syslinux prompt should appear on the screen.

When attempting to boot from a USB device, be sure to select or enable USB drives as bootable devices in the system's firmware interface. If the ISO is UEFI capable, it may be also necessary to enable booting UEFI devices. This is typically performed via toggle options in the [BIOS](https://wiki.gentoo.org/wiki/BIOS) or UEFI firmware interface.

The boot order may need to be adjusted in the system's firmware for USB devices to boot first, although it is usually easier to hit the appropriate key (commonly either `F2` or `Delete`) and manually select the USB device as a one-time boot option from the list of bootable devices.

If installing Gentoo, follow the installation instructions found in the [Gentoo Handbook](https://wiki.gentoo.org/wiki/Handbook:Main_Page) appropriate to the system's architecture from here on.

## See also

- [Live image](https://wiki.gentoo.org/wiki/Live_image) — an operating system (OS) environment contained within a file that can be used to [boot](https://en.wikipedia.org/wiki/Booting) a system
- [CD/DVD/BD writing](https://wiki.gentoo.org/wiki/CD/DVD/BD_writing) — how to **burn optical disks** on Gentoo from the [command line](https://wiki.gentoo.org/wiki/Terminal_emulator) with the [app-cdr/cdrtools](https://packages.gentoo.org/packages/app-cdr/cdrtools) or [app-cdr/dvd+rw-tools](https://packages.gentoo.org/packages/app-cdr/dvd+rw-tools) packages
- [FAQ - How do I burn an ISO file?](https://wiki.gentoo.org/wiki/FAQ#How_do_I_burn_an_ISO_file.3F)

## External resources

- [Installing SystemRescueCd on a USB stick](https://www.system-rescue.org/Installing-SystemRescue-on-a-USB-memory-stick/#recommended-usb-installation-method-on-linux) — The very popular alternative
- [https://www.ventoy.net/](https://www.ventoy.net/) - Ventoy provides a simple means to  create and modify bootable media. One disk may be used to provide multiple boot iso options. Beware that there seem to be a large number of open bugs - [bug #736936](https://bugs.gentoo.org/show_bug.cgi?id=736936).

## References
