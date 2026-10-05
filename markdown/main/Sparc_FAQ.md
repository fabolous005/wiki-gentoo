<!-- source: https://wiki.gentoo.org/wiki/Sparc/FAQ | group: Gentoo Wiki (Main) | wiki-title: Sparc/FAQ -->
---
title: Sparc/FAQ
url: https://wiki.gentoo.org/wiki/Sparc/FAQ
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2022-01-31"
fingerprint: e6dd3c13d883bbce
license: CC BY-SA 4.0
---

# Sparc/FAQ

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)


This FAQ is designed to help answer the most commonly asked questions about Gentoo/SPARC and Linux on SPARC architecture. Feel free to send revisions and corrections to the Gentoo/SPARC team or the doc team.

### Documentation Notes

These are some notes to elaborate on acronyms and terms used in this document.

When mentioning `SPARC` , this refers to the type of `sun4x` system. SPARC represents the `sun4u` architecture seen in the UltraSPARC series of systems (Ultras, Blades, SunFires, etc). Older 32-bit systems are no longer supported by the Gentoo/SPARC team.

Acronyms: `OBP` = OpenBoot PROM, `SS` = SPARCstation.

## Installation

### How do I install/use my SPARC over a serial console?

First, make sure you do not have a keyboard and monitor plugged in. OBP will use the monitor for output by default if either are plugged in. You will need a null modem cable or a regular serial cable with a null modem adapter to be able to view output.

Standard serial port settings:

- 9600 baud
- 8 data bits
- None for parity
- 1 stop bits

Below is a list of commonly used programs for viewing a serial console. Check to see if your serial console application has the ability to send a break, as it can be very useful on a SPARC. The break signal on a serial console is the equivalent to issuing the Stop+A command on the keyboard.

| Linux | Windows | 
|---|---|
| Minicom | Hyperterm | 
| Kermit | TeraTerm Pro | 
| Console |  | 
| Seyon |  | 

### I have a CRT/LCD monitor attached to my SPARC that selects a resolution my monitor can't handle when the kernel boots. How do I get a visible and/or non-distorted display on my monitor?

This problem is a result of the framebuffer support the Linux kernel loads at boot time, and the modelines available on SPARC systems. Here we will assume your monitor and video card will agree on a resolution of 1024 by 768 at a refresh rate of 60 Hz. In OBP, you can set the resolution by adjusting the setting for `output-device` . To view the current setting, use the command:

`ok``printenv output-device`
\<output-device>        \<screen>          \<screen>

Now, to set this so it will start the display using the above mentioned resolution, we will use the `setenv` command as follows:

`ok``setenv output-device screen:r1024x768x60`
output-device =       screen:r1024x768x60

In order for this to take effect, you will need to reset the machine:

`ok``reset`
Additionally, for users using the onboard video card based on the ATI Mach64 chipset (Ultra 5/10 and Blade 100/150), you will want to append the following to your kernel boot options:

If you are booting from SILO, you can append the above string onto the end of a given boot image. The following example would be if you were booting from the LiveCD.

If you were booting from the network, you would append the string onto the end of the boot net command in OBP as illustrated below:

`ok``boot net video=atyfb:1024x768@60`
### I'm new to SPARC and am not sure what kernel options to use, HELP!

