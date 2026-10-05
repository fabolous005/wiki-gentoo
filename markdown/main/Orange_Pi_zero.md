<!-- source: https://wiki.gentoo.org/wiki/Orange_Pi_zero | group: Gentoo Wiki (Main) | wiki-title: Orange Pi zero -->
---
title: Orange Pi zero
url: https://wiki.gentoo.org/wiki/Orange_Pi_zero
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2025-08-21"
fingerprint: a73dc0e10e062bd
license: CC BY-SA 4.0
---

# Orange Pi zero

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

This guide covers a basic bootstrapping for the Orange Pi Zero.

## Orange pi zero quick install

### Download

[orangpizero](http://www.orangepi.cn/) product choice Orange Pi Zero (because cn page new than en).

Download the Ubuntu\_Desktop 2018-02-01 for orange pi zero img.

dd the ubuntu image file to an SD card.

### Backup modules firmware

`root #````
mount /dev/mmcblk0p2 /mnt
```
`root #````
cp -r /mnt/lib/firmware /home/username
```
`root #````
cp -r /mnt/lib/modules  /home/username
```
`root #````
mkdir etc
```
`root #````
cp -r /mnt/etc/firmware  /home/username/etc       ##this firmware for wifi bin file
```
`root #````
umount /dev/mmcblk0p2
```
`root #````
fdisk /dev/mmcblk0
```
d del 2 disk partition n add 2 partition

`root #````
resize2fs /dev/mmcblk0p2
```
`root #````
mkfs.ext4 /dev/mmcblk0p2
```
### Mount orangpi zero sd to mnt

`root #````
mkdir /mnt/gentoo
```
`root #````
mount /dev/mmcblk0p2 /mnt/gentoo
```
`root #````
mkdir /mnt/gentoo/boot
```
`root #````
mount /dev/mmcblk0p1 /mnt/gentoo/boot
```
### Down and install armv7a stage3 and portage

`root #````
cd /mnt/gentoo
```
`root #``wget -c` [http://distfiles.gentoo.org/releases/arm/autobuilds/20161129/stage3-armv7a-20161129.tar.bz2](http://distfiles.gentoo.org/releases/arm/autobuilds/20161129/stage3-armv7a-20161129.tar.bz2)
`root #````
tar xvpf stage3-armv7a-20161129.tar.bz2 -C /mnt/gentoo
```
`root #````
tar xjf portage-latest.tar.bz2 -C /mnt/gentoo/usr 
```
### Copy modules firmware to Gentoo

`root #````
cp /home/username/firmware /mnt/gentoo/lib
```
`root #````
cp /home/username/modules  /mnt/gentoo/lib
```
`root #````
cp /home/username/etc/firmware /mnt/gentoo/etc
```
### Edit fstab

`root #````
nano /mnt/gentoo/etc/fstab  
```
**`/etc/fstab`**

**Example**

### Change root passwd

`root #````
sed -i 's/^root:.*/root::::::::/' /mnt/gentoo/etc/shadow
```
### Start wifi insmod wifi driver

`root #````
cd /etc/init.d/
```
`root #````
cp net.lo net.eth0
```
`root #````
cp net.lo net.wlan0
```
`root #````
nano /etc/local.d/insmod.start
```
**`/etc/local.d/insmod.start`**

See the [Wireless section](https://wiki.gentoo.org/wiki/Handbook:AMD64/Networking/Wireless) of the Gentoo Handbook for more information.

Now put the SD card in the Orange PI Zero. Power requirements are 2 Amps. Wifi requires 3.3v.
