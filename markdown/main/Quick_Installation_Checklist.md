<!-- source: https://wiki.gentoo.org/wiki/Quick_Installation_Checklist | group: Gentoo Wiki (Main) | wiki-title: Quick Installation Checklist -->
---
title: Quick Installation Checklist
url: https://wiki.gentoo.org/wiki/Quick_Installation_Checklist
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2024-05-09"
fingerprint: "9a11347e11aeeb5f"
license: CC BY-SA 4.0
---

# Quick Installation Checklist

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

This article has been created for *experienced* users who desire a quick, less detailed installation guide. It doubles as a checklist so essential installation steps are not forgotten.

## Format drive

### BIOS/MBR

Create three partitions. 128MB for /boot, 1024MB for swap, and the rest for /

`root #``cfdisk /dev/sda`
Format the partitions:

`root #````
mkfs.ext4 /dev/sda1
```
`root #````
mkfs.ext4 /dev/sda3
```
`root #````
mkswap /dev/sda2 && swapon /dev/sda2
```
### BIOS/GPT

Create four partitions. 128MB for /boot, 2MB for BIOS Boot, 1024MB swap and the rest goes to /

`root #``gdisk /dev/sda`
Create GPT partition table:
Command: o ↵
This option deletes all partitions and creates a new protective MBR.
Proceed? (Y/N): y ↵
Create Partition 1 (/boot):
Command: n ↵
Partition Number: 1 ↵
First sector: ↵
Last sector: +128M ↵
Hex Code: ↵
Create Partition 2 (BIOS boot):
Command: n ↵
Partition Number: 2 ↵
First sector: ↵
Last sector: +2M ↵
Hex Code: EF02 ↵
Create Partition 3 (swap):
Command: n ↵
Partition Number: 3 ↵
First sector: ↵
Last sector: +1024MB ↵
Hex Code: 8200 ↵
Create Partition 4 (/):
Command: n ↵
Partition Number: 4 ↵
First sector: ↵
Last sector: ↵ (for rest of disk)
Hex Code: ↵
Write Partition Table To Disk:
Command: w ↵
Do you want to proceed? (Y/N): Y ↵

Format the partitions:

`root #````
mkfs.ext4 /dev/sda1
```
`root #````
mkfs.ext4 /dev/sda4
```
`root #````
mkswap /dev/sda3 && swapon /dev/sda3
```
### UEFI/GPT

Create four partitions. 128MB for /boot, 128MB for UEFI ESP, 1024MB swap and the rest goes to /

`root #``gdisk /dev/sda`
Create GPT partition table:
Command: o ↵
This option deletes all partitions and creates a new protective MBR.
Proceed? (Y/N): y ↵
Create Partition 1 (/boot):
Command: n ↵
Partition Number: 1 ↵
First sector: ↵
Last sector: +128M ↵
Hex Code: ↵
Create Partition 2 (UEFI ESP):
Command: n ↵
Partition Number: 2 ↵
First sector: ↵
Last sector: +128M ↵
Hex Code: EF00 ↵
Create Partition 3 (swap):
Command: n ↵
Partition Number: 3 ↵
First sector: ↵
Last sector: +1024MB ↵
Hex Code: 8200 ↵
Create Partition 4 (/):
Command: n ↵
Partition Number: 4 ↵
First sector: ↵
Last sector: ↵ (for rest of disk)
Hex Code: ↵
Write Partition Table To Disk:
Command: w ↵
Do you want to proceed? (Y/N): Y ↵

Format the partitions:

`root #````
mkfs.ext4 /dev/sda1
```
`root #````
mkfs.ext4 /dev/sda4
```
`root #````
mkswap /dev/sda3 && swapon /dev/sda3
```
`root #````
mkfs.vfat -F 32 /dev/sda2
```
## Mount partitions

### BIOS/MBR

