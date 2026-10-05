<!-- source: https://wiki.gentoo.org/wiki/ZFS/rootfs | group: Gentoo Wiki (Main) | wiki-title: ZFS/rootfs -->
---
title: ZFS/rootfs
url: https://wiki.gentoo.org/wiki/ZFS/rootfs
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-09-29"
fingerprint: "9e851a1fa5ebaba8"
license: CC BY-SA 4.0
---

# ZFS/rootfs

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

This article is focused on installing Gentoo to a ZFS rootfs and has been designed to supplement the [Handbook](https://wiki.gentoo.org/wiki/Handbook:Main_Page). It is not a complete replacement, and only contains information relevant to a ZFS rootfs.

## Preparing the disks

### Designing a partition scheme

This guide assumes Gentoo will be installed on a computer with one internal storage device and moderate amounts of RAM. The partition scheme presented throughout is a suggestion taking that into account. However, ZFS is most effective when it is able to manage entire disks. If multiple disks are available, users may wish to consider placing the root ZFS pool on its own disk.

| Partition | Filesystem | Size | Description | 
|---|---|---|---|
| /dev/sda1 | fat32 | 1 GiB | EFI System Partition | 
| /dev/sda2 | linux-swap | 2-16 GiB | Swap Partition | 
| /dev/sda3 | ZFS | Remainder of disk | Root ZFS Pool ZFS performs its own volume management. Traditional partitioning is not necessary. | 

### EFI System Partition

Create a FAT32 filesystem on the previously created EFI System Partition:

`root #``mkfs.vfat -F 32 /dev/sda1`
### Swap

Swap performance on a ZFS partition is known to be poor. Using a swap file is not supported either, hence it is recommended to use a dedicated swap partition.

`root #``mkswap /dev/sda2``root #``swapon /dev/sda2`
#### Alternative: Going swapless

Personal computers have become increasingly capable and increasingly portable over time, and the role of the swap partition has evolved along with them. RAM is, notwithstanding recent economic trends, so abundant on modern systems that significant activity on the swap partition is generally a sign that something has gone wrong. Simultaneously, the mass adoption of laptop and tablet computers that frequently enter low-power states like [suspend and hibernate](https://wiki.gentoo.org/wiki/Suspend_and_hibernate) have led to a need for swap partitions significantly larger than would ever be reasonable to use for memory paging alone.

Users with plenty of memory (keeping in mind ZFS uses more RAM than most filesystems) and no use for suspend or hibernate may opt to not create a swap partition. This is risky if the system does not have more RAM than its maximum expected workload requires, as the kernel will begin killing processes to manage memory. However, going swapless frees space and reduces the number of disk partitions required. Users particularly concerned with these aspects of a system might benefit from forgoing swap.

### ZFS Setup

#### Generate host ID

A host ID is a positive 32-bit integer (between 1 and 2^32-1) stored in the file /etc/hostid. It acts as a reasonably unique identifier for a Linux system. ZFS checks the host ID to prevent multiple systems from importing the same zpool simultaneously. Users should generate /etc/hostid, overwriting it if necessary, using the zgenhostid command from the ZFS package like so:

`root #``zgenhostid -f`
Alternatively, users can specify an 8-digit hexadecimal number (with or without a preceding *0x*) as the value of the host ID, e.g.

`root #``zgenhostid -f 0xc0ffee67`
### Create a ZFS pool

Load ZFS kernel module and create a ZFS pool `tank` on /dev/sda3.

`root #``modprobe zfs``root #````
zpool create -f \
```
-o ashift=12 \
-o autotrim=on \
-o compatibility=grub2 \
-O acltype=posixacl \
-O xattr=sa \
-O relatime=on \
-O compression=lz4 \

-m none tank /dev/sda3
Using LUKS involves creating an encrypted container on the physical partition first, then placing the ZFS pool inside that container.

Prepare the encrypted partition

First, initialize the physical partition (e.g., /dev/sda3) with LUKS. This will prompt the user to set a password.

`root #``cryptsetup luksFormat /dev/sda3`
Open the encrypted device. This creates a virtual block device at /dev/mapper/cryptroot.

`root #``cryptsetup luksOpen /dev/sda3 cryptroot`
Create the ZFS pool on LUKS

Now, create the pool using the mapper device instead of the raw partition.

`root #``modprobe zfs``root #````
zpool create -f \
```
-o ashift=12 \
-o autotrim=on \
-o compatibility=grub2 \
-O acltype=posixacl \
-O xattr=sa \
-O relatime=on \
-O compression=lz4 \

-m none tank /dev/mapper/cryptroot
After this, proceed to **Create ZFS file systems** as usual.

Update Dracut and Bootloader

For the system to boot, the initramfs must know how to unlock the LUKS device.

1\. Find the UUID of the physical partition (e.g., /dev/sda3)

`root #``blkid /dev/sda3`
2\. Configure Dracut to use the crypt module:

**`/etc/dracut.conf.d/zol.conf`**

**Dracut config for LUKS + ZFS**

3\. Specify the LUKS UUID in the kernel command line. For GRUB, edit /etc/default/grub:

**`/etc/default/grub`**

**Kernel parameters for unlocking LUKS**

Replace `PUT-UUID-HERE` with the UUID from the blkid command. The initramfs must be rebuilt and the bootloader configuration updated before preceding.

ZFS has native encryption available at pool creation. When specified with the flag `-O encryption=on`, the default encryption method is AES-256-GCM. ZFS accepts either passphrases or randomly generated 32-byte encryption keys to control access to encrypted data and users may choose to provide them at an interactive prompt or by specifying a file location at the command line.

`root #````
zpool create -f \
```
-o ashift=12 \
-o autotrim=on \
-o compatibility=openzfs-2.1-linux \
-O acltype=posixacl \
-O xattr=sa \
-O relatime=on \
-O compression=lz4 \
-O encryption=on \
-O keylocation=prompt \
-O keyformat=passphrase \

-m none tank /dev/sda3
### Create ZFS file systems

This guide will be only creating root and home file systems. However, the user is free to create additional file systems if desired.

`root #``zfs create -o mountpoint=none tank/os``root #``zfs create -o mountpoint=/ -o canmount=noauto tank/os/gentoo``root #``zfs create -o mountpoint=/home tank/home`
Set the preferred boot file system of the pool.

`root #``zpool set bootfs=tank/os/gentoo tank`
#### Export and re-import a pool, mount file systems

To export and re-import a pool with a specified mountpoint and without automatically mounting the file systems, run the following commands.

`root #``zpool export tank``root #``zpool import -N -R /mnt/gentoo tank`
It is then possible to mount root and home file systems.

`root #``zfs mount tank/os/gentoo``root #``zfs mount tank/home`
If it exists, users should mount the EFI System Partition.

`root #``mount --mkdir /dev/sda1 /mnt/gentoo/efi`
After mounting the file systems, it is advisable to verify mountpoints by checking the output of the command below.

`root #``mount -t zfs`
Here is an example of the command output in case of successful mounting of file systems.

tank/os/gentoo /mnt/gentoo type zfs (rw,relatime,xattr,posixacl)
tank/home on /mnt/gentoo/home type zfs (rw,relatime,xattr,posixacl)

Update device symbolic links:

`root #``udevadm trigger`
Return to the Handbook - [EFI system partition filesystem](https://wiki.gentoo.org/wiki/Handbook:AMD64/Installation/Disks#EFI_system_partition_filesystem) and return just before entering the chroot command.

### Copy host ID file

`root #``cp /etc/hostid /mnt/gentoo/etc`
Return to Handbook - [Installing Gentoo base system](https://wiki.gentoo.org/wiki/Handbook:AMD64/Installation/Base#Entering_the_new_environment) and return here at Kernel configuration and compilation.

## Configuring the Linux kernel

Users have a choice between manually configuring their kernel or using a distribution kernel. Users who wish to switch between the two on an existing Gentoo system may also benefit from this section. For more information and advice on which to pick, consult the [relevant Handbook section](https://wiki.gentoo.org/wiki/Handbook:AMD64/Installation/Kernel#Kernel_selection).

### Distribution kernel

The distribution kernel approach allows for a much tighter integration with Portage, and is recommended for new Gentoo users.

#### Enabling USE flag

Packages that install additional kernel modules (like ZFS) recognize the [dist-kernel](https://packages.gentoo.org/useflags/dist-kernel) [USE flag and will automatically be rebuilt during a distribution kernel upgrade. It is strongly recommended to enable this globally while using the distribution kernel:](https://wiki.gentoo.org/wiki/USE_flag)

**`/etc/portage/make.conf`**

**Enabling dist-kernel USE flag in make.conf**

#### Installing the distribution kernel

The distribution kernel is installed like any other package, with configuration, building, and installation all performed automatically. To build the distribution kernel from source, type:

`root #``emerge -av sys-kernel/gentoo-kernel`
Users who want to save time can forgo customization and use a prebuilt binary kernel:

`root #``emerge -av sys-kernel/gentoo-kernel-bin`
### Manual kernel configuration

Manually configuring the kernel is the traditional approach and allows users to manipulate the kernel sources before they are built. To fetch the kernel sources with Gentoo patches applied:

`root #``emerge -av sys-kernel/gentoo-sources`
This will install the Gentoo kernel sources to a directory in /usr/src, and with the [symlink](https://packages.gentoo.org/useflags/symlink) [USE flag enabled will create a link to them at /usr/src/linux. The symlink can also be managed using eselect kernel. Navigate to the kernel source directory, configure as desired, and build:](https://wiki.gentoo.org/wiki/USE_flag)

`root #``make -j$(nproc) && make modules_install`
Once the kernel has been built, build the ZFS kernel modules. Make sure to enable the [modules](https://packages.gentoo.org/useflags/modules) [and](https://wiki.gentoo.org/wiki/USE_flag) [rootfs](https://packages.gentoo.org/useflags/rootfs) [USE flags if they are not already.](https://wiki.gentoo.org/wiki/USE_flag)

`root #``emerge -av sys-fs/zfs`
The ZFS kernel modules are now ready. Rebuild the kernel and install it.

`root #``make -j$(nproc) && make modules_install && make install`
Users must repeat this process every time they upgrade or reconfigure their kernel.

### Initramfs

The initramfs is essential for systems with a ZFS rootfs, as the module must be loaded before the kernel is launched.

#### Configure Dracut

ZFS support can be enabled by creating a file like the one below in the Dracut configuration directory /etc/dracut.conf.d. If /etc/dracut.conf.d does not already exist, create it:

**`/etc/dracut.conf.d/zol.conf`**

**Dracut configuration for ZFS**

#### Building an initramfs for the distribution kernel

Users can (re)build the initramfs for their distribution kernel like so:

`root #``emerge --config sys-kernel/gentoo-kernel`
Or, if the binary version was installed,

`root #``emerge --config sys-kernel/gentoo-kernel-bin`
Continue following the Handbook at [Configuring the system](https://wiki.gentoo.org/wiki/Handbook:AMD64/Installation/System) and return at [Configuring the bootloader](https://wiki.gentoo.org/wiki/Handbook:AMD64/Installation/Bootloader).

## Configuring the bootloader

### ZFSBootMenu

ZFSBootMenu is a Linux bootloader that allows users to discover and manipulate ZFS pools and snapshots and to boot off of ZFS filesystems. The following instructions assume a UEFI-based AMD64 system. A more thorough overview of ZFSBootMenu installation and configuration can be found at [ZFS/ZFSBootMenu](https://wiki.gentoo.org/wiki/ZFS/ZFSBootMenu).

#### Mounting the EFI System Partition

If the ESP is not mounted already, it is necessary to do so now:

`root #``mkdir -p /efi``root #``mount /dev/sda1 /efi`
#### Installing ZFSBootMenu (from source)

ZFSBootMenu can be found in the [guru](https://repos.gentoo.org/#guru) repository. This repository must be enabled by the user, as it is not enabled by default. If this has not been done already, do so now using the eselect repository module (from [app-eselect/eselect-repository](https://packages.gentoo.org/packages/app-eselect/eselect-repository)).

`root #``emerge app-eselect/eselect-repository``root #``eselect repository enable guru``root #``emerge --sync`
The ZFSBootMenu package is not currently marked as stable on any architecture, and is thus masked by Portage on stable systems. To accept the unstable keyword for this package, create a file like the one below in the [/etc/portage/package.accept\_keywords](https://wiki.gentoo.org/wiki//etc/portage/package.accept_keywords) directory. If /etc/portage/package.accept\_keywords does not exist, create it.

**`/etc/portage/package.accept_keywords/zfsbootmenu`**

**Unmasking zfsbootmenu for installation**

It is now possible to install [sys-boot/zfsbootmenu::guru](https://github.com/gentoo-mirror/guru/tree/master/sys-boot/zfsbootmenu).

`root #``emerge --ask --verbose sys-boot/zfsbootmenu`
After that, the ZFSBootMenu configuration needs to be adjusted in order to:

- Enable automatic management of images: ManageImages:true
- Point to the EFI mount point: BootMountPoint: /efi
- Enable EFI binary generation: Enabled: true in the EFI section

**`/etc/zfsbootmenu/config.yaml`**

**ZFSBootMenu configuration**

```
Global:
  ManageImages: true
  BootMountPoint: /efi
  DracutConfDir: /etc/zfsbootmenu/dracut.conf.d
  PreHooksDir: /etc/zfsbootmenu/generate-zbm.pre.d
  PostHooksDir: /etc/zfsbootmenu/generate-zbm.post.d
  InitCPIOConfig: /etc/zfsbootmenu/mkinitcpio.conf
Components:
  ImageDir: /efi/EFI/BOOT
  Versions: 3
  Enabled: true
EFI:
  ImageDir: /efi/EFI/BOOT
  Stub: /usr/lib/systemd/boot/efi/linuxx64.efi.stub
  Versions: false
  Enabled: true
Kernel:
  CommandLine: ro loglevel=0
```
Then the bootloader image needs to be generated:

`root #``generate-zbm`
This will output an EFI image to /efi/EFI/BOOT/vmlinuz.EFI, which can then be copied to /efi/EFI/BOOT/BOOTX64.EFI, or [sys-boot/efibootmgr](https://packages.gentoo.org/packages/sys-boot/efibootmgr) can be used.

`root #``efibootmgr -c -d /dev/sdX -p 1 -L "ZFSBootMenu" -l '\EFI\BOOT\VMLINUZ.EFI'`
#### Installing ZFSBootMenu (prebuilt)

Create a directory for the bootloader and download the EFI binary into it.

`root #``mkdir -p /efi/EFI/BOOT`
#### Setting kernel command-line

`root #``zfs set org.zfsbootmenu:commandline="quiet loglevel=4" tank/os`
#### Creating an EFI boot entry

Install the package [sys-boot/efibootmgr](https://packages.gentoo.org/packages/sys-boot/efibootmgr).

`root #``emerge -av sys-boot/efibootmgr`
Then, an EFI boot entry can be created with the following command, if ZFSBootMenu was built from source:

`root #``efibootmgr -c -d /dev/sda -p 1 -L "ZFSBootMenu" -l \\EFI\\ZBM\\VMLINUZ.EFI`
Otherwise, in case the prebuilt ZFSBootMenu binary is used:

`root #``efibootmgr -c -d /dev/sda -p 1 -L "ZFSBootMenu" -l \\EFI\\BOOT\\BOOTX64.EFI`
#### Optional: Native Encryption

If the rootfs zpool was created with native encryption as described in [Alternative: Create a ZFS pool with native encryption](https://wiki.gentoo.org/wiki/ZFS/rootfs#zpool_Native_Encryption), it is possible to configure ZFSBootMenu to prompt for the passphrase once and have dracut have access to encryption keys at boot, or alternatively by setting the `org.zfsbootmenu:keysource` attribute and storing encryption keys on a separate dataset.

[See the ZFSBootMenu documentation](https://docs.zfsbootmenu.org/en/v3.1.x/general/native-encryption.html) for info on how to properly set this up.

### Grand Unified Bootloader (GRUB)

#### Enable ZFS support for GRUB

Set the libzfs flag on the sys-boot/grub package to enable support for ZFS (by building [sys-fs/zfs](https://packages.gentoo.org/packages/sys-fs/zfs) and [sys-fs/zfs-kmod](https://packages.gentoo.org/packages/sys-fs/zfs-kmod)):

`root #``echo "sys-boot/grub libzfs" >> /etc/portage/package.use/grub`
###### Build GRUB with ZFS support

Ensure the GRUB\_PLATFORMS setting is properly configured for systems with EFI partition

`root #``echo 'GRUB_PLATFORMS="efi-64"' >> /etc/portage/make.conf`
Build the [sys-boot/grub](https://packages.gentoo.org/packages/sys-boot/grub) package:

`root #``emerge --ask sys-boot/grub`
#### Install GRUB to mounted EFI partition

`root #``grub-install --target=x86_64-efi --efi-directory=/efi --bootloader-id=gentoo``root #``grub-mkconfig -o /boot/grub/grub.cfg`
#### Add CMDLINE for first boot only

**`/etc/default/grub`**

**ZFSBootMenu configuration**

execute "grub-mkconfig -o /boot/grub/grub.cfg" and when booting into gentoo remove it and reconfigure grub again with the command above

### Limine

Limine is a modern, advanced, portable, multi-protocol bootloader and boot manager, as well as the reference implementation of the Limine boot protocol. Limine supports booting from ZFS root filesystems.

## Finalizing

With Gentoo installed and the bootloader configured, the installation process is nearly complete. There are some final steps necessary to ensure the new system will reboot properly.

#### OpenRC

The `zfs-import` and `zfs-mount` services must be enabled for the system to boot.

`root #``rc-update add zfs-import sysinit``root #``rc-update add zfs-mount sysinit`
### Rebooting the system

Exit the chrooted environment and leave /mnt.

`root #``exit``root #``cd`
Unmount all mounted partitions, and then export the zpool.

`root #``umount -l /mnt/gentoo/dev{/shm,/pts,}``root #``umount -n -R /mnt/gentoo``root #``zpool export tank`
The installation is complete, and it is now safe to reboot the system.

### Troubleshooting

#### Gentoo does not boot after reboot

Press `E` key in the grub entry and add "refresh" to the cmdline
