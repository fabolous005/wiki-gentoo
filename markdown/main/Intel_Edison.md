<!-- source: https://wiki.gentoo.org/wiki/Intel_Edison | group: Gentoo Wiki (Main) | wiki-title: Intel Edison -->
---
title: Intel Edison
url: https://wiki.gentoo.org/wiki/Intel_Edison
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2024-04-06"
fingerprint: "21d965a95227973"
license: CC BY-SA 4.0
---

# Intel Edison

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

The **Intel Edison** is a tiny computer-on-module offered by Intel as a development system for wearable devices and Internet of Things devices.

![Intel-Edison.png](https://wiki.gentoo.org/images/thumb/8/88/Intel-Edison.png/200px-Intel-Edison.png)

## Gentoo for Edison

### License

U-boot: GPL-2+
[https://github.com/01org/edison-u-boot](https://github.com/01org/edison-u-boot)

### Pre-made image

[Gentoo-edison-170104.tar.gz](http://public.aliceinwire.net/edison/Gentoo-edison-170104.tar.gz) md5sum: 3f956280ba52de26fe95705e884e83c4

### Image password

User: root

Password: edison

### Features list

- Working WiFi
- Portage
- Squashfs Gentoo repository
- [specification](https://wiki.gentoo.org/wiki/Intel_Edison/specs)
- [Performance](https://wiki.gentoo.org#Performance)

### Image install

Download the Gentoo image on your local PC:

`root #````
tar -xvzf Gentoo-edison-*.tar.gz
```
`root #````
cd GentootoFlash
```
Connect and flash the Intel Edison.

This will delete all your previous data:

`root #````
./flashall.sh
```
#### Configure Wifi

symlink wlan0

`root #````
cd /etc/init.d/
```
`root #````
ln -s net.lo net.wlan0
```
`root #````
rc-config add net.wlan0 default
```
Create /etc/conf.d/net file:

**`/etc/conf.d/net`**

### Features

#### Cross compile preparation

Setup:

`root #``emerge --ask crossdev``root #``crossdev i686-pc-linux-gnu`
Copy package to Intel Edison:

`root #``scp -r /usr/i686-pc-linux-gnu/packages root@192.168.0.102:/var/cache/packages/`
Install in Intel Edison:

`root #``emerge --ask --oneshot --usepkgonly <package atom>` #### Squashfs Gentoo repository

Mount squashfs or use sd-card:

`root #````
mount /portage.squash /var/db/repos/gentoo/
```
#### Kernel multi-boot

Increase the Edison boot partition:

`root #````
mount /dev/mmcblk0p7 /mnt/boot/
```
`root #````
mkdir /tmp/boot
```
`root #````
mv /mnt/boot/* /tmp/boot
```
`root #````
umount /mnt/boot
```
`root #````
mkfs.vfat /dev/mmcblk0p7
```
`root #````
mount /mnt/boot
```
`root #````
cp /tmp/boot/* /boot
```
Add a new kernel in the /mnt/boot directory. For example vmlinuz\_01 and vmlinuz\_02:

multi boot with u-boot for start vmlinuz\_01

```
boot > setenv load_kernel fatload mmc 0:7 ${loadaddr} vmlinuz_01
```
```
boot > boot
```
#### Performance

### Install from source

## Support

For support contact:  [Arisu Tachibana (Alicef)](https://wiki.gentoo.org/wiki/User:Alicef)
