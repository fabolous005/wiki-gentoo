<!-- source: https://wiki.gentoo.org/wiki/MIPS/FAQ | group: Gentoo Wiki (Main) | wiki-title: MIPS/FAQ -->
---
title: MIPS/FAQ
url: https://wiki.gentoo.org/wiki/MIPS/FAQ
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2025-08-21"
fingerprint: de8bb91329201da7
license: CC BY-SA 4.0
---

# MIPS/FAQ

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)


This FAQ answers some of the most frequently asked questions relating to Gentoo/MIPS and Linux/MIPS in general.

This FAQ is intended to answer frequently asked questions about Gentoo/MIPS and Linux/MIPS that we receive from various users. It's aimed at both new users and experienced users alike. It has been split into a number of categories to make navigation easier.

If you'd like to contribute to the FAQ or, having read this guide, you still have questions that are left unanswered, feel free to [drop us a line](https://wiki.gentoo.org/wiki/Project:MIPS).

Gentoo/MIPS is a small project responsible for looking after the MIPS port of Gentoo Linux.

Okay, sure, some MIPS machines aren't the fastest boxes on the block these days. However, despite the age of some of these beasts, they still can make very functional, useful machines. A Cobalt Qube 2 could make a very nice broadband Internet router, capable of hosting websites, email, IRC and numerous other tasks. There are a number of reasons why you'd want to install Linux on this sort of hardware.

- It teaches you a lot about computer hardware by giving you an alternate frame of reference

- It allows you to turn what would otherwise be useless junk into a very functional system

- Status Symbol: Linux on x86 is so common these days it's not funny. However, Linux on MIPS is a lot less common and quite a talking point.

