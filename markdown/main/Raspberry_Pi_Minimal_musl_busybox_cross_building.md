<!-- source: https://wiki.gentoo.org/wiki/Raspberry_Pi/Minimal_musl%2Bbusybox_cross_building | group: Gentoo Wiki (Main) | wiki-title: Raspberry Pi/Minimal musl+busybox cross building -->
---
title: Raspberry Pi/Minimal musl+busybox cross building
url: https://wiki.gentoo.org/wiki/Raspberry_Pi/Minimal_musl%2Bbusybox_cross_building
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2017-12-16"
tags: ['https://github.com/armtux/gentoo-rpi3b-radio/releases/tag/0.1-beta1']
fingerprint: "13051e7b75cea992"
license: CC BY-SA 4.0
---

# Raspberry Pi/Minimal musl+busybox cross building

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

## Raspberry Pi 1 (A/B/A+/B+/zero/zero w) musl/busybox environment.

Build your own minimal system using libressl, musl-libc and busybox.

This entire process was done within an official gentoo-hardened stage3 chroot with USE="libressl", updated to ACCEPT\_KEYWORDS="\~amd64" (if you don't want to do this, set \~amd64 keyword individually where needed to obtain the same results from instructions below). Follow the Gentoo Handbook if you haven't already. Please read everything twice before trying - that's recommended when first installing Gentoo because it really helps! If you find mistakes below, ask for help in the #gentoo-arm or #gentoo-hardened channels on Libera.Chat irc.

### Make sure you have the following tools installed in your host system.

`root #``emerge -av bc eselect-repository squashfs-tools`
### Install crossdev.

`root #``emerge -av crossdev`
### Build the cross-toolchain for a Raspberry Pi 1 target.

`root #``export CHOST=armv6j-hardfloat-linux-musleabi``root #``crossdev -t armv6j-hardfloat-linux-musleabi``root #``nano -w /etc/portage/make.conf``root #``nano -w /etc/portage/make.conf`
#### Re-run the crossdev command until it completes successfully...

When done, re-edit /etc/portage/make.conf and revert temporary changes.

`root #``nano -w /etc/portage/make.conf`
Add -vtv and -sanitize to cross-gcc portage configuration; now that crossdev is finished, a world update should not overwrite the portage configuration files.

`root #``nano -w /etc/portage/package.use/cross-armv6j-hardfloat-linux-musleabi`
### Add the Gentoo musl overlay

`root #``eselect repository enable musl`
#### Disable the musl overlay in host portage configuration.

`root #``eselect repository disable musl`
#### Add the musl and your local overlays to target portage configuration.

`root #``echo 'PORTDIR_OVERLAY="/var/db/repos/musl /usr/local/portage"' >> /usr/armv6j-hardfloat-linux-musleabi/etc/portage/make.conf`
### Edit the cross make.conf to your liking.

`root #``nano -w /usr/armv6j-hardfloat-linux-musleabi/etc/portage/make.conf`
### Set the correct cross-environment make.profile

Replace the cross-environment's make.profile with the directory right before the hardened/musl armv7a profile.

`root #``rm /usr/armv6j-hardfloat-linux-musleabi/etc/portage/make.profile``root #``ln -s /usr/portage/profiles/default/linux/musl/arm /usr/armv6j-hardfloat-linux-musleabi/etc/portage/make.profile`
### Replace linux-headers with raspberry pi kernel headers.

`root #``mkdir -p /usr/armv6j-hardfloat-linux-musleabi/usr/src``root #``cd /usr/armv6j-hardfloat-linux-musleabi/usr/src``root #``git clone --depth=1 --branch=rpi-4.14.y https://github.com/raspberrypi/linux`
#### Backup the downloaded kernel sources.

`root #``cp -r linux rpi-4.14.y``root #``cd linux``root #``ARCH=arm CROSS_COMPILE=armv6j-hardfloat-linux-musleabi- make headers_install INSTALL_HDR_PATH=/usr/armv6j-hardfloat-linux-musleabi/usr`
### Time for the package salad buffet, but remember: this system is on a diet!

Check what the system would usually install if running emerge world, put it in a text file and remove the packages you don't want/need/are replacing with busybox, leaving only a very minimal, but functional list comprising of the customized stage3 install.

`root #``armv6j-hardfloat-linux-musleabi-emerge -ep @world > /root/armv6j-world``root #``nano -w /root/armv6j-world``root #``nano -w /usr/armv6j-hardfloat-linux-musleabi/etc/portage/profile/package.provided`
Here is an example of a slimmed down list of packages that were kept. It was formatted for an upcoming emerge command which causes 33 packages to install.

**`/root/armv6j-world`**

**Example package list.**

**`/usr/armv6j-hardfloat-linux-musleabi/etc/portage/profile/package.provided`**

**Here is an example package.provided file.**

Now, set use flags for packages you are going to be installing. Remember, it is meant to be tiny, and it'll all be running in the Raspberry Pi's RAM, so USE="-static make-symlinks savedconfig syslog" for busybox.

`root #``nano -w /usr/armv6j-hardfloat-linux-musleabi/etc/portage/package.use/world`
**`/usr/armv6j-hardfloat-linux-musleabi/etc/portage/package.use/world`**

**Here is an example of minimizing use flags.**

Configure busybox and choose the tools wanted/needed in order to have a fully bootable/usable system. Provided at the end of this document will be known working busybox configuration examples to base oneself on.

`root #``emerge --fetchonly busybox``root #``mkdir -p /root/work/busybox``root #``cd /root/work/busybox``root #``tar xjf /usr/portage/distfiles/busybox-1.27.2.tar.bz2``root #``cd busybox-1.27.2``root #``make menuconfig`
Save the busybox .config and place it in target portage configuration.

`root #``mkdir -p /usr/armv6j-hardfloat-linux-musleabi/etc/portage/savedconfig/sys-apps``root #``cp .config /usr/armv6j-hardfloat-linux-musleabi/etc/portage/savedconfig/sys-apps/busybox`
#### Start by rebuilding musl-libc first, then build the base system.

`root #``mkdir -p /usr/local/portage/sys-libs``root #``cp -a /usr/portage/eclass /usr/local/portage/eclass``root #``cp -a /usr/portage/sys-libs/musl /usr/local/portage/sys-libs/musl``root #``nano -w /usr/local/portage/sys-libs/musl/musl-1.1.18.ebuild``root #``ebuild /usr/local/portage/sys-libs/musl/musl-1.1.18.ebuild digest``root #``armv6j-hardfloat-linux-musleabi-emerge -av sys-libs/musl`
Now, emerge the packages from the minimal list. If you find errors, please make bug reports and/or see if you can find patches for your issues and submit them in your bug reports.

`root #``mkdir -p /usr/armv6j-hardfloat-linux-musleabi/etc/portage/env /usr/armv6j-hardfloat-linux-musleabi/etc/portage/package.env``root #``echo 'LDFLAGS="${LDFLAGS} -L/usr/armv6j-hardfloat-linux-musleabi/usr/lib"' > /usr/armv6j-hardfloat-linux-musleabi/etc/portage/env/muslpath.conf``root #``echo 'dev-libs/libressl muslpath.conf' > /usr/armv6j-hardfloat-linux-musleabi/etc/portage/package.env/libressl``root #``` armv6j-hardfloat-linux-musleabi-emerge -av `cat /root/armv6j-world` ``
Now wait. musl-libc is built twice for ease of use, once with crossdev and once with cross-emerge, as usually one would not be built, but once compiling has completed the binary package that this generates will be useful, along with all of the other binary packages.

### Start building your squashfs root tree

Now that most of the system is built, time to emerge the extra sotware that your Raspberry Pi project will require. For the sake of simplifying these instructions, no extra packages will be installed and the instructions will skip to the next step: creating a slimmed down version of your cross-built environment using the binary packages that cross-emerge generated.

`root #``mkdir /usr/armv6j-hardfloat-linux-musleabi/usr/src/squashfs``root #``` armv6j-hardfloat-linux-musleabi-emerge --root=/usr/armv6j-hardfloat-linux-musleabi/usr/src/squashfs --config-root=/usr/armv6j-hardfloat-linux-musleabi -Kav `cat /root/armv6j-world` ```root #``cd /usr/armv6j-hardfloat-linux-musleabi/usr/src/squashfs``root #``rm -rf usr/include var/db/pkg`
#### Install missing gcc libraries.

`root #``mkdir usr/lib/gcc``root #``cp -a /usr/lib/gcc/armv6j-hardfloat-linux-musleabi usr/lib/gcc/``root #``rm -rf usr/lib/gcc/*/*/include* usr/lib/gcc/*/*/plugin/include usr/lib/gcc/*/*/*.a usr/lib/gcc/*/*/*.la``root #``cd lib``root #``ln -s ../usr/lib/gcc/*/*/*.so ./`
#### Set/unset services manually in /etc/runlevels/\*

Here is a runlevel example.

`root #``cd ../etc/runlevels``root #``ln -s /etc/init.d/swclock boot/``root #``rm boot/hwclock`
Check what services/init scripts are available.

`root #``ls ../init.d``root #``cd ../init.d``root #``cp -a sysctl sysctl.old``root #``nano -w sysctl`
Backup the new sysctl init script just in case your change is overwritten during updates.

`root #``cp -a sysctl sysctl.bak`
### Configure everything relevant as you would a normal Gentoo system.

You can leave /etc/fstab empty though, for now. Read the gentoo handbook and see what configuration is needed before first boot if necessary. The other Gentoo Wiki Raspberry Pi pages are useful too.

#### At least set a root password so you can login.

`root #``cp -a /etc/shadow /etc/shadow.bak``root #``passwd``root #``grep root /etc/shadow >> /usr/armv6j-hardfloat-linux-musleabi/usr/src/squashfs etc/shadow``root #``mv /etc/shadow.bak /etc/shadow`
Replace the first root line with the bottom root line.

`root #``nano -w /usr/armv6j-hardfloat-linux-musleabi/usr/src/squashfs/etc/shadow`
#### Complete the root skeleton of needed but missing files/directories.

`root #``cd /usr/armv6j-hardfloat-linux-musleabi/usr/src/squashfs``root #``mkdir dev home media mnt opt proc root sys``root #``chmod 700 root``root #``cp -a /dev/null /dev/console /dev/tty /dev/loop0 /dev/random /dev/urandom dev/``root #``mknod -m 660 dev/ttyAMA0 c 204 64`
### Configure and build the kernel.

`root #``cd /usr/armv6j-hardfloat-linux-musleabi/usr/src/linux``root #``unset CHOST``root #``export KERNEL=kernel``root #``ARCH=arm CROSS_COMPILE=armv6j-hardfloat-linux-musleabi- make bcmrpi_defconfig``root #``ARCH=arm CROSS_COMPILE=armv6j-hardfloat-linux-musleabi- make menuconfig`
You probably already know what to enable/disable, and you can probably mostly leave everything as-is, but make sure that you enable tmpfs, squashfs with xz support and overlayfs for this guide, and set the default initramfs location to /usr/armv6j-hardfloat-linux-musleabi/usr/src/initramfs

`root #``mkdir /usr/armv6j-hardfloat-linux-musleabi/usr/src/initramfs`
We have a dependency loop here. The kernel needs an initramfs, and the initramfs needs to contain kernel modules. So, first we build the kernel and install its modules into our new slimmed down/configured gentoo root.

`root #``ARCH=arm CROSS_COMPILE=armv6j-hardfloat-linux-musleabi- make -j4``root #``ARCH=arm CROSS_COMPILE=armv6j-hardfloat-linux-musleabi- INSTALL_MOD_PATH=/usr/armv6j-hardfloat-linux-musleabi/usr/src/squashfs make modules_install`
### Build the initramfs

Take a break from the kernel to create the initramfs with a newly compiled ultra-minimal static busybox binary, the /init script that uses busybox, rudimentary device nodes and a compressed squashfs image of the /usr/armv6j-hardfloat-linux-musleabi/usr/src/squashfs directory.

`root #``cd /usr/armv6j-hardfloat-linux-musleabi/usr/src/initramfs``root #``mkdir bin dev mnt proc sys``root #``cp -a /dev/null /dev/console /dev/tty /dev/loop0 /dev/random /dev/urandom dev/``root #``mknod -m 660 dev/ttyAMA0 c 204 64`
#### Rebuild minimal reconfigured busybox with USE="static -syslog -make-symlinks".

`root #``cp /usr/armv6j-hardfloat-linux-musleabi/etc/portage/savedconfig/sys-apps/busybox /root/busybox.config``root #``cd /root/work/busybox/busybox-1.27.2``root #``make menuconfig``root #``cp .config ../../../busybox.mini.config``root #``cp .config /usr/armv6j-hardfloat-linux-musleabi/etc/portage/savedconfig/sys-apps/busybox``root #``export CHOST=armv6j-hardfloat-linux-musleabi``root #``USE="static -syslog -make-symlinks" armv6j-hardfloat-linux-musleabi-emerge -1Bav busybox`
#### Install minimal busybox in your initramfs.

`root #``armv6j-hardfloat-linux-musleabi-emerge --root=/usr/armv6j-hardfloat-linux-musleabi/usr/src/initramfs --config-root=/usr/armv6j-hardfloat-linux-musleabi -1Kav busybox`
#### Delete unneeded files from initramfs.

`root #``cd /usr/armv6j-hardfloat-linux-musleabi/usr/src/initramfs``root #``rm -rf etc var usr tmp`
##### Make sure you statically linked busybox.

`root #``file /usr/armv6j-hardfloat-linux-musleabi/usr/src/initramfs/bin/busybox`
#### Create a squashfs image of your minimal root for inclusion in initramfs.

`root #``cd /usr/armv6j-hardfloat-linux-musleabi/usr/src/squashfs``root #``mksquashfs . ../initramfs/squash -comp xz -b 1048576 -Xbcj arm -Xdict-size 1048576`
Your squashfs image should be somewhere around 5 to 10 megabytes if you didn't install anything other than the list of packages ealier in these instructions.

#### Write your initramfs init script and make it executable.

`root #``cd ../initramfs``root #``nano -w init``root #``chmod 700 init`
#### Regenerate an initramfs built into the kernel, including the entire OS.

`root #``cd ../linux``root #``rm usr/initramfs_data.cpio.gz``root #``unset CHOST``root #``ARCH=arm CROSS_COMPILE=armv6j-hardfloat-linux-musleabi- make -j4`
### Complete the installation.

Your cross-gentoo build is complete, all in only one small file! One last thing that needs doing is putting it on a FAT32 SD card partition along with the Raspberry Pi firmware.

`root #``mkdir /root/work/bootloader``root #``cd /root/work/bootloader``root #``git clone --depth=1 --branch=next https://github.com/raspberrypi/firmware``root #``mount /dev/SDCARD /media/sdcard``root #``cd /usr/armv6j-hardfloat-linux-musleabi/usr/src/linux``root #``cp arch/arm/boot/zImage /media/sdcard/kernel.img``root #``mkdir /media/sdcard/overlays``root #``cp arch/arm/boot/dts/overlays/*.dtbo /media/sdcard/overlays/``root #``cp arch/arm/boot/dts/*.dtb /media/sdcard/``root #``cd /root/work/bootloader/firmware/boot``root #``cp -r boot* fixup* start* /media/sdcard/``root #``umount /media/sdcard`
Once you see that the system is booting, it is safe to remove the SD card from the Raspberry Pi. This setup allows for SD card swapping, because you already loaded the entire operating system into the Raspberry Pi's RAM.

## Troubleshooting

System busybox config:
[https://gist.github.com/anonymous/d9a10a8d380a313e54f4435be15c21c1](https://gist.github.com/anonymous/d9a10a8d380a313e54f4435be15c21c1)

Minimal busybox config:
[https://gist.github.com/anonymous/749fd5f60d0c837cb42bfd921048ac71](https://gist.github.com/anonymous/749fd5f60d0c837cb42bfd921048ac71)

Example init script:
[https://gist.github.com/anonymous/8787dd24b6fc7e061b6637d9f61c2f8f](https://gist.github.com/anonymous/8787dd24b6fc7e061b6637d9f61c2f8f)
