<!-- source: https://wiki.gentoo.org/wiki/Kernel/Migrate_2.4_to_2.6 | group: Gentoo Wiki (Main) | wiki-title: Kernel/Migrate 2.4 to 2.6 -->
---
title: Kernel/Migrate 2.4 to 2.6
url: https://wiki.gentoo.org/wiki/Kernel/Migrate_2.4_to_2.6
hostname: gentoo.org
sitename: Kernel/Migrate 2.4 to 2.6
date: "2021-11-12"
fingerprint: "1793395f99b73dac"
license: CC BY-SA 4.0
---

# Kernel/Migrate 2.4 to 2.6

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

This article is **deprecated (obsolete)**. Contents are <u>no longer relevant</u>, and are intended for historical reference only!

TLDR:

**Do not use this article!**

This document will aid you in the process of migrating from Linux 2.4 to Linux 2.6, devfs to udev, OSS to ALSA, and LVM to LVM2.

## Introduction

### What is new in Linux 2.6?

That is not an easy question to answer. Linux 2.6 is the result of over 2 years of rapid development and stabilization of new features, and is architecturally quite different from its 2.4 counterpart. Some of the major changes are listed below:

- Scheduler/Interactivity improvements: Linux feels very smooth on desktop systems and copes much better than 2.4 while under load
- Scalability: Linux now scales much better at both ends - on small embedded devices and also systems with many processors
- Performance: Throughput from common applications is much improved
- Hardware support: Linux now supports many more architectures and hardware devices out-of-the-box than any other operating system.

