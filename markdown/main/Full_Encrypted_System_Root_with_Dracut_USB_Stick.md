<!-- source: https://wiki.gentoo.org/wiki/Full_Encrypted_System_Root_with_Dracut_USB_Stick | group: Gentoo Wiki (Main) | wiki-title: Full Encrypted System Root with Dracut USB Stick -->
---
title: Full Encrypted System Root with Dracut USB Stick
url: https://wiki.gentoo.org/wiki/Full_Encrypted_System_Root_with_Dracut_USB_Stick
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-07-25"
fingerprint: bd11301a95e65b79
license: CC BY-SA 4.0
---

# Full Encrypted System Root with Dracut USB Stick

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

This article is an example of using [dm-crypt](https://wiki.gentoo.org/wiki/Dm-crypt) for full disk encryption with [LVM](https://wiki.gentoo.org/wiki/LVM). It uses GPT (partitioning table), and may not work in older computers with only BIOS boot.

The starting point is an empty mass storage device (such as an SSD). The point is to encrypt everything with strong cryptography. A USB stick stores a big keyfile encrypted with a short password.

## Disk Layout

Given a /dev/sdX storage device, the partitioning layout will look as follows:

```
/dev/sdX
|--> GRUB BIOS                       2   MB       no fs       GRUB loader itself
|--> /boot                 boot      512 MB       fat32       GRUB and kernel
|--> LUKS encrypted                  100%         encrypted   encrypted block device
     |-->  LVM             lvm       100%                  
           |--> /          root      60  GB       btrfs        root filesystem
           |--> /var       var       20  GB       btrfs        var files
           |--> none       swap      7  GB        swap         swap
           |--> /home      home      100%         btrfs        user files
```
The filesystems for the volumes inside of the LVM can be different from [Btrfs](https://wiki.gentoo.org/wiki/Btrfs), and the sizes here are just examples.

## Steps

### Partitioning

Create, in order, a new GPT label, a 2M EFI System partition, a 512M Boot partition, and a partition for the LVM on top of LUKS, with as much space as desired.

In this guide, [util-linux#cfdisk](https://wiki.gentoo.org/wiki/Util-linux#cfdisk) is recommended to partition the disk, since the interface is quite intuitive.

The workflow for the [fdisk](https://wiki.gentoo.org/wiki/Fdisk) tool is as following:

`root #``fdisk /dev/sdX`
Now, to create the label and partitions:

`Command (m for help):``g`
Created a new GPT disklabel (GUID: XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX).

`Command (m for help)``n````
Partition number (1-128, default 1): 1
First sector (2048-4194270, default 2048): 2048
Last sector, +/-sectors or +/-size{K,M,G,T,P} (2048-4194270, default 4192255): +2M
Created a new partition 1 of type 'Linux filesystem' and of size 2 MiB.
```
`Command (m for help)``n````
Partition number (2-128, default 2): 2
First sector (6144-4194270, default 6144):
Last sector, +/-sectors or +/-size{K,M,G,T,P} (6144-4194270, default 4192255): +512M
Created a new partition 2 of type 'Linux filesystem' and of size 512 MiB.
```
`Command (m for help)``n````
Partition number (3-128, default 3): 3
First sector (1054720-4194270, default 1054720):
Last sector, +/-sectors or +/-size{K,M,G,T,P} (1054720-4194270, default 4192255): (empty for all available space)
Created a new partition 3 of type 'Linux filesystem' and of size 1,5 GiB.
```
Finally, change the type of the first partition to an EFI System:

`Command (m for help)``t`
Partition number (1-3, default 3): 1
Partition type or alias (type L to list all): 1
Changed type of partition 'Linux filesystem' to 'EFI System'.

### Formatting

First, the second partition (the boot partition) needs to have a FAT filesystem:

`root #``mkfs.vfat -F32 /dev/sdX2`
Then, if needed, load the dm-crypt kernel module with

`root #``modprobe dm-crypt`
Note: an error may happen while GPG asks for a password because of a confusion on the TTYs. To prevent that, run:

`root #``export GPG_TTY=$(tty)`
Considering the USB Stick is at /dev/sdW, and the partition designated for the keyfile is /dev/sdW1, mount it:

`root #``mount /dev/sdW1 /mnt/key`
Create the encrypted keyfile:

`root #``dd if=/dev/random count=64 | gpg --symmetric --output /mnt/key/key.gpg`
Encrypt and open the third (main) partition:

`root #``gpg --quiet --decrypt /mnt/key/key.gpg | cryptsetup --batch-mode --key-file - luksFormat /dev/sdX3 lvm``root #``gpg --quiet --decrypt /mnt/key/rootkey.gpg | cryptsetup --allow-discards --key-file - luksOpen /dev/sdX3 lvm`
Create the Volume Group and the Logical Volumes (sizes are just examples):

`root #``vgcreate vg0 /dev/mapper/lvm  # Create volume group vg0``root #``lvcreate -L 60G -n root vg0  # Create logical volume for /root filesystem, 60G in this example``root #``lvcreate -L 20G -n var vg0  # Create logical volume for /var filesystem``root #``lvcreate -L 7G -n swap vg0  # Create logical volume for swap filesystem``root #``lvcreate -l 100%FREE -n home vg0  # Create logical volume for /home filesystem`
Finally, format the LVs:

`root #``mkswap -L "swap" /dev/mapper/vg0-swap``root #``mkfs.btrfs -L "root" /dev/mapper/vg0-root``root #``mkfs.btrfs -L "var" /dev/mapper/vg0-var``root #``mkfs.btrfs -L "home" /dev/mapper/vg0-home`
### Mounting partitions

First, create the root directory:

`root #``mkdir /mnt/gentoo`
Then, mount the root filsystem. Depending on the filesystem chosen, compression may be available. If Btrfs was used, the option `-o compress-force=zstd` may be used for that purpose.

`root #``mount -o discard=async /dev/vg0/root /mnt/gentoo`
Finally, create the var directory and mount it:

`root #``mkdir /mnt/gentoo/var``root #``mount -o discard=async /dev/vg0/var /mnt/gentoo/var`
### Stage3 Installation

After downloading the stage3 file into /mnt/gentoo, enter the root directory and extract it.

`root #``cd /mnt/gentoo``root #``tar xpvf stage3-*.tar.xz --xattrs-include='*.*' --numeric-owner`
### Configuring

Finally, configure the system normally. The guide [Handbook:AMD64/Installation/Base](https://wiki.gentoo.org/wiki/Handbook:AMD64/Installation/Base) should be good enough for this.

Select a mirror with `mirrorselect` (**optional**):

`root #``mirrorselect -i -o >> /mnt/gentoo/etc/portage/make.conf`
Copy repos information:

`root #``mkdir --parents /mnt/gentoo/etc/portage/repos.conf``root #``cp /mnt/gentoo/usr/share/portage/config/repos.conf /mnt/gentoo/etc/portage/repos.conf/gentoo.conf`
Copy DNS nameservers information:

`root #``cp --dereference /etc/resolv.conf /mnt/gentoo/etc/`
If the software compiled by portage is going to be used on that computer only, the following command shows and highlights the specific CPU codename to set `-march`:

`root #``gcc -v -E -x c /dev/null -o /dev/null -march=native 2>&1 | grep /cc1 | grep mtune`
Edit /etc/portage/make.conf, replacing `-march` if possible or desired:

**`/etc/portage/make.conf`**

**make.conf example**

```
COMMON_FLAGS="-march=native -O2 -pipe"
MAKEOPTS="-j4"
```
Mount necessary filesystems (remember to add compression if possible and desired):

`root #``mount /dev/mapper/vg0-root /mnt/gentoo``root #``mount --types proc /proc /mnt/gentoo/proc``root #``mount --rbind /sys /mnt/gentoo/sys``root #``mount --make-rslave /mnt/gentoo/sys``root #``mount --rbind /dev /mnt/gentoo/dev``root #``mount --make-rslave /mnt/gentoo/dev``root #``mount --rbind /run /mnt/gentoo/run``root #``mount --make-rslave /mnt/gentoo/run``root #``mount -t vfat /dev/sdX2 /boot``root #``mount -t tmpfs tmpfs /tmp``root #``mount /dev/mapper/vg0-var /var/``root #``mount /dev/mapper/vg0-home /home/`
Chroot into the system:

`root #``chroot /mnt/gentoo /bin/bash`
Prepare the shell:

`root #``source /etc/profile``root #``export PS1="(chroot) ${PS1}"`
### Emerge

Sync the repositories:

`root #``emerge-webrsync`
Set some CPU flags before compiling anything else:

`root #``emerge --ask app-portage/cpuid2cpuflags``root #``cpuid2cpuflags >> /etc/portage/make.conf`
Install some necessary software:

`root #``emerge --ask --ask app-editors/nano sys-kernel/dracut sys-kernel/gentoo-sources sys-boot/grub sys-fs/lvm2 sys-fs/cryptsetup app-crypt/gnupg sys-fs/btrfs-progs net-misc/dhcpcd net-wireless/wpa_supplicant`
Install plymouth (make booting prettier):

`root #``USE="-gtk -pango -libkms" emerge --ask sys-boot/plymouth`
Enable LVM service:

`root #``rc-update add lvm boot`
### Kernel and Initramfs

Edit the fstab (/etc/fstab) file to match the current disk layout, and use UUIDs, preferably. Also, remember to add mount options if needed.

`root #``findmnt --verify --verbose # verify fstab`
Then, configure and build the Kernel. The recommended guide for this is [Handbook:AMD64/Installation/Kernel](https://wiki.gentoo.org/wiki/Handbook:AMD64/Installation/Kernel). Remember to activate options related to LUKS and LVM.

Edit the [Dracut](https://wiki.gentoo.org/wiki/Dracut) configuration file. Change the UUIDs and filesystems, if needed.

**`/etc/dracut.conf`**

```
# Gentoo specific - from official documentation
udevdir=/lib/udev
ro_mnt=yes
omit_drivers+=" i2o_scsi "
# for rd.luks.key
omit_dracutmodules+=" systemd systemd-initrd dracut-systemd i18n systemd-udevd "
# dm crypt
add_dracutmodules+=" lvm btrfs crypt crypt-gpg dm "
filesystems+=" btrfs "
early_microcode="no"
show_modules="yes"
use_fstab="yes"
hostonly="yes"
# rd.luks.key:UUID - partition at USB stick
# rd.luks.uuid - lvm partition with subpartition /root 
kernel_cmdline="rd.luks.key=/key.gpg:UUID=xxxx-xxxx-xxxx rd.luks.uuid=luks-xxxx-xxxx-xxxx rd.luks rd.lvm rd.lvm.vg=vg0 rd.lvm.lv=vg0/root root=/dev/mapper/vg0-root rootfstype=btrfs rootflags=rw,relatime,ssd,space_cache=v2,subvolid=5,subvol=/ rd.luks.allow-discards=xxxx-xxx-xxxx(sda3 uuid)"
add_drivers+=" i915 " # for X11 early KMS
```
Then, generate the Initramfs, **specifying the correct kernel version**:

`root #``dracut --kver 5.15.26-gentoo --force --hostonly --fstab 2>drac_log.txt`
And finally: copy the *CMDLINE* from /etc/dracut.conf over to /etc/default/grub, in the *GRUB\_CMDLINE\_LINUX* line.

### Bootloader

Install and generate the config for Grub:

`root #``grub-install /dev/sdX``root #``grub-mkconfig -o /boot/grub/grub.cfg`
Now, the system should be installed!

## Tips

If the header is damaged, the entirety of the contents of the filesystem become unreadable. Because of that, if desired, the backup can be made with the following command (change **DESTINATION** to the target directory):

`root #``cryptsetup luksHeaderBackup /dev/sdX3 --header-backup-file DESTINATION`
## TODO

- Use `--type LUKS2` parameter for cryptsetup. LUKS1 used by default.
- Move /boot partiton to the USB stick
- Replace LVM volumes with btrfs subvolumes

## Problems

- Keyfile can be stolen from USB stick
- LUKS partition is not headless - no plausible deniability.

## Links

1\. [Full Disk Encryption From Scratch Simplified](https://wiki.gentoo.org/wiki/Full_Disk_Encryption_From_Scratch_Simplified)

2\. [Dm-crypt\_full\_disk\_encryption](https://wiki.gentoo.org/wiki/Dm-crypt_full_disk_encryption)

3\. [Full\_Encrypted\_Btrfs/Native\_System\_Root\_Guide](https://wiki.gentoo.org/wiki/Full_Encrypted_Btrfs/Native_System_Root_Guide)

4\. [Dracut](https://wiki.gentoo.org/wiki/Dracut)
