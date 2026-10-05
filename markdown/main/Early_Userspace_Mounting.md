<!-- source: https://wiki.gentoo.org/wiki/Early_Userspace_Mounting | group: Gentoo Wiki (Main) | wiki-title: Early Userspace Mounting -->
---
title: Early Userspace Mounting
url: https://wiki.gentoo.org/wiki/Early_Userspace_Mounting
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2025-01-08"
fingerprint: "1b03579ab7e67f2c"
license: CC BY-SA 4.0
---

# Early Userspace Mounting

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

This article will detail how to build a custom minimal [initramfs](https://wiki.gentoo.org/wiki/Initramfs) that checks the /usr filesystem and pre-mounts /usr. This has become necessary for affected configurations because of various changes in [udev](https://wiki.gentoo.org/wiki/Udev) (see [bug #364235](https://bugs.gentoo.org/show_bug.cgi?id=364235)).

In this article we'll be working with the following:

- [Busybox](https://wiki.gentoo.org/wiki/Busybox)
- An initramfs content list
- The gen\_init\_cpio and gen\_initramfs.sh utilities, provided by the kernel itself.

The initramfs also contains the required libraries and binaries to run an [ext4](https://wiki.gentoo.org/wiki/Ext4) fsck. Most of the code to run the fsck is coming from the /etc/init.d/fsck script.

When using any other [filesystem](https://wiki.gentoo.org/wiki/Filesystem) than ext4, add the required binaries / libraries into the initramfs list.

Basically, the init script is doing following actions:

1. Mounts the root partition on /mnt/root as read-only.
2. Symlinks the [/etc/fstab](https://wiki.gentoo.org/wiki//etc/fstab) from the root partition to the initramfs environment.
3. Checks the filesystem of our /usr device using the embedded /sbin/fsck binary.
4. Mounts /usr, then moves it to /mnt/root/usr using the `--move` mount parameter.
5. Switches to real root and executes init.

The article also assumes we are working in /usr/src/initramfs, so for the sake of ease, begin with creating this directory.

## Requirements

The most important package here is [sys-apps/busybox](https://packages.gentoo.org/packages/sys-apps/busybox) as it provides utilities suitable for an initramfs. It is also critical that to emerge it with `static` USE flag enabled:

`root #``USE="static" emerge --ask sys-apps/busybox`
Make sure that the running kernel is built with the *devtmpfs* option enabled. It is required by the init script below and [udev](https://wiki.gentoo.org/wiki/Udev):

```
 Device Drivers  --->
   Generic Driver Options  --->
       [*] Maintain a devtmpfs filesystem to mount at /dev 
```
[Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for <code>CONFIG_DEVTMPFS</code> to find this item.
Next up is the initramfs\_list file which will tell gen\_initramfs.sh how to construct the initramfs:

**`/usr/src/initramfs/initramfs_list`**

Copy and save the contents of the above to /usr/src/initramfs/initramfs\_list after adjusting for the current architecutre.

Last up is the actual init file which will execute the initramfs:

**`/usr/src/initramfs/init`**

```
#!/bin/busybox sh
rescue_shell() {
    echo "$@"
    echo "Something went wrong. Dropping you to a shell."
    busybox --install -s
    exec /bin/sh
}
uuidlabel_root() {
    for cmd in $(cat /proc/cmdline) ; do
        case $cmd in
        root=*)
            type=$(echo $cmd | cut -d= -f2)
            echo "Mounting rootfs"
            if [ $type == "LABEL" ] || [ $type == "UUID" ] ; then
                uuid=$(echo $cmd | cut -d= -f3)
                mount -o ro $(findfs "$type"="$uuid") /mnt/root
            else
                mount -o ro $(echo $cmd | cut -d= -f2) /mnt/root
            fi
            ;;
        esac
    done
}
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
    trap : INT QUIT
    # using our own fsck, not the builtin one from busybox
    /sbin/fsck -p $fsck_opts $1
    case $? in
        0)      return 0;;
        1)      echo "Filesystem repaired"; return 0;;
        2|3)    if [ "$RC_UNAME" = Linux ]; then
                        echo "Filesystem repaired, but reboot needed"
                        reboot -f
                else
                        rescue_shell "Filesystem still have errors; manual fsck required"
                fi;;
        4)      if [ "$RC_UNAME" = Linux ]; then
                        rescue_shell "Fileystem errors left uncorrected, aborting"
                else
                        echo "Filesystem repaired, but reboot needed"
                        reboot
                fi;;
        8)      echo "Operational error"; return 0;;
        12)     echo "fsck interrupted";;
        *)      echo "Filesystem couldn't be fixed";;
    esac
    rescue_shell
}
# temporarily mount proc and sys
mount -t proc none /proc
mount -t sysfs none /sys
mount -t devtmpfs none /dev
# disable kernel messages from popping onto the screen
echo 0 > /proc/sys/kernel/printk
# clear the screen
clear
# mounting rootfs on /mnt/root
uuidlabel_root || rescue_shell "Error with uuidlabel_root"
# space separated list of mountpoints that ...
mountpoints="/usr" #note: you can add more than just usr, but make sure they are declared in /usr/src/initramfs/initramfs_list
# ... we want to find in /etc/fstab ...
ln -s /mnt/root/etc/fstab /etc/fstab
# ... to check filesystems and mount our devices.
for m in $mountpoints ; do
    check_filesystem $m
    echo "Mounting $m"
    # mount the device and ...
    mount $m || rescue_shell "Error while mounting $m"
    # ... move the tree to its final location
    mount --move $m "/mnt/root"$m || rescue_shell "Error while moving $m"
done
echo "All done. Switching to real root."
# clean up. The init process will remount proc sys and dev later
umount /proc
umount /sys
umount /dev
# switch to the real root and execute init
exec switch_root /mnt/root /sbin/init
```
Copy and save the contents of the above to /usr/src/initramfs/init.

## System preparation

In fstab, we must set the sixth field for the `/usr` entry to `0`, this will prevent the [OpenRC](https://wiki.gentoo.org/wiki/OpenRC) fsck init script to try to check the filesystem for the already mounted /usr:

**`/etc/fstab`**

## Generating the Initramfs

### Building as an embedded Initramfs

It is not necessary to compile gen\_init\_cpio or make it executable because these steps will be handled when building the kernel with make. Both files, initramfs\_list and init must be copied into /usr/src/initramfs/. For an embedded initramfs one line is missing and must be **added** to initramfs\_list

**`/usr/src/initramfs/initramfs_list`**

This kernel configuration will do all steps to include all needed files in an embedded initramfs.

For embedding the initramfs directly into the kernel image, the initramfs\_list must be coded in **Initramfs source file(s)** (`CONFIG_INITRAMFS_SOURCE`) in the kernel (directly under the **Initial RAM filesystem and RAM disk (initramfs/initrd) support** (`CONFIG_BLK_DEV_INITRD`) option):

**CONFIG\_INITRAMFS\_SOURCE="/usr/src/initramfs/initramfs\_list"**

General setup  --->
   \[\*\] Initial RAM filesystem and RAM disk (initramfs/initrd) support [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_BLK\_DEV\_INITRD\</code> to find this item.
   (/usr/src/initramfs/initramfs\_list) Initramfs source file(s) [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_INITRAMFS\_SOURCE\</code> to find this item.
   \[\*\]   Support initial ramdisk/ramfs compressed using gzip [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_RD\_GZIP\</code> to find this item.
   Built-in initramfs compression mode (Gzip)  --->

### Building as an external CPIO archive

The [kernel](https://wiki.gentoo.org/wiki/Kernel) sources provide the gen\_init\_cpio and gen\_initramfs.sh utilities. The gen\_init\_cpio utility does not come prepackaged and needs to be built:

`root #``make -C /usr/src/linux/usr/ gen_init_cpio`
Make sure that these two are executable:

`root #````
cd /usr/src/linux
```
`root #````
chmod +x usr/gen_init_cpio usr/gen_initramfs.sh
```
Run the gen\_initramfs.sh script with the `-o` argument pointing to where we want the initramfs image to be placed followed by the path to our initramfs\_list file:

`root #````
cd /usr/src/linux
```
`root #````
usr/gen_initramfs.sh -o /boot/initrd.cpio /usr/src/initramfs/initramfs_list
```
After that compress the file /boot/initrd.cpio via gzip:

`root #``gzip --best /boot/initrd.cpio`
This will create the archive /boot/initrd.cpio.gz.

## Bootloader configuration

To use the external initramfs, the [bootloader](https://wiki.gentoo.org/wiki/Bootloader) needs to be configured as shown below for GRUB and LILO as examples. For an embedded Initramfs this is not necessary !

### Configuring GRUB

Add the `initrd` line to /boot/grub/grub.conf:

**`grub.conf`**

### Configuring LILO

Add the `initrd` and `append` line to /etc/lilo.conf:

**`lilo.conf`**

## Using a Stub Kernel

If no bootmanager is used (UEFI boots a stub kernel directly) the UUID of the root partition must be configured into the built-in kernel command line or as parameter in the UEFI boot entry (see next paragraph):

Processor type and features  --->
   \[\*\] Built-in kernel command line [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_CMDLINE\_BOOL\</code> to find this item.
   (root=UUID=xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx) Built-in kernel command string [Search](https://wiki.gentoo.org/wiki/Kernel/Configuration#Search_modules) for \<code>CONFIG\_CMDLINE\</code> to find this item.

When using an external Initramfs initrd.cpio.gz must be copied to the [EFI System Partition](https://wiki.gentoo.org/wiki/EFI_System_Partition) and initrd= must use the correct path. Set the parameter initrd= only in an UEFI boot entry. It does not work when setting it in the built-in kernel command line. In this case it is recommended to set both parameter in this UEFI boot entry. Example:

`root #``efibootmgr -c -d /dev/sda -p 1 -L "Gentoo" -l '\EFI\gentoo\bzImage.efi' -u 'root=UUID=xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx initrd=\EFI\gentoo\initrd.cpio.gz'`
See more here: [Efibootmgr#Creating\_a\_boot\_entry](https://wiki.gentoo.org/wiki/Efibootmgr#Creating_a_boot_entry)

## Result

When booting, the output looks like this:

## See also

- [Custom Initramfs](https://wiki.gentoo.org/wiki/Custom_Initramfs) — the successor of *initrd*. It provides early userspace which can do things the kernel can't easily do by itself during the boot process.
- [cpio](https://wiki.gentoo.org/wiki/Cpio) — a file [archiving](https://wiki.gentoo.org/wiki/Data_compression) utility
