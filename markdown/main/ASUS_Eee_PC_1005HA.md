<!-- source: https://wiki.gentoo.org/wiki/ASUS_Eee_PC_1005HA | group: Gentoo Wiki (Main) | wiki-title: ASUS Eee PC 1005HA -->
---
title: ASUS Eee PC 1005HA
url: https://wiki.gentoo.org/wiki/ASUS_Eee_PC_1005HA
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2024-04-29"
fingerprint: "14878959d5a795d1"
license: CC BY-SA 4.0
---

# ASUS Eee PC 1005HA

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)


The Asus Eee PC 1005HA is a laptop manufactured by Asus in 2009. It features an Intel Atom N280 with 1GB of RAM (upgradable to 2GB), a 160GB hard disk, and a Mobile Intel 945ME Express with 250MB of VRAM. Some versions of the laptop had an Intel Atom N270.

This article supplements the [Gentoo Linux x86 Handbook](https://wiki.gentoo.org/wiki/Handbook:X86) with specific information regarding the Eee PC 1005HA.

Kernel configuration is based on the [sys-kernel/gentoo-sources](https://packages.gentoo.org/packages/sys-kernel/gentoo-sources)-3.0.6.

### Choosing the right install medium

As the Eee PC 1005HA does not have a CD/DVD drive, the easiest way to install Gentoo is using a LiveUSB. Follow the instructions given in the [Gentoo Linux LiveUSB HOWTO](https://wiki.gentoo.org/wiki/LiveUSB).

The ath9k module that ships with the gentoo-minimal disk image has not been compiled with PCI support, which means wireless networking is not available during install. As a Gentoo install is dependent on a network connection, either install over ethernet (atl1c driver is supported), use another distro to install (e.g. [System Rescue CD](https://www.system-rescue.org/)), or recompile the ath9k module for a custom LiveUSB.

To boot from a LiveUSB press `Esc` as the machine powers on, then select the appropriate device from the list presented. If Boot Booster is enabled (see later), first enter the BIOS configuration by pressing `F2` as the machine powers on, then exit without saving changes by pressing `F10`, then press `Esc` as described previously.

### Partitioning

Follow the guidance on partitioning as given in the [handbook](https://wiki.gentoo.org/wiki/Handbook:X86), with the following exception to enable Boot Booster:

#### Boot Booster

The Asus Eee PC 1005HA features a technology called Boot Booster, which can reduce boot times by a few seconds by caching the results of the [POST](https://en.wikipedia.org/wiki/Power-on_self-test) on the hard disk. When partitioning the internal hard disk, create a 16MB primary partition of type EFI (0xEF). The location on the disk is unimportant but the partition must be a primary one. The system does not boot using the EFI (Extensible Firmware Interface), but rather uses the partition type to identify which partition to read/write the cache.

The example fdisk output below shows a 100MB boot partition, a 16MB EFI partition (for Boot Booster) a 2GB swap partition and a \~147GB partition for the rest of the system.

`root #``fdisk -l`
Disk /dev/sda: 160.0 GB, 160041885696 bytes
255 heads, 63 sectors/track, 19457 cylinders, total 312581808 sectors
Units = sectors of 1 \* 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disk identifier: 0xde0404c0
   Device Boot      Start         End      Blocks   Id  System
/dev/sda1   \*        2048      206847      102400   83  Linux
/dev/sda2          206848      239615       16384   ef  EFI (FAT-12/16/32)
/dev/sda3          239616     4433919     2097152   82  Linux swap / Solaris
/dev/sda4         4433920   312581807   154073944   83  Linux

Boot Booster must also be enabled in the BIOS configuration.

## Hardware

### CPU

The CPU is an Intel Atom N280 (or N270 on some revisions).

**Processor support**

To update the processor microcode, install [sys-apps/microcode-ctl](https://packages.gentoo.org/packages/sys-apps/microcode-ctl).

`root #``emerge --ask sys-apps/microcode-ctl`
The microcode patch will be lost at each reboot, but the init script can reload it.

`root #``/etc/init.d/microcode_ctl start``root #``rc-update add microcode_ctl boot`
In this example, the microcode is updated from version 0x212 to 0x218.

`root #``dmesg | grep microcode`
\[    0.250287\] microcode: CPU0 sig=0x106c2, pf=0x4, revision=0x212
\[    0.250302\] microcode: CPU1 sig=0x106c2, pf=0x4, revision=0x212
\[    0.250433\] microcode: Microcode Update Driver: v2.00 \<tigran@aivazian.fsnet.co.uk>, Peter Oruba
\[   11.330007\] microcode: CPU0 updated to revision 0x218, date = 2009-04-10
\[   11.335961\] microcode: CPU1 updated to revision 0x218, date = 2009-04-10

### Graphics

The GPU is a Mobile Intel 945ME Express. Follow the instructions in the [Intel](https://wiki.gentoo.org/wiki/Intel) article.

### Ethernet

The Ethernet card is an Atheros AR8132.

**Gigabit Ethernet**

### Wireless

The wireless card is an Atheros AR9285 (PCI-Express).

**Wireless support**

### Sound

The sound card is a Realtek ALC269 (Intel ICH7 Family).

**Sound card support**

See also the [ALSA](https://wiki.gentoo.org/wiki/ALSA) guide.

### Webcam

**Webcam support**

### ACPI, LEDs and hotkeys

### Bluetooth

Bluetooth is provided by a Broadcom BT-253, connected using USB. Follow the instructions given in the [Gentoo Linux Bluetooth Guide](https://wiki.gentoo.org/wiki/Bluetooth).

### TouchPad

The TouchPad is a Synaptics PS/2 TouchPad.

**TouchPad support**

Enable two-finger scrolling in X:

**`/etc/X11/xorg.conf.d/10-synaptics`**

```
Section "InputClass"
	Identifier "Touchpad"
        Driver "synaptics"
        MatchIsTouchpad "on"
	Option "VertTwoFingerScroll" "true"
	Option "HorizTwoFingerScroll" "true"
        Option "EmulateTwoFingerMinZ" "40"
        Option "EmulateTwoFingerMinW" "5"
EndSection
```
### Keyboard

Configuration for the keyboard in X (adjust as required):

**`/etc/X11/xorg.conf.d/30-keyboard`**

```
Section "InputClass"
        Identifier "Keyboard"
	Driver "evdev"
        Option "XkbLayout" "us"
        Option "XkbModel" "asus_laptop"
	Option "XkbRules" "xorg"
        Option "XkbVariant" ",qwerty"
        MatchIsKeyboard "on"
EndSection
```
