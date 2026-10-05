<!-- source: https://wiki.gentoo.org/wiki/S390/Hercules | group: Gentoo Wiki (Main) | wiki-title: S390/Hercules -->
---
title: S390/Hercules
url: https://wiki.gentoo.org/wiki/S390/Hercules
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2020-03-12"
fingerprint: "3e0bfa1b05a758e4"
license: CC BY-SA 4.0
---

# S390/Hercules

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

## Overview

This guide is about installing Gentoo in a emulated S390 machine using Hercules.

## Requirements

To be able to install Gentoo, you'll need the following:

- A machine that is able to run Hercules
- Hercules, the emulator (app-emulation/hercules in Gentoo)
- A network connection
- iptables if you want to add networking to hercules
  - In this case you'll also need a kernel with TUN/TAP support (CONFIG\_TUN=y/m)

## Setting up the Hercules environment for installing

### Overview

For installing Gentoo, we're going to need to download some files for the installation environment. Once we boot into the installation environment, we'll be able to install Gentoo.

Keep reading.

### Choosing the userland

Before going further with the process, we have to choose which userland we want to install.

In Gentoo we provide two userlands: a 31-bit one, which in the Linux on zSeries is named as ***s390*** and a 64-bit one, which in the Linux on zSeries is called ***s390x***.

### Create the work directory

Let's create a directory where our VM will be located, so we can put in there all the files we need. For example, **/home/user/hercules**.

`root #````
mkdir hercules
```
`root #````
cd hercules
```
Create the following directories: **dasd**, **rdr** and **prt**

`root #````
mkdir dasd rdr prt
```
### Downloading the installation images

Once you've made up your choice of userland, you can now download the needed files from the mirrors. In the s390 autobuilds directory, this means releases/s390/autobuilds/, inside the directory with the last date, there should be two files: **netboot-ARCH-kernel-DATE** and **netboot-ARCH-initramfs-DATE**.

**ARCH** should be either **s390** or **s390x**, and **DATE** should be the date of the build. So choose if you want to download **s390** or **s390x**, select one of them depending in the choice you made in the previous step, whether you want 31bit or 64bit. In the example we'll use s390x. Once you've made your choice, download the files and put them into the **rdr** directory.

Create a file called **gentoo.parmfile** with the following content:



**`gentoo.parmfile`**

**Example**

And put into the **rdr** directory as well.



### Setup the hercules environment

Create a configuration file for hercules, let's call it **s390x.cnf** with this content:

CPUSERIAL 002623        # CPU serial number
CPUMODEL  2098          # CPU model number
MAINSIZE  1024           # Main storage size in megabytes
XPNDSIZE  0             # Expanded storage size in megabytes
CNSLPORT  3270          # TCP port number to which consoles connect
NUMCPU    2             # Number of CPUs
LOADPARM  0120....      # IPL parameter
OSTAILOR  LINUX         # OS tailoring
PANRATE   SLOW          # Panel refresh rate (SLOW, FAST)
ARCHMODE  ESAME         # Architecture mode ESA/390 or ESAME
# .-----------------------Device number
# |     .-----------------Device type
# |     |       .---------File name and parameters
# |     |       |
# V     V       V
#---    ----    --------------------
#---    ----    --------------------
# console
001F    3270
# terminal
0009    3215
# reader
000C    3505    ./rdr/netboot-s390x-kernel-20141003 ./rdr/gentoo.parmfile ./rdr/netboot-s390x-initramfs-20141003 autopad eof
# printer
000E    1403    ./prt/print00e.txt crlf
# dasd
0120    3390    ./dasd/3390.LINUX.0120
# tape
0581    3420
# network                               s390     realbox
0A00,0A01  CTCI -n /dev/net/tun -t 1500 10.1.1.2 10.1.1.1

From the first part, the important parameters are **CPUMODEL**, which specifies which kind of machine we are emulating. Note that Hercules doesn't support all the machines. **MAINSIZE** the RAM quantity you want to use for the VM. **NUMCPU** is the number of cores you want the VM to have. There are some reports in Debian that Hercules doesn't work well with a high quantity of cores(more than 4)