Joseph Pranevich has written a very detailed document, [The Wonderful World Of Linux 2.6](http://www.kniggit.net/wwol26.html) which you may be interested to glance over. If you are interested in the more technical details, you can refer to [The post-halloween document](http://www.codemonkey.org.uk/docs/post-halloween-2.6.txt) - but do bear in mind that this is somewhat outdated now.

### Gentoo and 2.6

From the 2005.0 release of Gentoo Linux, the default kernel is the latest 2.6 series kernel. The default profile of the system is picked up from /etc/portage/make.profile which on 2005.0 or above would be a symlink to /usr/portage/profiles/default-linux/x86/2005.0 under which various important settings are taken care of.

If you are using a 2.4 profile, please change it to one that supports a 2.6 kernel by doing the following.

`root #````
rm -f /etc/portage/make.profile
```
`root #``ln -snf /usr/portage/profiles/default-linux/x86/2005.1 /etc/portage/make.profile`
### What is udev?

In the past, Gentoo has instructed users to use *devfs* for managing the /dev directory, which contains a series of device interfaces to allow system applications to communicate with hardware (through the kernel).

*devfs* , whilst a good concept, has some internal problems, and has been completely removed from the main stable tree as of 2.6.13.

*udev* is the new way of managing device nodes. It addresses issues with previous device managers, and also attempts to solve some other problems.

The above may not mean much to you, but fear not, the hard working Gentoo developers have put effort into making the migration from devfs very easy. Please read the [Udev article](https://wiki.gentoo.org/wiki/Udev) for more information regarding the same.

### What is ALSA?

With Linux 2.4, chances are that you used OSS (open sound system) drivers to power your sound card. OSS has been replaced by a newer and better set of sound drivers: ALSA.

ALSA, the Advanced Linux Sound Architecture, is a new set of sound drivers with a new and improved API, present in the Linux 2.6 kernel. It is backwards compatible with OSS applications, provided that you select the right kernel configuration options!

### What is LVM?

*Logical Volume Management* (LVM) exists as set of tools allowing you to manage your disk storage in a very flexible manner. Amongst other things, it allows powerful control over partitions (e.g. resizing without reboot), and makes operations like device changes relatively simple. LVM acts as an alternative to standard partition-based disk management.

LVM support has historically been implemented in Linux 2.4. Linux 2.6 features a new version of LVM, named *LVM2* . The migration process requires you to install new versions of the user-level tools (covered later in this document) but will leave your data intact!

*If you do not currently use LVM for data-storage management, then LVM2 migration does not apply to you.* If this is the case, you can safely ignore any parts of this document referencing LVM/LVM2. Upgrading to Linux 2.6 does *not* require you to store your data on LVM partitions - you can keep your data in the standard partition-format as it always has been.

If you are not a LVM user, but you think LVM2 sounds useful for you, you can convert your disks to this format at a later date, by following the [LVM article](https://wiki.gentoo.org/wiki/LVM). For now, let us just concentrate on getting a smooth 2.6 migration underway.

## Preparation

### Get your system up-to-date

Some of the changes brought in with Linux 2.6 also required some changes in the base system applications. Before continuing, you should ensure that your system is relatively up-to-date, and to be perfectly sure, you should update all world and system packages where updates are available.

In particular, make sure you have the latest stable versions of the following packages:

- [sys-apps/baselayout](https://packages.gentoo.org/packages/sys-apps/baselayout)
- [sys-apps/util-linux](https://packages.gentoo.org/packages/sys-apps/util-linux)
- [sys-kernel/genkernel](https://packages.gentoo.org/packages/sys-kernel/genkernel) (only if you wish to use genkernel as opposed to manual configuration)

`root #````
emerge --sync
```
`root #``emerge -uaDN world`
### modutils vs module-init-tools

`sys-apps/modutils` is the package that provides tools such as`modprobe` ,`rmmod` and`insmod` for Linux 2.4.

Linux 2.6 introduces a new module format, and therefore requires new tools for handling modules. These are bundled up into the `sys-apps/module-init-tools` package.

You should now remove modutils and install module-init-tools:

`root #``emerge --ask --depclean --verbose sys-apps/modutils``root #``emerge module-init-tools`
### Installing udev

There is no configuration involved here. Simply use `emerge` to install udev:

`root #``emerge --ask udev`
You should now read the [Udev article](https://wiki.gentoo.org/wiki/Udev) to get a more complete idea about the differences between udev and devfs.

### Checking for essential device nodes

When the system boots up, the system requires some essential device nodes. As udev is not included in the kernel, it is not activated immediately. To work around this, you must ensure that you have some essential device nodes on your disk.

Our installation stage files will have created the required devices during the initial installation. However, some users have reported that this is not the case. We will use this opportunity to check that the device files exist, and create them if they do not.

As your existing device manager will be mounted at /dev , we cannot access it directly. So we will bind-mount your root partition to another location and access the /dev directory from there.

`root #````
mkdir -p /mnt/temp
```
`root #````
mount --rbind / /mnt/temp
```
`root #````
cd /mnt/temp/dev
```
`root #``ls -l console null`
If the above *ls* command reported that either `console` or `null` do not exist, then you must create them yourself, as shown below.

`root #````
mknod -m 660 console c 5 1
```
`root #``mknod -m 660 null c 1 3`
You should now unmount your bind-mounted root partition, even if you did not have to create those devices:

`root #````
cd
```
`root #````
umount /mnt/temp
```
`root #``rmdir /mnt/temp`
### Installing ALSA utilities

ALSA requires you to have some packages installed, so that applications can use the ALSA API. These packages will also allow you to control the mixer and volume levels. Install the required utilities as follows:

`root #``emerge --ask alsa-lib alsa-utils alsa-tools alsa-headers alsa-oss`
## Installing the Linux 2.6 sources

### Choosing and installing a kernel

The first thing you need to do is install sources of a 2.6 kernel of your choice. The two Gentoo-supported 2.6 kernels are currently *gentoo-sources* (for desktops) and *hardened-sources* (for servers). There are others available, see the \[\[Kernel/Overview|Gentoo Linux Kernel Guide\] \] for more choices.

In this guide, we'll use `gentoo-sources` as an example. Install your chosen set of kernel sources using the `emerge` utility:

`root #``emerge -a gentoo-sources`
These are the packages that I would merge, in order:
Calculating dependencies ...done!
\[ebuild  NS   \] sys-kernel/gentoo-sources-2.6.10-r4
  
Do you want me to merge these packages? \[Yes/No\] y

When performing the above `emerge` command, if you find that it wants to install a 2.4 version of gentoo-sources (e.g. `gentoo-sources-2.4.26` ), then it means that the profile you are using is not designed for Linux 2.6 users. Please follow the [Gentoo Upgrading Document](https://wiki.gentoo.org/wiki/Upgrading_Gentoo) to switch to a 2.6-based profile, and retry installing 2.6 kernel sources.

### Updating the /usr/src/linux symbolic link

Various components of the Gentoo utilities rely on /usr/src/linux being a symbolic link to the kernel sources that you are running (or wish to compile against).

We will now update our /usr/src/linux link to point at the kernel sources we just installed. Continuing our example:

`root #````
cd /usr/src
```
`root #``ln -sfn linux-2.6.10-gentoo-r4 linux`
## Known pitfalls with Linux 2.6 migration

Before we get stuck into configuring the kernel, I'll attempt to detail the most common errors that people make when migrating to Linux 2.6, as some of these points will influence the way you configure the new kernel.

### Don't use "make oldconfig" with a 2.4 .config

You'll be asked many many questions, since there have been a large amount of changes. Many people who do try a `make oldconfig` from a 2.4 config end up creating an unworkable kernel (e.g. no output on-screen, no input from keyboard, etc). Please save yourself the trouble, and use the traditional `menuconfig` configuration method just this once.

### Don't use ide-scsi for CD/DVD writing

In Linux 2.4, the only way to achieve good CD/DVD writing results was to enable the (rather ugly) `ide-scsi` emulation. Thankfully, the IDE layer in Linux 2.6 has been extended to support CD/DVD writers much better.

You don't need to enable any extra options to support CD writing. Just be sure *not* to enable `ide-scsi` as you used to.

### PC Speaker is now a configurable option

You won't get your normal console beeps (or any response from the PC speaker at all) unless you specifically enable the new PC speaker option ( `CONFIG_INPUT_PCSPKR` ):

**Location of PC speaker option**

### New USB Storage block device driver sometimes problematic

Very recently, a new USB storage device driver has been added to the kernel. At the time of writing, this driver ("ub") is still in its early stages and some users find it to be unreliable. If you have problems accessing your USB hard disk, USB flash disk, USB card reader, or USB digital camera, then you could try reverting to the older SCSI-style driver:

**Disabling ub**

### usbdevfs renamed to usbfs

If you have edited your /etc/fstab file to customise the way that the USB device filesystem gets mounted, you may have to modify the filesystem type from *usbdevfs* to *usbfs*.

### Don't renice X

If you are a desktop 2.4 user, you may have hacked your system into running X at a higher priority, as in some cases it seems to provide better desktop performance.

There have been many scheduler changes in 2.6 which change this behavior. If you continue to run X at a higher priority, it will do exactly what it is supposed to (run the *display server* at a very high priority) and you will notice consequences such as sound stuttering and slow application load times because your CPU is spending too long serving X and only X.

In Linux 2.6, you no longer need to renice desktop applications to get good interactivity. Please remove your "niceness" hacks!

### X11 config file should now use /dev/input/mice for PS/2 and USB mice

One of the changes that a default udev configuration introduces is different organisation of the mouse device nodes. Previously, you would have had nodes such as /dev/psaux and /dev/mouse . You will now have nodes such as /dev/input/mouse0 , /dev/input/mouse1 , and a collective /dev/input/mice node which combines movements from all mice.

Since the old X configurations typically reference /dev/mouse or /dev/psaux then you may get an error similar to the one shown below when you attempt to start X11:

(EE) xf86OpenSerial: Cannot open device /dev/mouse
  No such file or directory.
(EE) Mouse0: cannot open input device
(EE) PreInit failed for input device "Mouse0"
No core pointer

To correct this, open your X11 config in a text editor, and update the mouse *InputDevice* section to use the /dev/input/mice device. An example is shown below:

`root #``nano -w /etc/X11/xorg.conf`
**`/etc/X11/XF86Config`**

```
Section "InputDevice"
  Identifier  "Mouse0"
  Driver      "mouse"
  Option      "Protocol" "auto"
  Option      "Device" "/dev/input/mice"
EndSection
```
### New Serial-ATA (SATA) drivers name the devices differently

If you used the original Serial ATA drivers under Linux 2.4, you probably observed your SATA devices having names such as /dev/hde .

Linux 2.6 introduces some new SATA drivers (libata) which are based on the SCSI subsystem. As these drivers are based on SCSI, your SATA disks will now show up as SCSI devices. Your first SATA disk will be named /dev/sda . You will need to update your /etc/fstab file to reflect this, and you will need to bear this in mind when choosing the root/real\_root kernel boot parameter later on.

### bootsplash no longer maintained

If you used the `gentoo-sources-2.4` kernel, you may have used the *bootsplash* functionality in order to provide yourself with a colourful framebuffer console.

The developer of bootsplash appears to have lost interest in his project, given some design problems. However, Gentoo developer *MichaÅ‚ Januszewski* is developing a successor, `gensplash` , which in included in the gentoo-sources-2.6 kernel. You can follow MichaÅ‚'s [Gensplash in 5 easy steps](http://dev.gentoo.org/~spock/projects/gensplash/archive/gensplash-in-5-easy-steps.txt) document in order to familiarize yourself with how gensplash is operated.

### I2C drivers now included in the kernel

If you use `lm-sensors` to monitor system temperatures and power levels, you previously needed to install the `i2c` package in order to provide hardware support.

The I2C hardware drivers are now included in the Linux 2.6 kernel, no external i2c package is required. Remember to compile support for your specific I2C devices into the kernel configuration. You will then be able to use `lm-sensors` as usual.

## Configuring, building, and installing the kernel

As with Linux 2.4, you have two options for managing your new kernel build.

1. The default method is to configure your kernel manually. This may seem daunting but is the preferred way as long as you know your system. If you wish to configure your new kernel manually, please continue on to the [#manual](https://wiki.gentoo.org#manual) .

1. The alternative option is to use our `genkernel` utility to automatically configure, compile, and install a kernel for you. If you wish to use `genkernel` then skip over the next chapter and proceed with [#genkernel](https://wiki.gentoo.org#genkernel) .

## Default: Manual configuration

### Configuring the kernel

We'll now get on with configuring the kernel. Open menuconfig in the usual way:

`root #````
cd /usr/src/linux
```
`root #``make menuconfig`
You will probably be familiar with using menuconfig from configuring 2.4 kernels. Fortunately, the front end has barely changed at all, but you will observe much better organisation of kernel options, plus *many* new options that weren't present in 2.4.

Be sure to enable the following important kernel options:

**Required kernel options**

Also, remember to enable support for the filesystems that you use, and the hardware present in your system. Be sure to enable support for the IDE controller on your motherboard if you wish to benefit from fast DMA disk access. Refer to the \[\[Handbook:X86/Installation/Kernel|Configuring the Kernel\] section of the [Gentoo Handbook](https://wiki.gentoo.org/wiki/Handbook:Main_Page) for additional guidance here.

### Building the kernel

Now that we have configured the kernel, we can start the compilation process:

`root #``make && make modules_install`
Wait for the kernel compilation to complete (and observe the much more readable compilation output).

### Installing the kernel

The next step is mounting your /boot partition and copying the kernel image over. You must then update your bootloader config manually.

`root #````
mount /boot
```
`root #``cp arch/i386/boot/bzImage /boot/bzImage-2.6.10-gentoo-r4`
Note that the above instructions are examples only, you should follow your usual procedure of updating kernels by following the instructions in the [Gentoo Handbook](https://wiki.gentoo.org/wiki/Handbook:Main_Page) (see the [Configuring the Kernel](https://wiki.gentoo.org/wiki/Handbook:X86/Installation/Kernel) chapter).

When updating your bootloader config, do not remove the old entry pointing at your 2.4 kernel. This way, you will easily be able to switch between the two if something is not working.

Now continue onto the modules section.

## Alternative: Using genkernel

If you prefer to use genkernel instead of manually configuring your kernel, you will be happy to hear that using genkernel to produce 2.6 kernels is very similar to the process you performed when producing your previous 2.4 kernel.

You should invoke genkernel as shown below:

`root #``genkernel --udev --menuconfig --bootloader=grub all`
In the above example, we also take advantage of genkernel features to open menuconfig to allow you to customise the kernel configuration (if you wish), and to update the grub bootloader configuration after compilation.

You should choose genkernel arguments that suit you, but do not forget to include the `--udev` argument! Refer to the [Gentoo Linux Genkernel Guide](https://wiki.gentoo.org/wiki/Genkernel) and the [Configuring the Kernel](https://wiki.gentoo.org/wiki/Handbook:X86/Installation/Kernel) chapter of the [Gentoo Handbook](https://wiki.gentoo.org/wiki/Handbook:Main_Page) for additional information.

If you choose to update your bootloader config yourself, then you must remember to include the `udev` kernel parameter. A sample *grub* config section is shown below, but remember to adjust the *root* parameter for the system.

**`grub.conf`**

**Sample GRUB config for genkernel + udev**

## Module Configuration

### Installing external modules

Many users will additionally rely on kernel modules that are built outside of the kernel tree. Common examples are the binary ATI and Nvidia graphics drivers. You now need to install those modules, which will compile against the 2.6 sources found at /usr/src/linux . This is the usual case of `emerge packagename` for all the external modules you are used to using with 2.4.

Refer again to the [Configuring the Kernel](https://wiki.gentoo.org/wiki/Handbook:X86/Installation/Kernel) chapter of the [Gentoo Handbook](https://wiki.gentoo.org/wiki/Handbook:Main_Page) for more info.

### Autoloading modules

You may have decided to compile some kernel components as modules (as opposed to compiled directly into the kernel) and would like to have them autoloaded on bootup like you did with 2.4. Also, if you installed any external modules from the portage tree (as described above) you will probably want to autoload them too.

To achieve this, edit /etc/conf.d/modules in your favorite text editor and list the names of the modules you would like autoloaded.

`root #``nano -w /etc/conf.d/modules`
**`/etc/conf.d/modules`**

**Sample configuration entry to load the 3c59x and nvidia modules**

## LVM to LVM2 migration

### Upgrading to LVM2 tools

Fortunately, upgrading from the LVM1 user tools to the LVM2 versions is very simple:

`root #``emerge --ask --depclean --verbose lvm-user``root #``emerge lvm2`
## Booting into Linux 2.6

It's now time to boot into Linux 2.6. Close all applications and reboot:

`root #````
umount /boot
```
`root #``reboot`
When you reboot, if you followed this document correctly so far, you will have the option of either loading Linux 2.4 or Linux 2.6 from your bootloader. Choose Linux 2.6.

Once the system has booted, check that things are working. If you made a mistake in the kernel configuration, don't worry, you can skip back to the [#conf](https://wiki.gentoo.org#conf) section, make your change, recompile and install new kernel image, reboot, and try again!

### Configuring and unmuting ALSA

We will now complete the ALSA configuration and unmute the audio channels. The ALSA packages provide a useful utility to make this process relatively simple:

`root #``alsaconf`
The process is straightforward: allow the */etc/modules.d/alsa* file to be automatically updated, and then allow ALSA to be reloaded. alsaconf will then terminate, however you will need to run it multiple times if you have multiple sound devices installed in your system.

You should now add `alsasound` to your boot runlevel, so that volumes will be saved on shutdown and restored on bootup:

`root #``rc-update add alsasound boot`
### Any immediate problems?

At this stage you should refer back to the pitfalls section which may be helpful with any issues you encounter immediately.

## Header files and NPTL

By now you are running Linux 2.6 and hopefully have all issues ironed out. You should now update your Linux kernel header files and re-merge glibc so that userspace applications can take advantage of new Linux 2.6 features.

`root #``emerge -u linux-headers`
After updating your headers package, you should generally re-merge glibc. There is a new feature here that you may be interested in - NPTL. NPTL is a new threading model present in Linux 2.6, which features much quicker thread create and destroy times. This won't make much of a difference to most systems, but you may wish to enable it during this migration process! To enable NPTL, edit /etc/portage/make.conf , adding *nptl* to your USE variable.

Now re-merge glibc (you should do this even if you did not choose to enable NPTL).

`root #``emerge -a glibc`
If you enabled NPTL, existing binaries will not use it until they are recompiled. However, any binaries compiled from this point onwards *will* use NPTL. You may wish to recompile all binaries now, e.g.:

`root #``emerge -e world`
Alternatively, you can just let your system "naturally" convert itself to NPTL as you update to newer versions of packages when they are released.

## Closing remarks

### Problems?

With the incredible amount of work that went into Linux 2.6, it is sometimes inevitable that things which used to work fine, no longer function as expected.

If you have any problems with your 2.6 kernel, and you can confirm that this problem does not exist with Linux 2.4, then please open a bug with us on our [Bugzilla](https://bugs.gentoo.org) . We will investigate the issue, and if we find that it is a problem in the mainline kernel, we may then ask you to file a report at the central kernel bugzilla.

### Conclusion

Hopefully you have just completed a smooth migration and you are enjoying the benefits which Linux 2.6 brings over 2.4.

I would like to say a word of thanks to the many users who effectively 'tested' this document while it was in its early stages, and provided feedback about how the migration process went. Sorry that I did not reply to all the emails (there were a lot!), but I did read every one, and refined this document where appropriate. Enjoy your 2.6-enhanced systems :)

### Removing Linux 2.4 from your system

After you have been running 2.6 for a while, you may decide that you no longer have any requirement to be able to use Linux 2.4. The steps you can take to clean up your system are detailed below. *Only follow the procedure in this section if you are sure that you don't want/need to use 2.4 again!*

The 2.4 kernel source code can be removed, using the emerge utility as usual. For example, assuming you have 2.4 versions of vanilla-sources and gentoo-sources installed, you could use the following command to remove them while keeping the 2.6 versions intact:

`root #``emerge --ask --depclean --verbose =vanilla-sources-2.4.* =gentoo-sources-2.4.*`
Portage will not completely clean out your 2.4 kernel source installations, because some temporary files are created during compilation. It is safe to remove these remnants with the following command:

`root #``rm -rf /usr/src/linux-2.4.*`
You can additionally remove modules and information files relating to your old 2.4 kernel installations, as these are no longer needed.

`root #``rm -rf /lib/modules/2.4.*`
The 2.4 kernel binaries you used to boot from can also be safely removed. You should mount your `/boot` partition, and remove those images. You should also update your bootloader configuration so that it no longer references these deleted kernel images.

Some Linux 2.4 users will have previously installed the `alsa-driver` package to benefit from the new audio capabilities included in Linux 2.6. If you were one of these users, and you followed the advice given earlier in this document about building ALSA with the 2.6 kernel sources (as opposed to using the `alsa-driver` package), then you can safely remove this to prevent future conflicts.

Additionally, `lm-sensors` users will have previously used the `i2c` package to provide the hardware drivers. As already mentioned, I2C drivers are now included in the kernel, so this package can also be removed in order to prevent future conflicts.

The devfs management daemon, `devfsd` , can also safely be removed, now that we are using `udev` for device management.

`root #``emerge --ask --depclean --verbose alsa-driver i2c devfsd`
If you are LVM2 user, you may wish to convert your data into the LVM2 data format in order to benefit from the advantages which LVM2 provides. However, this operation will prevent you from ever accessing your LVM data from a 2.4 kernel. If you want to continue with the conversion (this is totally optional!), then you should examine the `vgconvert` man page for instructions on how to carry this out. An example is shown below, where `main` is the volume group name.

`root #``vgconvert -M2 main`
