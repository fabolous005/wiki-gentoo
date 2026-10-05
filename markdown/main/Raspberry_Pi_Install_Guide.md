<!-- source: https://wiki.gentoo.org/wiki/Raspberry_Pi_Install_Guide | group: Gentoo Wiki (Main) | wiki-title: Raspberry Pi Install Guide -->
---
title: Raspberry Pi Install Guide
url: https://wiki.gentoo.org/wiki/Raspberry_Pi_Install_Guide
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-09-11"
fingerprint: "9261785b41c61226"
license: CC BY-SA 4.0
---

# Raspberry Pi Install Guide

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

## Overview

Having produced several arm64 Raspberry Pi install guides, first the the Pi3, then the Pi4, building on one another and the handbook, with the arrival of the Pi5, it's becoming a house of cards. A new approach is required.

This Pi install guide aims to cover a general method, rather than a step by step guide. The method will work for any Pi and it will only depend on the handbook for the generic Gentoo things. The method should work for the Pi6 and beyond.

No chrooting into an arm/arm64 environment will be required. It will be installed to a (micro)SD card, including enough setup to boot and login before the arm/arm64 environment is required.

In short, it's a Gentoo arm or arm64 stage3 on top of a Raspberry Pi Foundation binary kernel with some text files added to make it work. No target CPU code will be executed during the install.



### Hardware table

