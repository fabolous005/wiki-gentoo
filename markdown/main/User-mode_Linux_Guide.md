<!-- source: https://wiki.gentoo.org/wiki/User-mode_Linux/Guide | group: Gentoo Wiki (Main) | wiki-title: User-mode Linux/Guide -->
---
title: User-mode Linux/Guide
url: https://wiki.gentoo.org/wiki/User-mode_Linux/Guide
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2024-09-25"
fingerprint: bf9db83fdd93bfa4
license: CC BY-SA 4.0
---

# User-mode Linux/Guide

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

## Obtaining User-Mode Linux

As the [user-mode Linux website](http://user-mode-linux.sourceforge.net) states, user-mode Linux allows a user to "run Linux inside itself". Specifically, user-mode Linux provides a virtual machine on which a user can "\[r\]un buggy software, experiment with new Linux kernels or distributions, and poke around in the internals of Linux, all without risking your main Linux setup." Experimental changes to Gentoo core packages such as [sys-apps/baselayout](https://packages.gentoo.org/packages/sys-apps/baselayout) or [sys-libs/glibc](https://packages.gentoo.org/packages/sys-libs/glibc) have the potential to break the system and render it unbootable; with user-mode Linux we can test these changes without having to worry about breaking the live system.

Most 2.6 kernels have UML support. Although you can use your current kernel sources, it might be wiser to keep the UML kernel tree(s) separate. After all, you'll be building a new kernel with a different configuration and you might want to have heterogeneous systems on your main Linux system (several different UML kernels).

So download a nice kernel tree (like the vanilla one from [kernel.org](http://www.kernel.org)) and extract it to some local development location.

Next, configure this UML kernel as you would do for any other system, but append `ARCH=um` so that the kernel build software knows that the kernel is meant to run as a guest process on the main system.

`root #````
cd /srv/aegis/src/uml-linux
```
`root #``cp linux /usr/local/bin/linux`
On a default Gentoo system, /usr/local/bin is in your `$PATH`. If it isn't, you should find a definition of `PATH` in the /etc/profile and fix it:

`user $``echo $PATH | grep /usr/local/bin`
Don't forget to run `source /etc/profile` for the change to take effect.

For the user-mode Linux kernel to properly boot a Gentoo system the kernel needs to be configured to *not* automatically mount /dev (devfs) by default. Also, you will almost certainly want to make sure that you have *tmpfs* (the "Virtual Memory Filesystem") compiled in, since by default the Gentoo Linux bootscripts store their information in a small tmpfs partition. (The binary kernels available from the user-mode website do automatically mount /dev, and they don't have tmpfs compiled in; don't bother with them).

I highly recommend reading the user-mode Linux documentation, but the basic idea is that running the /usr/local/bin/linux program boots the user-mode kernel and tries to bring up the system stored in the file root\_fs that should be located in the current working directory.

It won't hurt to also install the user-mode Linux tools.

`root #``emerge --ask sys-apps/usermode-utilities`
These tools facilitate networking (among other things) between the user-mode Linux virtual system and the host Linux system.

## Creating root\_fs

### Making the Gentoo chroot

The root\_fs file needed for user-mode Linux is a single file that contains an entire Gentoo Linux file system. To generate this file you will need to have loopback device support enabled in the host (non-user-mode) kernel.

Generating the root\_fs file itself will be our last step. First we will generate a Gentoo file system in an ordinary chroot. We need the stage tarball available, which could be downloaded separately, extracted from an Installation CD, or extracted from an Installation CD .iso.

`root #````
mkdir /mnt/loop
```
`root #``mount -o loop /path/to/install-<TAB>.iso /mnt/loop`
Setting up the chroot is essentially identical to an ordinary Gentoo Linux build.

`root #````
mkdir /mnt/gentoo
```
`root #````
cd /mnt/gentoo
```
`root #``tar xvjpf /path/to/stage3.*.tar.bz2`
Go ahead and unmount the .iso. You don't need it anymore.

Build the system in the usual fashion: chroot into /mnt/gentoo and follow the Gentoo installation instructions.

Add any additional packages you desire. Feel free to give your virtual Gentoo system a hostname, if you so desire. In /etc/fstab you will want /dev/ROOT to be /dev/ubda, with a fs type of either ext2, ext3, or reiserfs. Set /dev/SWAP to be /dev/ubdb, and comment out /dev/BOOT.

At this point, remember to set your root password.

`root #``passwd`
Now we need to make some changes to the boot scripts. Remove consolefont and keymaps from the boot runlevel:

`root #````
rc-update del consolefont boot
```
`root #``rc-update del keymaps boot`
Exit the chroot, unmount all of the bind mounts, tar up the new Gentoo distro, and clean up.

`root #````
cd /mnt/gentoo
```
`root #````
tar cvjpf ~/gentoo.tbz2 *
```
`root #````
cd
```
`root #``rm -rf /mnt/gentoo`
### Making root\_fs

Our Gentoo chroot is nearly 300 MB in size, so root\_fs needs to be at least that size. We'll choose 0.5 GB as a reasonable size.

`root #````
dd if=/dev/zero of=root_fs seek=500 count=1 bs=1M
```
`root #````
mke2fs -F root_fs
```
`root #````
mount -o loop root_fs /mnt/loop
```
`root #````
tar xvjpf gentoo.tbz2 -C /mnt/loop
```
`root #``umount /mnt/loop`
It would also be nice to have a 0.5 GB swap partition.

`root #````
dd if=/dev/zero of=swap_fs seek=500 count=1 bs=1M
```
`root #``mkswap -f swap_fs`
Now see if it works!

`root #``linux ubd0=root_fs ubd1=swap_fs`
User-mode Linux uses xterms for the virtual consoles that are run at boot time, so you need to make sure that the terminal from which you run user-mode Linux has $DISPLAY properly set (along with proper xhost/xauth permissions).

With any luck you should be able to log into your user-mode Linux Gentoo system. The only thing keeping this user-mode Linux version of Gentoo from being fully functional is networking from the virtual machine to the host.

## Networking

### Using an Existing Network

Make sure that the host kernel has the following settings compiled as modules:

**Host kernel configuration**

Run the following commands on the *host* machine:

`root #``modprobe tun``root #````
modprobe iptable_nat
```
`root #````
iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE
```
`root #``echo 1 > /proc/sys/net/ipv4/ip_forward`
The iptables line sets up IP Masquerading between the private network that our user-mode system will be on and the internet (reachable via `eth0` in our case). The echo line then turns on packet forwarding between the private network and the interface that the default gateway is on (eth0 for us).

Now we bring up the user-mode system and see if networking is functional.

`root #``linux ubd0=root_fs ubd1=swap_fs eth0=tuntap,,,192.168.0.254`
Login to user-mode system, and then run:

`root #````
ip addr add 192.168.0.1/24 dev eth0
```
`root #``ping -c 2 192.168.0.254`
PING 192.168.0.254 (192.168.0.254): 56 octets data
64 octets from 192.168.0.254: icmp\_seq=0 ttl=255 time=0.8 ms
64 octets from 192.168.0.254: icmp\_seq=1 ttl=255 time=0.6 ms
--- 192.168.0.254 ping statistics ---
2 packets transmitted, 2 packets received, 0% packet loss
round-trip min/avg/max = 0.6/0.7/0.8 ms

`root #````
route add default gw 192.168.0.254
```
`root #``netstat -rn`
Kernel IP routing table
Destination  Gateway        Genmask        Flags MSS Window irtt Iface
192.168.0.0  0.0.0.0        255.255.255.0  U      40 0         0 eth0
0.0.0.0      192.168.0.254  0.0.0.0        UG     40 0         0 eth0

`root #````
scp user@192.168.0.254:/etc/resolv.conf /etc/resolv.conf
```
`root #``ping -c 2 www.gentoo.org`
PING www.gentoo.org (207.170.82.202): 56 octets data
64 octets from 207.170.82.202: icmp\_seq=0 ttl=240 time=119.6 ms
64 octets from 207.170.82.202: icmp\_seq=1 ttl=240 time=92.0 ms
--- www.gentoo.org ping statistics ---
2 packets transmitted, 2 packets received, 0% packet loss
round-trip min/avg/max = 92.0/105.8/119.6 ms

On the user-mode system we assign the user-mode eth0 interface the private IP address 192.168.0.1 and bring up the interface. The host has private IP address 192.168.0.254, and we ping it to make sure that our networking is, indeed, up. The route line adds a default gateway, namely our host, we use scp to retrieve a working /etc/resolv.conf (if necessary), and we ping www.gentoo.org to make sure that name resolution (and general access to the internet) is working from our user-mode system. Now the user-mode system can emerge at will!

### Using a Virtual Network

UML supports an additional networking facility, accessible only to UML guests. The [sys-apps/usermode-utilities](https://packages.gentoo.org/packages/sys-apps/usermode-utilities) package provides a tool called uml\_switch which defines the end points of the switch.

If the switch information should stay in the foreground:

`user $``uml_switch -unix ~/tmp/switch.sock`
If it should be backgrounded:

`user $``uml_switch -unix ~/tmp/switch.sock &> ~/tmp/switch.log &`
To start the UML instances on the switch, run the next command. Your (virtual) network interface will be connected to the uml\_switch process and will be using the given MAC address.

`user $``linux ubd0=first_rootfs ubd1=first_swapfs eth0=daemon,10:00:01:02:00:00,,~/tmp/switch.sock`
You can still connect the system to the existing network, or have a second process attached to both the virtual one and the existing one:

`user $``linux ubd0=second_rootfs ubd1=second_swapfs eth0=daemon,10:00:01:02:00:01,,~/tmp/switch.sock eth1=tuntap,,,192.168.1.43`
More information about the tuntap setting can be found in the previous section.

## Testing the .iso

Perhaps the true ideal of Gentoo Linux testing would be to boot the .iso with user-mode Linux and do the complete Gentoo install from within the user-mode Linux virtual system.

Booting the .iso, or actually the initrd from the .iso, is pretty straightforward.

`root #````
mount -o loop /path/to/install-<TAB>.iso /mnt/loop
```
`root #````
cp /mnt/loop/isolinux/gentoo.igz .
```
`root #``linux load_ramdisk=1 prompt_ramdisk=0 ramdisk_size=22000 initrd=rescue.gz root=/dev/ram0 ubd0=root_fs ubd1=swap_fs ubd2=/dev/cdroms/cdrom0 eth0=tuntap,,,192.168.0.254`
Now you can follow the Gentoo install doc essentially verbatim, although you'll need to know that the root file system will be /dev/ubd/0, the swap "partition" will be /dev/ubd/1, and the CDROM will be /dev/ubd/2.

## External resources

- [http://edeca.net/articles/bridging/index.html](https://edeca.net/articles/bridging/index.html) - Bridging with UML
- [http://user-mode-linux.sourceforge.net/](http://user-mode-linux.sourceforge.net/) - UML Homepage
- [http://www.theshore.net/\~caker/uml/](http://www.theshore.net/~caker/uml/) - Caker's UML Notes
- [http://sourceforge.net/mailarchive/forum.php?forum\_id=3647](http://sourceforge.net/mailarchive/forum.php?forum_id=3647) - UML Mailinglist archives
