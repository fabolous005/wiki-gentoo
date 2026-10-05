<!-- source: https://wiki.gentoo.org/wiki/Toshiba_Satellite_Pro_460CDT | group: Gentoo Wiki (Main) | wiki-title: Toshiba Satellite Pro 460CDT -->
---
title: Toshiba Satellite Pro 460CDT
url: https://wiki.gentoo.org/wiki/Toshiba_Satellite_Pro_460CDT
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2024-12-19"
fingerprint: fe859d5d59a239c4
license: CC BY-SA 4.0
---

# Toshiba Satellite Pro 460CDT

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

![Toshiba-satellite-pro-460CDT.jpg](https://wiki.gentoo.org/images/thumb/9/96/Toshiba-satellite-pro-460CDT.jpg/300px-Toshiba-satellite-pro-460CDT.jpg)

The Toshiba Satellite Pro 460CDT is a laptop manufactured in 1996 with a 32-bit, i586 Pentium MMX clocked at 165MHz. The laptop has multiple memory variants. It comes from 8MB to the maximum capacity of 160MB of RAM.

The point of this guide is to prove that not only is it possible to install a modern Linux system to such old hardware, but that it's also possible to use it for many tasks such as playing music files and audio CDs, advanced text-editing, text-mode web-browsing, chatting over the matrix protocol using gomuks and much more. All of this without having to resort to using linux systems that are over 20 years old or having to run proprietary windows operating systems.

This guide also assumes that the user has already installed gentoo before at least once, so that it can avoid repeating the x86 handbook.

## Hardware

| Device | Desc./model | Status | Kernel driver(s) | Kernel version | Notes | 
|---|---|---|---|---|---|
| CPU | Pentium MMX |  | N/A | 6.6.1 | Tested on 165MHz | 
| GPU + VGA port | Chips and Technologies F65554 |  | N/A | 6.6.1 | Modern linux kernels dont's seem to have a driver for this GPU. Works in tty. | 
| USB 1.1 Port | USB corporation |  | ohci-pci | 6.6.1 | Ethernet/Wifi adapters don't appear to work for more than 20 seconds until they disconnect. | 
| PS/2 Port | PS/2 mouse connector |  | N/A | 6.6.1 | - | 
| Internal modem | 33.6 /14.4 Kbps Data/Fax/Voice modem |  | N/A | 6.6.1 | Probably shouldn't be used anyway | 
| Parallel Port | 25-pin Parallel Port |  | N/A | 6.6.1 | - | 
| Serial Port | 9-pin Serial Port |  |  | 6.6.1 | - | 
| Infrared Port | - |  | N/A | 6.6.1 | No way to test this | 
| Keyboard | built-in keyboard |  | N/A | 6.6.1 | - | 
| TrackPoint | Built-in pointing device |  | N/A | 6.6.1 | Tested in TTY, works via [sys-libs/gpm](https://packages.gentoo.org/packages/sys-libs/gpm) | 
| Sound | Yamaha OPL3-SA3 |  | N/A | 6.6.1 | See the sound section. | 
| PCMCIA/CardBus | two 16-bit or 32-bit PCMCIA slots |  | yenta\_cardbus | 6.6.1 | - | 
| CD-drive | TOSHIBA XM-1502BN |  | N/A | 6.6.1 | /dev/sr0 | 

## System installation

It is recommended to use the [x86 handbook](https://wiki.gentoo.org/wiki/Handbook:X86) for reference. While the gentoo handbook is a great resource, there are many device-speciffic steps that need to be adressed.

#### Hardware Requirements

RAM: 16MB minimum, recommended 32MB or more
HDD: 2GB minimum, recommended 10GB or more 

USB to IDE adapter for installation on a different, more powerful computer (technically not required but compiling everything on the toshiba would take several weeks to complete).

#### Partitioning

It is necessary to use ext2 filesystem for the boot partition. Swap is also optional but recommended on systems with low memory. It is recommended to set 2x the size of the systems memory for the swap partition.

| Partition | Function | Filesystem | 
|---|---|---|
| /dev/sda1 | Boot Partition | ext2 | 
| /dev/sda2 | Swap Partition | swap | 
| /dev/sda3 | Root partition | ext4 | 

It may be also possible to make an encrypted installation + separated home directory, but that is outside of the scope of this guide. See [Dm-crypt\_full\_disk\_encryption](https://wiki.gentoo.org/wiki/Dm-crypt_full_disk_encryption).

### Choosing the right make.conf

A decision has to be made to optimize the binaries according to the hardware's needs. If the system for example has very limited RAM (16M) or low disk space (>2GB), then it is recommended to optimize GCC to compile smaller binaries. If however the there is not limited memory and disk space (64MB, \<2GB), it's recommended to optimse GCC to compile larger, but more performant binaries. Both of these configs have been tested as of 10. 12. 2023.

Note that the following config **drastically** decreases runtime performance. Use only with extremely limited resources.

```
CFLAGS="-march=pentium-mmx -Os -flto -fomit-frame-pointer -pipe"
USE="minimal alsa lto"
ACCEPT_LICENSE="@FREE" #for those who wish to have 100% fully free system
```
The following config is recommended for everyone who has at least 32MB of ram and 2GB of disk space:

Optimize for performance:

```
CFLAGS="-march=pentium-mmx -O2 -flto -pipe"
USE="minimal alsa lto"
ACCEPT_LICENSE="@FREE" #for those who wish to have 100% fully free system
```
**`/etc/portage/package.use/00cpu-flags`**

```
 CPU_FLAGS_X86: mmx
```
**`/etc/portage/package.use/00grub`**

```
 GRUB_PLATFORMS: pc # For use with the grub bootloader. Lilo users can skip this.
```
Note to CFLAGS

`-O2` The default optimization for GCC, faster CPU runtime at the cost of more RAM usage.

`-O3` Can improve performance at the cost of higher memory usage, unlikely to help with a Pentium MMX CPU.

`-Os` is a flag designed to reduce code size and therefore memory usage as well. On this particular laptop it dramatically decreases performance, although that may not be the case on different hardware

There is also a more aggressive `-Oz` flag, but this flag can significantly degrade runtime performance and therefore it's only recommended to be used if disk space or memory capacity are extremely limited.

`-fomit-frame-pointer` is a flag which is also designed to reduce code size. This flag is enabled by default on all -O levels, but on x86\_32 in combination with -Os it must be enabled manually.

From [GCC optimization](https://wiki.gentoo.org/wiki/GCC_optimization#-fomit-frame-pointer): "It's still necessary to explicitly enable the -fomit-frame-pointer option, to activate it on x86-32 with GCC up to version 4.6, or when using -Os on x86-32 with any version of GCC. However, using -fomit-frame-pointer will make debugging hard or impossible."

`-flto`: Link Time Optimisation which significantly increase performance and lower RAM usage at the cost of more compilation time. Since performance a greater concern with this hardware than compilation times, we believe it's useful to enable this flag. `-flto` should not be compiled natively on the hardware but instead using a system such as [Binary\_package\_guide#Chrooting](https://wiki.gentoo.org/wiki/Binary_package_guide#Chrooting)

Don't forget to update the system after changing make.conf!

`root #``emerge --sync``root #``emerge --ask --update --deep --newuse @world`
### Installing the kernel

It is necessary to create a custom kernel because some disk drivers aren't included by default for this device. Creating a custom kernel also yields significant performance benefits if configured properly.

Disable 64-bit support.

**Disable 64-bit support (CONFIG\_64BIT)**

**Configure optional CPU optimizations (CONFIG\_SMP) (CONFIG\_EFI)**

If you plan on using internet connection, enable Networking support

**Enable/Disable Networking support (CONFIG\_NET)**

This is the most important part of the kernel configuration, as the user decides which drivers should or should not be build into the kernel. The guide will carefully go through most device drivers and enable those that are needed and disable those which would only create overhead otherwise.

**Enable PCI support (CONFIG\_PCI) (CONFIG\_HOTPLUG\_PCI)**

**Enable PCMCIA support (CONFIG\_PCCARD)**

**Enable Parallel port support (CONFIG\_PARPORT)**

This step is very important, not following it will lead into the laptop not being able to find the root partition.

**Enable ATA/PATA support (CONFIG\_ATA\_ACPI,CONFIG\_ATA\_GENERIC, CONFIG\_PATA\_LEGACY)**

**Enable USB 1.1 support (CONFIG\_USB\_OHCI\_HCD)**

Moving on from device drivers, necessary support for filesystems must be enabled

**Enable necessary filesystem support (CONFIG\_EXT2\_FS) (CONFIG\_EXT4\_FS)**

Optionally, you can enable support for btrfs (if you formated your root partition as btrfs instead of ext4) or microsoft NTFS (for intercompatibility with microsoft windows).

**Enable optional btrfs and ntfs support**

### Booting

The laptop should not have have issues booting Grub or lilo which are installed in BIOS mode. It is assumed that the disk's name is /dev/sda.

#### Grub

Installing grub is relatively straight-forward. In Gentoo Linux the user must enable legacy BIOS support in make.conf as mentioned in the guide previously.

```
GRUB_PLATFORMS="pc"
```
The boot partition must be mounted to install the bootloader.

`root #``mount -v /dev/sda1 /boot`
Then emerge and install grub.

`root #``emerge --ask --verbose sys-boot/grub``root #``grub-install /dev/sda``root #``grub-mkconfig -o /boot/grub/grub.cfg`
#### Lilo

Lilo is a fine choice for a system this old because it loads much faster than grub. However it does not support some features like raid or encryption. See [this bootloader comparison](https://wiki.archlinux.org/title/Arch_boot_process#Feature_comparison) for more information.

Lilo is configured in /etc/lilo.conf. It will not install with the lilo package, the user must create it manually.

```
boot=/dev/sda             # Install LILO in the MBR
prompt                    # Give the user the chance to select another section
timeout=50                # Wait 5 (five) seconds before booting the default section
default=gentoo            # When the timeout has passed, boot the "gentoo" section
compact                   # This drastically reduces load time and keeps the map file smaller; may fail on some systems
  
image=/boot/vmlinuz-6.X.X # Change this according to your kernel version (ls /boot/vmlinuz*)
  label=gentoo            # Name we give to this section
  read-only               # Start with a read-only root. Do not alter!
  root=/dev/sda3          # Location of the root filesystem
```
After creating a config file, you can safely install lilo.

`root #``emerge --ask --verbose sys-boot/lilo``root #``/sbin/lilo`
See [LILO](https://wiki.gentoo.org/wiki/LILO) for more information.

## Optional post-installation tasks

After these installation steps, the system will be a pretty bare but functional system. To make it more functional then the following can be setup:

### Sound support

The laptop uses Yamaha OPL3-SA3 sound card which is on the ISA bus. It was common for sound cards to use ISA in the past but nowadays it has been pretty much deprecated. The user will need to enable the following kernel options:

**Enable ISA support (CONFIG\_ISA)**

**Enable EISA support (CONFIG\_EISA)**

**Enable ISA PnP support (CONFIG\_ISAPNP)**

**Enable OSS emulation + HR-timer backend (CONFIG\_SND\_OSSEMUL) (CONFIG\_SND\_HRTIMER)**

**Enable support for PC speaker (CONFIG\_SND\_PCSP)**

**Enable support for Yamaha OPL3-SA2/SA3 (CONFIG\_SND\_OPL3SA2)**


These steps should make the sound fully functional. It is recommended to install the following packages and test the sound:

`root #``emerge --ask media-sound/alsa-utils media-sound/alsa-tools`
To detect sound cards:

`root #``aplay -L`
If it finds the Yamaha OPL3-SA3, then the sound should be functional. The user may test it with speaker-test:

`root #``speaker-test -c6 -twav`
### Setting up internet

#### USB adapters

It is possible to connects USB Ethernet adapters to establish internet connectivity. We will use Realtek RTL8153 as an example.

First it is necessary set some options in the kernel.

**Enable networking support (CONFIG\_NET)**

**Enable support for your USB adapter (CONFIG\_USB\_RTL8152)**

Assuming USB 1.1 support is enabled as well, the network adapter should work. It is recommended to use [net-misc/dhcpcd](https://packages.gentoo.org/packages/net-misc/dhcpcd) for network management because of its small size and low resource usage.

#### PCMCIA Ethernet

The laptop has two 16-bit/32-bit PCMCIA slots which are capable of delivering Ethernet connectivity up to 100M/s. It is assumed that a realtek PCMCIA card is used. **It is necessary to set PCMCIA mode to 16-bit PCMCIA in the BIOS!**

First it is necessary set some options in the kernel.

**Enable PCMCIA support (CONFIG\_PCMCIA and/or CONFIG\_CARDBUS)**

Test whether PCMCIA slots are found with {{Package|sys-apps/pcmciautils}

`root #``emerge --ask sys-apps/pcmciautils``root #``lspcmcia`
If the output is blank, then the slots are not detected and further configuration is needed.

Now enable support for the chip (example RTL8139)

**Enable support for the network chip**

At this point the network adapter should work. It is recommended to use [net-misc/dhcpcd](https://packages.gentoo.org/packages/net-misc/dhcpcd) for network management because of its small size and low resource usage.

## Putting the hardware to good use

As stated in the beginning of this guide, this guide proves that it is possible to daily use such old hardware for basic tasks even today. These are the programs that were tested on this hardware and that do work well, this in no way means that the user cannot choose different software of their choosing.

### Playing Music files and Audio CDs

There are many command line audio players available for Linux. In this guide we will focus on setting up [media-sound/cmus](https://packages.gentoo.org/packages/media-sound/cmus) as it is one of the easier to set up and it also supports audio CDs via a use flag.

Now emerge cmus

`root #``emerge --ask media-sound/cmus`
cmus should be ready to go

`user $``cmus`
Transfer your music to $HOME/Music. Then open cmus and command it to add the folder using `:`

`:``add ~/Music`
To add contents of a CD do:

`:``add cdda://`
See more at [cmus](https://wiki.gentoo.org/wiki/Cmus)

### text communication via Gomuks

There are several chat protocols that can be used on this hardware, there are command line clients for matrix, xmpp, IRC and others. In this guide it will focus on setting up [Gomuks](https://github.com/tulir/gomuks) (matrix) on this hardware.

There are no official 32-bit binaries or even unofficial ebuilds for this program, therefore it is necessary to manually compile it.

Install git, go and olm:

`root #``emerge --ask dev-vcs/git dev-lang/go dev-libs/olm`
Clone the repository and compile gomuks:

`root #``cd gomuks``root #``sh build.sh`
Assuming there were no errors during compilation, a gomuks binary should be generated in the root folder. Now copy it to /bin

`root #``cp -v ./gomuks /bin/`
Run Gomuks from the user shell without issues.

`user $``gomuks`
To learn how to use Gomuks refer to [The usage faq](https://docs.mau.fi/gomuks/faq.html).

### Neomutt E-mail client

Neomutt is an E-mail client that runs entirely on the command-line. It's very light-weight and isn't heavy on the CPU, therefore it is a fine choice for this system. We will set up neomutt via [mail-client/emailwiz](https://packages.gentoo.org/packages/mail-client/emailwiz) for the sake of ease.

`root #``emerge --ask mail-client/emailwiz`
It is necessary to set up a GPG key and initialise password-store if the user has not already.

`user $``gpg --full-generate-key``user $``pass init`
mw -h gives you a list of available options:

`user $``mw -h`
mw
mw: mutt-wizard, auto-configure email accounts for mutt
including downloadable mail with \`isync\`.
Main actions:
  -a your@email.com     Add an email address
  -l                    List email addresses configured
  -d                    Remove an already added address
  -D your@email.com     Force remove account without confirmation
  -t number             Toggle automatic mailsync every \<number> minutes
  -T                    Toggle automatic mailsync
  -r                    Reorder account numbers
Options allowed with -a:
  -u    Account login name if not full address
  -n    "Real name" to be on the email account
  -i    IMAP/POP server address
  -I    IMAP/POP server port
  -s    SMTP server address
  -S    SMTP server port
  -x    Password for account (recommended to be in double quotes)
  -p    Add for a POP server instead of IMAP.
  -P    Pass Prefix (prefix of the file where password is stored)
  -X    Delete an account's local email too when deleting.
  -o    Configure address, but keep mail online.
  -f    Assume typical English mailboxes without attempting log-on.
NOTE: Once at least one account is added, now run
\`mbsync -a\` to begin downloading mail.
To change an account's password, run \`pass edit ''your@email.com\`.
\[jacob@clawie \~\]$

The easiest way is to simple add an account with -a, mw will ask for the following information:

`user $``mw -a john@example.tld`
Give your email server's IMAP address (excluding the port number):
mail.example.tld
Give your email server's SMTP address (excluding the port number):
mail.example.tld
Enter password for john@example.tld:
Retype password for john@example.tld:

Recommended: change the default text editor.

For information on neomutt usage see [neomutt](https://wiki.gentoo.org/wiki/Neomutt).

## Closing thoughts

The Toshiba Satellite Pro 460CDT despite its age has proven to be very useful even today. We hope that this guide will be useful not only for retro PC enthusiasts but also for regular linux users who want to bring life back into their old Toshibas.

## External resources

- [https://support.dynabook.com/support/staticContentDetail?contentId=638342&isFromTOCLink=false](https://support.dynabook.com/support/staticContentDetail?contentId=638342&isFromTOCLink=false) - detailed specs of the laptop
- [https://www.funet.fi/pub/linux/kernel/v2.1/patch-html/patch-2.1.132/linux\_Documentation\_sound\_OPL3-SA2.html](https://www.funet.fi/pub/linux/kernel/v2.1/patch-html/patch-2.1.132/linux_Documentation_sound_OPL3-SA2.html) - sound card documentation
