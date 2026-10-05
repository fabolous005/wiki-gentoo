<!-- source: https://wiki.gentoo.org/wiki/Sparc/Netboot | group: Gentoo Wiki (Main) | wiki-title: Sparc/Netboot -->
---
title: Sparc/Netboot
url: https://wiki.gentoo.org/wiki/Sparc/Netboot
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2022-01-31"
fingerprint: "709ddd521029fb94"
license: CC BY-SA 4.0
---

# Sparc/Netboot

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)


This article is a guide for setting up a netboot server for use with the Gentoo/SPARC netboot installation images.

## Introduction

This document will describe how to setup a network booting environment for a Sun Microsystems SPARC or UltraSPARC based computer. The document assumes that you have an existing Gentoo Linux computer available to act as the netboot server.

Both the netboot server and netboot client will need to be on the same network subnet, as the ARP protocol is typically not forwarded across different network subnets.

A generic overview of what happens during the netboot process is as follows:

1. Client machine sends out a reverse ARP (RARP) request to get an IP address.
2. A server machine returns a response to the client with the IP address.
3. The client then attempts to download a boot image from the RARP server using the TFTP protocol.
4. Once the image is downloaded, the netboot client then boots the image.

Based on this overview, we will need to install software for a reverse ARP daemon and a TFTP daemon.

## Software Installation And Configuration

### The reverse ARP daemon

A reverse ARP daemon is already installed on the system; it's part of the [net-misc/iputils](https://packages.gentoo.org/packages/net-misc/iputils) package.

**Setting up common rarpd elements** : /etc/ethers

You will need to setup the /etc/ethers file. This file indicates which hosts rarpd should respond to when a request is seen, and what address to reply with.

The format of /etc/ethers is the MAC address of the NIC the machine will be netbooting and its hostname. Whitespace delimits the MAC address from the hostname, and each entry should have its own line. The following example is for a host named sparc-netboot.gentoo.org:

**`/etc/ethers`**

If you desire to add additional hosts to /etc/ethers, you do not need to restart the rarpd services as the file is checked each time a request is received.

**Resolving hostnames** : /etc/hosts

Since each entry in /etc/ethers has a hostname, the netboot server needs to be able to resolve the hostname into its IP address. This can be done two ways, /etc/hosts or the nameserver the netboot server uses.

An /etc/hosts entry for resolving a hostname will look very similar to the one that probably exists from when you installed Gentoo on the netboot server. For our example host, sparc-netboot.gentoo.org, we'll assume that it has an IP address of 10.0.1.15. So the /etc/hosts entry would look like:

**`/etc/hosts`**

If you use a nameserver, then the DNS server administrator will need to add a record for the hostname, in our example sparc-netboot.gentoo.org, to point to the appropriate IP address. Please consult your DNS server administrator and/or the documentation for the DNS server's DNS software for how to add the entry.

**Setting up rarpd**

First, we will need to determine the options to use for rarpd. While there are more options than we'll cover here, these options should get you started As there is currently no init.d script for rarpd, an entry will need to be added to /etc/conf.d/local.start if you want to enable rarpd services at boot time. A sample entry is as follows:

**`/etc/conf.d/local.start`**

An explanation of the above rarpd options (as taken from the man page):

- `-v` Be verbose.
- `-e` Do not check for the presence of a boot image, reply if MAC address resolves to a valid IP address using /etc/ethers database and DNS.
- eth0 represents the interface which rarpd should bind to.

For more options, consult man 8 rarpd.

### The tftpd daemon

