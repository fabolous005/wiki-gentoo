<!-- source: https://wiki.gentoo.org/wiki/Diskless_nodes | group: Gentoo Wiki (Main) | wiki-title: Diskless nodes -->
---
title: Diskless nodes
url: https://wiki.gentoo.org/wiki/Diskless_nodes
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-09-21"
fingerprint: "179bb15bd1a23f17"
license: CC BY-SA 4.0
---

# Diskless nodes

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

This article provides instructions for creating and setting up diskless nodes with Gentoo Linux.

## Introduction

### About this article

This article will help setting up *diskless* workstations based on the Gentoo Linux distribution. This guide is intended to make the process as user friendly as possible and cater to the Linux newbie, because everyone was at a certain point. While an experienced user could easily tie the multiple articles available on diskless nodes and networking together it's hoped that this guide can ease the installation for all interested users, geeks, or not.

### What is a diskless machine?

A diskless machine is a PC without any of the usual boot devices such as hard disks, floppy drives or CD-ROMs. The diskless node boots off the network and needs a server that will provide it with storage space as a local hard disk would. From now on the server will be the *master*, while the diskless machine gets called the *slave* (what's in a name :). The slave node needs a network adapter that supports PXE booting or Etherboot; check [Etherboot.org](http://www.etherboot.org) for support listings. Most modern cards support PXE and many built-in adapters on motherboards will also work.

### Before starting

Gentoo should be installed on the master node and enough space on the master to store the file systems of the slave nodes that are going to be hosted. Also make sure there is one interface to the internet separated from the local area connection.

## Configuration

### Master and the slaves

#### About kernels

The kernel is the software that sits between the hardware and all other software that is loaded on the machine, essentially the heart of a kernel based operating system. When a computer is started, the BIOS executes the instructions found at the reserved boot space of the hard drive. These instructions are typically a boot loader that loads a kernel. After a kernel has been loaded all processes are handled by the kernel.

For more information on kernels and kernel configuration check out the [kernel](https://wiki.gentoo.org/wiki/Kernel) article.

#### The master kernel

The master kernel can be as large and as customized as desired but there are a few required kernel options that need to be selected. Go into the kernel configuration menu by typing:

`root #````
cd /usr/src/linux
```
`root #``make menuconfig`
There should be a grey and blue GUI that offers a safe alternative to manually editing the /usr/src/linux/.config file. If the kernel is currently functioning well it might be a good idea to save the current configuration file by exiting the GUI and typing:

`root #``cp .config .config_working`
Go into the following sub-menus and make sure the listed items are checked as built-in (and *NOT* as modular). The options show below are taken from the 2.6.10 kernel version. If a different version is used, the text or sequence might differ. Just make sure to select at least those shown below.

**master's kernel options**

```
[*] Networking support --->
  Networking options --->
    <*> Packet socket
    <*> Unix domain sockets
    [*] TCP/IP networking
    [*]   IP: multicasting
    [ ] Network packet filtering (replaces ipchains)
  
File systems --->
  Network File Systems  --->
    <*> NFS server support
    [*]   Provide NFSv3 server support
```
If access to internet through the master node is required and/or a secure firewall is needed make sure to add support for iptables:

**Enable iptables support**

```
[*] Network packet filtering (replaces ipchains)
  IP: Netfilter Configuration  --->
    <*> Connection tracking (required for masq/NAT)
    <*> IP tables support (required for filtering/masq/NAT)
```
If packet filtering is required, add the rest as modules later. Make sure to read the [Gentoo Security Handbook Chapter about Firewalls](https://wiki.gentoo.org/wiki/Security_Handbook/Firewalls_and_Network_Security) on how to set this up properly.

After the master kernel has been re-configured, it needs to be rebuilt:

`root #````
make && make modules_install
```
`root #``cp arch/i386/boot/bzImage /boot/bzImage-master`
Then add an entry for that new kernel into lilo.conf or grub.conf depending on which bootloader is being used and make the new kernel the default one. Now that the new bzImage has been copied into the boot directory all that has to be done is to reboot the system in order to load these new options.

#### About the slave kernel

It is recommended that the slave kernel be compiled without any modules, since loading and setting them up via remote boot is a difficult and unnecessary process. Additionally, the slave kernel should be as small and compact as possible in order to efficiently boot from the network. The slave's kernel is going to be compiled in the same place where the master was configured.

To avoid confusion and wasting time it is probably a good idea to backup the master's configuration file by typing:

`root #``cp /usr/src/linux/.config /usr/src/linux/.config_master`
The slave's kernel is now to be configured in the same fashion as the master's kernel. If a fresh configuration file is needed it can be recovered from the default /usr/src/linux/.config file by typing:

`root #````
cd /usr/src/linux
```
`root #``cp .config_master .config`
Now go into the configuration GUI by typing:

`root #````
cd /usr/src/linux
```
`root #``make menuconfig`
Make sure to select the following options as built-in and *NOT* as kernel modules:

**slave's kernel options**

```
[*] Networking support --->
  Networking options --->
    <*> Packet socket
    <*> Unix domain sockets
    [*] TCP/IP networking
    [*]   IP: multicasting
    [*]   IP: kernel level autoconfiguration
    [*]     IP: DHCP support
  
File systems --->
  Network File Systems  --->
    <*> file system support
    [*]   Provide NFSv3 client support
    [*]   Root file system on NFS
```
Now the slave's kernel needs to be compiled. Be careful here not to overwrite or mess up the modules (if any) that have been built for the master:

`root #````
cd /usr/src/linux
```
`root #``make`
Now create the directory on the master that will be used to hold slaves' files and required system files. The /diskless is used but any location preferred may be chosen here. Now copy the slave's bzImage into the /diskless directory:





`root #````
mkdir /diskless
```
`root #````
cp /usr/src/linux/arch/i386/boot/bzImage /diskless
```
#### The preliminary slave file system

The master and slave filesystems can be tweaked and changed a lot. Right now the only point of interest is in getting a preliminary filesystem of appropriate configuration files and mount points. First it's required to create a directory within /diskless for the first slave. Each slave needs its own root file system because sharing certain system files will cause permission problems and hard crashes. These directories can be called anything the administrator deems appropriate but the article suggests using the slaves IP addresses as they are unique and not confusing. The static IP of the first slave will be, for instance, `192.168.1.21`:

`root #``mkdir -p /diskless/192.168.1.21/etc`
Various configuration files in /etc need to be altered to work on the slave. Copy the master's /etc directory onto the new slave root by typing:

`root #``cp -r /etc/* /diskless/192.168.1.21/etc/`
Still this filesystem isn't ready because it needs various mount points and directories. To create them, type:

`root #````
mkdir /diskless/192.168.1.21/home
```
`root #````
mkdir /diskless/192.168.1.21/dev
```
`root #````
mkdir /diskless/192.168.1.21/proc
```
`root #````
mkdir /diskless/192.168.1.21/tmp
```
`root #````
mkdir /diskless/192.168.1.21/mnt
```
`root #````
chmod a+w /diskless/192.168.1.21/tmp
```
`root #````
mkdir /diskless/192.168.1.21/mnt/.initd
```
`root #````
mkdir /diskless/192.168.1.21/root
```
`root #````
mkdir /diskless/192.168.1.21/sys
```
`root #````
mkdir /diskless/192.168.1.21/var
```
`root #````
mkdir /diskless/192.168.1.21/var/empty
```
`root #````
mkdir /diskless/192.168.1.21/var/lock
```
`root #````
mkdir /diskless/192.168.1.21/var/log
```
`root #````
mkdir /diskless/192.168.1.21/var/run
```
`root #````
mkdir /diskless/192.168.1.21/var/spool
```
`root #````
mkdir /diskless/192.168.1.21/usr
```
`root #````
mkdir /diskless/192.168.1.21/opt
```
Most of these "stubs" should be recognizable; stubs like /dev, /proc, or /sys will be populated when the slave starts, the others will be mounted later. The /diskless/192.168.1.21/etc/conf.d/hostname file should also be changed to reflect the hostname of the slave. Binaries, libraries and other files will be populated later in this HOWTO right before attempting to boot the slave.

Even though /dev is populated by `udev` later on, the console entry needs to be created. If not, the error message "unable to open initial console" will be encountered.

`root #``mknod /diskless/192.168.1.21/dev/console c 5 1`
### The DHCP server

#### About the DHCP server

DHCP stands for Dynamic Host Configuration Protocol. The DHCP server is the first computer the slaves will communicate with when they PXE boot. The primary purpose of the DHCP server is to assign IP addresses. The DHCP server can assign IP addresses based on hosts ethernet MAC addresses. Once the slave has an IP address, the DHCP server will tell the slave where to get its initial file system and kernel.

#### Before getting started

There are following things to make sure, they are working properly before beginning. First check the network connectivity using the ip link show eth0 command. Verify following 3 entries are shown:

- `state UP`
- `UP`
- `MULTICAST`

on the command line output.

`user $``ip link show eth0````
2: eth0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast state UP mode DEFAULT group default qlen 1000
    link/ether 0c:2e:53:f2:00:00 brd ff:ff:ff:ff:ff:ff
```
Using the ip maddress show eth0 command it is easy to display multicast addressing assinged on the interface **eth0**. The result shows 3 active multicast address ranges:

`user $``ip  maddress show eth0`
2:	eth0
	link  33:33:00:00:00:01
	link  33:33:00:00:00:02 users 2
	link  01:00:5e:00:00:01
	link  33:33:ff:a5:c9:45
	link  33:33:ff:00:00:00
	inet  224.0.0.1
	inet6 ff02::1:ff00:0
	inet6 ff02::1:ffa5:c945
	inet6 ff05::2
	inet6 ff01::2
	inet6 ff02::2
	inet6 ff02::1
	inet6 ff01::1

Command output overview table:

| Address layer | Address range | Description | 
|---|---|---|
| `link` | **33:33:**xx:xx:xx:xx | Ethernet Address Mapping Space - *IPv6 Packets over Ethernet* | 
| `inet` | **224.0.0.0/4** | IP Multicast Address Space | 
| `inet6` | **ff00::/8** | IPv6 Multicast Address Space | 

#### Installing the DHCP server

If the network does not already have a DHCP server installed, one needs to be installed now:

`root #``emerge --ask dhcp`
If the network already has a DHCP server installed, edit the configuration file to get the PXE boot to function correctly.

#### Configuring the DHCP server

There is only one configuration file that needs to be edited before starting the DHCP server: /etc/dhcp/dhcpd.conf. Copy and edit the provided sample file:

`root #````
cp /etc/dhcp/dhcpd.conf.sample /etc/dhcp/dhcpd.conf
```
`root #``nano -w /etc/dhcp/dhcpd.conf`
The general layout of the file is set up in an indented fashion and looks like this:

The `shared-network` block is optional and should be used for IPs that are required to be assigned that belong to the same network topology. At least one `subnet` must be declared and the optional `group` block allows options to be grouped between items. A good example of dhcpd.conf looks like this:

The IP address after `next-server` will be asked for the specified `filename`. This IP address should be the IP of the tftp server, usually the same as the master's IP address. The `filename` is relative to the /diskless directory (this is due to the tftp server specific options which will be covered later). Inside the `host` block, the `hardware ethernet` option specifies a MAC address, and `fixed-address` assigns a fixed IP address to that particular MAC address. There is a pretty good man page on dhcpd.conf with options that are beyond the scope of this HOWTO. The man page can be read by typing:

`user $``man dhcpd.conf`
#### Starting the DHCP server

Before starting the dhcp initialization script edit the /etc/conf.d/dhcp file so that it looks something like this:

The `IFACE` variable is the device that the DHCP server will be running on, in this case `eth0`. Adding more arguments to the `IFACE` variable can be useful for a complex network topology with multiple Ethernet cards. To start the dhcp server type:

`root #``rc-service dhcpd start`
To add the dhcp server to the start-up scripts type:

`root #``rc-update add dhcpd default`
#### Troubleshooting the DHCP server

To see if a node boots, take a look at /var/log/messages. If the node successfully boots, the messages file should have some lines at the bottom looking like this:

If the following message is encountered it probably means there is something wrong in the configuration file but that the DHCP server is broadcasting correctly.

Every time after changing the configuration file the DHCP server must be restarted. To restart the server type:

`root #``rc-service dhcpd restart`
### TFTP server and PXE Linux Bootloader and/or Etherboot

#### About the TFTP server

TFTP stands for Trivial File Transfer Protocol. The TFTP server is going to supply the slaves with a kernel and an initial filesystem. All of the slave kernels and filesystems will be stored on the TFTP server, so it's probably a good idea to make the master the TFTP server.

#### Installing the TFTP server

A highly recommended tftp server is available as the tftp-hpa package. This tftp server happens to be written by the author of SYSLINUX and it works very well with pxelinux. To install simply type:

`root #``emerge --ask tftp-hpa`
#### Configuring the TFTP server

Edit /etc/conf.d/in.tftpd. The tftproot directory needs to specified with `INTFTPD_PATH` and any command-line options with `INTFTPD_OPTS`. It should look something like this:

**`/etc/conf.d/in.tftpd`**

```
INTFTPD_PATH="/diskless"
INTFTPD_OPTS="-l -v -s ${INTFTPD_PATH}"
```
The `-l` option indicates that this server listens in stand alone mode so inetd does not have to be run. The `-v` indicates that log/error messages should be verbose. The `-s /diskless` specifies the root of the tftp server.

#### Starting the TFTP server

To start the tftp server type:

`root #``rc-service in.tftpd start`
This should start the tftp server with the options that were specified in the /etc/conf.d/in.tftpd. If this server is to be automatically started at boot type:

`root #``rc-update add in.tftpd default`
#### About PXELINUX

This section is not required if only Etherboot is being used. PXELINUX is the network bootloader equivalent to LILO or GRUB and will be served via TFTP. It is essentially a tiny set of instructions that tells the client where to locate its kernel and initial filesystem and allows for various kernel options.

#### Before getting started

Now the file pxelinux.0 is required, which comes in the SYSLINUX package by H. Peter Anvin. This package can be installed by typing:

`root #``emerge --ask syslinux`
#### Setting up PXELINUX

Before starting the tftp server pxelinux needs to be set up. First copy the pxelinux binary into the /diskless directory:

`root #````
cp /usr/share/syslinux/pxelinux.0 /diskless
```
`root #````
mkdir /diskless/pxelinux.cfg
```
`root #````
touch /diskless/pxelinux.cfg/default
```
This will create a default bootloader configuration file. The binary pxelinux.0 will look in the pxelinux.cfg directory for a file whose name is the client's IP address in hexadecimal. If it does not find that file it will remove the rightmost digit from the file name and try again until it runs out of digits. Versions 2.05 and later of syslinux first perform a search for a file named after the MAC address. If no file is found, it starts the previously mentioned discovery routine. If none is found, the default file is used.

Let's start with the default file:

The `DEFAULT` tag directs pxelinux to the kernel bzImage that was compiled earlier. The `APPEND` tag appends kernel initialisation options. Since the slave kernel was compiled with `NFS_ROOT_SUPPORT` , the nfsroot will be specified here. The first IP is the master's IP and the second IP is the directory that was created in /diskless to store the slave's initial filesystem.
Other NFS options [may also be supplied](https://www.kernel.org/doc/Documentation/filesystems/nfs/nfsroot.txt). For example, to use NFS v4.1 over TCP, append `,tcp,vers=4.1` to the nfsroot kernel option: `nfsroot=192.168.1.1:/diskless/192.168.1.21,tcp,vers=4.1`.

#### Troubleshooting the network boot process

There are a few things that can be done to debug the network boot process. Primarily a tool called `tcpdump` can be used. To install `tcpdump` type:

`root #``emerge --ask tcpdump`
Now various network traffic can be listened to, to make sure the client/server interactions are functioning. If something isn't working there are a few things that could be checked. First make sure that the client/server is physically connected properly and that the networking cables are not damaged. If the client/server is not receiving requests on a particular port make sure that there is no firewall interference. To listen to interaction between two computers type:

`root #``tcpdump host client_ip and server_ip`
The `tcpdump` command can also be configured to listen on particular port such as the tftp port by typing:

`root #``tcpdump port 69`
A common error that might be received is: "PXE-E32: TFTP open time-out". This is probably due to firewall issues. If `TCPwrappers` is being used, it might be worth checking /etc/hosts.allow and /etc/hosts.deny and make sure that they are configured properly. The client should be allowed to connect to the server.

### The NFS server

#### About the NFS server

NFS stands for Network File System. The NFS server will be used to serve directories to the slave. This part can be somewhat personalized later, but right now all that is wanted is a preliminary slave node to boot diskless.

#### About Portmapper

Various client/server services do not listen on a particular port, but instead rely on RPCs (Remote Procedure Calls). When the service is initialised it listens on a random port and then registers this port with the Portmapper utility. NFS relies on RPCs and thus requires Portmapper to be running before it is started.

#### Before starting

The NFS Server needs kernel level support so if the kernel does not have this, the master's kernel needs to be recompiled. To double check the master's kernel configuration type:

`root #``grep NFS /usr/src/linux/.config_master`
The output should look something like this if the kernel has been properly configured:

**Proper NFS specific options in the master's kernel configuration**

```
CONFIG_PACKET=y
# CONFIG_PACKET_MMAP is not set
# CONFIG_NETFILTER is not set
CONFIG_NFS_FS=y
CONFIG_NFS_V3=y
# CONFIG_NFS_V4 is not set
# CONFIG_NFS_DIRECTIO is not set
CONFIG_NFSD=y
CONFIG_NFSD_V3=y
# CONFIG_NFSD_V4 is not set
# CONFIG_NFSD_TCP is not set
```
#### Installing the NFS server

The NFS package that can be acquired through portage by typing:

`root #``emerge --ask nfs-utils`
This package will emerge a portmapping utility, nfs server, and nfs client utilities and will automatically handle initialisation dependencies.

#### Configuring the NFS server

There are three major configuration files that will have to be edited:

The /etc/exports file specifies how, to who and what to export through NFS. The slave's fstab will be altered so that it can mount the NFS filesystems that the master is exporting.

A typical /etc/exports for the master should look something like this:

**`/etc/exports`**

**master exports file**

```
# one line like this for each slave
/diskless/192.168.1.21   192.168.1.21(sync,rw,no_root_squash,no_all_squash)
# common to all slaves
/opt   192.168.1.0/24(sync,ro,no_root_squash,no_all_squash)
/usr   192.168.1.0/24(sync,ro,no_root_squash,no_all_squash)
/home  192.168.1.0/24(sync,rw,no_root_squash,no_all_squash)
# if you want to have a shared log
/var/log   192.168.1.21(sync,rw,no_root_squash,no_all_squash)
```
The first field indicates the directory to be exported and the next field indicates to who and how. This field can be divided in two parts: who should be allowed to mount that particular directory, and what the mounting client can do to the filesystem: `ro` for read only, `rw` for read/write; `no_root_squash` and `no_all_squash` are important for diskless clients that are writing to the disk, so that they don't get "squashed" when making I/O requests. The slave's fstab file, /diskless/192.168.1.21/etc/fstab , should look like this:

In this example, *master* is just the hostname of the master but it could easily be the IP of the master. The first field indicates the directory to be mounted and the second field indicates where. The third field describes the filesystem and should be NFS for any NFS mounted directory. The fourth field indicates various options that will be used in the mounting process (see mount(1) for info on mount options). Some people have had difficulties with soft mount points so here they are made hard mounts, a look into various /etc/fstab options should be done to make the cluster more efficient.

The last file that should be edited is /etc/conf.d/nfs which describes a few options for nfs when it is initialised and looks like this:

The `RPCNFSDCOUNT` should be changed to the number of diskless nodes on the network.

#### Starting the NFS server

The nfs server should be started with its init script located in /etc/init.d by typing:

`root #``rc-service nfs start`
If this script is to be started every time the system boots simply type:

`root #``rc-update add nfs default`
### Completing the slave filesystem

#### Copy the missing files

Now the slave's file system will be made in sync with the master's and provide the necessary binaries while still preserving slave specific files.

`root #````
rsync -avz /bin /diskless/192.168.1.21
```
`root #````
rsync -avz /sbin /diskless/192.168.1.21
```
`root #``rsync -avz /lib /diskless/192.168.1.21`
#### Configure diskless networking

In order to prevent the networking initscript from killing the connection to the NFS server, an option needs to be added to /etc/conf.d/net on the diskless client's filesystem.

#### Initialization scripts

Init scripts for slaves are located under /diskless/192.168.1.21/etc/runlevels for services needed on the diskless nodes. Each slave can be set up and customized here, it all depends on what each slave is meant to do.

Now is a good time to boot the slave and cross some fingers. It works? Congratulations, you are now the proud owner of (a) diskless node(s).
