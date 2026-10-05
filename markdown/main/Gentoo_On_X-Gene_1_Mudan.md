<!-- source: https://wiki.gentoo.org/wiki/Gentoo_On_X-Gene_1_Mudan | group: Gentoo Wiki (Main) | wiki-title: Gentoo On X-Gene 1 Mudan -->
---
title: Gentoo On X-Gene 1 Mudan
url: https://wiki.gentoo.org/wiki/Gentoo_On_X-Gene_1_Mudan
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2024-04-06"
fingerprint: ad90715a21c71d27
license: CC BY-SA 4.0
---

# Gentoo On X-Gene 1 Mudan

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

The Mudan is an AARCH64 (arm64) server board based on the APM X-Gene 1 CPU. It has a Board Management Controller (BMC) based on the ASPEED-2400 chipset.

The BMC provides remote power on/off, JavaSOL for a remote serial link and for a remote (graphical?) console viewer jviewer. These are both Java Webstart applications and offer low or zero functionality with current javaws.

Plan to do the install either without a console, or using the serial console via sys-apps/ipmitool.

The idea is to prepare the boot disk, install it in the Mudan, boot, then ssh in from a safe distance.

## Preparation

It's an EFI based system. Follow the [AMD64 handbook](https://wiki.gentoo.org/wiki/Handbook:AMD64/Installation/Disks) for partitioning and so on. **Do remember to use the [arm64 stage3](https://distfiles.gentoo.org/releases/arm64/autobuilds/)!!**

Use grub as the boot-loader.

A bootable arm64 Linux is required. There are a few steps that must be carried out on the Mudan itself. The Debian one was used ~~after discovering that the Fedora offering would not drive the console.~~