`root #````
mkdir -p /mnt/gentoo
```
`root #````
mount /dev/sda3 /mnt/gentoo
```
`root #````
mkdir /mnt/gentoo/boot
```
`root #````
mount /dev/sda1 /mnt/gentoo/boot
```
### BIOS/GPT

`root #````
mkdir -p /mnt/gentoo
```
`root #````
mount /dev/sda4 /mnt/gentoo
```
`root #````
mkdir /mnt/gentoo/boot
```
`root #````
mount /dev/sda1 /mnt/gentoo/boot
```
### UEFI/GPT

`root #````
mkdir -p /mnt/gentoo
```
`root #````
mount /dev/sda4 /mnt/gentoo
```
`root #````
mkdir /mnt/gentoo/boot
```
`root #````
mount /dev/sda1 /mnt/gentoo/boot
```
`root #````
mkdir /mnt/gentoo/boot/efi
```
`root #````
mount /dev/sda2 /mnt/gentoo/boot/efi
```
## Stage 3

Find nearest mirror from [this list](https://www.gentoo.org/downloads/mirrors/).

Navigate to the /mnt/gentoo directory:

`root #``cd /mnt/gentoo`
Download the [stage 3](https://wiki.gentoo.org/index.php?title=Stage_3&action=edit&redlink=1). Be sure to replace the keyword (**amd64** and date stamp in the example below) with the correct architecture and date:

`root #``wget https://distfiles.gentoo.org/releases/amd64/autobuilds/current-stage3-amd64-openrc/stage3-amd64-openrc-20220320T170531Z.tar.xz`

Unpack the stage 3 file:

`root #``tar xpf stage3*`
(or with xattrs):

`root #``tar --xattrs-include='*.*' --numeric-owner -xpf stage3*`
## Chroot

Chroot into the extracted stage3:

`root #````
cd /mnt/gentoo
```
`root #``mount --types proc /proc /mnt/gentoo/proc``root #``mount --rbind /sys /mnt/gentoo/sys``root #``mount --make-rslave /mnt/gentoo/sys``root #````
mount --rbind /dev /mnt/gentoo/dev
```
`root #````
mount --make-rslave /mnt/gentoo/dev
```
`root #````
mount --bind /run /mnt/gentoo/run 
```
`root #````
mount --make-slave /mnt/gentoo/run
```
`root #````
cp /etc/resolv.conf etc && chroot . /bin/bash
```
`root #``source /etc/profile`
or if using Gentoo installation media:

`root #``cd /mnt/gentoo && cp /etc/resolv.conf etc && arch-chroot .`
## Portage

Sync the Gentoo repository and update the @world set:

`root #``emerge-webrsync``root #``emerge -avuDN @world`
## User accounts

Change the root password:

`root #``passwd`
Create user(s):

`root #````
useradd -g users -G wheel,portage,audio,video,usb,cdrom -m username
```
`root #``passwd username`
## Configure system

### Install vi (optional)

`root #``emerge -vq vim`
### Editing the fstab file

Configure /etc/fstab to match the actual partitioning performed in the step above:

**`/etc/fstab`**

### Locale

Set system [locale](https://wiki.gentoo.org/wiki/Localization/Guide#Setting_a_locale):

**`/etc/env.d/02locale`**

**Example locale configuration**

Add the locale to /etc/locale.gen:

**`/etc/locale.gen`**

**Example locale configuration**

then generate the locale:

`root #``locale-gen`
### Hostname

Edit /etc/conf.d/hostname

**`/etc/conf.d/hostname`**

**Example Hostname configuration**

### Timezone

Set the appropriate timezone:

`root #``ln -sf /usr/share/zoneinfo/Europe/Helsinki /etc/localtime`
## Kernel

### Emerge

The [sys-kernel/gentoo-sources](https://packages.gentoo.org/packages/sys-kernel/gentoo-sources) package is the vanilla kernel with the Gentoo patchset applied. Choose between kernel sources. The [sys-kernel/linux-firmware](https://packages.gentoo.org/packages/sys-kernel/linux-firmware) package contains binary blobs needed for some hardware (WLAN cards).

If [sys-kernel/gentoo-sources](https://packages.gentoo.org/packages/sys-kernel/gentoo-sources) has been selected:

`root #````
echo "sys-kernel/linux-firmware @BINARY-REDISTRIBUTABLE" | tee -a /etc/portage/package.license
```
`root #````
emerge -av sys-kernel/gentoo-sources sys-kernel/linux-firmware sys-kernel/installkernel
```
`root #````
ln -s /usr/src/linux* /usr/src/linux
```
`root #````
cd /usr/src/linux
```
### Configure

#### Manual

Install [sys-apps/pciutils](https://packages.gentoo.org/packages/sys-apps/pciutils)

`root #``emerge -av pciutils`
Discover which modules are required for the system's hardware:

`root #``lspci -nnk`
Shorter version:

`root #``lspci -nnk | grep "Kernel driver in use:"`
Configure kernel by enabling each necessary module in the menuconfig interface. Search for specific module names by pressing `/` in menuconfig. Navigate to the associated feature by pressing the corresponding number listed on the left of the search results.

`root #``make menuconfig`
Once finished build kernel and modules:

`root #``make -j2`
#### Automatic

If things are working nicely in the current install environment, it is possible to use `localyesconfig` to select all modules currently loaded by the LiveCD:

`root #``make localyesconfig`
Build the kernel and modules:

`root #``make -j2`
### Install

Install the kernel and modules:

`root #````
make modules_install
```
`root #````
make install
```
## Alternative: Using a distribution kernel

If manually installing the kernel looks too hard. try distribution kernels.

Distribution kernels are ebuilds that automate the process of unpacking, configuring, compiling, installing the kernel.

For systemd-boot:

`root #``emerge --ask sys-kernel/installkernel-systemd`
For GRUB, LILO, etc:

`root #``emerge --ask sys-kernel/installkernel-gentoo`
### Install

To build the kernel from source:

`root #``emerge --ask sys-kernel/gentoo-kernel`
To install precompiled kernel images rather than compiling the kernel:

`root #``emerge --ask sys-kernel/gentoo-kernel-bin`
## Bootloader

Specify the correct setting for the system's firmware. BIOS/MBR is `pc`, 64-bit UEFI is `efi-64`, 32-bit UEFI is `efi-32`:

if you decide to use systemd-boot install [sys-boot/systemd-boot](https://packages.gentoo.org/packages/sys-boot/systemd-boot)

Emerge GRUB:

`root #``echo 'GRUB_PLATFORMS="efi-64"' >> /etc/portage/make.conf``root #``emerge --ask sys-boot/grub`
Assuming the system has BIOS firmware:

`root #``grub-install /dev/sda`
Supposing the system has UEFI firmware and the EFI partition is mounted in the /boot/efi directory:

`root #``grub-install --target=x86_64-efi --efi-directory=/boot/efi`
Use the grub-mkconfig command to generate the configuration file:

`root #``grub-mkconfig -o /boot/grub/grub.cfg`
Found vmlinuz-3.14.4-gentoo

## Network

For ethernet install dhcpcd:

`root #``emerge --ask net-misc/dhcpcd`
To start dhcpcd at boot:

`root #``rc-update add dhcpcd default``root #``rc-service dhcpcd start`
For wireless networks install wpa\_supplicant and iw:

`root #``emerge --ask sys-apps/iproute2 net-wireless/wpa_supplicant net-wireless/iw net-wireless/wireless-tools`
## Clean up

Exit chroot, unmount partitions, and reboot:

`root #````
exit
```
`root #``umount -R /mnt/gentoo``root #``reboot`
## See also

- [Gentoo Handbook](https://wiki.gentoo.org/wiki/Gentoo_Handbook) — an effort to centralize essential documentation for initial Gentoo installation and basic system administration.
- [Installation](https://wiki.gentoo.org/wiki/Installation) — an overview of the principles and practices of installing Gentoo on a running system.
