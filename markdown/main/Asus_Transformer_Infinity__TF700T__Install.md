<!-- source: https://wiki.gentoo.org/wiki/Asus_Transformer_Infinity_(TF700T)/Install | group: Gentoo Wiki (Main) | wiki-title: Asus Transformer Infinity (TF700T)/Install -->
---
title: Asus Transformer Infinity (TF700T)/Install
url: https://wiki.gentoo.org/wiki/Asus_Transformer_Infinity_(TF700T)/Install
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2024-12-20"
tags: ['here on GITHUB', 'this']
fingerprint: "8725585b1ffe3d87"
license: CC BY-SA 4.0
---

# Asus Transformer Infinity (TF700T)/Install

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**2016-01-01**, the information in this article is probably

**outdated**. You can help the Gentoo community by verifying and

[updating this article](<https://wiki.gentoo.org/index.php?title=Asus_Transformer_Infinity_(TF700T)/Install&action=edit>).

# Installing Gentoo Linux on Asus Transformer Infinity (TF700T)

# Preliminary notes

## Keyboard dock

You MUST have the keyboard dock, otherwise you must try to plugin some kind of USB keyboard via the OTG adapter, which i guess would be a pain to use. After you get to the point of starting X you could do without the dock, using the on-screen virtual keyboard... but i emphasize on "could", because why would you want such a pain?

Using a Bluetooth keyboard could be achieved, but clearly again only after you have successfully installed part of the system, so you still need some USB keyboard.

## Installation device and "emerging" device

I strongly recommend to install Gentoo on the microSD. The SD slot on the dock might work as well (and maybe can be even faster) but the risk of unplugging the dock by accident is high and the result can be fairly nasty.

Using the internal memory is another option, but due to Asus being cheap, it is also very slow... and of course while you can replace a microSD after it wears out, if it happens to the internal memory you can kiss your device goodbye. Linux is much more write-intensive than Android and while you can mitigate it, i would worry about write cycles and such.

Another good approach is to plug an external hard-drive to the USB port of the dock (it is USB 2.0), setup your swap partition on it and move your portage build folder to it. This do increase your emerge performances and make the device much more responsive while emerging stuff. Both the internal memory and the microSD controller tend to cause severe lags when under heavy loads, like during an emerge operation.

## RAM and SWAP considerations

The tables has only 1gb of ram. While it might seems a lot, and it surely was up to a few years ago, it is surprisingly small by today standards. Don't ask me why Asus choose such a limited amount on a top-level device, just live with it. While enabling swap is the traditional way of coping with limited ram, traditional swap should be avoided on the TF700T due to the extreme lagginess of the internal storage and of the microSD controller under heavy loads. Not considering wear-and-tear of the memory itself due to increased write cycles of swapping to it.

So for day-to-day usage i suggest to disable any swap file, with the exception of ZRAM, which can be enabled with the latest kernel from "that" because it is been pack ported from newer kernels.

Unfortunately compilation do require a huge amount of memory. Some packages like Firefox, Chromium and LibreOffice don't compile on my tablet if i don't enable at least 4gb of swap space, so i created a 4gb swap file on an external USB hard drive which i turn on when something fails to compile. This issue is quite arcane... as a rule of thumb, whenever your compilation fails with "internal compiler error" you want to enable your swap space and issue an emerge --resume.



# Before the first Linux boot

The Asus Infinity (TF700T) comes out of the box as an Android tablet, it is provided with android 4.2.1 or something similar but it is not so important because we want to install Linux on it, so these steps will provide information on how to enable dual boot on this device.

You will need to do some steps on Android before starting with Linux.

## Unlock and root

You must unlock your boot-loader first, follow these instructions:

Go to this link: [support page on Asus website](http://support.asus.com/download.aspx?SLanguage=en&p=28&s=1&m=ASUS+Transformer+Pad+Infinity+TF700T&os=&hashedid=fq2B3oq64av1O95u), select OS Android and from utilities download the "Unlock Device App: Unlock boot loader". Now install the APK on android, follow directions and enjoy your boot-unlocked tablet (yes, this will void your warranty, you have been warned).
(the download is mirrored [here](http://www.gardiol.org/tf700/UnLock_app_V8.apk) in case the original source goes offline)

For more directions, check this XDA thread: [XDA on unlock TF700T](http://forum.xda-developers.com/showthread.php?t=2252563)

At this point you need root. My suggestion is to install a custom ROM which will do everything for you. Which one is on you, check this link on XDA [how to root and custom ROMS](http://forum.xda-developers.com/showthread.php?t=2688891) for detailed instructions and a list of ROMS, but bear in mind that at the moment the multi boot has been tested only with a few ROMS: either CROMi-X, CROMBi-KK or ZOMBi-X latest versions, on insternal SD only, no rom2sd, data2sd or anything fancy. I suggest you to stick with one of these.

If you prefer to root the original Asus ROM, again check on XDA how to do it.

## Making your tablet unbrickable

While there is basically no threat of bricking your tablet, it is strongly advised to follow these steps [here](https://www.androidroot.mobi/pages/guides/tegra3-guide-nvflash-jellybean/) on how to make your tablet "safe" from any messing up you might do.

In short:

- download this: [flatline](https://download.androidroot.mobi/flatline_tf700.img)
- install google's ADB and fastboot (from SDK) on oyur linux pc
- flash the downloaded custom recovery: **fastboot flash recovery flatline\_device.im**
- reboot device to recovery, select "advanced" and "wheelie", select "Step 1: Flash AndroidRoot BL"
- it will poweroff. Reboot to android, let it boot, use a bit, then reboot to recovery again
- select "Step 2: Generate wheelie blobs" from the same menu as before
- when finished, reboot and save from the device the files it has created (check under /data or /tmp for AndroidRoot or similar)

Keep these files safe, they are your lifeline. (NOTE: these instructions are given with no warranty, use at your risk!)

## Prepare the storage

As noted above, i suggest you install Gentoo on the microSD.

I suggest to plug in a very fast microSD, but be aware that the TF700T microSD slot is a bit picky. I managed to get a Sandisk 32gb Class 10 microSD to work great, while no 64gb card do work properly so far: they seems to be working, but at some random point under heavy load will drop to read-only with no warning. It seems to be an hardware issue since moving the same card to the SD slot on the dock fix the issue.

I assume we are using the microSD slot here on at least 16gb microSD card. The bigger, the better. 8Gb might be tight, and will definitely NOT work if you plan to emerge something like LibreOffice, Firefox or Chromium on it.

You need to format it, since i am aware of no good tools on Android for doing this, put the card in to your pc and prepare two partitions:

- the first formatted as EXT4 or [f2fs](https://wiki.gentoo.org/wiki/F2FS), at lest 16gb (better 32gb).
- the second as VFAT, for data exchange (not required, just optional if you have spare space)

Ext4 is the classical choice of filesystem indeed, but you can also experiment with the relatively new "f2fs", which claims to be specially tuned for flash storage improving both life and speed. I have not tested it, but remember to use a compatible kernel if you choose it. The latest "that" kernel is compatible with f2fs.

If you don't want to use a microSD, you could use the internal memory either by replacing the Android partition (not recommended, too small) or using the Android data partition (not recommended due to possible wear and tear).

(Repartitioning the internal memory is STRONGLY NOT ADVISED. I was thinking about it but was discouraged by the fact it does not use any common partition scheme and no linux tools are able to mess with it. Plus the fact that it can really brick your tablet requiring you to grab the lifeline from last paragraph)

## Memory devices and partitions

As a reference, the device with the dock has the following storage devices / mount points:

mmcblk0: internal memory

- /dev/mmcblk0p1: Android /system partition
- /dev/mmcblk0p2: Android /cache partition
- /dev/mmcblk0p3: Recovery image
- /dev/mmcblk0p4: Recovery staging
- /dev/mmcblk0p5: Android device-specific hardware stuff
- /dev/mmcblk0p6: Android encrypted DRM keys
- /dev/mmcblk0p7: unused
- /dev/mmcblk0p8: Android /data partition, the real internal memory


mmcblk1: the tablet MicroSD slot
/dev/sdb: the dock SD slot

Usually, the first block device you attach to the USB port of the dock get the /dev/sda device group.

More details [here (XDA)](http://forum.xda-developers.com/showthread.php?t=2004273)

## Setting up dual-boot

Dual-booting the tablet is done using a kexec method which is just brilliant. The full thread on XDA is [here](http://forum.xda-developers.com/showthread.php?t=2387133).

In short, **with the changes we need for Gentoo**:

- Check you have a supported ROM and recovery (CROMBi-KK and TRWP for me worked well)
- boot into your ROM and make sure you have terminal emulator, root and busybox installed
- download this: [scripts](https://www.androidfilehost.com/?fid=23991606952593990) (Mirrored [here](http://www.gardiol.org/tf700/TF700t-AKBI-v2.6.7.zip))
- extract in the internal memory, you should get a folder called *TF700t-AKBI-v2.6.7*
- don't download any rootfs, you will be making your own with Gentoo on it soon enough.
- become root: *su bash*
- run the script: *sh firstintall.sh* (it will install the multiboot blob).
- When prompted, select the proper kernel for your android from the list (a mistake here will make your Android installation unbootable, but you can fix it from the multiboot prompt later on, left as an exercise in patience)
- When asked for the rootfs image, just terminate the script. We will not setup a rootfs image, we will install Gentoo ourselves.
- If the script gives an error (missing rootfs image, for example) just ignore it, all you need is the kexec blob for the multiboot to be installed and the android kernel to be copied.

**WARNING:** *take your time to understand each step, carefully read the original guide on XDA and make double-sure of what you are doing, this is again a critical step which can lead to a bricked device and the need for the lifeline we created before.*

(remember you are on Android here, not Linux, so things are different. For once, you must run shell script by invoking the shell itself and not by directly run the script, as the partition you have normally access to is mounted as no-exec)

Very well, at this point we should have the multiboot installed and the android kernel restored for the multi-boot. We now need to configure the multiboot for booting linux from the microSD card:

- prepare to mount the multiboot partition: *mkdir -p /data/media/0/kexecbootcfg/*
- mount the multiboot partition: *mount -t vfat /dev/block/mmcblk0p5 /data/media/0/kexecbootcfg/*
- Create the config folder: *mkdir -p /data/media/0/kexecbootcfg/multiboot/*
- Create/edit the file called boot.cfg in this directory, this is mine as an example:

#Android#
LABEL=CROMBi-KK
BOOT=3
DEVICE=/dev/mmcblk0p1
DIR=/
KERNEL=/boot/zImage
INITRD=/boot/initrd.img
PRIORITY=10
#Linux ext partition image on microsd partition 1 #
LABEL=Gentoo on microSD partition 1
BOOT=3
DEVICE=/dev/mmcblk1p1
DIR=/
KERNEL=/boot/zImage
INITRD=/boot/initrd.img
PRIORITY=110

- Adapt the example to your needs!
- Unmount the multiboot partition: *umount /data/media/0/kexecbootcfg/*

The Linux entry will not boot until we set up the kernel for it, but at least you should be able to reboot into Android. Try it out! The tablet will boot twice, the first time will show you the menu, the second will actually boot Android. While this is a bit annoying, it is a necessary evil as you will actually first boot the bootloader-kernel which will in turn boot your selected OS.

As Workdowg himself explains on XDA:

*"What happens here is by flashing the kexecboot kernel blob (to mmcblk0p4) you are given a menu to choose a rootfs to boot from, ie Android, Linux image file, Linux on a partition... So in order for the kexecboot kernel to be of any use to you, you need to install Android and Linux kernels in your rootfs. It then reboots using the kernel you chose. The config file, boot.cfg, is installed to a small 5MB vfat partition (mmcblk0p5)*

*ANDROID: Android mounts root (mmcblk0p1) "/" as /system. So we install a kernel (without modules, you did that when you installed your ROM) to a directory we make called /boot which needs to be in the mount /system directory. This kernel (zImage and initrc.img) is just extracted from \_that's cm112 or \_that9oc+ kernel blobs and a copied to the /boot directory.*

*LINUX: This kernel was especially compiled by JoinTheRealms from \_that5 source using configs moreD\_cn came up with. It needs to be installed in /boot of a Linux rootfs."*

At this point you have a working dual-boot on your tablet, we need to build Gentoo so you also have something to dual-boot!

# Installing Gentoo: before first boot

This is basically the standard Gentoo Handbook, so i will not bore you on the basics, but a few important things must be noted, so please keep reading.

## Prepare for first Gentoo boot

I assume you want to install on the first partition of your microSD card. If not, just follow these instructions but perform them on the media you want to use (loopback image, internal memory, SD card, whatever)

These instructions will allow you to do everything from your tablet on Android, but if you prefer to make things easier, put the microSD into your Linux PC and perform them there, then move the microSD to the tablet and skip to the reboot part.

I assume your microSD has already been formatted and that the first partition is the Gentoo root partition. If you have a more complex partitioning scheme, have fun adapting these instructions for your case.

We need to mount the partition, so:

- From android, open your Android Terminal Emulator and become su: *su bash*
- create a temporary folder: **mkdir tmpGentoo**
- mount the microSD: **mount -t ext4 /dev/mmcblk1p0 tmpGentoo** (mmcblk1 is the microSD, use the filesystem you used if not ext4)

## Download stage3 for ARM-hf

The tablet is an ARMv7 with hard floating point. The binary NVIDIA packages which we are forced to install do require you to install this architecture, so go to the [gentoo ARM download page](http://distfiles.gentoo.org/releases/arm/autobuilds/current-stage3-armv7a_hardfp/) and download on your tablet the latest Stage3.

Extracting it into the mounted partition can be hard depending on your busybox, decompressing the stage3 is left to your exercise (if you are desperate, just decompress it on a linux pc then mount the microSD on the tablet).

## Prepre your stage3 before first boot

(make sure to do these steps from your tablet and **not** from the PC)
Follow the gentoo handbook to install it. In short, these are the steps:

- cd into your gentoo mounted parttion: *cd tmpGentoo*
- remount with bind dev, proc, run, sys: *for i in dev proc run sys; do mount -o bind /$i $i; done*
- create your *etc/resolv.conf*, put inside your DNS server (Android does not have one, so you must create it from scratch)
- chroot: *chroot ./ /bin/bash*
- change the root password immediately: *passwd*
- DO NOT INSTALL A KERNEL: you must use the provided "that" kernel.
- DO NOT INSTALL GRUB OR ANY BOOTLOADER: you have multiboot already in place.
- DO NOT INSTALL LOG MANAGERS: metalog, syslog-ng and the likes, will only slow down and add wear to your microSD.
- Take a look at the Gentoo Handbook to see if you need something else which i clearly forgot here.
- Don't forget to prepare your portage tree (**emerge --sync**) and update it as needed.

## Mask and unmask packages

The provided Tegra drivers works only with X up to 1.14 (which is not very stable with the Tegra driver) and since 1.13 has been masked due to security flaws, you must mask any xorg-server above or equal to 1.13. Also, many ebuilds are not yet classified as "stable" for ARM, but are nevertheless very much usable.

Here is what i suggest for **/etc/portage/packages.mask**:

\# Needed becaue NVIDIA provided drivers are stuck to Xorg 1.12 
# Actually, 1.13 should work, but due to security issues 1.13 
# has been masked in gentoo.
>=x11-base/xorg-server-1.13.0
# This is needed by xorg-server 1.12.x
>=x11-libs/libXfont-1.5.0
# This won't compile (will check for minimum 5gb of real RAM!)
>=www-client/chromium-44
# Bluez5 requires kernel 3.4 as minimum, we are stuck with bluez4...
>net-wireless/bluez-4.101-r9

Specifying per-package accept keywords is always a good idea, so i suggest to split the following under the new style /etc/portage/packages.use/\<pkg name> files style. Here i will report them all together for clarity sake (i use many more, but this is somehow the minimum needed):

\# For Wifi and bluetooth
=net-wireless/gnome-bluetooth-3.16.1 \~arm
=net-wireless/blueman-1.23\_p20140717-r1 \*\*
=app-mobilephone/obex-data-server-0.4.6 \~arm
=gnome-extra/nm-applet-1.0.2 \~arm
# For Chromium
=media-libs/libvpx-1.4.0 \~arm
=www-client/chromium-43.0.2357.73 \~arm
# Nice desktop status widget
=app-admin/conky-1.9.0-r3 \~arm
# Filesystems and such
=sys-fs/fuse-exfat-1.1.0 \~arm
=sys-fs/exfat-utils-1.1.1 \~arm
=sys-fs/mtpfs-1.1-r1 \*\*
=media-libs/libmtp-1.1.9 \~arm
=sys-apps/pmount-gui-0\_pre20130806 \*\*
# Audio / video
=media-sound/lxmusic-0.4.5 \*\*
=media-libs/gmtk-1.0.9 \*\*
=media-video/gnome-mplayer-1.0.9 \*\*
=media-sound/sndfile-tools-1.03 \*\*
=media-sound/xmms2-0.8-r2 \*\*
=media-sound/pnmixer-9999 \*\*
# Useful working apps
=media-gfx/gimp-2.8.14 \~arm
=app-office/libreoffice-4.4.3.2 \~arm
=app-office/libreoffice-l10n-4.4.3.2 \~arm
=media-gfx/xsane-0.999-r1 \~arm
=app-arch/p7zip-9.38.1-r1 \~arm
# LM sensors works, but only for CPU temp
=sys-apps/lm\_sensors-3.3.5 \~arm
# Development and Qt
=dev-qt/qt-creator-3.4.1 \~arm
=dev-qt/qtquickcontrols-5.4.1 \~arm
=dev-qt/qtgui-5.4.1-r2 \~arm
=dev-qt/qtpositioning-5.4.1 \~arm
# X server and such
=x11-misc/xbindkeys-1.8.6 \~arm
=x11-misc/xkbd-0.8.16 \~arm
=x11-misc/xtermcontrol-3.2 \*\*
# ZRAM works thanks to a kernel backport! Ignore the error emerging and use it
=sys-block/zram-init-3.0 \*\*

You might need to add some minor libraries and dependencies too.

As a rule of thumb, if a package has the \~arm keyword, try it out! They usually work very well (with Filezilla the only exception so far for me). I recommend avoiding Firefox due to a lot of graphical bugs, piss poor support for hi-dpi and no touch screen support whatsoever. Chromium instead behaves very nicely with all the above.

Since the tablet has only WiFi, i suggest strongly to install NetworkManager and use it. Since i am going to use LXDE i have also unmasked the needed packages for the nm-applet which works great with LXDE.

This is my /etc/portage/make.conf:

```
CFLAGS="-O2 -pipe -march=armv7-a -mfpu=neon -mfloat-abi=hard"
CXXFLAGS="${CFLAGS}"
CHOST="armv7a-hardfloat-linux-gnueabi"
PORTDIR="/usr/portage"
DISTDIR="${PORTDIR}/distfiles"
PKGDIR="${PORTDIR}/packages"
USE="acl wifi armvfp mp3 v4l xvid xvmc aac a52 alsa neon networkmanager X jpg jpeg tiff png dbus pdf cups udev samba bluetooth qml startup-notification elogind policykit usb sndfile"
MAKEOPTS="-j4"
VIDEO_CARDS="v4l"
INPUT_DEVICES="evdev keyboard"
FEATURES="ccache -sandbox -usersandbox"
LINGUAS="en"
```
**`/etc/portage/make.conf`**

```
CFLAGS="-O2 -pipe -march=armv7-a -mfpu=neon -mfloat-abi=hard"
CXXFLAGS="${CFLAGS}"
CHOST="armv7a-hardfloat-linux-gnueabi"
```
**`/etc/portage/package.use/00input`**

```
 INPUT_DEVICES: evdev keyboard
```
**`/etc/portage/package.use/00video`**

```
 VIDEO_CARDS: -* v4l
```
**`/etc/portage/package.use/00local`**

```
  LINGAS: en
```

I have enabled ccache to speed up build times and help the microSD. Since we have a quad-core i have set the -j4 to speed up make. I had to disable sandbox because some packages will not build on ARM. The various useflags are just an example, adapt to your needs. Of course, you need to *emerge ccache* and enable it too. 
The "neon" USE flag will make use of the supposedly faster neon FPU available on this tablet, and this is also why i have changed the default CFLAGS to enable neon FPU on all builds.

For reference only, i am reporting also my per-package user flags. YMMV, check it out but work your list as you see fit:

\>=sys-libs/zlib-1.2.8-r1 minizip
>=dev-libs/libxml2-2.9.2-r1 icu
www-client/chromium -neon hidpi
>=app-text/poppler-0.32.0 cairo
=media-libs/libcanberra-0.30-r4
>=media-libs/harfbuzz-0.9.38 icu
>=x11-libs/libfm-1.2.3 gtk
media-libs/mesa -dri3 egl -gallium -llvm -gbm osmesa gles1 gles2
# this will pull Bluez5 in, we cannot afford it...
gnome-extra/nm-applet -bluetooth
net-misc/networkmanager -bluetooth
>=app-crypt/gcr-3.14.0 gtk
>=dev-libs/libpcre-8.36 pcre16
=x11-libs/libxcb-1.11-r1 xkb
dev-qt/qtquickcontrols widgets
sys-fs/udev gudev
media-sound/xmms2 asf ffmpeg flac mp4

## Setup the kernel

No, you cannot recompile any sources provided by Gentoo nor i had any luck whatsoever with the NVIDIA provided kernel sources, according to what the NVIDIA devs say we should get an updated kernel from Asus itself. So at the moment we are stuck with kernel 3.1.10 (see links at the next sub-section) which is the latest provided by Asus. Since we already have a good working kernel ready, there is no need to add also this strain to the tablet, let's go with the provided one.

What you have to do now is copy the same kernel your Android is using (or choose a kernel from the package which contains the multiboot loader) to the /boot folder of your Gentoo partition. I leave this as an exercise for you, you want to end up with these files:

/boot/initrd.img
/boot/zImage

So copy both from your Android system partition.
I suggest you use the latest "that" kernel from [here](http://forum.xda-developers.com/attachment.php?attachmentid=2940945&d=1410986629)   (mirrored [here](http://www.gardiol.org/tf700/kernel-that11.zip)). The associated sources are [here on GITHUB](https://github.com/that1/android_kernel_asus_tf700t/releases/tag/release-that11).

Since some packages needs the kernel sources to be built, we need to download the sources associated to your kernel:

- Fetch any one of the kernel sources listed below and put it under  **/usr/src/linux**.
- Copy your current kernel config: *cp /proc/config.gz /usr/src/linux*
- decompress: *cd /usr/src/linux && gunzip config.gz*
- rename: mv config .config

You kernel sources tree is now configured.

### To play around with the kernel

In case you want to experiment:

- [this](http://dlcdnet.asus.com/pub/ASUS/EeePAD/TF700T/JB_10_6_1_14_10_kernel.rar) is the link to the latest official Asus kernel sources (try also from the official Asus support page in the very unlikely case there might be an update)
- [this](https://github.com/that1/android_kernel_asus_tf700t/releases/tag/release-that11) is the link to an already configured kernel for TF700T by "that" v.11 (XDA member)
- [this](https://github.com/CyanogenMod/android_kernel_asus_tf700t) is the link to the CyanogenMod kernel sources for the TF700T
- [my config.gz](http://www.gardiol.org/tf700/config.gz) taken from a *that* v.10 kernel

## Setup swap

Swap on the micro-sd or internal memory is just crap. Totally avoid it, trust me, it is pointless laggish and will hung up the tablet more often than any benefit as soon as you start swapping a lot.

Luckly, there are two other options: ZRAM and external swap on USB hard drive.

I recomend you install zram-init and configure it (see /etc/conf.d/zram-init). It works because it has been backported some time ago to kernel 3.1.10, but the Gentoo package will still warn you, just ignore the warning and install it anyway. Immediately enable it and add it to the boot runlevel. This will drastically make the tablet much more usable, and will help for emerge most of the packages.

Some packages, like Chromium or Libreoffice, will still fail build due to insufficent memory, so you want to add more swap for that. I strongly suggest you plug an external USB hard drive and provide some swap space on that, just while emerging those large packages. The speed increase is paramount and the tablet will still be usable during the emerge process. I have not tried using swap on the dock SD card, YMMV.

## Install driver packages

At this moment the only way to get X to start is using the NVIDIA provided binary blob. It sucks, albeit it works, but since i am not able to recompile any newer kernel and try OpenTegra, there is no other way to go.

Touch screen and trackpad works great but we need to download and install the proper drivers too.

- Build Xorg, it will take some time, so you can start building it while doing the rest of the steps: *emerge xorg-server xorg-drivers*
- While it builds, download the NVIDIA package [from here](http://developer.download.nvidia.com/mobile/tegra/l4t/r16.5.0/cardhu_release_armhf/Tegra30_Linux_R16.5_armhf.tbz2)
- then unpack it into your root user home folder in the gentoo partition (i suggest to keep it there in case you need some files later on)
- Now doenload the trackpad drivers from [this](http://www.gardiol.org/tf700/xorg-x11-drv-mtev-0.1.13.tar.bz2) link (this is a patched version of the sources you can find [here](http://gitorious.org/xorg/xf86-input-mtev), which will not compile)
- download the touch-screen drivers [here](http://www.gardiol.org/tf700/Multitouch-X-Driver.tar.bz2) (original package [here in git repository](https://github.com/rydberg/Multitouch-X-Driver.git)

Now wait for Xorg to be completely built and installed.

- decompress, build and install (*make install* is enough) both the touchpad and the touchscreen drivers.
- Decompress the Linux 4 Tegra package
- Decompress the archive nvidia\_drivers.tbz2 found within */root/Linux\_for\_Tegra/nv\_tegra*, it will create some folders like usr, etc and lib, DO NOT EXPAND IN YOUR ROOT FOLDER! For the following instructions i assume you will expand it into the nv\_tegra folder where the archive is.
- Copy the xorg driver from */root/Linux\_for\_Tegra/nv\_tegra/usr/lib/xorg/modules/drivers* to */usr/lib/xorg/modules/drivers*. Pick the right one (the ABI12 in our case) and copy to the destination folder and rename it to tegra\_drv.so.
- Copy MOST of the libraries from */root/Linux\_for\_Tegra/nv\_tegra/usr/lib* to */usr/lib* but be VERY CAREFULL not to replace *libjpeg.so*, you want to keep your copy of it.
- Poke around the */root/Linux\_for\_Tegra/nv\_tegra* folder, check if there are more files you want to copy to your file-system (probably not).

## Configure the dock keyboard

The dock keyboard is not exactly your standard keyboard and it requires a bit of fiddling to get it right. You can use it out of the box, but your top row of keys will be messed up badly and you will lack, among all function keys, also the ESC key, which comes pretty handy usually.

You need to fix the keyboard layout both for the console and for X. You might skip the console maybe, but it is so simple that it is a pity not to do it.

You want to add the following content as /etc/udev/rules.d/99-keyboard.rules:

```
ATTR{name}="asusdec", ACTION=="add", SUBSYSTEM=="input", RUN+="/etc/tf700/layoutswitcher_wrapper.sh"
```
to make sure the proper X modmap is applied each time the tablet is removed/plugged to the dock. You will need also the files:

tf700t \~ # cat /etc/tf700/layoutswitcher\_wrapper.sh 
#!/bin/bash
lock="/tmp/keyboard.lock"
exec 9>"$lock"
if ! flock -n 9; then
	exit 1
fi
/etc/tf700/layoutswitcher.sh &

and:

tf700t \~ # cat /etc/tf700/layoutswitcher.sh 
#!/bin/bash
DISPLAY=":0"
USER=$(ps -ef | grep '/usr/bin/lxsession' | grep ? | cut -d' ' -f1 | head -n 1)
HOME=/home/$USER
XAUTHORITY=$HOME/.Xauthority
export DISPLAY XAUTHORITY HOME
xmodmap /etc/tf700/SpecialKeysX
echo 12 > /sys/module/asusdec/parameters/key\_flags



### Fixing the keyboard layout in console

Create the following file (create also the missing tf700 folder, we will use it later)

Contents of file **/etc/tf700/tf700.map**:

keymaps 0-15
### Top row
# Back
keycode 158 = Escape
# Wifi on/off
keycode 238 = F1
	shift keycode 238 = F13
	control keycode 238 = F25
	alt keycode 238 = Console\_1
# Bluetooth on/off
keycode 237 = F2
	shift keycode 237 = F14
	control keycode 237 = F26
	alt keycode 237 = Console\_2
# Touchpad off
keycode 60 = F3
	shift keycode 60 = F15
	control keycode 60 = F27
	alt keycode 60 = Console\_3
# Brightness down
keycode 224 = F4
	shift keycode 224 = F16
	control keycode 224 = F28
	alt keycode 224 = Console\_4
# Brightness up
keycode 225 = F5 
	shift keycode 225 = F17
	control keycode 225 = F29
	alt keycode 225 = Console\_5
# Auto brightness
keycode 61 = F6
	shift keycode 61 = F18
	control keycode 61 = F30
	alt keycode 61 = Console\_6
# Camera
keycode 212 = F7
	shift keycode 212 = F19
	control keycode 212 = F31
	alt keycode 212 = Console\_7
# Browser
keycode 150 = F8
	shift keycode 150 = F20
	control keycode 150 = F32
	alt keycode 150 = Console\_8
# Settings
keycode 62 = F9
	shift keycode 62 = F21
	control keycode 62 = F33
	alt keycode 62 = Console\_9
# Media: previous
keycode 165 = F10
	shift keycode 165 = F22
	control keycode 165 = F34
	alt keycode 165 = Console\_10
# Media: play/pause
keycode 164 = F11
	shift keycode 164 = F23
	control keycode 164 = F35
	alt keycode 164 = Console\_11
# Media: next
keycode 163 = F12
	shift keycode 163 = F24
	control keycode 163 = F36
	alt keycode 163 = Console\_12
# Mute
keycode 113 = F13
	alt keycode 113 = Console\_13
# volume down
keycode 114 = F14
	alt keycode 114 = Console\_14
# Volume up
keycode 115 = F15
	alt keycode 115 = Console\_15
# Lock screen
#keycode 142 = SysRq
### Bottom row
# Home
keycode 172 = Alt
# Search
keycode 217 = Alt
# Menu
keycode 139 = Alt

Now edit your */etc/conf.d/keymaps* and change the default keymap like this:

keymap="/etc/tf700/tf700.map"

Done!

### Fixing the keyboard layout in X (easy way)

This is a bit more complex, because we need to patch the xkeyboard-config package. 
I have prepared a pre-patched source for it that you can download [from here](http://www.gardiol.org/tf700/xkeyboard-config.tar.bz2). In case you want to grab and patch the latest sources, check the following paragraph.

We will be reinstalling it on top of the current portage ebuild, which is ugly and not very nice. Unless somebody more practical than me is willing to create an ebuild and, maybe, can share it with us, this what we are going to do.

- Unpack the sources into /root (for example)
- get into the source folder and configure it: *./configure --with-xkb-base="/usr/share/X11/xkb" --enable-compat-rules --disable-runtime-deps --with-xkb-rules-symlink=xorg*
- The usual: *make && make install*

We are done here too.

### Fixing the keyboard layout in X (hard way)

If you prefer, go ahead and patch the sources yourself.

- grab the sources from [here (git)](git://anongit.freedesktop.org/git/xkeyboard-config)
- Decompress the sources
- Create a file called tf700dock into the keycodes subfolder, with the following content:

```
// translation from evdev scancodes to something resembling xfree86 keycodes.
default xkb_keycodes "tf700dock" {
minimum = 8;
maximum = 255;
<LSGT> = 94;
<TLDE> = 49;
<AE01> = 10;
<AE02> = 11;
<AE03> = 12;
<AE04> = 13;
<AE05> = 14;
<AE06> = 15;
<AE07> = 16;
<AE08> = 17;
<AE09> = 18;
<AE10> = 19;
<AE11> = 20;
<AE12> = 21;
<BKSP> = 22;
<TAB> = 23;
<AD01> = 24;
<AD02> = 25;
<AD03> = 26;
<AD04> = 27;
<AD05> = 28;
<AD06> = 29;
<AD07> = 30;
<AD08> = 31;
<AD09> = 32;
<AD10> = 33;
<AD11> = 34;
<AD12> = 35;
<BKSL> = 51;
alias <AC12> = <BKSL>;
<RTRN> = 36;
<CAPS> = 66;
<AC01> = 38;
<AC02> = 39;
<AC03> = 40;
<AC04> = 41;
<AC05> = 42;
<AC06> = 43;
<AC07> = 44;
<AC08> = 45;
<AC09> = 46;
<AC10> = 47;
<AC11> = 48;
<LFSH> = 50;
<AB01> = 52;
<AB02> = 53;
<AB03> = 54;
<AB04> = 55;
<AB05> = 56;
<AB06> = 57;
<AB07> = 58;
<AB08> = 59;
<AB09> = 60;
<AB10> = 61;
<RTSH> = 62;
<LALT> = 225; // Search - Left Alt
<LCTL> = 37;
<SPCE> = 65;
<RCTL> = 105;
<RALT> = 108;
<LWIN> = 180; // Home - Left Super
<RWIN> = 134;
<COMP> = 135;
alias <MENU> = <COMP>;
<ESC> = 166;   // Escape  - Back key
<FK01> = 246;  // F1      - Wifi
<FK02> = 245;  // F2      - Bluetooth
<FK03> = 68;   // F3      - Touchpad
<FK04> = 232;  // F4      - Backlight less
<FK05> = 233;  // F5      - Backlight more
<FK06> = 69;   // F6      - Backlight auto
<FK07> = 220;  // F7      - Screenshot
<FK08> = 158;  // F8      - Browser
<FK09> = 70;   // F9      - Setting
<FK10> = 173;  // F10     - Previous track
<FK11> = 172;  // F11     - Start/Stop track
<FK12> = 171;  // F12     - Next track
<MUTE> = 121;
<VOL-> = 122;
<VOL+> = 123;
<UP> = 111;
<LEFT> = 113;
<DOWN> = 116;
<RGHT> = 114;
<NMLK> = 77;
<KPDV> = 106;
<KPMU> = 63;
<KPSU> = 82;
<KP7> = 79;
<KP8> = 80;
<KP9> = 81;
<KPAD> = 86;
<KP4> = 83;
<KP5> = 84;
<KP6> = 85;
<KP1> = 87;
<KP2> = 88;
<KP3> = 89;
<KPEN> = 104;
<KP0> = 90;
<KPDL> = 91;
<KPEQ> = 125;
<FK13> = 191;
<FK14> = 192;
<FK15> = 193;
<FK16> = 194;
<FK17> = 195;
<FK18> = 196;
<FK19> = 197;
<FK20> = 198;
<FK21> = 199;
<FK22> = 200;
<FK23> = 201;
<FK24> = 202;
};
```
- Then modify the file keycodes/Makefile.am and add just before the README file:

tf700dock \

- Then modify the file rules/base.m\_k.part and add anywhere you want the line:

tf700dock = tf700dock

- Then modify the file rules/base.xml.in and add the following where you see fit:

```
<model>
    <configItem>
        <name>tf700dock</name>
        <_description>ASUS Transformer Prime TF700 Dock</_description>
        <vendor>Asus</vendor>
    </configItem>
</model>
```
- now run autogen.sh to generate the configure scripts
- go back to the previous paragraph and follow the instruction to configure, build and install.

## Ready to reboot?

You can now reboot android and try out the dual boot, if all goes well, you will get to a grentoo login console prompt and you can login as root!

# First boot: setup various, network and X

We need to do some thing to start up

copy this:

tf700t conky # cat /etc/local.d/01-tegra.start 
#!/bin/bash
echo Mount extra stuff..
mount /dev/pts
chmod 0666 /sys/power/state
#echo 1 > /sys/module/cpu\_tegra3/parameters/auto\_hotplug
echo "Y" > /sys/module/cpuidle/parameters/lp2\_in\_idle
echo 4096 > /sys/block/mmcblk0/queue/read\_ahead\_kb
echo 4096 > /sys/block/mmcblk1/queue/read\_ahead\_kb
echo balanced > /sys/devices/system/cpu/cpuquiet/current\_governor
#enable Fkeys and Home Key => Super\_L
echo 12 > /sys/module/asusdec/parameters/key\_flags
chmod 666 /sys/class/backlight/pwm-backlight/brightness
# load bluetooth drivers
brcm\_patchram\_plus --enable\_hci --scopcm=0,2,0,0,0,0,0,0,0,0 --baudrate 3000000 --use\_baudrate\_for\_download --patchram /lib/firmware/BCM4330B1\_002.001.003.0967.0970.hcd --enable\_lpm --no2bytes --tosleep=5000 /dev/ttyHS2&

This will configure a few peculirities of the tablet.

Our first boot will take us to the console prompt. The NVIDIA provided binary blob for the graphic driver has the nasty habit of not being able to go back to console after starting the X server, so if anything goes wrong you cannot go back. For this reason, it is better to do the first boot text-only and switch on the X server only after you have everythinf sorted out.

## Setup networking

For the above mentioned issue, it is a good idea at this point to setup your network and your SSH server. If you followed all the instructions to setup the keyboard, you should have a working keyboard at this point in console, including the ESC key.

So, let's install and configure the network with NetworkManager (of course feel free to use any other method you prefer):

- start ssh: */etc/init.d/sshd start*
- add sshd st boot: *rc-update add sshd default*
- install network manager: *emerge networkmanager*
- now use nmcli to connect to your preferred wifi: *nmcli d wifi connect \<your AP> password \<your password>*

At this point if you loose the console for any reason you can always login remotely via SSH and reboot the tablet, no harm done to your microSD.



## Try out X

First of all we need to setup a basic xorg.conf file which will work, this is mine as an example, copy it in a file called **/etc/X11/xorg.conf**:

```
Section "ServerLayout"
    Identifier    "Layout0"
    Screen      0 "ScreenLVDS" 0 0
EndSection
Section "Extensions"
    Option "Composite" "Enable"
EndSection
Section "Module"
    Load  "glx"
    Load  "record"
    Load  "i2c"
EndSection
Section "Screen"
    Identifier    "ScreenLVDS"
    Device        "DeviceLVDS"
    Monitor       "MonitorLVDS"
    DefaultDepth   24
EndSection
Section "Device"
    Identifier    "DeviceLVDS"
    Driver        "tegra"
    Option        "ARGBHWCursor"
    Option	  "RenderAccel" "true"
    Option	  "AllowGLXWithComposite" "true"
    Option        "Monitor-HDMI-1"  "MonitorHDMI"
EndSection
Section "Monitor"
    Identifier    "MonitorLVDS"
    VendorName    "Nvidia"
    ModelName     "T33 Tegra3"
    Option        "DPMS" "true"
    DisplaySize   218 136
EndSection
Section "Monitor"
    Identifier    "MonitorHDMI"
    VendorName    "Nvidia"
    ModelName     "T33 Tegra3"
    Option        "DPMS" "true"
EndSection
Section "InputClass"
    Identifier       "catched touchpad"
    Driver           "multitouch"
    MatchDevicePath "/dev/input/event*"
    MatchProduct    "elantech_touchscreen"
    Option "CorePointer" "true"
EndSection
Section "InputClass"
    Identifier       "catched touchscreen"
    Driver           "mtev"
    MatchDevicePath "/dev/input/event*"
    MatchProduct    "elan-touchscreen"
    Option "CorePointer" "true"
EndSection
Section "InputClass"
    Identifier      "tf700 keyboard"
    Driver          "evdev"
    MatchDevicePath "/dev/input/event*"
    MatchIsKeyboard "true"
    MatchProduct    "asusdec"
    Option "AutoRepeat"   "200 50"
    Option "XkbModel" "pc104"
    Option "XkbLayout" "us"
EndSection
```
It will also configure the touchscreen and the trackpad. Yes you need also to specify the tegra driver as it will NOT be detected automatically.

At this point, type X and enjoy your first X server on the TF700T. If everything goes well, you should get an X working! Type CTRL+ALT+BACKSPACE (or type CTRL+ALT+F1 to switch back to console and then type CTRL+C). Of course your console will be borked and unsuable, but if you just type **reboot** it should reboot...

So if you got the working X, you can proceed. If, otherwise, you got errors check your **/var/log/Xorg.0.log** file for errors and fix them. If you got a garbled/unusable screen, well, check a few things:

- you are using an ARMv7 hardfp Gentoo stage3 (a softfp will NOT work)
- the tegra\_drv.so you are using matches the same ABI of your xorg (ex. xorg v.12 requires the v.12 tegra\_drv.so)
- ... try to look for help, but it will be hard, since not many and playing with Gentoo on the TF700T. Still the Gentoo forums might be a good place to start at least to work out possible Gentoo-general issues.



## Install LXDE and slim

I assume you can now start a working X server, well you now need somthing to use it with.

Due to the limited RAM and slow storage, my recomendation goes to a clean LXDE installation. As a bonus point LXTerminal seems to be basically the only terminal which adapts properly to the odd DPI size of the tablet!

Also we need a login manager. For some reason loggin in is a slow process, with at least one or two seconds delay between username and password... i guess it is due to the slow storage, but i find the best way to overcome this is setup an autologging display manager. Yes some of you might don't like it, just skip the "slim" part and propose here your valid alternative.

So, let's proceed, sfter you have your WiFi turned on of course:

- Install LXDE, be patient, take a bath, have dinner and sleep a bit: *emerge lxde-meta* (you might have to unmask some packages here for arm)
- Install slim: *emerge slim*
- Set slim as your default XDM in */etc/conf.d/xdm*
- Configure slim for autologin by editing file */etc/slim.conf*, check the two values **default\_user** and **auto-login**

It is worth to do one last experiment before setting slim to start on boot, try it manually:

- */etc/init.d/xdm start*

If all goes well, you are in your new shimy LXDE session! At this point we can setup automatic X startup at boot:

- *rc-update add xdm default*

Now you can play around with X... you will immediately notice a few things like, there is no battery indicator, then you close the lid strange things will start happening on the screen, and so on. Read on to fix these issues.



## Fixing the display for touch interaction

## Lock screen and bind keys

You don't notice on Android, but then you close the lid Android will automatically lock your screen, which means actually it will lock your TOUCH screen. Linux does not do it for you, so you must set it up manually.

Also, it is handy to be able to use those "useless" volume keys for example to regulate screen brightness. 
We can use **xbindkeys** to achieve all this in combination with **xinput** and a bit of bash scripting as glue: the basic idea is to have xbindkeys react ti the keycode and invoke the script which will use xinput to enable or disable the screen.

First of all install the tools: *emerge xbindkeys xinput*.

Create the script **/etc/tf700-lock-screen.sh**:

#!/bin/bash
if \[ $(xinput --list-props 8 | grep Enabled | cut -d: -f 2) -gt 0 \]
then
	xinput disable 8
	xinput disable 9
	cat /sys/class/backlight/pwm-backlight/brightness > /tmp/.lock\_touchscreen\_last\_brightness
	echo 0 >> /sys/class/backlight/pwm-backlight/brightness
else
	LAST\_BRIGHTNESS=100
	if \[ -e /tmp/.lock\_touchscreen\_last\_brightness \]
	then
		LAST\_BRIGHTNESS=$(cat /tmp/.lock\_touchscreen\_last\_brightness)
	fi
	xinput enable 8
	xinput enable 9
	echo $LAST\_BRIGHTNESS >> /sys/class/backlight/pwm-backlight/brightness
fi

This script will lock the screen by disabling touchpad and touchscreen, and turns off the screen by setting the brightness to zero.

By the way this is the only way to turn your screen black, beware **DO NOT** use any screensaver, and **DO NOT** use any apm/dpms related methods to turn your diplay off, as 75% of the times this will hang your tablet and wil require an hard-reset to reboot!


Create the script **/etc/tf700/brightness.sh**:

#!/bin/bash
CURRENT=$(cat /sys/class/backlight/pwm-backlight/brightness)
MIN=5
MAX=$(cat /sys/class/backlight/pwm-backlight/max\_brightness)
if \[ "$1" == "" \]
then
	echo $CURRENT
else
	if \[ $1 -gt 0 \]
	then
		CURRENT=$(expr $CURRENT + $1 )
	elif \[ $1 -lt 0 \]
	then
		CURRENT=$(expr $CURRENT + $1 )
	fi
	if \[ $CURRENT -ge $MIN -a $CURRENT -le $MAX \]
	then
		echo $CURRENT >> /sys/class/backlight/pwm-backlight/brightness
	fi
fi

This script will let you turn up and down the screen brightness. If invoked with no parameters will show the current brightess level, while invoked with a number, it will be used to increase or decrease the brightness, between the minimum of 5 and the hardware maximum value.

To bind these scripts to the power button and the sound volume buttins, add this to your **\~/.xbindkeys** file:

```
"/etc/tf700/lock_touchpad.sh"
    m:0x0 + c:124
    XF86PowerOff
"/etc/tf700/lock_touchpad.sh"
    m:0x0 + c:150
    XF86Sleep
"/etc/tf700/brightness.sh -10"
    m:0x0 + c:122
    XF86AudioRaiseVolume
"/etc/tf700/brightness.sh 10"
    m:0x0 + c:123
    F12
```
Then add **xbindkeys** to LXDE startup, and now you can safely close yout lid, just remmber to press the lock button before!

## Conky

The proprietary NVIDIA drivers provides you with many more details like battery status... We would like to know how much battery we have left! I find **conky** a nifty little tool, so let's use it here:

## Install mixer and power management

emerge pnmixer and xfce4-power-manager

We need some specific configuration here for both.

## Make Chromium behaves better

Chromium can be setup to operate on touch screen, which is so much more cool. You need to set a specific value to --touch-devices, which depends on the xinput list of inputs. Unfortunately this value changes when you plug a USB mouse or a Bluetooth device...

So we need first to find the correct id, then set it up. Chromium is installed with the chromium-browser launch script which will check the /etc/chromium/defaults script first. Here is the correct place for this script:

```
tf700t ~ # cat /etc/chromium/default
TOUCH_SCREEN_ID=$(xinput list | awk '/elan-touchscreen/ { gsub(".*id=",""); print $1 }')
CHROMIUM_FLAGS="--touch-devices="$TOUCH_SCREEN_ID
echo TouchScreen ID=$TOUCH_SCREEN_ID
```
Enjoy a touch-screen enabled Chromium!

(i used to prefer Firefox, but it is really unusable on this device. It does not play well with hi-dpi leaving you with awfully small and unusable GUI items, it does not play well with touch screen at all, so no finger scroll and such, it do show a lot of graphical glitches and broken fonts, i guess due to endinaess issues)

## Improve OpenBox for touch screen

change window border size so that you can resize the windows with your finger.
Go to */usr/share/themes* and pick your theme, edit the *themerc* file, check any **border.width** and adapt it to your needs.



## Enabling WiFi

It works out of the box. NetworkManager works pretty well, and i suggest the nm-applet to go with lxpanel.

I don't know if it is really needed, but copying from the NVIDIA distribution i have added the following udev rules file (/etc/udev/rules.d/99-nvidia.rules):

ACTION=="remove" GOTO="nvidia\_end"
# root only devices
KERNEL=="knvrm" OWNER="root" GROUP="root" MODE="0660"
KERNEL=="knvmap" OWNER="root" GROUP="root" MODE="0660"
# graphics devices
KERNEL=="nvmap" OWNER="root" GROUP="video" MODE="0660"
KERNEL=="nvram" OWNER="root" GROUP="video" MODE="0660"
KERNEL=="nvhdcp\*" OWNER="root" GROUP="video" MODE="0660"
KERNEL=="nvhost\*" OWNER="root" GROUP="video" MODE="0660"
KERNEL=="tegra\*" OWNER="root" GROUP="video" MODE="0660"
# camera and related devices
KERNEL=="torch" OWNER="root" GROUP="video" MODE="0660"
KERNEL=="ov\*" OWNER="root" GROUP="video" MODE="0660"
KERNEL=="focuser\*" OWNER="root" GROUP="video" MODE="0660"
KERNEL=="sh5\*" OWNER="root" GROUP="video" MODE="0660"
KERNEL=="tps\*" OWNER="root" GROUP="video" MODE="0660"
LABEL="nvidia\_end"



## Enabling bluetooth

Due to the ancient kernel we are stuck with, using Bluez5 is out of the question. You must use Bluez4, which works quite well with blueman up to version 1.23. Not much is needed, except masking / unmasking the proper packages and set some use flags. All needed is already specified above.

In order to actually make bluetooth work you need to load the firmware, which can be done with this line:

brcm\_patchram\_plus -d --enable\_hci --scopcm=0,2,0,0,0,0,0,0,0,0 --baudrate 3000000 --use\_baudrate\_for\_download --patchram /lib/firmware/BCM4330B1\_002.001.003.0967.0970.hcd --enable\_lpm --no2bytes --tosleep=5000 /dev/ttyHS2 &> /root/brcm\_log &

which you can add to your /etc/local.d folder (see README in that folder)

Of course you need to populate your /lib/firmware folder with the right files which can be found in the NVIDIA provided binaries or you can download it from my website [here](http://www.gardiol.org/tf700/firmware.tar.bz2). Just copy the files to /lib/firmware.

## Enabling audio

Unfortunately, it does not wor out of the box. Asus provides an alsa driver, but the first card is the HDMI audio output, which is de-facto useless. So we need to exchange the two cards. More over, the card does not have an hardware mixer nor hardware volume control, so we need to emulate both via software. Luckly Alsa let's us do it quite simply.

Emerge alsa-utils and create /etc/asound.conf file with this content:

```
pcm.!default
{
	type plug
	slave.pcm "softvol"
}
pcm.tegra
{ 
	type hw 
	card 1 
	device 0 
}
pcm.dmixer
{
	type dmix
	ipc_key 1024
	slave 
	{
		pcm "hw:1,0"
	}
}
pcm.softvol
{
	type softvol
	slave
	{
		pcm "dmixer"
	}
	control 
	{
		name "Master"
		card 1
	}
}
```
This will create a software volume control called Master and a virtual default sound card which uses the dmix and softvol plugins to activate software mixing and volume control.

(i did some experiments with alsa equalizer, but went nowhere as it seems at the moment there is some problems with the packages at least for ARM)
