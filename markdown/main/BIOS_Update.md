<!-- source: https://wiki.gentoo.org/wiki/BIOS_Update | group: Gentoo Wiki (Main) | wiki-title: BIOS Update -->
---
title: BIOS Update
url: https://wiki.gentoo.org/wiki/BIOS_Update
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2025-06-25"
fingerprint: b30f5e1104f723a4
license: CC BY-SA 4.0
---

# BIOS Update

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

This article describes how to apply a BIOS update on a Gentoo system.

Hardware manufactures often provide updates for [BIOS](https://wiki.gentoo.org/wiki/BIOS) and other types of firmware. To apply (often referred to as "flash") the updates is sometimes not straight forward on GNU/Linux systems. This occasionally requires some extra work.

## Gather firmware information

First find the motherboard's manufacturer and the model. Check the user manual that came with the system. Most of the needed information can be found in the user manual.

The [dmidecode](https://wiki.gentoo.org/wiki/Dmidecode) package can be used to retrieve additional information on system hardware. dmidecode looks at the motherboard's DMI table in order to provide richer details about the firmware and hardware components.

`root #``dmidecode -t bios -t baseboard`
Lastly, if physical access to the motherboard is possible, the required information may be found directly on the motherboard itself.

After searching for the manufacturer's firmware update, proceed to download the package necessary to update the hardware. It is normal for a manufacturer to store firmware update packages in .zip, .exe, or .iso format.

`user $````
unzip 7235v1A.zip
```
Archive:  7235v1A.zip
   creating: 7235v1A/
inflating: 7235v1A/7235v1x.txt
inflating: 7235v1A/AWFL865.EXE
inflating: 7235v1A/How to flash the BIOS.DOC
inflating: 7235v1A/W7235IMS.1A0

## BIOS option

Many BIOSes have an option to read the new binary image from an external storage medium or from an internal disk. Enter the BIOS setup and look for the option. If the BIOS does not support this, continue with the next section.

## Boot-CD

Often the manufacturer offers a CD-ROM image to download as a boot medium. The file should have an .iso file extension which should be properly burned to an empty CD-R(W). One of the tools that supports this is cdrecord from [app-cdr/cdrtools](https://packages.gentoo.org/packages/app-cdr/cdrtools) package:

`root #``cdrecord BOOT-CD.iso`
Choose from the BIOS boot menu to boot from CD and follow the instructions on the manufacturers website or in the user manual.

## FreeDOS environment

FreeDOS can be used to run DOS-based BIOS update utilities. A "custom" FreeDOS image which includes the necessary BIOS tools must be created. After the custom image has been generated, boot the image via one of the methods shown below.

Download FreeDOS and tools:

- [FreeDOS](https://www.ibiblio.org/pub/micro/pc-stuff/freedos/files/distributions/1.0/) - Download the fdboot.img file.
- [FreeDOS bootsector](https://www.ibiblio.org/pub/micro/pc-stuff/freedos/files/dos/sys/sys-freedos-linux/) - Download the sys-freedos-linux.zip file.
- The DOS-Flash program and new BIOS from the manufacturers website.

### Create a custom FreeDOS image

First download the required software and enable the loopback device in the kernel:

**enable loopback device**

If the module has not been loaded use modprobe to load it:

`root #``modprobe loop`
Install the required software:

`root #``emerge --ask dev-lang/nasm app-arch/unzip sys-fs/dosfstools`
Create an image file of \~20MB using the dd command. The name needs to be freedos.img when replacing the one on the SystemRescue:

`root #``dd if=/dev/null of=freedos.img bs=1024 seek=20480`
Write a file system to the image:

`root #``mkfs.fat freedos.img`
Write the boot sector to the image file:

`root #``unzip sys-freedos-linux.zip && ./sys-freedos.pl --disk=freedos.img`
Now copy the FreeDOS files to the new image.

Create the mount points:

`root #``mkdir -p /mnt/freedos /mnt/freedos_new`
Mount the original image:

`root #``mount -o loop fdboot.img /mnt/freedos`
Mount the new image:

`root #``mount -o loop freedos.img /mnt/freedos_new`
Copy the FreeDOS system files to the new image:

`root #``cp -ar /mnt/freedos/* /mnt/freedos_new/`
Now copy the flash program and the new BIOS to the image file:

`root #``cp -ar FLASH-PROGRAM BIOS-UPDATE /mnt/freedos_new`
Unmount both images:

`root #``umount /mnt/freedos_new /mnt/freedos`
### Using SystemRescue to boot FreeDOS

The SystemRescue comes with a version of FreeDOS. This version can replace the original image and create a bootable memory stick which contains the needed programs to flash the firmware.

#### Download SystemRescue and prepare LiveUSB

- [SystemRescue](https://www.system-rescue.org/Download/) - Download the normal ISO image.

#### Create a bootable memory stick

Use the default method to create the SystemRescue boot medium, the script usb\_inst.sh will provide guidance through the installation.

Create the folder in /mnt:

`root #``mkdir /mnt/SysRescue`
Mount the CD image:

`root #``mount -o loop systemrescue-x86-VERSION.iso /mnt/SysRescue`
Start the installation script:

`root #``/mnt/SysRescue/usb_inst.sh`
Unmount the CD image:

`root #``umount /mnt/SysRescue`
#### Replace the FreeDOS image

It is time to replace the original FreeDOS image on the SystemRescue memory stick.

Mount the SystemRescue memory stick (/dev/sdX1 needs to be replaced by the device name of the memory stick):

`root #``mount /dev/sdX1 /mnt/SysRescue`
Replace the freedos.img file:

`root #``cp freedos.img /mnt/SysRescue/bootdisk/`
Unmount the SystemRescue memory stick:

`root #``umount /mnt/SysRescue`
### Booting the FreeDOS image from GRUB directly

To boot FreeDOS without any external media use the memdisk tool from syslinux to allow grub (or another bootloader) to boot the FreeDOS image directly.

`root #``emerge --ask sys-boot/syslinux`
Mount the /boot partition (if needed):

`root #``mount /boot`
Copy the memdisk binary and the newly built FreeDOS image to /boot:

`root #``cp /usr/share/syslinux/memdisk /boot``root #``cp freedos.img /boot`
Edit /boot/grub/grub.conf and add an entry for FreeDOS:

**`/boot/grub/grub.conf`**

**Example grub.conf entry**

### BIOS update

Restart and choose to boot from the USB memory stick *or* the new grub entry. When using SystemRescue, in the GRUB command line type:

`freedos`
This should boot into the new FreeDOS image. The DOS prompt should appear:

`C:\>``_`
Now start the BIOS update by following the manufacturers instructions. Some useful commands in DOS:

- cd \<dir>
- Change to the directory.

- dir
- List the files in the current directory.

- type \[drive\]\[path\]filename
- Display the contents of a file.

## Flashrom

Some motherboards can support flashing (via the [sys-apps/flashrom](https://packages.gentoo.org/packages/sys-apps/flashrom) package) directly from the system. In this case the only needed component is the BIOS image. Before continuing this path, first check the list of [supported hardware](https://flashrom.org/Supported_hardware).

If the hardware is supported, verify the new BIOS image:

`root #``flashrom -v W7235IMS.1A0`
If everything checks out, then flash it:

`root #``flashrom -vw W7235IMS.1A0`
## UEFI Firmware Capsule

For modern UEFI, updates for the system firmware can be provided using UEFI *Capsules* (introduced with UEFI 2.0<sup>[\[1\]](https://wiki.gentoo.org#cite_note-1)</sup>). This method is available on Linux and Windows.<sup>[\[2\]](https://wiki.gentoo.org#cite_note-2)</sup> Vendors have to provide firmware updates for the use with Linux, which, if they do, they do via the *Linux Vendor Firmware Service* (LVFS).[\[3\]](https://wiki.gentoo.org#cite_note-3)

Refer to [fwupd](https://wiki.gentoo.org/wiki/Fwupd) for further details.

## See also

- [BIOS](https://wiki.gentoo.org/wiki/BIOS) — the standard firmware of IBM-PC-compatible computers until it was phased out in 2020.
- [Bootable DOS USB stick](https://wiki.gentoo.org/wiki/Bootable_DOS_USB_stick) — describes how to prepare a **bootable USB stick which loads DOS** using tools available in Gentoo.