Most of the time, the default kernel options will be acceptable for hardware support. You will need to make sure that `virtual memory` and `/proc filesystem` support is enabled in the kernel as instructed in the [Installation Handbook](https://wiki.gentoo.org/wiki/Handbook:SPARC) .

### How do I dual boot Gentoo Linux/SPARC with Solaris?

Dual booting Gentoo/SPARC and Solaris can be done one of two ways. The two ways are: having both Gentoo/SPARC and Solaris on the same drive, or having them on separate drives. Below are example SILO configurations for each scenario. In the example configs, the following things are assumed:

Single Drive Setup:

- Boot drive is SCSI ID 0
- Gentoo boots from partition 1
- Solaris boots from partition 5

Separate Drive Setup:

- Second drive is SCSI ID 1
- Gentoo is on SCSI ID 0
- Solaris is on SCSI ID 1
- boot-device in OBP is SCSI ID 0

**`silo.conf`**

**Sample silo.conf for a single drive installation**

**`silo.conf`**

**Sample silo.conf for separate disk installation**

## Hardware

### Will Gentoo run on my SPARC?

It depends on the type of SPARC, but chances are yes. One exception seems to be the SPARC based laptops from Tadpole, particularly the newer models. If you have a working Tadpole on Gentoo/SPARC, please contact us and let us know the details on the model and anything special needed to get it to work.

The Gentoo/SPARC team maintains a [Sun hardware compatibility list](https://wiki.gentoo.org/wiki/Sparc/Sun_hardware_compatibility_list) for SPARC64 systems. In general, support for SPARC64 systems is good enough to allow some users to run it successfully in production environments.

### What additional hardware is supported under Gentoo/SPARC?

For a list of additionally supported Sun hardware, consult the following list from the [UltraLinux homepage](http://www.ultralinux.org/faq.html#q_2_2.) .

A partial list of Sun devices which do NOT currently have support under Gentoo/SPARC or SPARC/Linux in general are:

- SunPCI cards
- SBUS or PCI PCMCIA controllers
- Smart card readers on some Sun Blade workstations
- Floppy drive on some Sun Blade workstations

Also, the Gentoo/SPARC team maintains a [list of non-Sun PCI hardware that has been tested on Gentoo/SPARC](https://wiki.gentoo.org/wiki/Sparc/Non-Sun_hardware_compatibility_list) . To submit additions to the list, email [sparc@gentoo.org](mailto:sparc@gentoo.org)with the information on the hardware as displayed in the existing hardware list.

### How do I know which SPARC I have?

The [UltraLinux FAQ](http://www.ultralinux.org/faq.html#q_2_1) has the answer.

### My box has more than one Sun Ethernet device. Why do they all have the same MAC address and how do I fix it?

First, the reason they all they all have the same setting is due to a value in OpenBoot PROM (OBP). The story goes that in times past, it was assumed that if a host had multiple interfaces, the interfaces would not reside on the same Ethernet and therefore having the MAC address unique to a machine rather than to a NIC wouldn't be a problem. As time went on and different technologies developed, the opposite was proven.

As for fixing the issue, there are two ways to it; from OBP and from Linux. The OBP variable `local-mac-address?` controls this behavior. The following commands will help you to determine what `local-mac-address?` is set to:

`ok``printenv local-mac-address?`
local-mac-address? =  false

`root #``eeprom local-mac-address?`
local-mac-address?=false

As you'll notice from the samples above, `local-mac-address?` is set to **false** . This will cause all interfaces on a SPARC to use the same MAC address. To change this behavior, use either of the commands below (depending on whether you do it from OBP or Linux):

`ok``setenv local-mac-address?=true`
local-mac-address? =  true

`root #``eeprom local-mac-address?=true`
Note that you may need to reboot to see the changes take affect.

### Is SPARC big-endian or little-endian?

The version supported by Gentoo/SPARC (v9) is bi-endian. It uses big-endian instructions, but can access data in little-endian byte-order, such as PCI devices, which are little-endian by nature.

## Applications

### How do I get Xorg to find a usable modeline on my Ultra 5/10 or Blade 100/150?

The ATI Framebuffer normally expects a PC's BIOS to set the Reference Clock speed for the video card. However, on non-x86 architectures, this doesn't happen. So what needs to be done is to set the Reference Clock speed in the /etc/X11/xorg.conf .

In the "Device" section of your /etc/X11/xorg.conf , you will want to add a line similar to the following:

**`xorg.conf`**

**Adding clock speed**

```
Option      "reference_clock"   "28.636 MHz"
```
Other common speeds are 14.318 MHz and 29.5 MHz. If you have issues you might want to try without a similar setting first.

### How come package foo is masked on SPARC?

Chances are this is due to the fact that none of the Gentoo/SPARC developers have gotten around to testing this yet, and/or the package is currently marked with the `~sparc` keyword (meaning it's in a testing phase).

If you still want to install the package, you can follow the instructions from the [Portage Handbook](https://wiki.gentoo.org/wiki/Handbook:SPARC/Portage/Branches) .

### How can I help get package foo marked stable?

If you want to help ensure a package gets marked stable in a timely fashion, particularly if it's an application with a small userbase (i.e. [net-im/bitlbee](https://packages.gentoo.org/packages/net-im/bitlbee)), install the package and test to make sure it works. If it appears to be working well, feel free to file a bug via the [Gentoo Bugzilla](https://bugs.gentoo.org) and assign it to the Gentoo/SPARC team.

### I tried to emerge a package, but emerge failed complaining that a dependency is masked. How can I fix this?

This is a bug. Please file a bug report via the [Gentoo Bugzilla](https://bugs.gentoo.org) for the Gentoo/SPARC team indicating what package you tried to emerge, the error message emerge gave you, and the output of `emerge --info`.

The way SPARC/Linux handles the cross-compiler has changed with 2.6.25 ( [commit](https://git.kernel.org/?p=linux/kernel/git/torvalds/linux-2.6.git;a=commitdiff;h=54cd6ddec77b75e6926d73d263aec72255b24030) ), thus when building a recent kernel you'd get the following error message:

Because, for the moment (there's an **experimental** [multilib](https://wiki.gentoo.org/wiki/Sparc/Multilib) profile), Gentoo/SPARC native compiler can't generate 64-bit code (that's what [sys-devel/kgcc64](https://packages.gentoo.org/packages/sys-devel/kgcc64) is for).

What you need to do is to tell the SPARC/Linux Makefile what is your cross-compiler's binary, by issuing the following commands:

`root #````
cd /usr/src/linux
```
`root #````
make CROSS_COMPILE=sparc64-unknown-linux-gnu-
```
`root #``make CROSS_COMPILE=sparc64-unknown-linux-gnu- image modules_install`
## Miscellaneous

### I'm interested in becoming more involved with Gentoo/SPARC. How do I go about doing that?

There are various ways to improve the project:

- The best way is to become an [Arch Tester](https://wiki.gentoo.org/wiki/Project:SPARC/Arch_tester) , and aid the developers with package testing.
- File bug reports such as [stable, and keyword](https://dev.gentoo.org/~tcunha/reports/imlate-sparc.txt) requests.
- Join the [#gentoo-sparc](ircs://irc.libera.chat/#gentoo-sparc) ([webchat](https://web.libera.chat/#gentoo-sparc)) IRC channel on the Libera.Chat IRC network.
- Make use of the [Gentoo/SPARC forum](https://forums.gentoo.org/viewforum.php?f=27) .
- Propose solutions for Gentoo/SPARC related bugs via the [Gentoo Bugzilla](https://bugs.gentoo.org) . The bugs will be assigned, or CCed to sparc@gentoo.org.

## Additional Resources

### Where can I get more help or information about SPARC/Linux?

- [OBP Reference Guide](http://docs.sun.com/app/docs/coll/216.2) - Sun's guide to the OpenBoot PROM
- [OSIAH](http://www.sunstuff.org) - Online Sun Information Archive
- [Obsolyte](http://www.obsolyte.com) - Information on older SPARC systems and links to a lot of other great SPARC related sites
- [Sun Microsystems Documentation](http://docs.sun.com) - Additional hardware and software information for SPARC systems, including instructions for flashing OBP
- [Sunsolve](http://sunsolve.sun.com) - Great resource for Sun hardware documentation
- [UltraLinux Homepage](http://www.ultralinux.org) - Homepage of the SPARC/Linux port