| Model | CPU | Architecture | Stage3 | 
|---|---|---|---|
| Raspberry Pi (Original) | BCM2708 | ARM | [ARMv6j stage 3](http://gentoo.org/downloads/#arm) | 
| Raspberry Pi Zero | BCM2708 | ARM | [ARMv6j stage 3](http://gentoo.org/downloads/#arm) | 
| Raspberry Pi Zero W | BCM2708 | ARM | [ARMv6j stage 3](http://gentoo.org/downloads/#arm) | 
| Raspberry Pi 2B Before Ver 1.2 | BCM2709 | ARM | [ARMv7a stage 3](http://gentoo.org/downloads/#arm) | 
| Raspberry Pi 2B Ver 1.2 and 1.3 | BCM2710 | ARM/ARM64 | [ARMv7a stage 3](http://gentoo.org/downloads/#arm) or [arm64 stage3](http://gentoo.org/downloads/#arm64) | 
| Raspberry Pi 3B | BCM2710 | ARM/ARM64 | [ARMv7a stage 3](http://gentoo.org/downloads/#arm) or [arm64 stage3](http://gentoo.org/downloads/#arm64) | 
| Raspberry Pi 3B+ | BCM2710 | ARM/ARM64 | [ARMv7a stage 3](http://gentoo.org/downloads/#arm) or [arm64 stage3](http://gentoo.org/downloads/#arm64) | 
| Raspberry Pi Zero 2 W | BCM2710 | ARM/ARM64 | [ARMv7a stage 3](http://gentoo.org/downloads/#arm) or [arm64 stage3](http://gentoo.org/downloads/#arm64) | 
| Raspberry Pi 4B | BCM2711 | ARM/ARM64 | [ARMv7a stage 3](http://gentoo.org/downloads/#arm) or [arm64 stage3](http://gentoo.org/downloads/#arm64) | 
| Raspberry Pi CM4 | BCM2711 | ARM/ARM64 | [ARMv7a stage 3](http://gentoo.org/downloads/#arm) or [arm64 stage3](http://gentoo.org/downloads/#arm64) | 
| Raspberry Pi 5 | BCM2712 | ARM/ARM64 | [ARMv7a stage 3](http://gentoo.org/downloads/#arm) or [arm64 stage3](http://gentoo.org/downloads/#arm64) | 
| Raspberry Pi CM5 | BCM2712 | ARM/ARM64 | [ARMv7a stage 3](http://gentoo.org/downloads/#arm) or [arm64 stage3](http://gentoo.org/downloads/#arm64) | 
| Raspberry Pi 6 | TBA | TBA |  | 

### Raspberry Pi Booting

The very first Pi can be approximated to a mobile phone chip with an ARM CPU grafted on.
[Raspberry\_Pi\_Install\_Guide/Pi Booting](https://wiki.gentoo.org/wiki/Raspberry_Pi_Install_Guide/Pi_Booting)

## High Level Steps

In handbook order

- [Preparing the disks](https://wiki.gentoo.org/wiki/Handbook:AMD64/Installation/Disks)
- [Installing the Gentoo installation files](https://wiki.gentoo.org/wiki/Handbook:AMD64/Installation/Stage)
- Installing the Raspberry Pi Foundation files
- [Configuring the system](https://wiki.gentoo.org/wiki/Handbook:AMD64/Installation/System)


The handbook uses a working Gentoo Install (the minimal ISO) to perform the install and requires that the host and target for the install are compatible. This guide assumes that the host and target are incompatible. No attempt is made to execute any target code on the install host.

## Prerequisites

- A Raspberry Pi and peripherals
- Target media for the install
- A Linux install to write the target media (Random live media will probably work)

## The detail

Extra steps to expose the Compute Module eMMC as USB storage before Preparing the disk is possible (only to install directly to eMMC).
[Raspberry Pi Install Guide/Exposing the eMMC](https://wiki.gentoo.org/wiki/Raspberry_Pi_Install_Guide/Exposing_the_eMMC)

### Preparing the disks

These are standard handbook, outside the chroot steps and have been moved to the [Raspberry Pi Install Guide/Preparing the disks](https://wiki.gentoo.org/wiki/Raspberry_Pi_Install_Guide/Preparing_the_disks) sub page.

### Installing the Gentoo installation files

Mount the newly created root filesystem. The traditional mount point is /mnt/gentoo, which will be used here.

`root #``mount /dev/sdi4 /mnt/gentoo``root #``cd /mnt/gentoo`
Choose the correct stage3 for your Pi from [stage 3 downloads](https://www.gentoo.org/downloads/) or the arm or arm64 sub pages with the help of the [Hardware table](https://wiki.gentoo.org/wiki/Raspberry_Pi_Install_Guide#Hardware_table) above.

Readers wanting to try MUSL are welcome to contribute.

Copy the link of your choice from [https://www.gentoo.org/downloads/](https://www.gentoo.org/downloads/) or one of its sub pages. Then wget it into /mnt/gentoo.
Do check the prompt.

This example uses the stage3-arm64-desktop-openrc stage3

The checks for validating the stage 3 tarball described in the handbook are optional and only serve to authenticate the image contents.

Untar the stage 3. If this is done incorrectly it can destroy your host install.

Do check that the present working directory is /mnt/gentoo

`/mnt/gentoo #``ls` lost+found  stage3-arm64-desktop-openrc-20231015T223200Z.tar.xz

There is no root filesystem hierarchy there until the next step is complete.

`/mnt/gentoo #``tar xpvf stage3-*.tar.xz --xattrs-include='*.*' --numeric-owner`
The "v" tar option writes filenames to the console, which slows things down. It can be omitted.

If all is well, there is a root filesystem hierarchy in /mnt/gentoo together with the stage3 than provided it.

`/mnt/gentoo #``ls`
bin   dev  home  lib64       media  opt   root  sbin                                                 sys  usr
boot  etc  lib   lost+found  mnt    proc  run   stage3-arm64-desktop-openrc-20231015T223200Z.tar.xz  tmp  var

### Installing the Raspberry Pi Foundation files

#### Fetch the Raspberry Pi Foundation files

Some workspace and access to boot is required, so mount both /dev/sdi1 and /dev/sdi3 in our growing Raspberry Pi root filesystem tree.

`/mnt/gentoo #``mount /dev/sdi1 /mnt/gentoo/boot``/mnt/gentoo #``mount /dev/sdi3 /mnt/gentoo/home`
The Pi /home can be used as workspace.

`/mnt/gentoo #``cd /mnt/gentoo/home`
Check that its empty

`/mnt/gentoo/home #``ls`
lost+found

Fetch the binary kernel and Pi firmware from github

This is all the Raspberry Pi Foundation binary code to support the entire family of Raspberry Pis. Even Pi5 support is included.

`/mnt/gentoo/home #``ls firmware/`
boot  documentation  extra  hardfp  modules  opt  README.md

For a 64 bit install, only boot and modules will be used.

#### Populate boot

Copy the content of boot to the vfat partition

`/mnt/gentoo/home #``cp -a firmware/boot/* /mnt/gentoo/boot/`
and verify that it worked

`/mnt/gentoo/home #``ls /mnt/gentoo/boot`
bcm2708-rpi-b.dtb	bcm2709-rpi-cm2.dtb	  bcm2711-rpi-400.dtb     bootcode.bin   fixup.dat        kernel.img        start_cd.elf
bcm2708-rpi-b-plus.dtb  bcm2710-rpi-2-b.dtb	  bcm2711-rpi-4-b.dtb     COPYING.linux  fixup_db.dat     LICENCE.broadcom  start_db.elf
bcm2708-rpi-b-rev1.dtb  bcm2710-rpi-3-b.dtb	  bcm2711-rpi-cm4.dtb     fixup4cd.dat   fixup_x.dat	  overlays          start.elf
bcm2708-rpi-cm.dtb	bcm2710-rpi-3-b-plus.dtb  bcm2711-rpi-cm4-io.dtb  fixup4.dat     kernel_2712.img  start4cd.elf      start_x.elf
bcm2708-rpi-zero.dtb    bcm2710-rpi-cm3.dtb	  bcm2711-rpi-cm4s.dtb    fixup4db.dat   kernel7.img	  start4db.elf
bcm2708-rpi-zero-w.dtb  bcm2710-rpi-zero-2.dtb    bcm2712-rpi-5-b.dtb     fixup4x.dat    kernel7l.img     start4.elf
bcm2709-rpi-2-b.dtb     bcm2710-rpi-zero-2-w.dtb  boot                    fixup_cd.dat   kernel8.img	  start4x.elf

#### Copy the kernel modules

Install the kernel modules

`/mnt/gentoo/home #` `cp -a firmware/modules /mnt/gentoo/lib/`
and verify

`/mnt/gentoo/home #``ls /mnt/gentoo/lib/modules/`
6.1.58+  6.1.58-v7+  6.1.58-v7l+  6.1.58-v8+  6.1.58-v8_16k+

Kernel versions will change with time but the suffixes are probably fixed.

#### Raspberry Pi 5 WiFi/Bluetooth Firmware

To use WIFI and bluetooth, firmware files need to be copied to /mnt/gentoo/lib/firmware folder.

##### WIFI

1\. Clone wifi firmware repository

2\. Create /mnt/gentoo/lib/firmware/brcm if it doesn't exist

`root #``mkdir -p /mnt/gentoo/lib/firmware/brcm`
3\. The wifi mode for raspberry pi 5 is **brcmfmc43455**, so we only need to copy files for brcmfmc43455.

`root #````
cp firmware-nonfree/debian/config/brcm80211/cypress/cyfmac43455-sdio-standard.bin /mnt/gentoo/lib/firmware/brcm/brcmfmac43455-sdio.bin
```
`root #````
cp firmware-nonfree/debian/config/brcm80211/cypress/cyfmac43455-sdio.clm_blob /mnt/gentoo/lib/firmware/brcm/brcmfmac43455-sdio.clm_blob
```
`root #``cp firmware-nonfree/debian/config/brcm80211/brcm/brcmfmac43455-sdio.txt /mnt/gentoo/lib/firmware/brcm/`
4\. When raspberry pi 5 boots, it looks for firmware names with model name, like raspberry,5-model-b, so we need to create symlinks for the firmware files, make sure you have following symlinks.

`root #``ls -l /mnt/gentoo/lib/firmware/brcm/`
-rw-r--r-- 1 root root 643651 Jan 21 12:20 brcmfmac43455-sdio.bin
-rw-r--r-- 1 root root   2676 Jan 21 12:18 brcmfmac43455-sdio.clm\_blob
lrwxrwxrwx 1 root root     22 Jan 21 12:23 brcmfmac43455-sdio.raspberrypi,5-model-b.bin -> brcmfmac43455-sdio.bin
lrwxrwxrwx 1 root root     27 Jan 21 12:23 brcmfmac43455-sdio.raspberrypi,5-model-b.clm\_blob -> brcmfmac43455-sdio.clm\_blob
lrwxrwxrwx 1 root root     22 Jan 21 12:24 brcmfmac43455-sdio.raspberrypi,5-model-b.txt -> brcmfmac43455-sdio.txt
-rw-r--r-- 1 root root   2074 Jan 21 12:19 brcmfmac43455-sdio.txt

##### Bluetooth

1\. Clone bluetooth firmware repository

2\. Create /mnt/gentoo/lib/firmware/brcm if it doesn't exist

`root #``mkdir -p /mnt/gentoo/lib/firmware/brcm`
3\. For bluetooth, only **BCM4345C0.hcd** is needed.

`root #``cp bluez-firmware/debian/firmware/broadcom/BCM4345C0.hcd /mnt/gentoo/lib/firmware/brcm/`
4\. Similarly, we need to create a symlink for raspberry pi 5.

`root #``ln -s /mnt/gentoo/lib/firmware/brcm/BCM4345C0.hcd /mnt/gentoo/lib/firmware/brcm/BCM4345C0.raspberrypi,5-model-b.hcd`
## Recap

The selected Gentoo stage3 is now installed on top of a universal Raspberry Pi Foundation set of kernels and GPU firmware. The Kernel and GPU firmware will work on any Pi as it is all there and what is required is auto detected at boot.

The stage3 is not so flexible.

This process will work for any Raspberry Pi provided the correct stage3 is selected.

## Minimal Configuration

This involves describing the install to the Pi, from the Pi's view of the world.

No matter how the install host saw the target SD card, the Pi will see it as /dev/mmcblk0. As the files below here will be written on the install host to be read and used by the target, references to the SD card become /dev/mmcblk0.

Some text files need to be created so that the Pi will boot.

`/mnt/gentoo/home``cd /mnt/gentoo`


### cmdline.txt

`/mnt/gentoo/``nano boot/cmdline.txt`
**`/mnt/gentoo/boot/cmdline.txt`**

**cmdline.txt**

### config.txt

config.txt is used to enable features, and if missing or empty will prevent a Pi5 from booting.

Documentation regarding config.txt options can be found on the [Raspberry Pi website](https://www.raspberrypi.com/documentation/computers/config_txt.html).

`/mnt/gentoo/``nano boot/config.txt`
**`/mnt/gentoo/boot/config.txt`**

**config.txt**

### fstab

`/mnt/gentoo/``nano etc/fstab`
**`/mnt/gentoo/etc/fstab`**

**fstab**

### Networking Information

Set the [hostname](https://wiki.gentoo.org/wiki/Handbook:AMD64/Installation/System#Hostname).

Its not possible to install dhcpcd yet but the Pi will use dhcp to get started anyway.

Delay the dhcpcd install until after the @world update.

### root password

Set the root password hash by editing the shadow file directly Replace the root line with the line shown below.

`/mnt/gentoo/``nano etc/shadow`
**`/mnt/gentoo/etc/shadow`**

**root password hash**

This sets the root password to **raspberry**.  Don't leave it like that.

### conf.d/keymaps

Skip this step if the default QWERTY US keymap works.

`/mnt/gentoo/``nano etc/conf.d/keymaps`
**`/mnt/gentoo/etc/conf.d/keymaps`**

**keyboard setting**

### configure sshd

Are you really not going to watch the console before the first login?

`/mnt/gentoo/``nano etc/ssh/sshd_config`
**`/mnt/gentoo/etc/ssh/sshd_config`**

**Allow password root logins**

Add the `PermitRootLogin yes` entry. Its a security hazard, revert that as soon as possible.  Adding a ssh key is preferred.

#### OpenRC

Start the sshd service at boot time by adding a symbolic link to the default runlevel.

`/mnt/gentoo/``cd /mnt/gentoo/etc/runlevels/default/``/mnt/gentoo/etc/runlevels/default``ln -s ../../init.d/sshd sshd`
#### Systemd

Start the sshd service at boot time by adding a symbolic link to the service.

`/mnt/gentoo/``cd /mnt/gentoo/etc/systemd/system/multi-user.target.wants``/mnt/gentoo/etc/systemd/system/multi-user.target.wants``ln -s ../../../../usr/lib/systemd/system/sshd.service sshd.service`
## Tidy up and Test in the Pi

`root #````
cd
```
`root #````
umount /mnt/gentoo/boot
```
`root #````
umount /mnt/gentoo/home
```
`root #``umount /mnt/gentoo`
Remove the drive from the install host. Connect to the Pi and power up.

## IMPORTANT After the First Boot

It a really bad idea to use a root password from the internet - Change it as soon as your Pi boots.

`root #``passwd`
and follow the on screen instructions.

Permitting a root password login over ssh is not much better. Use key based authentication or create a normal user with membership of the wheel group, then set up sudo. Key based ssh authentication everywhere is preferred.

Revert the `/etc/ssh/sshd_config` change as soon as possible.

### Setting portage up

Unless the system time is approximately correct, web site certificates will appear to be invalid.

Time will start at `Thu Jan  1 00:00:00 -00 1970` every power on.

You can set the system time with

`root #``date -s "YYYY-MM-DD HH:MM"`
Afterwards, to configure all the repositories for portage you can run

`root #``emerge-webrsync`
### Making the system time monotonic

The default `hwclock` is not useful without a battery backed RTC, as the time will reset upon every reboot. This has the ability to break many packages and build systems as the time would stop flowing in a single direction.

To make clock be monotonic the following steps need to be taken

##### OpenRC

Remove `hwclock` from the default runlevel and replace it with `swclock`. `swclock` will ensure that time is monotonic by saving the time at shutdown and restoring it at power up.

`root #``rc-update add swclock boot`
* service swclock added to runlevel boot

`root #``rc-update del hwclock boot`
* service hwclock removed from runlevel boot

##### systemd

There is no `swclock` for systemd. The recommendation is to just install NTP service and run it.
Either you can install and enable it with

`root #``emerge -v net-misc/openntpd``root #``systemctl enable ntpd.service``root #``systemctl start ntpd.service`
Refer to [NTP for systemd](https://wiki.gentoo.org/wiki/NTP) for the details.

### CPU governor

The Raspberry Pi Foundation binary kernel is built to use the powersave CPU governor by default. That keeps the CPU at the lowest possible clock speed at all times. That's a bad choice for Gentoo. Changing it and actually making use of the change, requires a CPU heatsink since the Pi firmware looks after CPU thermal throttling, not the kernel.

`root #``cat /sys/devices/system/cpu/cpu0/cpufreq/scaling_governor`
powersave

Make the file `/etc/local.d/cpu_gov.start` to set the schedutil CPU governor.

`root #``nano /etc/local.d/cpu_gov.start`
**`/etc/local.d/cpu_gov.start`**

**Set schedutil as CPU governor**

make it an executable file.

`root #``chmod +x /etc/local.d/cpu_gov.start`
### Clear the install leftovers

The stage 3 file in / and the firmware in /home are no longer required and may be removed.

### Fix inittab

The stage3 tries to spawn agetty on the serial port at /dev/ttyAMA0 but the serial port is not set up or needed here. Console users will see repeated postings "INIT: Id "f0" respawning too fast: disabled for 5 minutes" every 5 minutes. To stop the repeated postings, disable agetty on the port by commenting out the last line of /etc/inittab and marking your edit as follows

`localhost #``nano /etc/inittab`
**`/etc/inittab`**

**inittab**

### CPU Temperature and clock monitoring

`localhost #````
cat /sys/class/thermal/thermal_zone0/temp
```
60374

Temp in milliCelcius or 60.374 Deg C.

`localhost #````
cat /sys/devices/system/cpu/cpu0/cpufreq/scaling_cur_freq
```
1500000

CPU clock in kHz. or 1.5GHz

## Everything skipped in the handbook

Not quite everything as some steps need to be omitted by design and others have already been accomplished by other means.

Until NTP is installed and configured, at every boot, time will be set from swclock, that is, the time at the last power off. Correct operation of https:// requires reasonably accurate time, so use date -s to set the time at every boot. This avoids "Certificate not valid errors" from the web.

The ordering is not the same as the handbook as some steps require packages to be installed and used. That requires a working emerge command. In turn that requires the ::gentoo repo to be installed.

### Configuring compile options

Setting `COMMON_FLAGS` requires a working portage and is covered below

`COMMON_FLAGS="-march=native ...` should be avoided on arm and arm64 systems.

### Chrooting

This step has been avoided by design.

### Gentoo ebuild repository

Fetch the [::gentoo repo](https://wiki.gentoo.org/wiki/Handbook:AMD64/Installation/Base#Installing_a_Gentoo_ebuild_repository_snapshot_from_the_web) snapshot from the web and update it.

### Reading news items

Continue with [reading the news](https://wiki.gentoo.org/wiki/Handbook:AMD64/Installation/Base#Reading_news_items).

so it really is important that reading the news is a part of regular updates.

### Choosing the right profile

The stage3 will have a profile already set. Follow [choosing a profile](https://wiki.gentoo.org/wiki/Handbook:AMD64/Installation/Base#Choosing_the_right_profile) to review and change it.

The profile will have /arm/ in its name for 32 bit installs or /arm64/ for 64 bit installs, not amd64 as illustrated. Arm64 does not support multilib, so that is not an option

### Copy DNS info

The Pi is using the default DHCP to obtain DNS information so this step is not required unless networking is reconfigured later.

### Mounting the necessary filesystems

Not Required. This step is preparation for chrooting.

### Entering the new environment

Not Required. This step is entering the chroot.

### Preparing for a bootloader

Already complete. The Pi has booted.

### Configure locales

Follow [configure locales](https://wiki.gentoo.org/wiki/Handbook:AMD64/Installation/Base#Configure_locales) to configure and select the system locales.

### Selecting mirrors

Copy `GENTOO_MIRRORS` from make.conf on the install host, or follow [Selecting mirrors](https://wiki.gentoo.org/wiki/Handbook:AMD64/Installation/Base#Optional:_Selecting_mirrors) on the Pi.

Follow [configuring the Gentoo ebuild repository](https://wiki.gentoo.org/wiki/Handbook:AMD64/Installation/Base#Gentoo_ebuild_repository).

### Timezone

Follow [Setting the timezone](https://wiki.gentoo.org/wiki/Handbook:AMD64/Installation/Base#Timezone).

### Updating the @world set

The handbook lists [updating @world](https://wiki.gentoo.org/wiki/Handbook:AMD64/Installation/Base#Updating_the_.40world_set) next. That can cause rebuilds due to changed USE settings later. Users building on the Pi may choose to configure the [USE settings](https://wiki.gentoo.org/wiki/Handbook:AMD64/Installation/Base#Configuring_the_USE_variable) first, as this may save some rebuilds.

The `VIDEO_CARDS` variable is internally to portage, a USE flag too. Users intending to install a GUI set [VIDEO\_CARDS](https://wiki.gentoo.org/wiki/Handbook:AMD64/Installation/Base#VIDEO_CARDS) now.

Only fbdev, v3d and vc4 are useful on a Pi.

The tool cited in [CPU\_FLAGS](https://wiki.gentoo.org/wiki/Handbook:AMD64/Installation/Base#CPU_FLAGS_.2A) will emit CPU\_FLAGS\_ARM. That's used on both arm and arm64.

`root #``emerge -av app-misc/resolve-march-native`
Then run it. A Pi Zero W reports.

`root #``resolve-march-native`
-march=armv6kz+fp

A Pi 3 reports.

`root #``resolve-march-native`
-mcpu=cortex-a53+crc

A Pi 4 reports.

`root #``resolve-march-native`
-mcpu=cortex-a72+crc

A Pi 5 reports.

`root #``resolve-march-native`
-mcpu=cortex-a76+crc+crypto


Use the output in `COMMON_FLAGS`. Add `-OX -pipe` where X is the selected optimisation level. `-O3` should probably be avoided on RAM constrained systems, like the Pi.

Set -mtune=\<CPU without the optional extras>

e.g. `COMMON_FLAGS="-mcpu=cortex-a76+crc+crypto -mtune=cortex-a76 -O2 -pipe"` for a Pi5.

With `USE`, `VIDEO_CARDS`, `COMMON_FLAGS`, and `CPU_FLAGS_ARM` all set, its time to actually update the @world set ... or maybe not.

Users wishing to run the @world update remotely will need to install [app-misc/screen](https://packages.gentoo.org/packages/app-misc/screen) or  [app-misc/tmux](https://packages.gentoo.org/packages/app-misc/tmux) first.

`root #``emerge -uDUav --jobs=2 --keep-going @world`
Portage will warn

which is expected as no kernel source tree is installed.

ebuilds are unable to run kernel configuration checks.

### dhcpcd

Follow [Network settings](https://wiki.gentoo.org/wiki/Handbook:AMD64/Installation/System#Network).

### Configuring the Linux kernel

Not required as this guide installs the Raspberry Pi Foundation binary kernel. There are no kernel sources installed to configure.

At the time of writing, only the Pi 4 can use the upstream kernel. Pi 5 is being upstreamed, so will be able to at some time in the future.

The other Pis depend on patches that will not (or cannot) be upstreamed.

Users intent on building a kernel should follow [Raspberry Pi official documentation](https://www.raspberrypi.com/documentation/computers/linux_kernel.html). Building a kernel on a Pi takes considerable time, therefore setting up a [crossdev toolchain](https://wiki.gentoo.org/wiki/Crossdev) and cross-compiling the kernel will save time.

For reference on a Pi 5, build times for a \`bcm2712\_defconfig\` kernel can take from 40 - 60 minutes, depending on other tasks the Pi is also running. Older Pi models take considerably longer which makes the use of cross-compiling more time efficient.

#### Gentoo Binary Kernels

These are expected to work an Pi4 and Pi5 as 'upstreaming' the patches and Pi 4 and Pi5 drivers is complete. (Not tested yet) However, while the kernel is precompiled the required initrd/initramfs is not. This makes them unsuitable for a first boot while following this guide.

### Filesystem information

/etc/fstab is already complete.

- Networking information

### System information

Follow [System information](https://wiki.gentoo.org/wiki/Handbook:AMD64/Installation/System#System_information)

### Installing system tools

Follow [Installing system tools](https://wiki.gentoo.org/wiki/Handbook:AMD64/Installation/Tools).

### Time synchronization - Important with no RTC

Follow [Time synchronization](https://wiki.gentoo.org/wiki/Handbook:AMD64/Installation/Tools#Time_synchronization).

### Filesystem tools

Follow [Filesystem tools](https://wiki.gentoo.org/wiki/Handbook:AMD64/Installation/Tools#Filesystem_tools). Both sys-fs/e2fsprogs and sys-fs/dosfstools are required.

Choices for the root filesystem are limited by the filesystems built into the Raspberry Pi Foundation binary kernel.

Readers that can build their own kernel or kernel and initrd before the first boot, can use whatever root filesystem they choose.

### Networking tools

Follow [Networking tools](https://wiki.gentoo.org/wiki/Handbook:AMD64/Installation/Tools#Networking_tools).

Wireless networking tools are required but not sufficient to use WiFi. The kernel drivers are present but the firmware is not.

### Configuring the bootloader

The Pi uses /boot/config.txt and /boot/cmdline.txt

Both are read by the GPU code at boot time. Reboot to test new configurations

### Finalizing

Follow [Finalizing](https://wiki.gentoo.org/wiki/Handbook:AMD64/Installation/Finalizing).

## Further Reading

### Cross compiling

Once a cross toolchain is installed, pure cross compiling then installing the resulting binary packages is only a small step away.

It's not a silver bullet. Some packages have broken build systems, so that they are not cross compile aware. Others are cross compile hostile, as they build code for the target during the build then continue by attempting to execute it on the build host.

See the [Crossdev](https://wiki.gentoo.org/wiki/Crossdev) guide.

### QEMU chroot

A [QEMU chroot](https://wiki.gentoo.org/wiki/Embedded_Handbook/General/Compiling_with_qemu_user_chroot) allows the build host to emulate (at the register level) the target CPU. It can bring the build hosts RAM, HDD space and CPU cores to bear but at reduced speed, due to the requirement to emulate the target CPU in software.

Its also possible to use cross distcc running on the host (outside the QEMU chroot) from inside the chroot. This exchanges the host CPU cycles required to emulate gcc with host CPU cycles for network emulation.

### Cross distcc

That's ordinary [distcc](https://wiki.gentoo.org/wiki/Distcc) with a [cross compiler](https://wiki.gentoo.org/wiki/Cross_build_environment) on the helpers. See also  [distcc cross compiling](https://wiki.gentoo.org/wiki/Distcc/Cross-Compiling).

Only compiling is distributed. The Pi still performs the configure and link steps. Not everything can be distributed.

Do set up and test standard distcc before adding cross compiler(s). It will make debug easier.

Keeping versions of gcc in sync is a manual process which distcc cannot check.

## Random Hints, Tips and Did You Know

The Random Hints, Tips and Did You Know items have moved the [Raspberry\_Pi\_Install\_Guide/HintsandTips](https://wiki.gentoo.org/wiki/Raspberry_Pi_Install_Guide/HintsandTips) subpage.

Includes:

- Default kernel configuration
- Enable discard over USB
- GPIO
- Unreliable USB Attached SCSI
- www-client/chromium
- Widevine DRM
- Zram

## Wifi

There is nothing to track down. The firmware is in [sys-kernel/linux-firmware](https://packages.gentoo.org/packages/sys-kernel/linux-firmware). As always, a method of dealing with the wifi encryption is required.

## Raspberry Pi 3

TODO Include the [Pi3 specific parts](https://wiki.gentoo.org/wiki/Raspberry_Pi_3_64_bit_Install) here, then deprecate that page.

### Bluetooth

The defaults tell

\[   11.495833\] Bluetooth: hci0: BCM: firmware Patch file not found, tried:
\[   11.495864\] Bluetooth: hci0: BCM: 'brcm/BCM4345C0.raspberrypi,3-model-b-plus.hcd'
\[   11.495875\] Bluetooth: hci0: BCM: 'brcm/BCM4345C0.hcd'
\[   11.495884\] Bluetooth: hci0: BCM: 'brcm/BCM.raspberrypi,3-model-b-plus.hcd'
\[   11.495894\] Bluetooth: hci0: BCM: 'brcm/BCM.hcd'

BCM4345C0.hcd is available from \[[Debian](https://salsa.debian.org/bluetooth-team/bluez-firmware/-/blob/debian/sid/debian/firmware/broadcom/BCM4345C0.hcd?ref_type=heads%7C)\]

## Raspberry Pi 4

TODO
Include the [Pi4 specific](https://wiki.gentoo.org/wiki/Raspberry_Pi4_64_Bit_Install) parts here, then deprecate that page.

## Raspberry Pi 5

The Raspberry Pi 5 specific items have moved the [Raspberry\_Pi\_Install\_Guide/Pi5](https://wiki.gentoo.org/wiki/Raspberry_Pi_Install_Guide/Pi5) subpage.
