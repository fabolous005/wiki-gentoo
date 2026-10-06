<!-- source: https://wiki.gentoo.org/wiki/Dell_Latitude_D810 | group: Gentoo Wiki (Main) | wiki-title: Dell Latitude D810 -->
---
title: Dell Latitude D810
url: https://wiki.gentoo.org/wiki/Dell_Latitude_D810
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2024-04-29"
fingerprint: "7f2c9b35d9a331e8"
license: CC BY-SA 4.0
---

# Dell Latitude D810

From Gentoo Wiki

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

## Dell Latitude D810

This article will describe configuration and kernel options to make make the hardware work in Gentoo.

**Work in progress.**

### Hardware

*The below list is based on the machine this configuration was done one. There might be some variance.*

- CPU: Intel Pentium M 2.0 GHz
- Video: Radeon X600 Graphics
- Display: 15.4" 1920x1200 display
- Audio: 82801FB/FBM/FR/FW/FRW (ICH6 Family) AC'97 Audio Controller
- USB: Intel Corporation 82801FB/FBM/FR/FW/FRW (ICH6 Family) USB UHCI
- USB: Intel Corporation 82801FB/FBM/FR/FW/FRW (ICH6 Family) USB2 EHCI Controller
- Wired network: Broadcom Corporation NetXtreme BCM5751 Gigabit Ethernet PCI Express
- Wireless network: Intel Corporation PRO/Wireless 2915ABG \[Calexico2\] Network Connection

### Choosing the right install medium

The D810 has a modular bay that could contain a DVD/CD drive, floppy drive or less useful to the install, a battery. Using a CD or DVD is fine, but the machine also supports booting from USB.

If your boot order is not already accommodating, tap `F2` (`F12`?) at the POST screen to get a menu of available choices and boot the install media.

### CPU

The CPU is an Intel Pentium M.

KERNEL **Processor Support**

```
Processor type and features --->
    Process family (Pentium M)
```
### Video

Video device is either an ATI (AMD) Radeon X300 or X600. The R300\_cp.bin firmware is used for both.

KERNEL **Video Device Support**

```
Device Drivers --->
    Generic Driver Options --->
        (radeon/R300_cp.bin) External firmware blobs to build into the kernel binary
    Graphics support --->
        <*> ATI Radeon
```
### Drive Controller

KERNEL **Drive Controller Support**

```
Device Drivers --->
    <*> Serial ATA and Parallel ATA drivers --->
        <*> Intel ESB, ICH, PIIX3, PIIX4 PATA/SATA support
```


### Audio

KERNEL **Audio Support**

```
Device Drivers --->
    <*> Sound card support --->
        <*> Advanced Linux Sound Architecture --->
            [*] PCI sound devices --->
                <*> Intel/SiS/nVidia/AMD/ALi AC97 Controller
```
### USB

KERNEL **USB Support**

```
Device Drivers --->
    [*] USB support
        <*> EHCI HCD (USB 2.0) support
        <*> UHCI HDC (most Intel and VIA) support
```
### Ethernet

KERNEL **Ethernet Support**

```
Device Drivers --->
    [*] Network devices support --->
        [*] Ethernet driver support --->
            [*] Broadcom devices
                <*> Broadcom Tigon3 support
```
### Wireless

KERNEL **Wireless Networking Support**

```
Device Drivers --->
    [*] Network devices support --->
        [*] Wireless LAN --->
            <*> Intel PRO/Wireless 2200BG and 2915ABG Network Connection
```