Here there are two options for a TFTP daemon, [net-ftp/atftp](https://packages.gentoo.org/packages/net-ftp/atftp) and [net-ftp/tftp-hpa](https://packages.gentoo.org/packages/net-ftp/tftp-hpa). You only need to install one of the TFTP daemons for proper operation.

**Setting up common tftpd elements**

Each TFTP daemon will need a directory from which to serve files to tftp clients. The directory we will use for this howto will be /tftpboot. This will appear as the root (/) directory to the clients when requests are received. Additionally, we'll setup the system to run the TFTP daemon with the user and group *nobody*.

If the directory you have chosen does not currently exist, it will need to be created with the mkdir command. The command for the example /tftpboot is:

`root #``/bin/mkdir /tftpboot`
Then we will need to change the owner of /tftpboot so that it is owned by user nobody and group *nobody*:

`root #``chown nobody:nobody /tftpboot`
### The atftp daemon

First, install the [net-ftp/atftp](https://packages.gentoo.org/packages/net-ftp/atftp) package as follows;

`root #``emerge --ask net-ftp/atftp`
After the atftp has been installed, it will need to be configured. If tftpd services are desired at boot time, an entry to /etc/conf.d/local.start will need to be added as atftp has no init.d, inetd, or xinetd scripts of its own. If you want to use inetd or xinetd for controlling the tftpd service, please see their respective man pages.

Below is an example entry for atftpd in /etc/conf.d/local.start.

**`/etc/conf.d/local.start`**

An explanation of the above options (as taken from the man page);

- `-v` Increase or set the logging level. No args will increase by one the current value. Default is LOG\_NOTICE, see syslog(3) for log level. Current value range from 0 (LOG\_EMERG) to 7 (LOG\_DEBUG).
- `--daemon` Run as a daemon. Do not use this option if atftpd is started by inetd.

For more options, consult man 8 atftpd.

### The tftp-hpa daemon

First, install the [net-ftp/tftp-hpa](https://packages.gentoo.org/packages/net-ftp/tftp-hpa) package:

`root #``emerge --ask net-ftp/tftp-hpa`
tftp-hpa comes with an /etc/init.d and the accompanying /etc/conf.d configuration file. Check to make sure that `INTFTPD_PATH` and `INTFTP_OPTS` variables in /etc/conf.d/in.tftpd match the values below:

**`/etc/conf.d/in.tftpd`**

The TFTP daemon can then be started via the init.d script:

`root #``/etc/init.d/in.tftpd start`
For more options, consult man 8 tftpd.

## Preparing a tftpboot image for use by a client

Make sure you have an image you want to use for netbooting. Please check the [local](https://www.gentoo.org/downloads/mirrors/) Gentoo [distfiles mirror](http://distfiles.gentoo.org/experimental/sparc/tftpboot/sparc64/) for the appropriate image. We'll assume you are planning to boot using the gentoo-sparc64-20100128.tftpboot image.

Once you have an image, copy the image into /tftpboot :

`root #````
cp gentoo-sparc64-20100128.tftpboot /tftpboot
```
`root #``chmod 644 /tftpboot/gentoo-sparc64-20100128.tftpboot`
Now, when the netboot client makes a TFTP request, it looks for a file that is the hexadecimal number of its current IP address, and on some platforms an .ARCH suffix. The hexadecimal number should use *capital* characters.

So for our example IP address, 10.0.1.15, let's look at its hexadecimal equivalent:

`user $``printf "%.2X%.2X%.2X%.2X\n" 10 0 1 15`
So for the example netboot client, it would look for a file named 0A00010F when it tftpboots.

Iif you are really, really lazy (like me), you can netboot the host to get the filename the client is looking for from the netboot server logs.

Make sure that both the rarpd and TFTP daemon you've chosen are currently running, then boot the host as described below in [Netbooting the client](https://wiki.gentoo.org#Netbooting_the_client).

The client will appear to hang after the boot net command is issued. Then on the netboot server, check the system logs for an entry for `in.tftpd`.

An example entry from a netboot server running sysklogd and tftp-hpa looks like:

The filename is shown above after "filename" in the log entry, which in this case is 0A00010F.

As a way to keep track of what netboot image you are using, and to allow multiple machines to use the same netboot image, you can use a soft link to create the file with the hexadecimal value. To create this using our sample sparc64 host and the gentoo-sparc64-20100128.tftpboot, use the following command:

`root #``/bin/ln -s /tftpboot/gentoo-sparc64-20100128.tftpboot /tftpboot/0A00010F`
Now everything should be set for netbooting!

## Netbooting the client

From OpenBoot PROM (OBP) on the SPARC, enter the command;

`ok``boot net`
Other methods for certain machines are:

`ok``boot net-tpe`
This will initiate the networking booting process. A constantly changing string of hexadecimal digits should appear. When the image has finished loading, the kernel will take over and start the OS booting process. In the case of our sparc64 install image, you will be left at a shell prompt from which you can begin the install process.

## Troubleshooting

**Building the prerequisite software**

If the netboot server is a Gentoo Linux system and experiences problems after installing the rarpd and tftpd packages, please search the [Gentoo Forums](http://forums.gentoo.org) and [Gentoo Bugzilla](http://bugs.gentoo.org) to see if this problem has been encountered by anyone else. If it has not, or the solutions found do not work, then please open a new bug.

**I've issued the boot net command but it appears to hang.**

This is presumably because the file your system is trying to load from the tftpboot server is not available. On a SPARC system, you would probably see the following:

Double check that the file the client needs does exist in /tftpboot. You can confirm the filename it is requesting by looking in the system logs. Also, once this file exists, the client will try to load it. Sometimes, when the file is missing originally, it will freeze downloading the file once it appears. To resolve this, just get back to an OBP prompt, and issue the "boot net" command again. The host should then start downloading the tftpboot image and boot the OS.

**I'm trying to netboot, but all I see are "Timeout waiting for ARP/RARP packet" messages.**

This could be due to a few different problems:

1. Make sure the entry in /etc/ethers exists for the client in question. If the MAC address is incorrect and/or the netboot server cannot resolve the hostname for the client, it cannot respond with the needed information.
2. Verify that the network hub or switch the netboot server and client are connected to allow RARP traffic to flow freely. If the client's request cannot reach the server, or vice versa, the host will be unable to continue.
3. No one is responding to the RARPD request because no services are listening. Verify that the rarpd service is up and running.
4. The client does not think its NIC has a link to the network hub/switch it is plugged into. Check to see if the NIC and the port on the network hub or switch has a link light. If the link light is on, check to see what the setting of tpe-link-test? is in OBP with the command; `printenv tpe-link-test?`. You should receive something like tpe-link-test? false true. The first column represents the parameter name, the second column shows the current value for the the parameter, and the third column shows the default value for the parameter. In the example above, we can see that the current value is false, which means that the client is not checking to see if the client and network hub or switch can establish a link before issuing its RARP request. Often times this can cause the problem.

To change the value of tpe-link-test? from an OBP prompt, issue the following command:

`ok``setenv tpe-link-test? true`
tpe-link-test? =      true

This shows the value of tpe-link-test? is now true. Try netbooting the client again.
