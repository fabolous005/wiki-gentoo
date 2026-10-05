<!-- source: https://wiki.gentoo.org/wiki/FAQ | group: Gentoo Wiki (Main) | wiki-title: FAQ -->
---
title: FAQ
url: https://wiki.gentoo.org/wiki/FAQ
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-02-28"
fingerprint: "90819a5a45a3b30c"
license: CC BY-SA 4.0
---

# FAQ

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

This **FAQ** is a collection of common questions about Gentoo, along with their corresponding answers.

Please note that this document is just a quick reference for some common questions - many of these questions are answered more fully in the official [Gentoo documentation](https://wiki.gentoo.org/wiki/Main_Page#Documentation_topics), on this wiki.

These questions are often collected from the [gentoo-dev](https://archives.gentoo.org/gentoo-dev/) mailing list and from [Gentoo channels](https://www.gentoo.org/get-involved/irc-channels/all-channels.html) on [Internet Relay Chat (IRC)](https://wiki.gentoo.org/wiki/IRC).

*Gentoo* ([/ˈdʒɛntuː/](https://en.wikipedia.org/wiki/Help:IPA/English)) is pronounced "gen-too" (the "g" in "Gentoo" is a soft "g", as in "gentle").

The Gentoo Linux distribution takes it's name from the [Gentoo penguin](https://en.wikipedia.org/wiki/Gentoo_penguin), who's scientific name is *Pygoscelis papua*. The name *Gentoo* was given to the penguin by the inhabitants of the [Falkland Islands](https://en.wikipedia.org/wiki/Falkland_Islands).

Gentoo uses a [BSD ports](https://en.wikipedia.org/wiki/Ports_collection)-like system called [Portage](https://wiki.gentoo.org/wiki/Project:Portage) - a package management system that allows **great flexibility** installing, maintaining, and updating software. Portage provides **compile-time option support** via [USE flags](https://wiki.gentoo.org/wiki/Handbook:AMD64/Working/USE), conditional dependencies, safe installation of software through sandboxing, use-case adaptable defaults thanks to [system profiles](<https://wiki.gentoo.org/wiki/Profile_(Portage)>), and [configuration file protection](https://wiki.gentoo.org/wiki/Handbook:AMD64/Portage/Variables#Configuration_file_protection) - amongst many other [features](https://wiki.gentoo.org/wiki/Handbook:AMD64/Working/Features). All this functionality comes together to make Gentoo a very adaptable operating system, that can conveniently be tailored to any specific usage when needed, but when left in the default configuration will yield a simple, "sane default", environment.

By default, Gentoo  **builds (compiles) and installs system packages from source code**, specifically to the user's choice of configuration and optimizations - many of which are only available at "compile time". Gentoo provides exceptionally fine-grained control of low-level parameters (compiler flags, architecture choices, base subsystem selection, etc.), both for the system globally, or for individual packages - when required.

Gentoo permits many **[alternatives](https://wiki.gentoo.org/wiki/Eselect) for core (system) software**, allowing users to adapt with ease the installation to their own needs and preferences - in fact, the user has almost complete control over which packages are installed, or left out. This is a key difference from many other distributions, which are often built around specific subsystems, which cannot be replaced. Because of Gentoo's flexibility, there are no "variants", "editions", "flavors", etc. - there is no need, as everything can be adapted for each use-case from the default installation.

Gentoo strives to do things in the simplest possible way, and core Gentoo principles and procedures are easy to understand and master, given just a little effort. The relatively small investment to learn how to use Gentoo will reap dividends for anyone who is to become a substantial user of a Unix(like) operating system. Gentoo may require some reading and a little thought to understand how to use it, but the payoff from the power gained by the new user is considerable.

Gentoo is very actively maintained, and the entire distribution uses a rapidly-paced development and distribution method, termed **[rolling release](https://wiki.gentoo.org/wiki/FAQ#Can_I_upgrade_Gentoo_from_one_release_to_another_without_reinstalling.3F)**: new and updated packages are frequently added to the [Gentoo ebuild repository](https://wiki.gentoo.org/wiki/Ebuild_repository#The_Gentoo_ebuild_repository), relevant patches are rapidly applied, documentation is updated on a daily basis, and Portage features are added frequently. The fast turnaround cycle does not compromise on quality: packages start life in the [testing branch](https://wiki.gentoo.org/wiki/Handbook:AMD64/Portage/Branches#Testing) and are only moved into [stable](https://wiki.gentoo.org/wiki/Handbook:AMD64/Portage/Branches#Stable) once proved to be reliable; generally the transition time target is a 30 days or less.

While Portage optimizes compilation to a specific processor according to the `CFLAGS`/`CXXFLAGS` setting, anything other than the defaults for a given processor risk issues and even performance *loss*. The goal of the Gentoo project has never specifically been to permit low level optimization, even if its architecture does lend itself to this.

Any required `CFLAGS` should be set on a per-package basis, system-wide optimization above defaults is not recommended.

The `-O2` flag is the highest that should always work. Anything above `-O3` is not supported by current versions of GCC. Very aggressive optimizations sometimes cause the compiler to streamline the assembly code to the point where it does not quite do the same thing anymore.

Please try to compile using `-O2 -march=native` with `CFLAGS`/`CXXFLAGS` before reporting a bug.

See the [GCC optimization](https://wiki.gentoo.org/wiki/GCC_optimization) article for more details.

Use the passwd command to change the password for the user that is logged in. The root user can change another user's password by issuing the command passwd username. For extra options and settings, see passwd's manual page ([passwd(1)](https://man.archlinux.org/man/passwd.1.en)[).](https://wiki.gentoo.org/wiki/Special:MyLanguage/man_page)

The command useradd larry will add a user called "larry". However, this method does not give the user many of the rights needed to work properly on the system, so the following command is preferred:

`root #``useradd -m -G users,audio,wheel larry`
This will add a user called "larry". The `-m` option creates a home directory. The `-G` option adds the user to the specified groups:

- `users` which is the standard group for interactive users on the system
- `audio` which allows the user to access sound devices
- `wheel` which allows the user to execute the su command to gain root privileges (if they know the root password)

For security reasons, users may only su to root if they belong to the wheel group. To add larry to the wheel group, issue the following command as root:

`root #``gpasswd -a larry wheel`
There are no Gentoo releases, packages are updated continually: it is a **[rolling release](https://en.wikipedia.org/wiki/rolling_release)** distribution (not to be confused with "*bleeding edge*" - Gentoo is stable by default).

Gentoo packages get updates every day, and though important core packages will be updated from time to time, and new profiles created, there are no specific events that could be termed *versions, releases, editions, variants* etc. Each time the system is [upgraded](https://wiki.gentoo.org/wiki/Upgrading_Gentoo), everything will be "up to date".

A well-maintained, regularly-updated, installation should never need reinstalling.

It isn't obligatory to redo every step of the installation. However, investigating the kernel and all associated steps is necessary. Suppose that Gentoo is installed to the following partition scheme /dev/sda1 being /boot, /dev/sda3 being rootfs (/), and /dev/sda2 being swap space.

Boot from a live environment, then escalate to superuser privileges (necessary for mounting filesystems).

First mount all the partitions:

`root #````
mount /dev/sda3 /mnt/gentoo # Mount rootfs (/)
```
`root #````
mount /dev/sda1 /mnt/gentoo/boot # Mount boot partition
```
`root #````
swapon /dev/sda2 # Activate swap
```
`root #````
mount --types proc /proc /mnt/gentoo/proc
```
`root #````
mount --rbind /sys /mnt/gentoo/sys
```
`root #````
mount --make-rslave /mnt/gentoo/sys
```
`root #````
mount --rbind /dev /mnt/gentoo/dev
```
`root #````
mount --make-rslave /mnt/gentoo/dev
```
`root #````
mount --bind /run /mnt/gentoo/run
```
`root #````
mount --make-slave /mnt/gentoo/run
```
Then chroot into the Gentoo environment and configure the kernel:

`root #````
chroot /mnt/gentoo /bin/bash
```
`root #````
env-update && source /etc/profile
```
`root #````
cd /usr/src/linux
```
`root #````
make menuconfig
```
Now (de)select anything that was selected wrongly on the previous attempt, recompile, and reinstall the kernel:

`root #``make $(portageq envvar MAKEOPTS) && make install modules_install`
If [LILO](https://wiki.gentoo.org/wiki/LILO) has been used as the bootloader, rerun lilo - [GRUB](https://wiki.gentoo.org/wiki/GRUB) users should skip this step:

`root #``/sbin/lilo`
Exit the chroot and reboot the system.

`root #````
exit
```
`root #````
umount -l /mnt/gentoo/dev /mnt/gentoo/sys
```
`root #````
umount /mnt/gentoo/proc /mnt/gentoo/boot /mnt/gentoo
```
`root #````
reboot #systemctl reboot for systemd users
```
Please see [this article](https://wiki.gentoo.org/wiki/Knowledge_Base:Recovering_from_a_kernel_boot_failure) from the Knowledge Base for further details.

If, on the other hand, the problem lies with the bootloader configuration, follow the same steps, but instead of configuring and compiling the kernel, reconfigure the bootloader (recompilation of the bootloader is usually not necessary).

To have Portage automatically use this scheme, define it in /etc/portage/make.conf:

**`/etc/portage/make.conf`**

**Setting proxy authentication for Portage**

```
http_proxy="http://username:password@yourproxybox.org:portnumber"
ftp_proxy="ftp://username:password@yourproxybox.org:portnumber"
RSYNC_PROXY="rsync://username:password@yourproxybox.server:portnumber"
```
Keep in mind that the proxy server must support the `CONNECT` method for the rsync port(s).

ISO files must be burned to an optical disk in raw mode - this means the image should **not** just be "placed" on the disk as a file, but interpreted as the entire disk, with the aid of specialized ISO burning software. Most CD/DVD writing software will be capable of mastering an ISO file to a disk. Use whatever is at hand on systems available to burn a disk, and consult the documentation relevant to that software.

There are lots of optical media burning tools available to make a disk from an ISO file, here is a small selection of a few popular tools, on different platforms, with a short description of how to use them:

- With [EasyCD Creator](https://www.roxio.com/en/products/easy-burning/standard/), on MS Windows: select File, Record CD from CD image. Then change the Files of type to ISO image file. Then locate the ISO file and click Open. After clicking Start recording the ISO image will be burned correctly onto the CD/DVD.

- With [Nero Burning ROM](https://en.wikipedia.org/wiki/Nero_Burning_ROM), on MS Windows: cancel the wizard which automatically pops up and select Burn Image from the File menu. Select the image to burn and click Open. Now click the Burn button and watch the brand new Gentoo Live CD being burnt.

- With cdrecord, part of [cdrtools](https://en.wikipedia.org/wiki/cdrtools) ([app-cdr/cdrtools](https://packages.gentoo.org/packages/app-cdr/cdrtools)) - a multi platform project (works on Linux, among others): simply type cdrecord dev=/dev/cdrom (replace /dev/cdrom with the CDROM drive's device path) followed by the path to the ISO file.

- With [K3B](https://en.wikipedia.org/wiki/K3B), on Unix(like) OSs: select Tools → Burn Image. Then locate the ISO file within the 'Image to Burn' area, and select the target medium within the 'Burn Medium' area. Click Start to begin the burn process.

- With Mac OS X Panther, and later, launch [Disk Utility](https://en.wikipedia.org/wiki/Disk_Utility) from Applications/Utilities, select Open from the Images menu, select the mounted disk image in the main window and select Burn in the Images menu.

First find out what CPU is in the system Gentoo is to be installed on (for instance a Pentium-M). Next find out what CPU type it is compatible with (instruction-wise) to find a proper match with Gentoo's ISO or [stages](https://wiki.gentoo.org/wiki/Stage_file). Consulting the CPU's vendor website for this information usually works, although querying a search engine of choice is usually more efficient.

When uncertain, take a "lower" ISO or stage file, for instance a i686 or even generic x86 (or the equivalent in the system's arch). This will ensure that the system will work, but may not be as fast as further optimizations.

Please note that many more options exist than those for which Gentoo builds binary stages. Please see the [GCC guide](https://gcc.gnu.org/onlinedocs/gcc-12.1.0/gcc/x86-Options.html) for setting the `-march` flag.

The Handbook has further information on [selecting the correct stage file](https://wiki.gentoo.org/wiki/Handbook:AMD64/Installation/Stage#Choosing_a_stage_file) and [choosing the right installation medium](https://wiki.gentoo.org/wiki/Handbook:AMD64/Installation/Media#Gentoo_Linux_installation_media).

First follow standard troubleshooting practices (cables, routers working etc.).

Verify that the network card is discovered properly by the kernel. Run ip link and look for network interfaces. Something such as eth0, eno1, enp2s0, enp0s8, wlan0, wlp5s6 (in case of certain wireless network cards) should be present. Specific kernel modules may be required for the kernel to properly detect the network card. If that is the case, make sure that the required kernel modules are listed via a file ending in `.conf` in /etc/modules-load.d.

If support for the system's network card has been left out of the kernel, it will need to be reconfigured and, in some cases, recompiled.

If the network card *is* found by the kernel, but the network configuration has been set to use DHCP, a DHCP client might not have been installed on the system. There are many DHCP clients available in Gentoo, a common one being dhcpcd. If necessary to get the connection to the Internet working, reboot to the installation CD and install [net-misc/dhcpcd](https://packages.gentoo.org/packages/net-misc/dhcpcd).

Information on how to rescue the system using the installation CD is available [here](https://wiki.gentoo.org/wiki/FAQ#My_kernel_does_not_boot.2C_what_should_I_do_now.3F) as well.

The Handbook contains information on [network setup](https://wiki.gentoo.org/wiki/Handbook:AMD64/Full/Networking), while the wiki has information on [Ethernet](https://wiki.gentoo.org/wiki/Ethernet), [WiFi](https://wiki.gentoo.org/wiki/Wifi), and [network management](https://wiki.gentoo.org/wiki/Network_management).

Yes! Probably the fastest way to do so is to install GRUB with [sys-boot/os-prober](https://packages.gentoo.org/packages/sys-boot/os-prober). Read about it in the [GRUB article](https://wiki.gentoo.org/wiki/GRUB) and specifically about dual booting with GRUB [here](https://wiki.gentoo.org/wiki/GRUB#Additional_software).

This is a known problem and only applies to older bootloaders such as [GRUB Legacy](https://wiki.gentoo.org/wiki/GRUB_Legacy) and [LILO](https://wiki.gentoo.org/wiki/LILO). Windows refuses to boot when it is not installed on the first hard drive and shows a black/blank screen. To handle this, it is necessary to "fool" Windows into believing that it is installed on the first hard drive with a little tweak in the boot loader configuration. Please note that in the below example, Gentoo is installed on /dev/sda (first disk) and Windows on /dev/sdb (second disk). Adjust the configuration as needed:

**`/boot/grub/grub.conf`**

**Example dual boot entry for Windows in grub.conf**

**`/etc/lilo.conf`**

**Example dual boot entry for Windows in lilo.conf**

This will make Windows believe it is installed on the first hard drive and boot without problems. More information can be found in official [GRUB documentation](https://www.gnu.org/software/grub/) and in man lilo.conf.

The Gentoo Handbook only describes a Gentoo installation using a [stage3 file](https://wiki.gentoo.org/wiki/Stage_file#Stage_3). Stage1 and stage2 files are for development purposes only (the [Release Engineering](https://wiki.gentoo.org/wiki/Project:RelEng) team starts from a stage1 file to obtain a stage3) and should not be used by users. A stage3 file can very well be used to bootstrap the system. A working Internet connection is a requirement.

Bootstrapping means building the toolchain (the C library and compiler) for the system after which all core system packages are installed. To bootstrap the system, perform a stage3 installation. Before starting the chapter on *Configuring the Kernel*, it might be necessary to modify the bootstrap.sh script to match personal requirements:

`root #````
cd /var/db/repos/gentoo/scripts
```
`root #``vi bootstrap.sh`
After modifications, run the script.

`root #``./bootstrap.sh`
Next, rebuild all core system packages with the newly built toolchain. We need to rebuild them since the stage3 file already offers them:

`root #``emerge -e @system`
Now continue with *Configuring the Kernel*.

Packages are not "stored" per se. Instead, Gentoo provides a set of scripts which can resolve dependencies, fetch source code, and compile a version of the package tailored to the user's needs. Generally Gentoo only builds binaries for releases and snapshots. The [Gentoo Developer Manual](https://devmanual.gentoo.org/ebuild-writing/index.html) covers the contents of an ebuild script in detail.

For full ISO releases, a full suite of binary packages will be created using an enhanced .tbz2 format, which is .tar.bz2 compatible with meta-information attached to the end of the file. These can be used to install a working (though not fully optimized) version of the package quickly and efficiently.

It is possible to create RPMs (Red Hat package manager files) using Gentoo's Portage, but it is not currently possible to use existing RPMs to install packages.

Yes, but it is not trivial, nor is it recommended. Since the method to do this requires a good understanding of Portage internals and commands, it is instead recommended that the ebuild is patched to do whatever it is that the user wants and place it in a Portage overlay (that is why overlays exist). This is *much* better for maintainability, and usually easier. See the [Gentoo Developer Manual](https://devmanual.gentoo.org/ebuild-writing/index.html) for more information.

When behind a firewall that does not permit rsync traffic through port 873, the emerge-webrsync command can be used to fetch and install a Portage snapshot through regular HTTP. See [this section](https://wiki.gentoo.org/wiki/FAQ#My_proxy_requires_authentication.2C_what_do_I_have_to_do.3F) for information on downloading source files and Portage snapshots via a proxy.

It is *possible* to download packages manually and copy them to an appropriate location to be used for installation, however this can be a very tedious process.

Run emerge --pretend package/atom to see what programs are going to be installed. To find out the sources for those packages, and where to download the sources from, run emerge -fp package/atom. Download sources and bring them on any media home. Put the sources into the /var/cache/distfiles/ folder and then simply run emerge package/atom.

Deleting these files will have no negative impact on day-to-day performance. However, it might be wise to keep the most recent version of the files; often several ebuilds will be released for the same version of a specific piece of software. If the archive is deleted and the software is upgraded or rebuilt it will be necessary to download them from the Internet again.

Use the [eclean](https://wiki.gentoo.org/wiki/Eclean) script from [app-portage/gentoolkit](https://packages.gentoo.org/packages/app-portage/gentoolkit) to manage the contents of /var/cache/distfiles/ and a few other locations. Please read [eclean(1)](https://man.archlinux.org/man/eclean.1.en) [man-page to learn more about its usage, as well as the](https://wiki.gentoo.org/wiki/Special:MyLanguage/man_page) [Gentoolkit article](https://wiki.gentoo.org/wiki/Gentoolkit).

During compilation, Gentoo saves the sources of the package in /var/tmp/portage (or in $PORTAGE\_TMPDIR/portage if the default is changed). These files and folder are usually deleted upon a successful emerge, but this sometimes fails. It is safe to clean out all contents of this directory *if* the emerge command is not running. Be sure to always pgrep emerge before cleaning out this directory.

Edit the `keymap` variable in /etc/conf.d/keymaps. To have console working correctly with extended characters in the keymap, it might be necessary to set the `consolefont` and `consoletranslation` variables  in the /etc/conf.d/consolefont file (for further information on localizing the environment, refer to the [localization guide](https://wiki.gentoo.org/wiki/Localization/Guide)). Then, issue a reboot, or restart the keymaps and consolefont scripts:

`root #````
/etc/init.d/keymaps restart
```
`root #````
/etc/init.d/consolefont restart
```
See [keyboard layout switching](https://wiki.gentoo.org/wiki/Keyboard_layout_switching) for more information.

/etc/resolv.conf has the wrong permissions; fix it as follows:

`root #``chmod 0644 /etc/resolv.conf`
See also [resolv.conf](https://wiki.gentoo.org/wiki/Resolv.conf).

Add that user to the cron group:

`root #``gpasswd -a <username> cron`
The following command will add the numlock service to the default runlevel, enabling numlock at boot:

`root #````
rc-update add numlock default
```
`root #``/etc/init.d/numlock start`
Each GUI provides different tools for this sort of thing; please check the help section or online manuals for the GUI of choice for further assistance.

To have the terminal cleared, add the clear command to the user's \~/.bash\_logout script:

`user $``echo clear >> ~/.bash_logout`
To have this happen automatically when adding a new user, do the same for the /etc/skel/.bash\_logout file:

`root #``echo clear >> /etc/skel/.bash_logout`
Use the [Bugzilla](https://bugs.gentoo.org) site to report bugs. Visit [#gentoo](ircs://irc.libera.chat/#gentoo) ([webchat](https://web.libera.chat/#gentoo)) on the Libera.Chat IRC network and ask around if it is unclear whether an issue is really a bug or not.

There are a couple of guides for reporting bugs on the wiki: [Bugzilla/Bug report guide](https://wiki.gentoo.org/wiki/Bugzilla/Bug_report_guide) and [Bugzilla/Guide](https://wiki.gentoo.org/wiki/Bugzilla/Guide). See also the [support](https://wiki.gentoo.org/wiki/Support) article.

Gentoo's packages are usually updated shortly after the upstream authors release new code, see [this section](https://wiki.gentoo.org/wiki/FAQ#Can_I_upgrade_Gentoo_from_one_release_to_another_without_reinstalling.3F) for more information.

The [Release Engineering Project](https://wiki.gentoo.org/wiki/Project:RelEng) page, the [gentoo-announce](https://archives.gentoo.org/gentoo-announce/) mailing list, and the [Gentoo ebuild repository news items](https://www.gentoo.org/support/news-items/) provide information on important changes to Gentoo Linux.

Console beeps can be turned off using setterm, like this:

`root #``setterm -blength 0`
To turn off the console beeps on boot, put the following command in the /etc/conf.d/local.start file. However, this only disables beeps for the current terminal. To disable beeps for other terminals, pipe the command output to the target terminal, like this:

`root #``setterm -blength 0 > /dev/vc/1`
Replace /dev/vc/1 with the terminal for which console beeps need to be disabled.

See [this article](https://wiki.gentoo.org/wiki/PC_speaker#How_do_I_mute_the_PC_speaker.3F) for more details.

The 'e' became a thing because Gentoo originally started as Enoch Linux. Many of Gentoo's tools and function names maintained the prefix 'e' for this reason.

Here's a quote from  [Daniel Robbins (Daniel Robbins)](https://wiki.gentoo.org/wiki/User:Daniel_Robbins) : "I think the 'e' likely came from enoch, and was picked as a single-character prefix in the vein of the 'iMac', which was initially released in August 1998. Enoch began in early 1999. (see [https://www.funtoo.org/Funtoo\_Linux\_History](https://www.funtoo.org/Funtoo_Linux_History))."

Most stores have stopped offering CDs and DVDs. With the short window between Gentoo ISO releases and technological advancements (especially higher internet bandwidth for the masses) these forms of installation media are now artifacts of history. Bootable media are readily available on the mirrors and accessible via [the downloads page](https://www.gentoo.org/downloads/).

Licensed stores for official merchandise of other types are listed on the [stores page](https://www.gentoo.org/inside-gentoo/stores/).

A good first step is to browse through the relevant [documentation](https://www.gentoo.org/support/documentation/), on the [Gentoo wiki](https://wiki.gentoo.org/wiki/Main_Page), in [man pages](https://wiki.gentoo.org/wiki/Man_page), [Info](https://wiki.gentoo.org/wiki/Info), [/usr/share/doc/](https://wiki.gentoo.org/wiki//usr/share/doc/), etc. Many commands also support the --help or -h switches.

The various Gentoo Linux [mailing lists](https://www.gentoo.org/get-involved/mailing-lists/), or the [forums](https://wiki.gentoo.org/wiki/Project:Forums) could help. Queries may be put to the Gentoo community on [IRC](https://wiki.gentoo.org/wiki/IRC) - ask questions directly in the [#gentoo](ircs://irc.libera.chat/#gentoo) ([webchat](https://web.libera.chat/#gentoo)) [Libera.Chat](https://libera.chat/) IRC channel.

The [Gentoo bug tracking system](https://wiki.gentoo.org/wiki/Bugzilla) contains information on many issues. Searching the web may yield good results for some questions. If having trouble with a particular package, check upstream documentation.
