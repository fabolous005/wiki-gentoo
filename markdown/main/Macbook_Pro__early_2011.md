<!-- source: https://wiki.gentoo.org/wiki/Macbook_Pro_(early_2011) | group: Gentoo Wiki (Main) | wiki-title: Macbook Pro (early 2011) -->
---
title: Macbook Pro (early 2011)
url: https://wiki.gentoo.org/wiki/Macbook_Pro_(early_2011)
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2024-04-28"
fingerprint: "148de81b1de5ecf1"
license: CC BY-SA 4.0
---

# Macbook Pro (early 2011)

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)


Apple's early 2011 13" MacBook Pro (model code 8,1) is capable of installing and running Gentoo Linux. Installation and configuration is decently easy using [Sakaki's EFI Install Guide](https://wiki.gentoo.org/wiki/User:Sakaki/Sakaki%27s_EFI_Install_Guide)!

### WIFI during Gentoo install

Booting from the Gentoo-LiveDVD (image on USB stick) the wifi connection can easily established using graphical tools. Installing Gentoo via SSH is untested.

In [Chapter 10](https://wiki.gentoo.org/wiki/User:Sakaki/Sakaki%27s_EFI_Install_Guide/Configuring_systemd_and_Installing_Necessary_Tools) re-establishing a wifi connection was unsuccessful, but using a wired connection should not be any problem. After the installation of Gnome the wifi connection can easily be established using graphical tools. Hence, using a wired connection during this step is recommended.

### Kernel Config

Appropriate kernel configuration [can be found here](https://wiki.gentoo.org/wiki/Apple_Macbook_Pro_Retina).

If you plan to use the proprietary Broadcom-Sta driver the kernel configuration has to look like below.
Ensure the following options are NOT set (required for proper Broadcom wireless):
**Warning: Broadcom-Sta driver no longer maintained**

### Disk Partitionin/Formatting/Layout for Triple Boot (OSX/WIN10/GENTOO)

**This guide is for a complete reinstall of all operating systems on the machine. Do NOT use this if you want to keep your current OSX or Windows installation. ALL PREVIOUS DATA WILL BE LOST!**


The easiest way to install Windows 10 on a Macbook Pro is Apple's Boot Camp Assistant. But it requires a disk, **without any other partition**, except for the one OS X is installed on. At the same time it uses **all** disk space from the beginning of the OS X partition to the end of the disk. Hence, the preliminary OSX partition, which will be split into the final OSX partition and the Win10 partition, has to be located at the very end of the disk.

Therefore, I recommend the following installation order:

- OSX first
- then Win10
- Gentoo last


To create the preliminary OSX partition in the disk sectors, boot from a Linux USB stick (e.g. using the Gentoo-LiveDVD image), open a console and enter:

`livecd~ $````
 sudo su
```
`livecd ~ #````
parted /dev/sdY
```
GNU Parted 3.2
... additional output suppressed ...
(parted) mklabel gpt
Warning: The existing disk label on /dev/sdY will be destroyed and all data on
this disk will be lost. Do you want to continue?
Yes/No? yes
(parted) unit mib
(parted) mkpart primary 1 1025
(parted) name 1 'EFI System Partition'
(parted) set 1 boot on
(parted) set 1 esp on

In the following example, *b* is the very end of the disk (the 'last' Mb), whereas *a* is *b* minus the desired size of the final OSX partition + Win10 partition (in Mb).

Example: If you have a total disk capacity of 500 Gb and want to use 200 Gb for OSX, 150 Gb for Win10 and the remaining space for Gentoo, then:

*b = 500000*

*a = b - 350000*

since we set the units to Mb.

`livecd ~ #````
(parted) mkpart primary a b
```
`livecd ~ #````
(parted) q
```
`livecd ~ #````
mkfs.vfat -F32 /dev/sdY1
```
Now reboot and

- install OS X on the partition we just created (350 Gb in this example, OSX will install the EFI on the EFI partition automatically),
- use Apple's Boot Camp Assistant to split this temporary OSX partition into the final OSX partition and the Windows partition and
- install Windows10 on the partition created for Windows by the Boot Camp Assistant.

Now that OSX and Win10 boot successfully, use the Linux USB stick again to boot. once again open a console and enter:

`livecd~ $````
 sudo su
```
`livecd ~ #````
parted /dev/sdY
```
GNU Parted 3.2
... additional output suppressed ...
(parted) unit mib
(parted) mkpart primary 1025 a
(parted) q

### Restore Win10 Boot option

In theory you could continue with the Gentoo installation as described by [Sakaki's EFI Install Guide](https://wiki.gentoo.org/wiki/User:Sakaki/Sakaki%27s_EFI_Install_Guide), but you will soon recognize that the Win10 partition does not show up as a selectable boot option anymore.

To be able to use Win10 again, we need to use *gdisk*:

`livecd ~ #````
gdisk 
```
GPT fdisk (gdisk) version 1.0.1
Type device filename, or press \<Enter> to exit: /dev/sdY

Partition table scan:
 ... additional output suppressed ...
Found valid GPT with hybrid MBR; using GPT.
Command (? for help): r
Recovery/transformation command (? for help): h
WARNING! Hybrid MBRs are flaky and dangerous! If you decide not to use one,
just hit the Enter key at the below prompt and your MBR partition table will
be untouched.
Type from one to three GPT partition numbers, separated by spaces, to be
added to the hybrid MBR, in sequence: 5
Place EFI GPT (0xEE) partition first in MBR (good for GRUB)? (Y/N): y
Creating entry for GPT partition #5 (MBR partition #2)
Enter an MBR hex code (default 07): <enter>
Set the bootable flag? (Y/N): y
Unused partition space(s) found. Use one to protect more partitions? (Y/N): n
Recovery/transformation command (? for help): o

You should have two entries. One type EE, one 07, with the 07 entry marked with \* under Boot. If you don't, report back. If you do, write out the update partition information, and hope a power failure doesn't occur for the next few seconds...

Recovery/transformation command (? for help): w

reboot. hold down option key and you should be able to boot into either Mac HD, Recovery HD, or Windows.

## External resources

- [https://support.apple.com/kb/SP619](https://support.apple.com/kb/SP619) - Apple's technical specifications page for this laptop.
- [https://www.gentoo.org/downloads/](https://www.gentoo.org/downloads/) - Obtain a Minimal Installation CD from a Gentoo mirror.
- [https://www.system-rescue.org/](https://www.system-rescue.org/) - A rescue CD that includes many helpful troubleshooting tools not included on Gentoo's Minimal Installation CDs.