Adjust the **reader** part with the filenames of the files you've downloaded.

The other important part is the last line, which defines the networking, the first IP is the one the VM will have while the second is the one the host machine will have.

### Create the disks

Go into the **dasd** directory and create the disks. In this guide, we'll use one partition for everything.

`root #````
dasdinit -lfs -linux 3390.LINUX.0120 3390-3 LIN120 6963
```
The **6963** is the number of cylinders. In this type of storage, each cylinder has 15 tracks per cylinder, and 56832 bytes per track, therefore one cylinder equals to 0.85MBytes. So if you wanted to create a 8GB disk, you should do **8192 \* 0.85** = 6963 rounded.

### Run Hercules

Let's go back to our root work directory and run hercules:

`root #````
hercules -f s390x.cnf
```
You can ignore the "tun0: Operation not supported" message.

### Setup networking in the host machine with iptables

In another console, setup the networking.

`root #````
iptables -t nat -A POSTROUTING -o eth0 -s 10.1.1.0/24 -j MASQUERADE
```
`root #````
iptables -A FORWARD -s 10.1.1.0/24 -j ACCEPT
```
`root #````
iptables -A FORWARD -d 10.1.1.0/24 -j ACCEPT
```
`root #````
echo 1 > /proc/sys/net/ipv4/ip_forward
```
`root #````
echo 1 > /proc/sys/net/ipv4/conf/all/proxy_arp
```
### Boot the Gentoo installation environment

Let's start the Gentoo installation environment, inside Hercules type:

**Command ==>** ipl c
It may take a while until you get a shell prompt, since it has to emulate all the code.

If everything went well, you should see something like this:

```
BusyBox v1.20.2 (2013-03-01 11:14:08 UTC) built-in shell (ash)                  
```
Enter 'help' for a list of built-in commands.

/bin/ash: can't access tty; job control turned off                              
/ #

Great, we have our installation environment ready!

### Typing into the shell

In Hercules, if you want to type into the shell, you have to use a . (dot) before the command, for example:

**Command ==>** .uptime
will run **uptime** in the shell.

### Setup the networking in the emulated system

Okay, we have the networking setup in the host system, but we have to configure it in the emulated machine as well. We'll use the networking to be able to ssh into the install environment, after all typing the commands with a dot in front and the Hercules console isn't very comfortable.

Inside Hercules, type:

**Command ==>**```
.modprobe ctcm
```
**Command ==>**```
.echo 0.0.0a00,0.0.0a01 > /sys/bus/ccwgroup/drivers/ctcm/group
```
**Command ==>**```
.echo 1 > /sys/devices/ctcm/0.0.0a00/online
```
**Command ==>**```
.ifconfig ctc0 10.1.1.2 pointopoint 10.1.1.1 netmask 255.255.255.255
```
**Command ==>**```
.route add default gw 10.1.1.1
```
**Command ==>**```
.ifconfig lo 127.0.0.1
```
We also need to setup the DNS.

**Command ==>** .echo 'nameserver 8.8.8.8' > /etc/resolv.conf

This should make the network functional.

### Setup a password and ssh into the emulated machine

Last step before ssh'ing into the machine is setup a password:

`root #````
passwd
```
Great, now let's ssh into the machine from our host machine, in this example the emulated machine should have the IP 10.1.1.2

`root #````
ssh root@10.1.1.2
```
type the password you've just set and now we should be inside the install environment!!!

Let's proceed with the installation.

## Installing Gentoo

### Overview

We now have an installation environment where we can prepare our Gentoo s390 system.

What we'll have to do to setup our installation is:

1. Initialize and format the DASD disks and mount them
2. Download a stage3 and a portage snapshot
3. Setup fstab
4. Setup root password
5. Configure hostname and networking (optional, but recommended)
6. Enable SSH access (optional, but recommended)
7. Enable serial console access

### Initializing and formatting the DASD disks

If you haven't formatted yet your DASD disks outside of Linux, you can do that now. Here are some informations:

- You need to use **dasdfmt** before you can use your DASD
- Swap Partitions cannot exceed 4GiB
- A maximum of three partitions can be created on a physical volume
- No gaps should be left between partitions
- There is no tool for moving or resizing partitions
- Before creating any partitions, you need to initialize the disks. You can do that with dasdfmt.

We initialize the first partition

`root #````
dasdfmt -f /dev/dasda --label=gentoo
```
For creating partitions, we'll use **fdasd**. There are several different modes to run **fdasd** in. The first mode is an interactive mode. You can simply do that by running **fdasd /dev/dasda**.

There is also a mode that will automatically create one partition on the disk. To keep the setup simple, we'll do that.

`root #````
fdasd -a /dev/dasda -a -l gentoo
```
Now we'll have one partition, **/dev/dasda1**, which we can format with the known Linux filesystems. In our case, we'll format this partition with **ext4**.

`root #````
mkfs.ext4 /dev/dasda1
```
Now let's mount this partition on /mnt/gentoo.

`root #````
mount /dev/dasda1 /mnt/gentoo
```
And let's change to the mounted directory

`root #````
cd /mnt/gentoo
```
### Downloading a stage3

Remember the choice you made before between 31-bit **s390** and 64-bit **s390x**? You have to download a stage3 for the choice you've made before from your favorite mirror.

### Extracting a stage3

`root #````
tar xjpf stage3-s390x-latest.tar.bz2 -C /mnt/gentoo
```
### Extracting a portage snapshot

`root #````
tar xjpf portage-latest.tar.bz2 -C /mnt/gentoo/usr
```
### Chrooting into your new system

To chroot into the real system we need to mount some directories.

`root #````
mount -t proc none /mnt/gentoo/proc
```
`root #````
mount --rbind /sys /mnt/gentoo/sys
```
`root #````
mount --rbind /dev /mnt/gentoo/dev
```
Copy the **/etc/resolv.conf** file to the new system

`root #````
cp -L /etc/resolv.conf /mnt/gentoo/etc
```
Finally, let's chroot into the new system

`root #````
chroot /mnt/gentoo /bin/bash
```
`root #````
source /etc/profile
```
`root #````
env-update
```
### Fix permissions

Unfortunately, the permission in some character devices are wrong, so we need to fix them before we run emerge.

`root #````
chmod 0666 /dev/null /dev/urandom
```
### Setup fstab

Edit the **/mnt/gentoo/etc/fstab** file to look like this:

**`/etc/fstab`**

**Example**

### Setting the default root password

This is the most important part of the installation. As without the root password we won't be able to login!

For setting the password, simply run passwd

`root #``passwd`
### Setup hostname and networking

Edit the **/etc/conf.d/hostname** file to adjust the hostname:

**`/etc/conf.d/hostname`**

**Example**

Edit the **/etc/conf.d/net** file like this, to fit our example:

**`/etc/conf.d/net`**

**Example**

Let's create some symlinks to start the network at boot

`root #````
ln -sf /etc/init.d/net.lo /etc/init.d/net.ctc0
```
`root #``rc-update add net.ctc0 default`
### Adjust inittab

You have to adjust the following lines in the **/etc/inittab**:

**`/etc/inittab`**

**Example**

In other words, you have to comment the tty1-6 lines.



### Enabling SSH access

We add sshd to the startup of our system so we can access our system using ssh.

`root #````
rc-update add sshd default
```
### Remove errors from boot process

There are two init scripts that in our s390 machine aren't going to work: **termencoding** and **keymaps**. The failures will show up as a failures in the console.

To avoid the useless messages, because they aren't going to work in our machine, let's remove them from the boot process:

`root #````
rc-update del termencoding boot
```
`root #````
rc-update del keymaps boot
```
### Kernel

To continue, kernel has to be built.

Unfortunately s390 is nowadays an unstable arch in Gentoo, so this means that some packages aren't stable. One of those packages is gentoo-sources. So before emerge **gentoo-sources**, allow all keywords:

`root #````
echo 'sys-kernel/gentoo-sources **' > /etc/portage/package.accept_keywords
```
Next, install the sources:

`root #````
emerge gentoo-sources
```
Once that is done, the kernel sources will exist in /usr/src/linux. Change into that directory:

`root #````
cd /usr/src/linux
```
A kernel configuration file is necessary. To ensure the ability to boot the Hercules VM, use the kernel configuration from the installation environment:

`root #````
zcat /proc/config.gz > .config
```
Now compile it and its modules:

`root #````
make && make modules_install
```
Once that is done, copy the kernel file to the **/boot** directory:

`root #````
cp arch/s390/boot/image /boot/image
```
### Bootloader

Edit the **/etc/zipl.conf** file like this, to fit our example:

**`/etc/zipl.conf`**

**Example**

Create the /boot/zipl directory

`root #````
mkdir -p /boot/zipl
```
Now let's run **zipl**:

`root #````
zipl
```
If everything went fine it should output something like this:

Using config file '/etc/zipl.conf'
Building bootmap in '/boot/zipl'
Building menu 'menu'
Adding #1: IPL section 'Gentoo' (default)
Preparing boot device: dasda (0120).
Done.

## Finishing the installation

Great, we're done. Let's exit from the chroot and umount the filesystem

`root #````
exit
```
`root #````
cd
```
`root #````
umount /mnt/gentoo/sys /mnt/gentoo/proc /mnt/gentoo/dev/shm /mnt/gentoo/dev/pts /mnt/gentoo/dev /mnt/gentoo
```
And let's power off the machine:

`root #````
poweroff -f
```
## Boot our installed system

Now, to boot our new installed system, we have to type into Hercules:

**Command ==>** ipl 120
If everything went successfully, we should see the kernel booting and after a few minutes, the login prompt.

If we configured SSH properly, we should be able to SSH into it.

### Procedure for booting our VM in a fresh booted system

This would be the procedure for booting the VM in a fresh booted system, with clear iptables rules, etc...

cd into our work directory

Run hercules:

`root #````
hercules -f s390.cnf
```
In another console, setup iptables:

`root #````
iptables -t nat -A POSTROUTING -o eth0 -s 10.1.1.0/24 -j MASQUERADE
```
`root #````
iptables -A FORWARD -s 10.1.1.0/24 -j ACCEPT
```
`root #````
iptables -A FORWARD -d 10.1.1.0/24 -j ACCEPT
```
`root #````
echo 1 > /proc/sys/net/ipv4/ip_forward
```
`root #````
echo 1 > /proc/sys/net/ipv4/conf/all/proxy_arp
```
Run the VM in hercules:

**Command ==>** ipl 120
And that should be it.

Have fun!

## Issues

These are some issues when running hercules:

### Reboot doesn't work

Rebooting doesn't seem to work. When you reboot, hercules will start displaying some messages like:

CPU0000: Operand exception CODE=0015 ILC=4                            
CPU0000:  PSW=00000000 80000000 0000000000111170 INST=B232D116     MSCH  278(13)
CPU0000:  R:000000000011125C:K:06=00000000 00000000 00000000 00000000  .........
CPU0000: R0=0000000000000000 R1=00000000001110A0                                
CPU0000: R2=00000000001110A0 R3=0000000080000000                                
CPU0000: R4=00000000003EA250 R5=0000000000000000                                
CPU0000: R6=0000000000000000 R7=0000000000772010                                
CPU0000: R8=000000003FFFF000 R9=000000000010DAB0
...

The only way I have found to stop these messages and boot again is with the following commands:

**Command ==>**```
stopall
```
**Command ==>**```
ipl 120
```
### Unable to boot the install environment after issuing 'ipl 120'

I have found that if you have been running your system with 'ipl 120', after stopping it and issuing 'ipl c', hercules answers:

```
HHCCP029E z/Arch mode IPL failed: CSW status=0D00                               
           Sense=00000000           
```
To fix this, you need to reinitialize the card reader, you can do that with:

**Command ==>** devinit c
And then you can run **ipl c** again.

## Tips and tricks

- To power off the machine when you can't login, type **ssd** into the hercules command line. It should send a shutdown signal to the machine.