Hey, great idea. Unfortunately, a lot of the Gentoo/MIPS team already have their hands full looking after Linux/MIPS as well as other commitments. A project like this would fall under the umbrella of the [Gentoo Prefix project](https://wiki.gentoo.org/wiki/Project:Prefix). Some work has been done for IRIX, the remnants of which can be found in [Bugzilla](https://bugs.gentoo.org/buglist.cgi?quicksearch=irix) .

[MIPS Technologies](http://www.mips.com) is a company that produce a number of RISC CPU cores which implement the [MIPS Architecture](https://en.wikipedia.org/wiki/MIPS_architecture). These processors appear in all sorts of hardware ranging from small embedded devices to large servers.

It also happens to be an acronym; **M** illions of **I** nstructions **P** er **S** econd.

In short... lots. MIPS Processors see use inside all sorts of machines, ranging from small PDAs (such as the early Windows CE powered Casio PDAs), X Terminals (e.g. Tektronix TekXPress XP330 series), through to workstations such as the Silicon Graphics Indy and O2 and even high end servers such as the Silicon Graphics Origin 2000.

A comprehensive list can be found on the [Linux/MIPS website](http://www.linux-mips.org/wiki/index.php/Systems)

... and that's only scratching the surface. These machines are wide and varied. Many of them do not currently run Linux. Of those that do, we only support a handful, although you're welcome to port Gentoo/MIPS to any MIPS machine if you so wish. Some of these machines are also the focus of the [Embedded Gentoo Project](https://wiki.gentoo.org/wiki/Project:Embedded) such as the Linksys WRT54G.

For the first one an easy way to find out is to have a look at the [Gentoo/MIPS requirements page](https://wiki.gentoo.org/wiki/MIPS/Hardware_Requirements). This will tell you if the system you've got can theoretically run Gentoo/MIPS.

If you don't find your machine listed there, you may wish to have a look on the [Linux/MIPS website](http://www.linux-mips.org/wiki/index.php/Systems) to find it there. Installation won't be straightforward however, as the actual process of producing a kernel and suitable boot media for your hardware will have to be done largely by yourself. Naturally though, we'll try to help where we can.

If you've looked at the Gentoo/MIPS Hardware Requirements page, you've probably noticed there are a lot of machines we don't support. In the case of SGI hardware, very little is known about some of them, not enough to successfully port Linux to them.

If you managed to get Linux working on a box currently listed as *unsupported* however, please tell us. We'd be interested to know.

This will depend on the CPU type running in your system. The stage filename is named as follows:

For R4000-class CPUs, use a `mips3` or `mipsel3` stage tarball.

For R5000-class or later CPUs, use a `mips4` or `mipsel4` stage tarball.

This is generally caused by using the wrong stage tarball. If you try to run a `mips4` userland on a `mips3` CPU, you'll get an *illegal instruction* error message. Likewise, if you have a Big Endian CPU and you try to run Little Endian code on it, you'll get *cannot execute binary file* .

The fix is simple: clean out your partition, then unpack the correct tarball.

This could be for any number of reasons, ranging from cabling issues, through to issues on the server. The best way to troubleshoot any problem is a step-by-step approach...

1. **Have you got the SGI machine (and server) plugged into the right network ports?**
Make sure the network is cabled correctly. Also note that some machines have special needs. For instance the Challenge S cannot obtain network connectivity under Linux via its UTP port, you need to use the AUI port via a transceiver.
2. **Are there any firewalls in use?**
Make sure your firewall is not blocking DHCP/BOOTP requests (ports 67 and 68 on UDP) or TFTP (port 69 on UDP).`iptables -I INPUT 1 -p udp --dport 67:69 -j ACCEPT` should get things rolling.
3. **Have you disabled packet MTU discovery and set the port range?**
SGI boxes require /proc/sys/net/ipv4/ip\_no\_pmtu\_disc = 1 and /proc/sys/net/ipv4/ip\_local\_port\_range = "2048 32767". See[the Gentoo/MIPS handbook](https://wiki.gentoo.org/wiki/Handbook:MIPS/Installation/Media#Netbooting_on_SGI_stations).
4. **Is the server giving out the correct details via BOOTP?**
Double check your /etc/dhcp/dhcpd.conf . ISC's dhcpd won't dish out addressing information via BOOTP unless the machine has been statically defined with a fixed address.
5. **Which TFTP server are you using?**
`tftp-hpa` is known to work. `atftp` is a lot more advanced, this can cause problems. If in doubt, try installing `tftp-hpa` and see if the problem clears up.
6. **Are the daemons running?**
`dhcpd` should show up when typing `ps ax` . As for TFTP, it'll largely depend on whether its a standalone server, or if its running from `(x)inetd` . `tftp-hpa` runs as a process called `in.tftpd` . Look for that in the `ps ax` output and start any services not currently running.
7. **Does the kernel exist in /tftpboot ?**
Make sure you place the kernel image to be booted in this directory and that it is world-readable. (`chmod 644 /tftpboot/foo` ) Also, in your /etc/dhcp/dhcpd.conf , note that the path to the kernel will be relative to the /tftpboot directory if you're using `tftp-hpa` .
8. **Have you unset the `netaddr` and `dlserver` PROM variables?**
Try running`unsetenv netaddr` and `unsetenv dlserver` .

Unfortunately, not all graphics frame buffers are supported under Linux yet. This doesn't mean you can't use the machine... it just means you'll need a null-modem serial cable to interact with it. It is quite possible that the machine is in fact running, however, the system is outputting to the serial console rather than the screen.  Try using a default serial connection of ***9600 8N1***, with a null modem adapter.

This could be for a number of reasons. Our easiest bet however is to run through a checklist and make sure everything is correct.

1. **Have you got the Cobalt machine (and server) plugged into the right network ports?**
Make sure the network is cabled correctly. Please note, the Cobalt firmware will only boot via the Primary network port.
2. **Are there any firewalls in use?**
Make sure your firewall is not blocking DHCP/BOOTP requests (ports 67 and 68 on UDP) or RPC/Portmap (port 111 on UDP and TCP). The following firewall rules should get things rolling:`root #``iptables -I INPUT 1 -p udp --dport 67:68 -j ACCEPT``root #``iptables -I INPUT 1 -p udp --dport 111 -j ACCEPT``root #``iptables -I INPUT 1 -p tcp --dport 111 -j ACCEPT`
3. **Is the server giving out the correct details via BOOTP?**
Double check your /etc/dhcp/dhcpd.conf . ISCs dhcpd won't dish out addressing information via BOOTP unless the machine has been statically defined with a fixed address.
4. **Are you exporting /nfsroot in your /etc/exports ?**
Make sure you are exporting that to the Cobalt machine. It only needs read-only access. Also remember to run`exportfs -av` after you edit the file.
5. **Are the daemons running?**
`dhcpd` should show up when typing `ps ax` . Likewise with `portmap` and the other RPC daemons. The following commands should look after this for you: `/etc/init.d/dhcp start` `/etc/init.d/nfs start`
6. **Does the kernel exist in /nfsroot ?**
Make sure you place the kernel image to be booted in this directory and that it is world-readable. (`chmod 644 /nfsroot/foo` )

The Qube 2700 was the first of the Cobalt servers. While they are very nice machines, unfortunately, they lack a serial port. In other words, any interaction with the machine has to be done through a network. At present, our netboot images do not support this.