To use VNC with Fedora, follow [this guide](https://docs.fedoraproject.org/en-US//fedora/f26/install-guide/advanced/VNC_Installations/).

TL;DR just add the following to the boot options:

inst.vnc inst.vncpassword=fedora99

## Gotchas

### Ethernet ports

The Mudan has two 1GBit Ethernet ports. The one on top of the two USB ports is dedicated to the BMC. The one on its own, or by the two 10Gbit ports is for the server itself. Connect both of them.

### Framebuffer console

After some hints that the framebuffer console is broken sometime after the 4.14 kernel, its confirmed that gentoo-sources:4.14.188 provides a working framebuffer console. Further work shows that EFI framebuffer is broken but astdrmfb, free with the AST DRM driver, works.

The remote graphical console, at [http://BMC\_IP](http://BMC_IP),  is supposed to work with the jviewer application but jviewer would not start for this writer.
All the bits that do not want to push a java application work. Its also at [https://BMC\_IP](https://BMC_IP) with a self signed certificate but the BMC struggles with the crypto as its much slower than http://

### Serial console

Its Serial console at 115200 baud delivered over Ethernet. In the Mudan, its a real 115200 baud port, redirected. That means its as fast/slow as 115200 baud link. Its pretending to be talking to a \[\[wikipedia:VT100|DEC VT100\] terminal.

The JavaSOL serial console works in gentoo-sources:4.14.188. However, JavaSOL should be avoided as it keeps crashing. Especially when provoked by ncurses.

The [sys-libs/freeipmi](https://packages.gentoo.org/packages/sys-libs/freeipmi) package offers a better alternative in that it is much faster to restart when it locks up.

There are command line ipmi commands for all the other information and commands the remote management web interface provides too.

## The install - Off the Mudan

Do as much as possible away from the Mudan. This author chrooted in to the Mudan install on a Raspberry Pi4 to do the rc-config setup and build grub then cross compiled the kernel.

- Partition the drive.
- Untar the Stage3.
- Untar the Portage snapshot.

### The Kernel

Installing crossdev and the kernel build process is covered by [Raspberry\_Pi\_3\_64\_bit\_Install](https://wiki.gentoo.org/wiki/Raspberry_Pi_3_64_bit_Install).

The key differences are:

- The kernel options required
- As the Mudan is an EFI system, the EFI firmware provides the Device Tree Binary (\*.dtb)

The manual kernel configuration and install process is assumed to create a kernel that can boot without the aid of an initrd.

### Kernel options

This author used the Raspberry Pi kernel tree and added APM-X-Gene and AST options. This provides a system that can boot on both a Raspberry Pi and the Mudan. That's a very good thing for user space testing and configuration and is an excellent excuse to invest in a Pi 4 too.

Be aware that the kernel can be compiled to run on X-Gene, which in the main processor or on the AST-2400 BMC. The options for building the kernel to run on the BMC are hidden behind ARCH\_ASPEED, which will be off and hidden by the ARCH=arm64 inherited from the environment. The AST-2400 BMC is a 32 bit arm system.

Under Platform selection

\[\*\] Broadcom BCM2835 family 
\[\*\] AppliedMicro X-Gene SOC Family

will do nicely.

Use the make menuconfig search to find all the symbols with XGENE in their names and enable them as \<\*>.

\<\*> AST server chips

Should be the DRM driver for AST GPU but I don't get anything on the VGA output once the kernel loads.

Install the kernel to suit both the Pi and Mundan under the stage3 and boot it on the Pi. Unless your Pi does USB booting, the kernel binary will need to be installed into both the Mudan and Pis /boot.

### Grub

On the Pi, install Grub2 following [Handbook:AMD64/Installation/Bootloader#Default:\_GRUB2](https://wiki.gentoo.org/wiki/Handbook:AMD64/Installation/Bootloader#Default:_GRUB2).

The /etc/default/grub file requires the following additions:

**`/etc/default/grub`**

**Preparing grub for the Mudan**

```
# fix boot error    error: file /boot/grub/locale/C.gmo not found
LANG=C
#  Do not use UUID do use PARTUUID
GRUB_DISABLE_LINUX_UUID=true
GRUB_DISABLE_LINUX_PARTUUID=false
```
Grub can be installed but as the Pi is not an EFI system, it will fail trying to to update the efivars. Repeating this step is the only thing that must be done on the Mudan.

### Userspace

Adding the following packages is a good start:

app-admin/sudo
app-misc/screen
app-portage/eix
app-portage/gentoolkit
app-text/wgetpaste
net-misc/dhcpcd
net-misc/ntp
sys-apps/pciutils
sys-apps/usbutils
sys-boot/grub
sys-process/htop

Access will be via ssh. You will need a way to obtain root over ssh. Be sure that you have a way to do that before the HDD is moved to the Mudan.

## Move to the Mudan

### IPMI Tool

`user $``ipmitool  -I lanplus -H 192.168.100.128 -U admin -P admin shell`
gets a shell on the Board Management Computer. (BMC)

The username and password are case sensitive.

`user $``ipmitool  -I lanplus -H 192.168.100.128 -U admin -P admin sol activate`
provides access to the Serial Over LAN (SoL) console.

Its possible to set up the BIOS here, watch and interact with the boot sequence and do everything you can at the console. Its a real serial port running at 115000n1 on the Mudan, redirected over the BMC LAN.

The first firmware screen via SoL is:

```
       Aptio Setup Utility - Copyright (C) 2017 American Megatrends, Inc.       
    Main  Advanced  Security  Boot  Save & Exit  Server Mgmt                    
/----------------------------------------------------+-------------------------\
|  BIOS Information                                  |Platform Board           |
|  BIOS Vendor             American Megatrends       |Information              |
|  Core Version            5.011                     |                         |
|  Compliancy              UEFI 2.4; PI 1.3          |                         |
|  Project Version         MUDAN 0.19                |                         |
|  Build Date and Time     01/17/2017 11:28:54       |                         |
|> Platform Board Information                        |                         |
|                                                    |                         |
|  System Language         [English]                 |                         |
|                                                    |-------------------------|
|  System Date             [Mon 10/17/1983]          |><: Select Screen        |
|  System Time             [21:38:38]                |^v: Select Item          |
|                                                    |Enter: Select            |
|  Access Level            Administrator             |+/-: Change Opt.         |
|                                                    |F1: General Help         |
|                                                    |F2: Previous Values      |
|                                                    |F3: Optimized Defaults   |
|                                                    |F4: Save & Exit          |
|                                                    |ESC: Exit                |
\----------------------------------------------------+-------------------------/
        Version 2.17.1254. Copyright (C) 2017 American Megatrends, Inc.    
```
The date suggests that the CMOS battery needs to be replaced.



### Power requirements

The test system comprises the APM X-Gene 1 motherboard, fitted with 4x16G PC-1333 RDIMMs a single ex k6-2 fan fitted to the CPU, (no case fans yet) and a single 1TB 'spinney' HDD.

Its all powered by a Coolermaster White 500w PSU. The following power measurements are AC Input Power to the PSU.

Standby 3.5w (PSU on and system powered off)
Idle 63w (After booting but doing nothing)
Max 90w (Building gcc on all 8 cores)

At such low loads for a 500w PSU, the power factor is terrible but that only matters for sizing a UPS. That 90w is 110 VA.

In the UK, the running cost is just under £10/month, running the system flat out 24/7

### The first boot

Boot the Debian arm64 install from a USB stick and use its Rescue Mode to get a shell on the SoL.

Mount /boot if its not already mounted and rerun grub-install. This time it will update the efivars.

Shutdown the system.

### The first Gentoo boot

Go into the setup and choose the 'gentoo' entry as the first boot option. Save and reset then wait while it does its thing. You may or may not see the grub menu.

Discover the IP address that the Mudan is on and log in over ssh.

## A bootable USB

Since I want to try root in LVM without an initrd, I've made a bootable USB stick. Its a arm64 stage3, with gentoo-sources:4.14.188, so the framebuffer console works, with a few other packages to make logging in over ssh 'just work'.

Its \[[provided](http://bloodnoc.org/~roy/X-Gene/)\] as a single tarball for root and /boot. There is no ::gentoo repo, distfiles or packages. emerge works if the missing parts are provided.

The root password is root and the normal user, named demouser has 'demouser' for a password. Root password based logins over ssh are not permitted but demouser is in the wheel group.

### How to use it

A 4 GB USB stick or larger is required.

Filesystem      Size  Used Avail Use% Mounted on
/dev/sdb2        13G  1.9G   11G  16% /mnt/gentoo
/dev/sdb1       122M   22M  100M  19% /mnt/gentoo/boot

Make a GPT partition table on a USB stick:

`root #``gdisk /dev/sdX`
Then create a vfat filesystem for /boot, and an ext4 filesystem for root. Add swap to taste but its intended as an install tool, not a working system.

`root #````
mkfs -t vfat /dev/sdX1
```
`root #````
mke2fs -t ext4 -b 2048 -i 2048 -O^has_journal,dir_index,sparse_super2 /dev/sdX2
```
That's a ext4 filesystem

with a 2kB block size
an inode for every block
without a journal
with b-tree directories
only two superblock backups

`root #``mount /dev/sdX2 /mnt/gentoo``root #``mkdir /mnt/gentoo/boot``root #``mount /dev/sdX1 /mnt/gentoo/boot`
Fetch the \[[X-gene.xz](http://bloodnoc.org/~roy/X-Gene/X-gene.xz)\] tarball and untar it:

`root #``tar --xattrs --strip-components=2 --show-transformed-names -Jxpf <X-gene.xz> -C /mnt/gentoo/`
### Finishing up

The USB stick does not need any efivars written to boot. It's almost like a floppy.

**You will need to edit its /etc/fstab as that uses my UUIDs and rootfsck will fail.**

grub.cfg contains root=/dev/sdb2 which may not be correct for your configuration. Either **edit /boot/grub/grub.cfg** or **use the grub editor** during boot.

None of these edits require an arm64 system.

### Booting

Connect the USB stick and go into the firmware to select the stick as the first boot option. Save the change and reset. The system will restart. Let it get to the grub menu, then edit the root filesystem as required. Continue the boot.

You should have both a framebuffer and serial console. The serial console works with javaSOL.

eth0 will have started with DHCP, so once the IP address is known, its possible to use demouser@ to log in via ssh.

Information on the Mudan server is very thin on the ground. Links to things that might be useful go here.

### Gigabyte MP30-AR0

Gigabyte marketed a server based on the X-Gene 1 and AST2400 BMC. The bits are scattered around on the motherboard differently to the Mudan [https://www.gigabyte.com/uk/Server-Motherboard/MP30-AR0-rev-11/support#support-manual](https://www.gigabyte.com/uk/Server-Motherboard/MP30-AR0-rev-11/support#support-manual) but its the same major components.

### WikiChip

WikiChip has a page or two [https://en.wikichip.org/wiki/apm/x-gene](https://en.wikichip.org/wiki/apm/x-gene)

There are product briefs for a 1U and 2U server.

### Fitting RAM

From trial and error the Mudan is very picky about where RAM is fitted and which order RAM is fitted.

The Blue sockets must be populated first.

Its worse than that. RAM must be fitted in powers of 2 sticks. That's 1, 2, 4 or 8. Trial and error suggests that mixed RAM sizes are not tolerated but mixed speeds are.

Low voltage (1.35v) DIMMs are accepted but still operated at 1.5v, which will not harm the RAM.

### Ethernet ports

The Mudan has four. One on its own, that is for the IPMI.

One with the two SFP+ ports, which the kernel sees as eth0. None of the Ethernet ports are on the PCIe bus, so its unlikely that persistent device renaming will change the names.

Ethertool reports

\# ethtool eth0
Settings for eth0:
	Supported ports: \[ TP	 MII \]
	Supported link modes:   10baseT/Full
	                        100baseT/Full
	                        1000baseT/Full
# ethtool eth1
Settings for eth1:
	Supported ports: \[ FIBRE \]
	Supported link modes:   10000baseT/Full
# ethtool eth2
Settings for eth2:
	Supported ports: \[ FIBRE \]
	Supported link modes:   10000baseT/Full

Having tested an SFP 1Gb/sec SFP insert in eth1 and eth2 the author can confirm that the Mudans SFP+ are not backwards compatible with SFP 1Gbit/sec RJ45 inserts.

## A Pi 4 chroot

With 8 cores and lots of RAM, the Mudan is an attractive build system for a Raspberry Pi binhost.

As always, there are a few problems with building on a CPU that cannot always execute the code its just built.

The X-Gene 1 is the only armv8 CPU that lacks the crc extensions. That does not appear to be a problem with core system packages as my toolchain still works.

### /etc/portage/env/x-gene

We need to tell portage what the X-Gene 1 CPU really is, for those packages that need it. Avoiding Illegal instruction issues like

/var/log/portage/net-libs:signond-8.60-r2:20201007-172655.log-/usr/lib64/qt5/bin/qdbusxml2cpp -a backup\_adaptor.h: ../../lib/signon /com.nokia.SingleSignOn.Backup.xml
/var/log/portage/net-libs:signond-8.60-r2:20201007-172655.log:make\[2\]: \*\*\* \[Makefile:752: backup\_adaptor.h\] Illegal instruction

```
# revert the +crc which the X-Gene 1 does not have
# Hmm, maybe its not cortex-a72 either.
# just on a per package basis   -mcpu=armv8-a
CFLAGS="${CFLAGS} -march=armv8-a  -mtune=cortex-a72" CXXFLAGS="${CXXFLAGS} -march=armv8-a  -mtune=cortex-a72"
```
### /etc/portage/package.env/x-gene

Now tell portage to build a few packages specifically for the X-Gene 1.

dev-qt/qtdbus x-gene
dev-qt/qtcore x-gene

We may still be able to build these packages for the Pi 4 with `FEATURES=buildpkgonly`. The trick is not in try to execute them on the X-Gene 1.

`FEATURES=buildpkgonly` requires that all dependencies already be installed, as portage is not permitted to install anything to the filesystem while it is in effect.
