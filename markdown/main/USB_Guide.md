<!-- source: https://wiki.gentoo.org/wiki/USB/Guide | group: Gentoo Wiki (Main) | wiki-title: USB/Guide -->
---
title: USB/Guide
url: https://wiki.gentoo.org/wiki/USB/Guide
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2025-10-16"
fingerprint: b681c815e80b3bc4
license: CC BY-SA 4.0
---

# USB/Guide

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

This **USB guide** helps users setup and configure various USB devices on Gentoo systems.

USB stands for **U**niversal **S**erial **B**us which is an external interface standard that enables communication between computers and various peripherals. Some of the most commonly used USB devices are keyboards, mice, pen drives, digital cameras, external CD and DVD writers, printers, MP3 players, smart phones, and biometric devices.

## About USB

There are currently three versions of USB in use, USB 2.0, USB 3.0, and USB 3.1. Since USB has always been backward compatible with previous versions, USB 2.0 is backwards compatible with USB 1.1. Likewise USB 3.0 and 3.1 can work with versions 2.0 and 1.1. The latest USB devices are typically USB 3.0 compatible, although some motherboards still include USB 2.0. USB 2.0 supports a maximum data transmission speed of 480 Mbit/s or 60 MB/s, 3.0 can transfer at a rate of 5.0 Gbit/s or 625 MB/s<sup>[\[1\]](https://wiki.gentoo.org#cite_note-1)</sup>, and 3.1 tops out at a whopping 10.0 Gbit/s or 1.25 GB/s. Another advantage with USB is that the devices are all *hot-pluggable*, which means the system does not need a restart in order for the devices that have been plugged in to work.

### A technical perspective

Before the exact configuration options in the kernel, it would be apt to look at USB in a little more detail. For readers in a hurry or want to skip this section, please go to the [kernel configuration](https://wiki.gentoo.org#Kernel_configuration) section.

A USB system has a host controller, hubs, a *root hub* amongst others and can support up to 127 USB devices including the hubs. The host controller is nothing but the hardware interface between the USB device and the operating system. There are a couple of HCIs (Host Controller Interfaces) in use today. They are the OHCI (Open HCI) by Compaq, UHCI (Universal HCI) and EHCI (Enhanced HCI) which are both from Intel, and xHCI (eXtensible Host Controller Interface). The OHCI/UHCI are the two industry standard USB 1.1 interfaces whereas EHCI is for USB 2.0 and xHCI is compatible with *all* of the previously mentioned interfaces including USB 3.0.[\[2\]](https://wiki.gentoo.org#cite_note-2)

The hardware vendor provides an interface for the programmer that allows the system to interact with the hardware and this is called the HCD or Host Controller Device. It is through this HCD that the device interacts with the system software. The following diagram should make things easier to comprehend.

A USB device can either use a custom driver or use one already present in the system and this is based on the concept of a device *class*. This means that if a device belongs to a certain *class*, then other devices belonging to the same *class* can make use of the same device driver. Some of these *classes* are the USB HID (Human Interface Devices) class which covers input devices like keyboards and mice, the USB Mass Storage devices class which covers devices like pen drives, digital cameras, audio players etc and the USB CDC (Communication Devices Class) which essentially covers USB modems and similar devices.

## Listing USB peripherals

It is very simple to find out whether the machine in question has USB support. The [lspci](https://wiki.gentoo.org/wiki/Pciutils) command (comes with the [sys-apps/pciutils](https://packages.gentoo.org/packages/sys-apps/pciutils) package) is perfect for this purpose.

Example for a USB 1.1 compliant system (note the **UHCI** in the output):

`user $``/usr/sbin/lspci -v | grep USB`
0000:00:04.2 USB Controller: Intel Corp. 82371AB/EB/MB PIIX4 USB (rev 01) (prog-if 00 \[UHCI\])

A system that is USB 2.0 compliant, note the **EHCI** and **UHCI**:

`user $``/usr/sbin/lspci -v | grep USB`
00:1d.0 USB Controller: Intel Corp. 82801DB USB (Hub #1) (rev 01) (prog-if 00 \[UHCI\])
00:1d.1 USB Controller: Intel Corp. 82801DB USB (Hub #2) (rev 01) (prog-if 00 \[UHCI\])
00:1d.2 USB Controller: Intel Corp. 82801DB USB (Hub #3) (rev 01) (prog-if 00 \[UHCI\])
00:1d.7 USB Controller: Intel Corp. 82801DB USB EHCI Controller (rev 01) (prog-if 20 \[EHCI\])

So using the lspci command it is easy to determine if the system supports USB 2.0, or 3.0. This is useful for the section to come so that corresponding options can be correctly set in the kernel.

## Kernel configuration

### Getting the kernel

First emerge the preferred kernel sources. Here, we'll use [sys-kernel/gentoo-sources](https://packages.gentoo.org/packages/sys-kernel/gentoo-sources). For more information on the various kernel sources available on Portage, please look up the [Gentoo Linux kernel packages](https://wiki.gentoo.org/wiki/Kernel/Packages).

`root #``emerge --ask gentoo-sources`
Now, lets get on with the task of configuring the kernel.

`root #````
cd /usr/src/linux
```
`root #````
make menuconfig
```
### Config options for the kernel

Now we will look at some of the options we will have to enable in the kernel to ensure proper USB support for our devices.

**make menuconfig options**

If you have a USB keyboard, mouse, joystick, or any other input device, you need to enable HID support. Go back one level to "Device drivers" and enable HID support as shown:

**Enabling HID support**

#### USB Type-C and Thunderbolt

Some newer USB Type-C root hubs are only enabled when a device is connected to the port. This requires PCI-hotplug support to function. This should also allow hotplugging Thunderbolt devices.

**PCI Hotplugging**


As of kernel 5.9, [CONFIG\_THUNDERBOLT](https://cateee.net/lkddb/web-lkddb/THUNDERBOLT.html) has been replaced by [CONFIG\_USB4](https://cateee.net/lkddb/web-lkddb/USB4.html). This option provides support for both USB4 and Thunderbolt.

**USB4 and Thunderbolt support**

#### USB Audio

The USB Audio kernel driver must be enabled to support USB microphones, headsets and speakers to work properly. This is the case for some laptops since their microphones are connected on the USB bus.

**Enable support for`SND_USB_AUDIO`**


Now that your options are set, you can [(re)compile the kernel](https://wiki.gentoo.org/wiki/Kernel/Rebuild) and USB support should be functional once you reboot into the new kernel.

## Seeing USB at work

### dmesg is a friend!

In this section, we'll see how the system responds to various USB devices. We'll start by plugging in a 512 MB removable USB drive. Any similar mass storage device can be used for this test. For this experiment dmesg will be the primary utility used to see what is happening and how the system responds to plugged devices.

Plug in USB drive into available USB port and then...

`root #``dmesg | less`
\## (The device is picked up as a USB 1.1 and allocated an address. Also says what HCD it is using.)
usb 1-1: new full speed USB device using uhci\_hcd and address 2
  
## (SCSI emulation automatically kicks in)
scsi0 : SCSI emulation for USB Mass Storage devices
usb-storage: device found at 2
  
## (Now the device information including model number is retrieved)
usb-storage: waiting for device to settle before scanning
  Vendor: JetFlash  Model: TS512MJF2A        Rev: 1.00
  Type:   Direct-Access                      ANSI SCSI revision: 02
SCSI device sdb: 1003600 512-byte hdwr sectors (514 MB)
  
## (The write-protect sense is EXPERIMENTAL code in the later kernels)
sdb: Write Protect is off
sdb: Mode Sense: 0b 00 00 08
sdb: assuming drive cache: write through
SCSI device sdb: 1003600 512-byte hdwr sectors (514 MB)
/dev/scsi/host0/bus0/target0/lun0: p1
Attached scsi removable disk sdb at scsi0, channel 0, id 0, lun 0
Attached scsi generic sg0 at scsi0, channel 0, id 0, lun 0,  type 0
usb-storage: device scan complete
## (At this point, the device is generally accessible by mounting /dev/sdb1)
  
## (When the device is disconnected, the system acknowledges the same)
usb 1-1: USB disconnect, address 2

How would a USB mouse show up in case you had one? It will show up as an HID device.

`root #``dmesg | grep USB`
drivers/usb/input/hid-core.c: v2.0:USB HID core driver
usb 1-1: new low speed USB device using address 2
input: USB HID v1.10 Mouse \[Logitech USB-PS/2 Optical Mouse\] on usb-0000:00:07.2-1

## Mounting a USB mass storage device

See the [removable media](https://wiki.gentoo.org/wiki/Removable_media#Mounting_removable_media) article.

Once the device is connected and mounted, it can be accesses like a normal hard disk. Usual operations like cp, mv, rm, etc work fine. You could also create a filesystem on the USB stick/format it.

## Userspace USB

To list USB devices, use [lsusb](https://wiki.gentoo.org/wiki/Usbutils).

`root #``emerge --ask sys-apps/usbutils`
Once installed, run lsusb to get simple info on the USB devices attached to the machine.

`user $``lsusb`
\## (This is the 512 MB USB flash drive from Transcend)
Bus 001 Device 003: ID 0c76:0005 JMTek, LLC. USBdisk
## (This is the Optical Mouse)
Bus 001 Device 002: ID 046d:c00e Logitech, Inc. Optical Mouse
## (This is the root hub)
Bus 001 Device 001: ID 0000:0000

For those who love to see lots of information run lsusb with the `-v` option. Another good option is that lsusb dumps the current physical USB hierarchy as a tree and thus makes it easier to understand the exact device map. The command is lsusb -t. For example:

`user $``lsusb -t`
Bus#  1
\`-Dev#   1 Vendor 0x0000 Product 0x0000
  |-Dev#   2 Vendor 0x046d Product 0xc00e
  \`-Dev#   3 Vendor 0x0c76 Product 0x0005

You can easily correlate the outputs of lsusb and lsusb -t, which helps debugging as well as understanding how USB works.

## See also

- [LiveUSB](https://wiki.gentoo.org/wiki/LiveUSB) — explains how to create a *Gentoo LiveUSB* or, in other words, how to emulate a **x86** or **amd64** Gentoo LiveCD using a USB drive.
- [Allow only known usb devices](https://wiki.gentoo.org/wiki/Allow_only_known_usb_devices) — describes how to protect a GNU/Linux system against rogue USB devices via a white listing policy.

## External resources

The following documents are highly technical but truly interesting:

## References
