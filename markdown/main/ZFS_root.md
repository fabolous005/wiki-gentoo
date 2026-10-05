<!-- source: https://wiki.gentoo.org/wiki/ZFS/root | group: Gentoo Wiki (Main) | wiki-title: ZFS/root -->
---
title: ZFS/root
url: https://wiki.gentoo.org/wiki/ZFS/root
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2023-11-30"
fingerprint: "1423e41aaa2463dc"
license: CC BY-SA 4.0
---

# ZFS/root

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

This article is **archived (obsolete)**. Contents are surely incorrect for current usage, and are intended for historical reference only.

TLDR:

**Do not use this article!**



## Root on ZFS

This is set of tips why and how to have ZFS on root, making your Gentoo/Linux life much easier.

**This is not step-by-step guide. Also it is considered for experience users, and users with some ZFS experience.**



### Rationale

Because you can do...

`root #``zfs snap pool/root@my-snapshot-or-restore-point-description`
... and it takes no time at all to execute. Once you've taken a snapshot, you're totally safe to break things, which is the fastest way to learn!

### Kernel

### Initramfs & BE & Grub

Many of the scripts mentioned in this text come from my github repository.
See [https://github.com/robertek/root-scripts](https://github.com/robertek/root-scripts).
**Please be sure to check these scripts before you start using them**, there are
many hardcoded paths and you would probably need to change them (mainly the
ones that are not beadm). Feel free to update these script to more generic way
and share with me.

I have cloned this in /root/bin.

#### Initramfs

Since zfs module is already compiled in the kernel, initramfs is here to do only one job and it is import the root pool and switch to correct dataset.

The initramfs than needs to be only very minimal. So I have developed easy script for genrating such initramfs.

\# make\_initramfs 
composing initramfs in /tmp/initramfs.AmH
backing up /boot/initramfs-0.7.1
creating new /boot/initramfs-0.7.1


What does it do. It will collect all the zfs userspace tools and its libs,
busybox and some other extras. Adds this to some temporary directory.
Create init file from template and than pack this to cpio archive.

The most important is the init script. Check the
[https://raw.githubusercontent.com/robertek/root-scripts/master/init.tpl](https://raw.githubusercontent.com/robertek/root-scripts/master/init.tpl) what it
is doing.

The main work-flow is this: import pool -> mount root -> switch\_root.

You may have defined root= or rpool= variables on kernel command line. If none is defined than "rpool" will be imported and what is in its bootfs variable will be root.

#### beadm

Under my scripts you may find slightly updated "beadm" command.

This command is the way how to manage boot environments (BE). It has similar functionality as the original beadm command in Solaris.

```
# beadm
usage:
  beadm activate <beName>
  beadm create [-e nonActiveBe | -e beName@snapshot] <beName>
  beadm create <beName@snapshot>
  beadm destroy [-F] <beName | beName@snapshot>
  beadm list [-a] [-s] [-D] [-H]
  beadm rename <origBeName> <newBeName>
  beadm mount <beName> [mountpoint]
  beadm { umount | unmount } [-f] <beName>
```
To have this tool working correctly you need to have correct layout of zfs root datasets. It needs to be in format "pool/somedataset/root" and the pool needs to have the bootfs set.

Example:

\# zpool get bootfs rpool
NAME   PROPERTY  VALUE                       SOURCE
rpool  bootfs    rpool/GENTOO/root-20171004  local
# zpool set bootfs=rpool/GENTOO/root-20171004 rpool

If this is set correctly you will have something like this in the beadm output:

\# beadm list
BE            Active Mountpoint  Space Created
root          -      /            0.0K 
root-20170418 -      -          855.0M 2017-04-18 12:01
root-20170509 -      -          351.7M 2017-05-09 15:56
root-20170530 -      -          808.3M 2017-05-30 12:33
root-20170925 -      -          709.0M 2017-09-26 17:39
root-20171004 NR     /           14.2G 2017-10-04 14:55

Using beadm activate you can activate new BE after reboot. Other options are self explanatory.

#### Grub

I have found out updating grub the old way is easier and more convenient when I want some non standard expectation.

I have generated static grub2 image with all the needed modules loaded, which than can be put anywhere what efi boots.

\# grub-mkimage -O x86\_64-efi -o bootx64.efi -p "" part\_gpt fat normal boot configfile linux gzio xzio gfxterm
# mount /boot
# mkdir -p /boot/EFI/boot
# mv bootx64.efi /boot/EFI/boot

And sample of my config. Nothing special, rest of the config file are only defined another kernels or BE. Note the second menu entry, this define boot to specific BE.

```
# head -n 25 /boot/EFI/boot/grub.cfg
################################################################################
### variables
loadfont /EFI/boot/unicode.pf2
set gfxmode="auto"
terminal_output gfxterm
set default="0"
set timeout="5"
set root=(hd0,gpt1)
set splash_par="splash=verbose,theme:natural_gentoo console=tty1"
set crash_par="crashkernel=64M"
################################################################################
################################################################################
### menu
menuentry 'Gentoo Linux systemd 4.9.52 zfs-bootfs' --class gnu-linux --class os {
        linux /vmlinuz-4.9.52+ $splash_par $crash_par quiet
        initrd /initramfs-systemd-0.7.1
}
menuentry 'Gentoo Linux systemd 4.9.52 20171101' --class gnu-linux --class os {
        linux /vmlinuz-4.9.52+ oot=rpool/GENTOO/root-20171101 $splash_par $crash_par quiet
        initrd /initramfs-systemd-0.7.1
}
 
```


### System update

In the old days system update (and specific Gentoo linux) took a long time, it improved a bit on latest hw, but not much. It is time you keep your computer do its work and on many systems (Windows, Linuxes) you are not allowed to work at all. On Gentoo if you run update in the background you may expect some inconveniences during updated, or if it fails you may have screwed system.

With BE you can let the history behind. The simple idea (something that Solaris do for years) is to clone current root and update on the clone. After the clone is updated successfully you reboot to the clone. If anything goes wrong you still has working system in the previous BE (until you delete it). This is something that is not ZFS specific (you may implement this on top of LVM or btrfs), but I find this the easiest and most convenient way.

#### Prerequisites

It is good to keep the portage tree somehow local to the BE. If you return to the older BE you will have consistent portage tree with the contents of BE.

I have found the easiest way to keep portage tree under squashfs. Nowadays it is much easier, since portage tree is already delivered also as squashfs images.

I have sqfs image in /var/lib/portage/gentoo/gentoo.xz.sqfs mounted as /var/lib/portage/gentoo/gentoo.

\# grep gentoo /etc/fstab
/var/lib/portage/gentoo.xz.sqfs /var/lib/portage/gentoo squashfs defaults 0 0
# cat /etc/portage/repos.conf/gentoo 
\[DEFAULT\]
main-repo = gentoo
\[gentoo\]
location = /var/lib/portage/gentoo

But you may left the portage tree unpacked normally under /usr/portage and have it on the root dataset.

On the other side it is good to have something shared between BE. Perfect example is distfiles. I have special dataset for distfiles and point the portage to this.

\# zfs list | grep distfiles
rpool/GENTOO\_data/distfiles        2.43G  44.6G  2.43G  /var/lib/portage/distfiles
# grep DISTDIR /etc/portage/make.conf 
DISTDIR="/var/lib/portage/distfiles"

#### Chroot update

Simple use-case how to update using boot environment. In the chroot do what you are used to do when updating (other than syncing portage tree).

\# beadm create root-updated
# beadm mount root-updated /mnt
# wget http://distfiles.gentoo.org/snapshots/squashfs/gentoo-current.xz.sqfs
# mv gentoo-current.xz.sqfs /mnt/var/lib/portage/gentoo/gentoo.xz.sqfs
# chroot\_start /mnt
# chroot /mnt /bin/bash --login
# PS1="(chroot) $PS1"
(chroot) # emerge -avuDN @world
(chroot) # .....
(chroot) # exit
# chroot\_stop /mnt
# beadm umount root-updated
# beadm activate root-updated
# reboot

As was noted in GRUB section you may add new GRUB entry to reflect the old BE. Otherwise you will always boot to the active BE.

Something like:

```
# cat <<EOF >>/boot/EFI/boot/grub.cfg
menuentry 'Gentoo Linux 4.9.30 root-updated' --class gnu-linux --class os {
        linux /vmlinuz-4.9.30+ root=rpool/GENTOO/root-updated
        initrd /initramfs
}
EOF
```
