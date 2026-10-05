<!-- source: https://wiki.gentoo.org/wiki/DreamPlug | group: Gentoo Wiki (Main) | wiki-title: DreamPlug -->
---
title: DreamPlug
url: https://wiki.gentoo.org/wiki/DreamPlug
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2022-05-04"
fingerprint: "26198a5d04eab587"
license: CC BY-SA 4.0
---

# DreamPlug

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

## Hardware

| Technical specifications of the DreamPlug |  | 
|---|---|
| System on Chip | Kirkwood MV88F6281-A1 | 
| CPU | Feroceon 88FR131 rev 1 (v5l) a.k.a. Marvel Sheeva 1.2 GHz (ARMv5TE) | 
| RAM | 512 MiB 16bit DDR2-800 MHz | 
| Internal storage | 4 GiB on board micro-SD | 
| Internet connectivity |  | 
| External storage and connectivity |  | 
| Audio interfaces Headphone (analogue) out |  | 
| Power suppy | 5V3A DC power supply (yes, it consumes \<5 W!) | 
| Physical dimensions | 120mm (L) x 90mm (W) x 30mm (H) | 

## Disk partitions

While unless you have a good reason you should be mounting partitions using UUID or Label, the existing disk partitions are as follows:

- /dev/sda is the internal SD card and separated so sda1 is a \~100 MiB FAT16 partition with kernel uImages and sda2 is a \~4 GiB ext3 partition carrying the stock (in my case Debian) OS.
- /dev/sdb is the external SD card slot
- /dev/sdc is the first external disk via USB or eSATA
- The rest follows in the same logic.

## U-boot

A clean u-boot environment that first boots from the external USB and has an option to boot from the internal SD (check yours with printenv).

To switch just change the bootcmd to include either gentoo\_bootcmd (for USB) or stock\_bootcmd (for internal SD).

bootdelay=3
baudrate=115200
ethact=egiga0
ethaddr=F0:AD:4E:00:B0:8E
eth1addr=F0:AD:4E:00:B0:8F
clear\_kernel\_in\_mem=echo Purging kernel in memory; mw 0x6400000 0x0 0x300000
ipaddr=192.168.1.103
serverip=192.168.1.111
x\_bootcmd\_usb=usb start
x\_bootcmd\_ethernet=192.168.1.1
stock\_bootargs=root=/dev/sda2 rootdelay=10 console=ttyS0,115200
gentoo\_bootargs=root=/dev/sdc2 rootdelay=10 console=ttyS0,115200
stock\_load\_kernel=fatload usb 0 0x6400000 uImage
stock\_bootcmd=usb start; fatload usb 0 0x6400000 uImage; setenv bootargs root=/dev/sda2 rootdelay=10 console=ttyS0,115200; bootm 0x6400000;
gentoo\_bootcmd=usb start; fatload usb 0 0x6400000 uImage; setenv bootargs root=/dev/sdc2 rootdelay=10 console=ttyS0,115200; bootm 0x6400000;
bootcmd=clear\_kernel\_in\_mem; run gentoo\_bootcmd
stdin=serial
stdout=serial
stderr=serial
Environment size: 838/4092 bytes

Don’t forget to \`saveenv\` if you want to keep the environment after the reboot.

## Kernel

### Roll your own

The stock version of U-Boot that ships with the DreamPlug does not pass a device tree as required by newer kernels. Flashing a newer version of U-Boot is an option but due to the way the hardware initializes, this is extremely tedious to do, if not almost impossible. On [Freedombox wiki](http://www.freedomboxfoundation.org/ubootUpgradeInstructions/index.en.html) there are probably the most up-to-date uBoot images for DreamPlug and instructions.  [This poor user](https://wiki.gentoo.org/wiki/User:Chewi) tried unsuccessfully for two days. Alternatively, newer kernels (3.2+) support appending the device tree to the kernel image manually. Give yourself an easy life with the following steps.

#### Manually appending the device tree

This method is tested to work on 3.6 kernels. [This other poor user](https://wiki.gentoo.org/index.php?title=User:Hook&action=edit&redlink=1) failed at getting it to work on 3.5, 3.6 worked as a charm though.

1. `emerge dev-embedded/u-boot-tools`
2. Configure and build the kernel as usual with CONFIG\_ARM\_APPENDED\_DTB enabled.
  1. `make kirkwood-dreamplug.dtb`
  2. `cat $dtb_file_from_the_output_of_previous_command >> arch/arm/boot/zImage`
  3. `make uImage`
3. Copy arch/arm/boot/uImage to the device. Don't forget the modules!

#### Cross-compiling

The DreamPlug is not very fast. Save some time and build the kernel on another machine. Only a stage 1 toolchain is required for building a kernel.

1. `emerge sys-devel/crossdev`
2. `crossdev -s1 -t arm-none-eabi`
3. Append **ARCH=arm CROSS\_COMPILE=arm-none-eabi-** to all invocations of make.

#### Built-in vs modular drivers

Be aware that building the SATA driver (sata\_mv) into the kernel will result in any drives attached to the DreamPlug at boot time taking priority over any SD cards. In other words, /dev/sda will be a SATA drive, not an SD card. This will most likely cause a boot failure. The simplest option is to build the driver as a module. Another option is to use an initrd. If neither of these appeal, there is a third option as of 3.7. You can now specify the root device in the form **root=PARTUUID=XXXXXXXX-XX** with an NT disk signature and partition number. This has been verified to work on the DreamPlug.

#### Kernel configuration

A minimal(ish) kernel configuration for 3.6.5 can be found [here](http://dev.gentooexperimental.org/~chewi/dreamplug-kernel-config.txt). Only DreamPlug hardware drivers have been enabled, except for wireless and Bluetooth, which have been left disabled for security reasons.

### Pre-built images

You can still use a pre-built kernel image, like the ones available on [PlugComputer Forums](http://www.plugcomputer.org/plugforum/index.php?board=2.0) …but where's the fun in that? ;)
