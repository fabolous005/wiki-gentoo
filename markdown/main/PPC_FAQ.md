<!-- source: https://wiki.gentoo.org/wiki/PPC/FAQ | group: Gentoo Wiki (Main) | wiki-title: PPC/FAQ -->
---
title: PPC/FAQ
url: https://wiki.gentoo.org/wiki/PPC/FAQ
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-03-31"
fingerprint: "96871b1b49a229cd"
license: CC BY-SA 4.0
---

# PPC/FAQ

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)


The PPC FAQ is a collection of questions and answers collected from the Gentoo PPC mailing lists and from Internet Relay Chat (IRC). Readers seeking support beyond this article should do so via the [Gentoo forums](https://forums.gentoo.org/) or the [#gentoo-powerpc](ircs://irc.libera.chat/#gentoo-powerpc) ([webchat](https://web.libera.chat/#gentoo-powerpc)) channel on Libera.Chat IRC.

## Installation

### What kind of PowerPC computer do I have and where can I find more information about it?

Welcome to Gentoo PPC! The PowerPC processor has been used in a number of popular computing devices, here are some links to help you figure out what type of hardware is in the machine on your desk.

### How can I verify with OSX that the downloaded ISO is not corrupted?

First be sure you did not mount the ftp server with `Finder` and then copy the file with drag'n'drop. This will corrupt the ISO. Instead, use curl:

`user $``curl -O http://path/to/iso`
You can check your MD5 sum using the md5sum tool provided with OSX. On versions of OSX without md5sum, you can use openssl capability to produce an md5 checksum.

`user $``openssl md5 /path/to/iso`
Now compare this output with the appropriate file found on the server where you downloaded the ISO (the file will end with .md5 ). If it is the same, the ISO image downloaded correctly. Be sure you have *not* mounted it (e.g. with Disk Copy) yet!

### Can I dual-boot Gentoo Linux and Mac OS X?

Yes you can! In fact, yabootconfig will automatically detect and setup /etc/yaboot.conf with Linux, Mac OS X and even classic Mac OS (see the [PPC Gentoo Handbook](https://wiki.gentoo.org/wiki/Handbook:PPC/Installation/Bootloader) for details on yabootconfig). The trick is in how you partition your hard drive. Before installing Gentoo, startup with the Mac OS X Install CD. Use the Disk Utility to partition the drive in your machine into two or three partitions. Alternatively, use parted from a recent Gentoo InstallCD, since that has been patched to handle HFS and HFS+ partitions. Parted is also able to shrink a partition so you don't need to delete your existing install. The first partition should be big enough to contain all your Linux partitions (root+swap+/home etc.). The format of this partition does not matter. The second partition should be for OS X. The third (and optional) partition should be for classic Mac OS. It is only required if you intend of being able to triple boot, since OS9 must be on a separate partition from OSX for this to work.

After partitioning, install Mac OS X and Mac OS 9 onto their respective partitions. Once installation is complete, start following the [PPC Gentoo Handbook](https://wiki.gentoo.org/wiki/Handbook:PPC). When you get to the section on partitioning, use mac-fdisk to *delete* the large partition you created for Gentoo. In its place, create a bootstrap partition, root, swap, and any other partitions you wish to use with Gentoo.

If you have an OldWorld Macintosh (pre-iMac), follow a similar procedure as described above. However, use the Mac OS 9 install CD and the Drive Setup utility. OldWorld machines do not need a bootstrap partition.

### I have an early NewWorld Mac such as the Blue and White G3. It should be compatible with the InstallCD, but on boot it returns an "Unknown or corrupt filesystem" error.

As a workaround, boot into Open Firmware by holding down the `Apple`+`Option`+`O`+`F` keys on startup. When the prompt appears, type:

`boot``cd:,\\yaboot`
The CD should boot as expected now, thanks to John Plesmid for this workaround.

### I have an OldWorld Macintosh and need to use BootX. However, I am having trouble opening the BootX installer archive. What can I do?

You will need a new version of StuffIt Expander to open the archive. You can [download it](http://www.stuffit.com/expander/download.html) for free.

### I have a Beige G3 machine. Can I install Gentoo Linux/PPC on a second IDE hard drive?

Early Beige G3 machines (those with a "Rev 1" motherboard with an ATI RageII+ chip) do not support slave IDE drives. To use a second hard drive with this machine, you would have to attach it to the internal or external SCSI connector or install a PCI IDE card. Later G3 machines do not have this limitation. See [http://www.xlr8yourmac.com/G3-ZONE/IDE/index.html](http://www.xlr8yourmac.com/G3-ZONE/IDE/index.html) for more information.

### I have a revision A or B iMac. I've replaced the hard drive with a larger one and now I can't boot into anything!

There is an 8GB limit for the root partition on these machines. Ensure that your root partition is less than 8GB and it should work as expected. This also applies to OS X and later versions of OS 9.

### My /etc/fstab file has /dev/BOOT in it. Do I need to setup this line for my Apple bootstrap partition?

Maybe. If you are not using a separate boot partition ( /boot ) then you do not need this line and can safely just delete it from /etc/fstab The Apple bootstrap partition is *not* /boot and is not related in any way.

If you are using a partition type that's not natively supported by the bootloader (such as ext4 and yaboot), you will need to use a separate boot partition. In this case, you will want to specify /dev/BOOT in /etc/fstab.

### When I rebooted, yaboot didn't work! Now, I'm stuck. How can I fix this?

It is probably just a misconfigured yaboot.conf The easiest way to fix this is to boot the InstallCD again, mount the Gentoo partition and check yaboot.conf for errors. You'll also need to run ybin -v -C /mnt/gentoo/etc/yaboot.conf to update the boot loader once you have finished. This can all be done without chrooting back into your install.

### When I try to boot, yaboot only says "read error"! Why can't I boot?

You're probably using [XFS](https://wiki.gentoo.org/wiki/XFS) for the root partition. While this is an option, yaboot has some issues accessing files with holes. To ensure that your kernel will boot, instead of copying the vmlinux file to /boot, simply cat the kernel as shown below.

`root #``cat /usr/src/linux/vmlinux > /boot/vmlinux`
### I installed an OSX update and now my computer only boots into OSX! How can I fix it?

Hold down the option key on boot. This will bring you to a graphical boot device selector. Wait until a hard drive with a penguin shows up and click on the icon. This will boot Linux install as usual. Once you've booted, rerun ybin -vto fix the bootloader.

### I want to set the hardware clock from Open Firmware, is that possible?

To set the clock boot into Open Firmware (using the key combination of `Apple`+`O`+`F`). Once you are at the Open Firmware prompt, you can set the clock using the command below:

Open Firmware will return with an ok after the clock is set. Type `mac-boot` to continue booting after the clock is set.

### I have a NuBus (pre-PCI) Power Macintosh or Powerbook. Can I run Gentoo on it?

Official Linux support for NuBus PPC Macintoshs has been dropped starting with Kernel 2.6, so an Installation of Gentoo Linux is actually not possible. You might try the kernel sources provided by the [PPC/Linux for NuBus Power Macs Project](http://sourceforge.net/projects/nubus-pmac/) and combine them with the official Gentoo PPC32 stage archive. Share your experiences with the Gentoo community.

### I have an even older Powerbook or a Macintosh Quadra/Macintosh II/Performa/LC equipped with a Motorola (m68k) CPU. What about that?

Information for Linux in general and Gentoo in particular on these machines can be found on [m68k](https://wiki.gentoo.org/wiki/M68k).

## Hardware support

### I'm using a PC keyboard on my Mac. Which keys do I press to get the Apple equivalents?

| Mac key | PC key | 
|---|---|
| `Option` | `Alt` | 
| `Control` | `Ctrl` | 
| `Command` | `Windows` | 
| `Delete` | `Backspace` | 
| `Return` | `Enter` | 

### How do I open/eject the CD-ROM tray on my Mac in the OpenFirmware environment?

The CD-ROM can be opened by pressing and holding the left mouse button during the POST process.

### I'm running Gentoo on a PowerPC laptop. What packages should I install to enable laptop-specific functionality?

There are three main packages you will need to install:

- [app-laptop/pbbuttonsd](https://packages.gentoo.org/packages/app-laptop/pbbuttonsd)
- A tool that enables the brightness and volume keys on most PPC laptops as well as other Apple specific functionality. The current versions also work as a power manager and replaces pmud.

- [sys-power/powernowd](https://packages.gentoo.org/packages/sys-power/powernowd)
- allows CPU speed scaling for longer battery life. Other packages also provide the same functionality, such as cpudyn, cpufreqd, and speedfreq. It does not really matter which you chose, just ensure that you have the *userspace* CPU frequency scaling governor enabled in the kernel. This is the only governor that will work on PPC.

These packages can be installed by running the appropriate commands from the list below:

`root #``emerge --ask app-laptop/pbbuttonsd sys-power/powernowd`
It is useful to add pbbuttonsd and powernowd to the boot runlevel so that they are started when the computer boots. To do this on OpenRC issue:

`root #````
rc-update add pbbuttonsd boot
```
`root #````
rc-update add powernowd boot
```
There is no way to use the `Ctrl` key like on Mac OS X, but just about any key or combination of keys can be mapped to emulate the second and third mouse-button. The values can be obtained with the command line utility showkey from the console. Note that this program does not work as expected from X and should only be run from the console. To use this feature, enable the following options in the kernel:

**Macintosh Device Drivers**

The keyboard to mouse button mapping values will need to be set in /etc/sysctl.conf Here is an example using the `F11` and `F12` keys:

**`/etc/sysctl.conf`**

**Mouse button emulation with the F11 and F12 keys**

To activate these settings:

`root #``sysctl -p /etc/sysctl.conf`
Apple Powerbooks and iBooks that use the Appletouch driver, can alternatively configure multi-finger tapping to allow 2nd and 3rd button control. See the documentation for this driver for configuration options.

### How can I configure my Airport card?

The Airport card is an 802.11b device based on the Lucent Orinoco chipset. To use this card in Linux enable these kernel options:

**Wireless LAN**

Once the kernel is configured, userland tools will be needed to configure the wireless settings. Emerge the [net-wireless/wireless-tools](https://packages.gentoo.org/packages/net-wireless/wireless-tools) for iwconfig and other wireless tools.

As an example of using iwconfig, to set the ESSID to home\_essid:

`root #``iwconfig eth1 essid home_essid`
For a generic guide to setting up wireless with Gentoo, please see the [Wireless guide](https://wiki.gentoo.org/wiki/Handbook:PPC/Networking/Wireless) in the PPC Handbook.

### How can I configure my Airport Extreme card?

The Airport Extreme is an 802.11b/g card that is included with modern Apple portable computers. It is based on the Broadcom 43xx chipset which is supported by the native b43 driver project. It has been included in the 2.6.24 kernel. Note that the driver should select the proper driver between b43 and b43-legacy. The firmware used for these two drivers is different, with the legacy version supporting older 4306 cards. Systems with a 4306 card need to build both. Otherwise it should be safe to only build b43. To enable either driver, mac80211 must be selected first.

**Adding b43 support to the kernel**

After compiling the kernel, be sure to emerge the [net-wireless/b43-fwcutter](https://packages.gentoo.org/packages/net-wireless/b43-fwcutter) package to install the firmware and get a copy of the firmware. The instructions for this are found at: [linuxwireless.org](http://linuxwireless.org/en/users/Drivers/b43#Other_distributions_not_mentioned_above), and follow the directions for extracting the firmware.

For a generic guide to setting up wireless with Gentoo, please see the [Wireless guide](https://wiki.gentoo.org/wiki/Handbook:PPC/Networking/Wireless) in the PPC Handbook.

### Will the internal modem work?

On older Macs, the internal modem is a regular serial device. In addition to PPP support and I2C support, enable this kernel option to use it:

**Serial Drivers**

Newer Macs use a USB device which only has a driver from Linuxant. Unless you have purchased a key from Linuxant, this driver will only work at 14.4 and will not support FAX. To install the driver, emerge ~~[net-dialin/hcfusbmodem](https://packages.gentoo.org/packages/net-dialin/hcfusbmodem)~~ (this package is no longer in the Gentoo ebuild repository) which will provide /dev/ttySHCF0. Then set up PPP like usual. Further information about the installation can be found at [http://www.linuxant.com/drivers/hcf/install.html](http://www.linuxant.com/drivers/hcf/install.html)

### Does sleep work on my iBook/PowerBook?

Sleep used to work well with the User Mode Setting kernel drivers, but modern X servers using KMS do not sleep properly at this time.

### How do I get sound playback working?

There are two approaches to sound on modern kernels: OSS or ALSA. OSS is the older of the two options and has been deprecated, but still functions well for many older Macs. ALSA is the newer method and will continue to be supported in the future. We recommend that you use ALSA unless you have problems with it.

In addition to configuring the kernel, users who should have access to the audio devices need to be added to the appropriate group with the `gpasswd` command:

`root #``gpasswd -a username audio`
If the kernel is set up correctly, but you can't hear any sound, ensure that the main mixer volume is turned up and that the device is not muted!

### Sound setup with ALSA

For ALSA, we'll set up both the regular ALSA drivers and the OSS emulation layer so that we can still use legacy OSS only programs. Use make menuconfig to select the following:

**Kernel options for ALSA**

Depending on the age of the machine, the correct sound driver should also be enabled. On more modern machines such as Aluminum PowerBooks, new iBooks or G5's the `Apple On-board Audio` driver should be used. For older machines, try the ALSA PowerMac driver instead.

**ALSA Drivers - Choose only one**

You can also configure these as modules and ensure they're loaded on boot, but it's easier to simply compile it all in.

For more information on configuring ALSA, please see [ALSA](https://wiki.gentoo.org/wiki/ALSA).

### Sound setup with OSS

With make menuconfig, select the following:

**Kernel options for OSS**

Or configure as modules and ensure they're loaded on boot.

If the `dmasound_pmac` module does not load, try enabling `i2c-core` in the kernel or as a module.

### How do I make my Mac power up after a power failure?

The Power Management Unit (PMU) in most Macs has the ability to turn the Mac on again after a power loss. This feature is controlled by the PMU options found in /proc . To enable this feature, set `server_mode` to 1, to disable it, set `server_mode` to 0.

`root #``echo "server_mode=1" > /proc/pmu/options`
Unfortunately, this setting is turned off again after the machine restarts. To ensure that your system always starts with power on after power failure enabled, add the line above to /etc/conf.d/local.start

### I see lots of "IN from bad port" errors in my kernel dmesg, how do I fix that?

This is caused by drivers written for devices that you don't have on your system. These messages are usually harmless, but it's a good idea to disable the unused drivers anyway. See below to match up port numbers with kernel configuration options. Note that even if the numbers don't match, try disabling these kernel options anyway, most PPC machines don't use these drivers.

**IN from bad port 61 - Disable PC Speaker support**

**IN from bad port 64 - Disable i8042 keyboard support**

**IN from bad port 2E9,2F9,3E9,3F9 - Disable 8250/16550 support**

If you have another IN from bad port message that you're not sure about, please let us know!

## Applications

### I can't emerge a package, and get an error about it being masked. What should I do?

There are two common reasons why a package will not emerge: either it hasn't been tested on PPC or it has been tested, but hasn't yet been marked stable.

If your package has not yet been tested on PPC, you will need to first mark the ebuild with the **ppc** keyword. This is done using the ekeyword tool which is part of the [app-portage/gentoolkit-dev](https://packages.gentoo.org/packages/app-portage/gentoolkit-dev) package. Mark the ebuild **ppc** and regenerate the manifest file so that it reflects the changes:

`root #````
ekeyword ppc /var/db/repos/gentoo/app-foo/bar/bar-1.0.ebuild
```
`root #``ebuild /var/db/repos/gentoo/app-foo/bar/bar-1.0.ebuild manifest`
Once the ebuild has been keyworded, you can now emerge it as usual. Test the application and if it works, **please** report it as a bug so one of the developers can add the keyword.

If your package already has the **\~ppc** keyword, you have two options. Either follow the above instructions and replace the **\~ppc** keyword with **ppc**, or you can use /etc/portage/package.unmask to unmask the specific package. The second option is preferred simply because it will not be undone by an `emerge --sync`.

`root #``echo "=app-foo/bar-1.0" > /etc/portage/package.accept_keywords`
Now emerge the package as normal. Once you're sure the application is stable, **please** report this as a bug so the developers can add the keyword.

### Does Gentoo/PPC have a Java plugin?

Yes, a Java plugin is available with IBM's Java 1.6. Note that not all Java applications work with this Java version yet.

As of June 2015 a binary version of Icedtea (compliant to Java 6 and 7) built for Gentoo/PPC is now available.

Alternatively, you can [build Icedtea from source](https://forums.gentoo.org/viewtopic-t-901036-start-0.html).

### What is required for DRM?

When emerging [x11-base/xorg-server](https://packages.gentoo.org/packages/x11-base/xorg-server), make sure the `dri` USE flag is enabled. The `VIDEO_CARDS` variable should be set in /etc/portage/make.conf with a value aligning on the video card: `VIDEO_CARDS="radeon"` for Radeon support, `VIDEO_CARDS="nouveau"` for Nvidia support and so on. For a backup option, add `fbdev` to the list of `VIDEO_CARDS`, this driver should always work.

Make sure in the kernel configuration the following options are set:

**Rage128 DRM kernel configuration**

**Radeon DRM kernel configuration**

**nVidia kernel configuration**

### I've installed X.org and configured it correctly, but the radeon or nouveau driver is missing. What's wrong?

Add the appropriate `VIDEO_CARDS` value to /etc/portage/make.conf and recompile [x11-base/xorg-server](https://packages.gentoo.org/packages/x11-base/xorg-server) and [media-libs/mesa](https://packages.gentoo.org/packages/media-libs/mesa) to build the drivers.

### I have a VBIOS or "Invalid I/O allocation" error in my xorg.log. Is this a problem?

No, this is simply X.org probing your video card. It attempts to open the x86 only video BIOS and fails, leaving one of these messages. It's harmless, don't worry about it!

### When using OpenGL DRI or playing movies, the system sometimes freezes hard unexpectedly. What can I do?

Your system may not cope properly with AGP support. The easiest solution is to add the following line to the "Device" section of /etc/X11/xorg.conf

**`/etc/X11/xorg.conf`**

**Forcing PCI mode for AGP-Cards**

If you are still having trouble, please try disabling dri in the xorg.conf to see if the problems go away.

### I recently emerged Xorg, but when using it, my international keyboard layout does not work correctly. The main keys work fine but I am unable to compose special characters like the "at" sign. The startx output says something like "could not load keymap, falling back to default keymap". What is wrong?

Check that the permissions on /var/tmp are set to drwxrwxrwx. If not, change them by running chmod a+rwx /var/tmp. This will allow Xorg to load keymaps properly again.

### How do I play audio CDs?

NewWorld machines have no audio connection between their CD-ROM drive and their sound hardware. Therefore, audio information must be read as data and then piped to the sound hardware. This is possible using Audacious, which reads the audio off the CD over the ATA cable, decodes it in software and sends it to the sound card over the PCI bus as digital data.

`root #``emerge --ask media-sound/audacious`
To play a CD, just fire up audacious. If you're having playback trouble, you may need to open up the Preferences Plugins CD Audio Plugin menu and select the CD drive. Make sure Digital audio extraction is checked.

### Can I access my ext2/ext3 partitions from OSX?

OSX can support the reading and writing of ext2/ext3 partitions by using the kernel extension provided by the [ext2fsx project.](http://sourceforge.net/projects/ext2fsx)

### Which Portage USE flags do not work on PowerPC machines?

If you're using an old flat profile, (deprecated, pre 2005.0) look at /etc/portage/make.profile/use.mask for a list.

If you're using a cascaded profile, (2005.0 and onwards) look at /var/db/repos/gentoo/profiles/default-linux/ppc/use.mask for a list.

### How can I set up distcc with my x86 box?

If you've got another faster computer, you can set up distcc to compile binaries for your slower PPC processor. It is better to do as much building as possible on the faster machine; if possible, do not build on the slower computer. Read the [DistCC Cross-compiling article](https://wiki.gentoo.org/wiki/Distcc/Cross-Compiling) to learn how. You may also want to read the [Gentoo Embedded Handbook](https://wiki.gentoo.org/wiki/Embedded_Handbook) to learn more about cross-compiling.

## Resources

### Can I buy a CD of Gentoo Linux/PPC?

Unfortunately not. Currently, the stores listed on [Stores offering Gentoo products](https://www.gentoo.org/inside-gentoo/stores/) only offer media with Gentoo for the x86 and x86-64 (Intel 32-bit and 64-bit) platforms.

### This FAQ hasn't answered my question. What do I do now?

A good first step is to browse through the relevant PPC documentation here on [the Wiki](https://wiki.gentoo.org/wiki/PPC). The PPC-specific [mailing list](https://archives.gentoo.org/gentoo-powerpc/). [Gentoo on PPC](http://forums.gentoo.org/viewforum.php?f=24) (Forums topic) is an open bulletin-board for discussion of all PowerPC issues. Also, there is usually a lively discussion on IRC. Join us on [#gentoo-powerpc](ircs://irc.libera.chat/#gentoo-powerpc) ([webchat](https://web.libera.chat/#gentoo-powerpc)) on Libera.Chat IRC (`irc.libera.chat`).

General information about Linux on PowerPC can be found at [penguinppc.org](http://penguinppc.org/).
