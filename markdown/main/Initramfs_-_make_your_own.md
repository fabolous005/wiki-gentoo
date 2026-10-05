<!-- source: https://wiki.gentoo.org/wiki/Initramfs_-_make_your_own | group: Gentoo Wiki (Main) | wiki-title: Initramfs - make your own -->
---
title: Initramfs - make your own
url: https://wiki.gentoo.org/wiki/Initramfs_-_make_your_own
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-05-13"
fingerprint: "3b0b1fbaf75f3b70"
license: CC BY-SA 4.0
---

# Initramfs - make your own

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

## Introduction

This page describes how to build an initramfs which does not contain kernel modules. That results in a component that is not tied to a specific kernel version. The original author's initramfs, built in April 2009 still functions.

The following example covers root in [LVM](https://wiki.gentoo.org/wiki/LVM) on top of [mdadm](https://raid.wiki.kernel.org/index.php/A_guide_to_mdadm) raid. This is just an example, potential configuration options are limitless. [LUKS](https://wiki.gentoo.org/wiki/Dm-crypt) could be added, and anything added as an example in this guide can be omitted.

### Why make your own

The [initramfs](https://wiki.gentoo.org/wiki/Initramfs) can be [automatically created](https://wiki.gentoo.org/wiki/Initramfs/Guide#Creating_an_initramfs) with [sys-kernel/dracut](https://packages.gentoo.org/packages/sys-kernel/dracut) and [sys-kernel/genkernel](https://packages.gentoo.org/packages/sys-kernel/genkernel). While both tools are highly capable and can build functional initramfs', they may not meet all use cases. For some users, the initramfs is worth creating manually, even if only to understand how it works, and what is happening inside.

### Why use an initramfs

An initramfs is typically used to assist in the boot process. Some form of initramfs is required to mount systems using [Full Disk Encryption](https://wiki.gentoo.org/wiki/Full_Disk_Encryption).

### What makes an initramfs

The initramfs is a root filesystem in a file. It is generally used to contain whatever is required to boot the system, but can contain the entire root filesystem used once booted if that's desired.

#### Filesystem structure

An initramfs generally must have some basic filesystem structure, for required mounts and for the included binaries.

The following directory tree describes a basic initramfs:

#### Userspace tools

Typically, an initramfs provides a filesystem with some tools which are used to assist in booting, or provide a recovery environment.

Common components include:

- Binaries required to mount the root
  - *btrfs-progs*
  - cryptsetup
  - switch\_root
- Filesystem utilities
  - blkid
  - mount
  - ls
  - cat
- Shells
  - sh
  - bash

#### Kernel Modules

An initramfs may contain kernel modules, but when making a custom one, it often makes more sense to embed components required to boot into the kernel.

### How to use an Initramfs

Follow [Custom Initramfs Packaging](https://wiki.gentoo.org/wiki/Custom_Initramfs#Packaging) to embed the initramfs into the kernel or built it into a [CPIO](https://wiki.gentoo.org/wiki/Cpio) archive to include via [bootloader](https://wiki.gentoo.org/wiki/Bootloader) or cmdline argument.

## Construction

### Example List of Requirements

- Assemble and start mdadm RAID.
- Activate Logical Volumes
- Mount root and possibly other filesystems from inside their Logical Volumes
- A filesystem checker for non root filesystem mounted in the initramfs
- Interactive Shell for debug
- Init script to control everything.
- Other things to suit the install at hand

Now to discover all the binaries required to meet the explicit requirements. They must be included in the initramfs\_list.

### Assemble and start mdadm RAID

This requires Multiple Device support built into the kernel and the mdadm user space tool.

That's /sbin/mdadm and all the libraries that it depends on

`user $``lddtree /sbin/mdadm`
|/sbin/mdadm (interpreter => /lib64/ld-linux-x86-64.so.2)
|    libc.so.6 => /lib64/libc.so.6

### Activate Logical Volumes

This requires Multiple Device and Logical Volume Manager support built into the kernel and the lvm userspace tool.

`user $``lddtree /sbin/lvm````
/sbin/lvm (interpreter => /lib64/ld-linux-x86-64.so.2)
    libdevmapper-event.so.1.02 => /lib64/libdevmapper-event.so.1.02
        libdevmapper.so.1.02 => /lib64/libdevmapper.so.1.02
            libm.so.6 => /lib64/libm.so.6
    libreadline.so.8 => /lib64/libreadline.so.8
        libtinfow.so.6 => /lib64/libtinfow.so.6
    libblkid.so.1 => /lib64/libblkid.so.1
    libaio.so.1 => /lib64/libaio.so.1
    libc.so.6 => /lib64/libc.so.6
```
It has a bigger list of dependencies, also including /lib64/libc.so.6. Duplicates need only be provided once.

### Mount root and possibly other filesystems

Most users will want to use mount by filesystem UUID. Not all filesystem are on partitions, so PARTUUID cannot be used.

Mount by filesystem UUID requires the user space mount command

`root #``lddtree /bin/mount````
/bin/mount (interpreter => /lib64/ld-linux-x86-64.so.2)
    libmount.so.1 => /lib64/libmount.so.1
        libblkid.so.1 => /lib64/libblkid.so.1
    libc.so.6 => /lib64/libc.so.6
```
### Interactive Shell for Debug

That will be busybox. Everyone uses busybox. Its small and has lots of utilities too.

`user $``lddtree /bin/busybox`
/bin/busybox (interpreter => None)

My example busybox is statically linked. That's a lifesaver when almost nothing works.

### Init script to control everything

This is the hard bit. Its all the commands that need to be entered at a root shell, using the initramfs to get started.

If the initramfs only contained busybox, what would need to be entered to boot?

Some error handing is a good idea too, so that debug is possible.

#### Elements of the Init Script

Its a shell script so it must start with the shebang line. Its not a comment.

#!/bin/busybox sh

The error handler is a function which will be called when something goes wrong. It takes one parameter, which is a text string to be printed when it is invoked.

Comments are good for maintenance later.

```
rescue_shell() {
    echo "$@"
    echo "Something went wrong. Dropping you to a shell."
    # The symlinks are not required any longer
    # but it helps tab completion
    /bin/busybox --install -s
    exec /bin/sh
}
```
Parse the root filesystem out of the kernel command line and mount it.

```
# allow the use of UUIDs or filesystem lables
uuidlabel_root() {
    for cmd in $(/bin/cat /proc/cmdline) ; do
        case $cmd in
        root=*)
            type=$(echo $cmd | /bin/cut -d= -f2)
            echo "Mounting rootfs"
            if [ $type == "LABEL" ] || [ $type == "UUID" ] ; then
                uuid=$(echo $cmd | /bin/cut -d= -f3)
                /bin/mount -o ro $(/bin/findfs "$type"="$uuid") /mnt/root
            else
                /bin/mount -o ro $(echo $cmd | /bin/cut -d= -f2) /mnt/root
            fi
            ;;
	esac
    done
}
```
We only do that once, so it need not be a function but it makes the main flow of the script easier to read.

When things are mounted inside the initramfs, its good to be able check the filesystems first. The localmount service cannot check mounted filesystems.

```
# We need this for things that are mounted before localmount runs
# like /usr and possibly /var
check_filesystem() {
    # most of code coming from /etc/init.d/fsck
    local fsck_opts= check_extra= RC_UNAME=$(uname -s)
    # FIXME : get_bootparam forcefsck
    if [ -e /forcefsck ]; then
        fsck_opts="$fsck_opts -f"
        check_extra="(check forced)"
    fi
    echo "Checking local filesystem $check_extra : $1"
    if [ "$RC_UNAME" = Linux ]; then
        fsck_opts="$fsck_opts -C0 -T"
    fi
    trap : INT QUIT
    # using our own fsck, not the builtin one from busybox
    /sbin/fsck -p $fsck_opts $1
    ret_val=$?
    case $ret_val in
        0)	return 0;;
        1)	echo "Filesystem repaired"; return 0;;
        2|3)    if [ "$RC_UNAME" = Linux ]; then
                        echo "Filesystem repaired, but reboot needed"
                        /sbin/reboot -f
                else                       rescue_shell "Filesystem still have errors; manual fsck required"
                fi;;
        4)	if [ "$RC_UNAME" = Linux ]; then
                        rescue_shell "Fileystem errors left uncorrected, aborting"
                else
                    	echo "Filesystem repaired, but reboot needed"
                        /sbin/reboot
                fi;;
        8)	echo "Operational error"; return 0;;
        16)     echo "Use or Syntax Error"; return 16;;
        32)     echo "fsck interrupted";;
        127)    echo "Shared Library Error"; sleep 20; return 0;;
        *)	echo $ret_val; echo "Some random fsck error - continuing anyway"; sleep 20; return 0;;
    esac
# rescue_shell can't find tty so its broken
    rescue_shell
}
```
With those functions in support, we can do what's needed.

PATH="/sbin:/bin"
# start for real here
# temporarily mount proc,sys and dev
mount -t proc proc /proc
mount -t sysfs sysfs /sys
mount -t devtmpfs devtmpfs /dev
#mdam arrays to assemble
#boot  UUID : a25b05eb:3db18cbe:afb9312b:d1d97546
#host  UUID : de8f2cbc:17ca3275:0b69db3c:b9f91a6b
#kvm   UUID : a3aab047:413ed52d:b15158fc:cdb637ef
# boot
/sbin/mdadm --assemble /dev/md0 --uuid=a25b05eb-3db18cbe-afb9312b-d1d97546 || echo "boot failed to assemble"
/sbin/mdadm --assemble /dev/md1 --uuid=de8f2cbc-17ca3275-0b69db3c-b9f91a6b || rescue\_shell "The host RAID set failed to assemble"
/sbin/mdadm --assemble /dev/md2 --uuid=a3aab047:413ed52d:b15158fc:cdb637ef || echo "THE KVM space did not assemble"

Use `/sbin/mdadm --assemble --run` to start the raid set with missing members, if possible.

Its left as an exercise for the reader to parse the RAID UUID(s) out of the kernel command line.

If boot failed to assemble, it does not impact the boot process as both BIOS and UEFI are not raid aware. Indeed, the boot loader has to read boot to load initramfs to work out that /boot did not assemble. There is no need to call rescue\_shell here.

Boot on RAID requires a RAID level and on disk RAID data layout leaves the filesystem untouched. RAID 1 and a raid metadata that lives at the end of the volume works.

Being lazy, start all the logical volumes and call the rescue shell if any one fails. In practice, only the one housing root is required. That's a local design decision.

\# Then start LVM
vgchange -ay || rescue\_shell "Some/All Volume Groups failed to start"

With the logical volumes started, we can see root, so mount it

\# mounting rootfs on /mnt/root - from the UUID on the kernel command line
uuidlabel\_root || rescue\_shell "Error with uuidlabel\_root"

Now mount other filesystems if needed. Typically /usr and /var

```
# space separated list of mountpoints that ...
mountpoints="/usr"
# /var"
# ... we want to find in /etc/fstab ...
/bin/ln -s /mnt/root/etc/fstab /etc/fstab
# loop through the list of mountpoints
for m in $mountpoints ; do
#echo $m
    check_filesystem $m
    echo "Mounting $m"
    # mount the device and ...
    mount $m || rescue_shell "Error while mounting $m"
    # ... move the tree to its final location
    mount --move $m "/mnt/root"$m || rescue_shell "Error while moving $m"
done
```
\# That's put all the pieces together, now tidy up
echo "All done. Switching to real root."
# clean up. The init process will remount proc sys and dev later
umount /proc
umount /sys
umount /dev
# switch to the real root and execute init
exec /sbin/switch\_root /mnt/root /sbin/init

That final exec call never returns so nervous readers could add `rescue_shell "Fell off the end of init"` as the very last line.

Actually, with /proc and /dev unmounted, the console output has gone so you won't see it. To got any further you need the static /dev nodes created for the initrd in the real /dev before DEVTMPFS is mounted.

### Building the Binaries

To avoid using binaries from the live filesystem, the initramfs binaries will be installed in /root/initramfs/bins. This allows trivial changes to the init script in years to come without, tracking down all the changed dependencies on the live filesystem. Both ways work. Its a design decision.

The down side of a separate build is that all the dependencies that will not go into the initramfs will be built too.

`root #``emerge -av --root=/root/initramfs/bins <list_of_packages>`
Set the USE flags to your liking and build your binaries. This authors preference is to build everything that supports static linking with USE=static.

\<list\_of\_packages> depends on what is required of the initramfs. Like the rest of Gentoo its easy to add to if needed.

It looks a bit dated now but the bins package list on the example install is

`user $``ls bins/var/db/pkg/*/*/*.ebuild`
app-arch/bzip2-1.0.8-r1/bzip2-1.0.8-r1.ebuild                  sys-block/thin-provisioning-tools-0.9.0-r1/thin-provisioning-tools-0.9.0-r1.ebuild
app-arch/gzip-1.11/gzip-1.11.ebuild                            sys-fs/e2fsprogs-1.46.4/e2fsprogs-1.46.4.ebuild
dev-libs/expat-2.4.3/expat-2.4.3.ebuild                        sys-fs/lvm2-2.02.188-r2/lvm2-2.02.188-r2.ebuild
dev-libs/libaio-0.3.112/libaio-0.3.112.ebuild                  sys-fs/mdadm-4.2-r1/mdadm-4.2-r1.ebuild
dev-libs/libpcre-8.45/libpcre-8.45.ebuild                      sys-libs/e2fsprogs-libs-1.46.4-r1/e2fsprogs-libs-1.46.4-r1.ebuild
dev-libs/libpcre2-10.39/libpcre2-10.39.ebuild                  sys-libs/glibc-2.33-r7/glibc-2.33-r7.ebuild
dev-libs/libunistring-0.9.10-r1/libunistring-0.9.10-r1.ebuild  sys-libs/libcap-2.62/libcap-2.62.ebuild
net-dns/libidn2-2.3.2/libidn2-2.3.2.ebuild                     sys-libs/libxcrypt-4.4.25-r1/libxcrypt-4.4.25-r1.ebuild
sys-apps/acl-2.3.1/acl-2.3.1.ebuild                            sys-libs/ncurses-6.2\_p20210619/ncurses-6.2\_p20210619.ebuild
sys-apps/attr-2.5.1/attr-2.5.1.ebuild                          sys-libs/pam-1.5.1\_p20210622-r1/pam-1.5.1\_p20210622-r1.ebuild
sys-apps/baselayout-2.7-r3/baselayout-2.7-r3.ebuild            sys-libs/readline-8.1\_p1-r1/readline-8.1\_p1-r1.ebuild
sys-apps/busybox-1.34.1/busybox-1.34.1.ebuild                  sys-libs/timezone-data-2021a-r1/timezone-data-2021a-r1.ebuild
sys-apps/gentoo-functions-0.14/gentoo-functions-0.14.ebuild    sys-libs/zlib-1.2.11-r4/zlib-1.2.11-r4.ebuild
sys-apps/grep-3.7/grep-3.7.ebuild                              virtual/awk-1/awk-1.ebuild
sys-apps/systemd-tmpfiles-249.9/systemd-tmpfiles-249.9.ebuild  virtual/libcrypt-2/libcrypt-2.ebuild
sys-apps/util-linux-2.37.2-r1/util-linux-2.37.2-r1.ebuild      virtual/libiconv-0-r2/libiconv-0-r2.ebuild
sys-auth/pambase-20210201.1/pambase-20210201.1.ebuild          virtual/libintl-0-r2/libintl-0-r2.ebuild
sys-auth/passwdqc-2.0.2-r1/passwdqc-2.0.2-r1.ebuild            virtual/tmpfiles-0-r1/tmpfiles-0-r1.ebuild

### Putting the Pieces Together

Thats what the initramfs\_list file is for.

Describe the directory structure for the initramfs this example is from an arm64 system. amd64/x86 may differ.

\# directory structure
dir /proc	755 0 0
dir /usr        755 0 0
dir /bin        755 0 0
dir /sys        755 0 0
dir /var        755 0 0
#dir /lib        755 0 0
dir /lib64	755 0 0
dir /sbin	755 0 0
dir /mnt        755 0 0
dir /mnt/root   755 0 0
dir /etc        755 0 0
dir /root	700 0 0
dir /dev        755 0 0

Make a few critical device nodes

nod /dev/null   666 0 0 c 1 3
nod /dev/tty    666 0 0 c 5 0
nod /dev/console        600 0 0 c 5 1

They are probably not required with modern DEVTMPFS in the kernel. The last line (nod /dev/console) is always required when building an embedded initramfs (even if DEVTMPFS is used).

All the main commands

\# busybox
# Output file name              Input file name
file /bin/busybox               /root/initramfs/bins/bin/busybox        755 0 0
# Need real mount as busybox did not support UUID
file /bin/mount                 /root/initramfs/bins/bin/mount          755 0 0
# for raid on lvm
# Output file name              Input file name
file /sbin/mdadm                /root/initramfs/bins/sbin/mdadm         755 0 0
file /sbin/lvm.static           /root/initramfs/bins/sbin/lvm.static    755 0 0

Add some symbolic links to make life easier.

slink /sbin/vgchange                    /sbin/lvm.static                777 0 0
slink /sbin/vgscan                      /sbin/lvm.static                777 0 0
slink /bin/cat                          /bin/busybox                    777 0 0
slink /bin/cut                          /bin/busybox                    777 0 0
slink /bin/findfs                       /bin/busybox                    777 0 0
slink /bin/ln                           /bin/busybox                    777 0 0
slink /sbin/switch\_root                 /bin/busybox                    777 0 0
slink /lib64/libdl.so.2                 /lib64/libdl-2.33.so            777 0 0
# libraries required by /sbin/fsck.ext4 and /sbin/fsck
# The /lib -> /lib64 symlink is mostly harmless but its not right on arm64
slink   /lib                            /lib64                          777 0 0

The symlinks to /bin/busybox are probably not required as busybox assumes internal commands when a command is not found.

\# libraries required by /sbin/fsck.ext4 and /sbin/fsck
# The /lib -> /lib64 symlink is mostly harmless but its not right on arm64
slink   /lib                            /lib64                          777 0 0

All the required libraries too

\# Output file name                      Input file name
file    /lib/ld-linux-aarch64.so.1	/root/initramfs/bins/lib/ld-linux-aarch64.so.1      755 0 0
file    /lib64/libext2fs.so.2           /root/initramfs/bins/lib64/libext2fs.so.2           755 0 0
file    /lib64/libcom\_err.so.2          /root/initramfs/bins/lib64/libcom\_err.so.2          755 0 0
file    /lib64/libpthread.so.0          /root/initramfs/bins/lib64/libpthread.so.0          755 0 0
file    /lib64/libblkid.so.1            /root/initramfs/bins/lib64/libblkid.so.1            755 0 0
file    /lib64/libuuid.so.1             /root/initramfs/bins/lib64/libuuid.so.1             755 0 0
file    /lib64/libe2p.so.2              /root/initramfs/bins/lib64/libe2p.so.2              755 0 0
file    /lib64/libc.so.6                /root/initramfs/bins/lib64/libc.so.6                755 0 0
file    /lib64/libmount.so.1            /root/initramfs/bins/lib64/libmount.so.1            755 0 0
file    /lib64/libdl-2.33.so            /root/initramfs/bins/lib64/libdl-2.33.so            755 0 0
file    /sbin/fsck              /root/initramfs/bins/sbin/fsck                      755 0 0
file    /sbin/fsck.ext4         /root/initramfs/bins/sbin/fsck.ext4                 755 0 0
# our init script
file    /init                   /root/initramfs/init               755 0 0

Mount /boot if its not mounted.

`root #``/usr/src/linux/usr/gen_init_cpio /root/initramfs/initramfs_list > /boot/<initramfs_name>`
Tell your boot loader about /boot/\<initramfs\_name> and reboot to test.

## Ideas For Further Contributions

- Cover LUKS
- Cover root over NFS (Possible without an initlamfs too)
- Bring up the network in the initramfs with ssh access
- Other things

## See also

- [Initramfs/Guide](https://wiki.gentoo.org/wiki/Initramfs/Guide) — covers the concepts of the initramfs as well as how to properly create and manage initramfs instances.
