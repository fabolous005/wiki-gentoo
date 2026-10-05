<!-- source: https://wiki.gentoo.org/wiki/TrimSlice | group: Gentoo Wiki (Main) | wiki-title: TrimSlice -->
---
title: TrimSlice
url: https://wiki.gentoo.org/wiki/TrimSlice
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2024-04-12"
fingerprint: "9e11303b058eb8f6"
license: CC BY-SA 4.0
---

# TrimSlice

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

This guide shows how to install Gentoo on the TrimSlice. Consult the Embedded Handbook for [supplementary information about the TrimSlice hardware](https://wiki.gentoo.org/wiki/Embedded_Handbook/Boards/TrimSlice). Furthermore it's possible to [migrate the TrimSlice to mainline U-Boot](https://wiki.gentoo.org/wiki/Trimslice_U-Boot).

## Overview

Installing Gentoo in the TrimSlice is pretty simple if you're already a Gentoo user. You need to use an SD card, at least of 2 GB of size. If you are familiar with the Gentoo Linux installation process, there is not much different here.

## Requirements

To be able to install Gentoo, you'll need the following:

- An **x86**/**amd64** based PC with Gentoo and an SD card reader on it.
- A TrimSlice.
- One SD card (2 GB is enough).
- A network connection.

## Preparing to install your TrimSlice

### Overview

Before we start the installation process, we need to get/build a kernel for the TrimSlice.

### Emerging needed tools

For building the stuff needed to boot our TrimSlice, we need the following tools emerged on the host system where we're going to build them.

- [sys-devel/crossdev](https://packages.gentoo.org/packages/sys-devel/crossdev) - To create a crosscompiler.
- [dev-embedded/u-boot-tools](https://packages.gentoo.org/packages/dev-embedded/u-boot-tools) - to create a kernel image U-Boot can understand.

`root #``emerge --ask sys-devel/crossdev dev-embedded/u-boot-tools`
### Build a crosscompiler

Building a crosscompiler:

`root #``crossdev -S armv7a-unknown-linux-gnueabi`
## Obtaining/Building a kernel

For booting the TrimSlice we need a kernel. The vanilla kernel.org doesn't have all the drivers, etc needed for the TrimSlice yet. Therefore we'll use the kernel maintained by Compulab (the company behind TrimSlice).

Compulab's git tree is available at [http://gitorious.org/trimslice-kernel](http://gitorious.org/trimslice-kernel). On this guide we'll be using the `1.01-upstream` branch:

Extract and change to the kernel sources directory:

`root #``tar zxvf 1.01-upstream && cd trimslice-kernel-trimslice-kernel`
Configure the kernel:

`root #``make ARCH=arm trimslice_defconfig`
At this point make menuconfig could be ran to customize the kernel further.

Cross-compiling the kernel (replace `9` with the number of cores you have in the local computer):

`root #``make -j9 ARCH=arm CROSS_COMPILE=armv7a-unknown-linux-gnueabi- uImage`
Once it gets built we'll have a kernel image on arch/arm/boot/uImage.

## Creating the boot script

Since we're using an SD card to do our install, we'll have to create a boot script that U-Boot will read from the SD card upon startup so it knows where to load the kernel from and the specific parameters to boot from SD.

Create a file called boot.script with the following contents:

**`boot.script`**

Now we need to convert it to a format U-Boot can understand. Converting boot.script to boot.scr:

`root #``mkimage -A arm -T script -C none -n "TrimSlice boot script" -d boot.script boot.scr`
This will generate a file called boot.scr that contains the commands U-Boot will execute upon starting. We'll copy that file to the /boot directory.

## SD card setup

### Overview

We'll just create a partition that uses all the space on the card.

### Creating the partition table and formatting the SD card

First we need to create the partition table on the SD card. This will erase all the contents on the card, so make sure you don't have any important data on it.

`root #``fdisk /dev/mmcblk0````
Command (m for help): o
Command (m for help): n
Command action
   e   extended
   p   primary partition (1-4)
p
Partition number (1-4, default 1): 1
First sector (2048-15654911, default 2048): (Hit enter) 
Using default value 2048
Last sector, +sectors or +size{K,M,G} (2048-15654911, default 15654911): (Hit enter)
Using default value 15654911
Command (m for help): w
```
Now format the new partition:

`root #``mkfs.ext3 -i 4096 /dev/mmcblk0p1`
### Copying the kernel to the SD card

Now mount the first partition on the card and copy the needed files (the ones that we built before) to boot the TrimSlice. Copy the kernel to the SD card:

`root #````
mkdir /mnt/p1 ; mount /dev/mmcblk0p1 /mnt/p1 ; mkdir /mnt/p1/boot
```
`root #````
cp boot.scr /mnt/p1/boot
```
`root #````
cp arch/arm/boot/uImage /mnt/p1/boot
```
## Installing Gentoo

### Overview

The installation on this device is a bit different, and therefore easy, as we can't install Gentoo on it by booting an installation environment. For installing Gentoo (and any other distro, really) you need to put the SD card on your PC and prepare there the minimal installation.

What we'll have to do to setup our installation is:

1. Extract stage3 to the 1st partition of the SD card.
2. Extract portage snapshot (required to emerge things and ntp(see below)).
3. Setup /etc/fstab.
4. Setup root password.
5. Configure hostname and networking (optional, but recommended).
6. Enable SSH access (optional, but recommended).
7. Enable serial console access (optional, but recommended).

### Stages information

Here's some information about the stages.

- Architecture: arm
- Subarchitecture: armv7a
- `CHOST`: `armv7a-unknown-linux-gnueabi`
- Profile: default/linux/arm/10.0
- We'll be using the new EABI, also called gnueabi. That is `armel` on Debian.

Therefore, we need an armv7a-unknown-linux-gnueabi stage3 for best performance, available under the releases/arm/autobuilds directory in favorite Gentoo mirror.

Optionally you can also grab a Portage snapshot.

### Extracting a stage3

Extract the downloaded stage3:

`root #``tar xjpf stage3-armv7a-20101118.tar.bz2 -C /mnt/p1`
### Extracting a portage snapshot (optional)

Extracting the snapshot:

`root #``tar xjpf portage-latest.tar.bz2 -C /mnt/p1/usr`
### Setup fstab

Edit the /mnt/p1/etc/fstab file to look like this:

**`/mnt/p2/etc/fstab`**

### Setting the default root password

This is the most important part of the installation. As without the root password we won't be able to login!

For setting the password, we need to be able to run passwd. However that is not possible since our PC cannot run ARM binaries. Therefore we need to modify the file that contains the passwords (/etc/shadow) inside the chroot, so we can [set a default root password](https://wiki.gentoo.org/wiki/Setting_a_default_root_password). Since passwords may not be stored in plaintext use openssl to convert the password:

`user $``openssl passwd -6`
Password:
Verifying - Password:
$6$I9Q9AyTL$Z76H7wD8mT9JAyrp/vaYyFwyA5wRVN0tze8pvM.MqScC7BBm2PU7pLL0h5nSxueqUpYAlZTox4Ag2Dp5vchjJ0

In this example the string corresponds to the password "gentoo".

`root #``openssl passwd -1`
The resulting string needs to be placed in TARGET/etc/shadow. Fire up your editor:

`root #``nano -w /mnt/p1/etc/shadow`
Now replace the line beginning with `root:` with the line shown below, substituting SHADOW\_COMMAND\_OUTPUT with the string provided by the openssl command above.

**`/etc/shadow`**

For example, this line makes the password be "gentoo":

**`/etc/shadow`**

### Setup hostname and networking

Please read the [network configuration chapter](https://wiki.gentoo.org/index.php?title=Network_configuration_chapter&action=edit&redlink=1) of the ARM handbook to configure the network.

### Enabling SSH access (optional)

Add sshd to the startup of the system to access the TrimSlice using ssh. When using OpenRC, run:

`root #``ln -s /mnt/p1/etc/init.d/sshd /mnt/p1/etc/runlevels/default`
### Enabling serial console access (optional)

By default the ttyS0 port is configured at 9600 bps. However, almost all of the ARM devices run the serial port at 115200 bps. So this should be added to the /etc/inittab file:

`root #``nano -w /mnt/p1/etc/inittab`
Replace `9600` with `115200` on the `ttyS0` line:

**`/mnt/p1/etc/inittab`**

### Finishing the installation

Let's unmount the SD card:

`root #``umount /mnt/p1`
This is pretty much all of the installation. I'd highly recommend that you read all the recommendations of the handbook.

## Booting up our new system

Once you have the card ready, put it into the TrimSlice... and you should be able to boot it.

## After booting

One of the problems of the TrimSlice is that it doesn't save the date because it doesn't have a battery for the clock.

After logging into our new Gentoo on TrimSlice installation, I'd recommend setting a date and emerging [net-misc/ntp](https://packages.gentoo.org/packages/net-misc/ntp) to keep the clock up-to-date. Also it's recommended to put both ntp-client and ntpd to boot on startup, so you get a proper date setup.

However, keep in mind that NTP requires a network connection and a NTP server being reachable, either on the local network or on the Internet.

`root #``emerge --ask net-misc/ntp`
### OpenRC

Put both ntp-client and ntpd in the boot runlevel on startup:

`root #````
rc-update add ntpd default
```
`root #````
rc-update add ntp-client default
```
Start ntp-client and ntpd:

`root #````
/etc/init.d/ntp-client start
```
`root #````
/etc/init.d/ntpd start
```
## Accelerated graphics drivers

### Overview

The TrimSlice has a graphic chip that needs a binary driver provided by Nvidia for accelerated graphics. Those can be found on the overlay below.

### Tegra overlay

I've created an overlay for providing ebuilds for installing the accelerated graphics drivers and libraries that are available on the Nvidia L4T.

The overlay is available in the official Gentoo overlays repository: [http://git.overlays.gentoo.org/gitweb/?p=proj/tegra.git;a=summary](http://git.overlays.gentoo.org/gitweb/?p=proj/tegra.git;a=summary).

Please check the official documentation about [using overlays](https://wiki.gentoo.org/wiki/Project:Overlays/User_Guide).

### Emerging the accelerated graphics driver

Once you've added the overlay to your list of overlays, you can emerge the drivers:

`root #``emerge --ask --verbose tegra-drivers`
## External resources

## Special thanks

- [Trimslice.com](http://trimslice.com/) for providing me a TrimSlice devkit to document and support Gentoo on it.
- Siarhei Siamashka ([ssvb](https://wiki.gentoo.org/index.php?title=User:Ssvb&action=edit&redlink=1) ) for giving helpful hints.
