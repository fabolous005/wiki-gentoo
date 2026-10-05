<!-- source: https://wiki.gentoo.org/wiki/HUAWEI_QingYun_W515 | group: Gentoo Wiki (Main) | wiki-title: HUAWEI QingYun W515 -->
---
title: HUAWEI QingYun W515
url: https://wiki.gentoo.org/wiki/HUAWEI_QingYun_W515
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2025-12-18"
fingerprint: fea3b812cdb297f1
license: CC BY-SA 4.0
---

# HUAWEI QingYun W515

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

HUAWEI QingYun W515 is a desktop computer featuring a [Kirin 990](https://www.hisilicon.com/en/products/kirin/kirin-flagship-chips/kirin-990) mobile phone SoC.

This product is not meant for the consumer market but government usage, which makes it niche and expensive to get (as brand new).

Despite in the form factor of a desktop computer, this device is designed more like an oversized SBC rather than an ARM SystemReady compliant desktop computer/server.

[Standard Build Unit](https://www.linuxfromscratch.org/~bdubbs/about.html) time: \~1 min 30 s (2.44-r4, `USE=nls plugins zstd`)

## Hardware

### Standard

| Device | Make/model | Status | Vendor ID / Product ID | Kernel driver(s) | Kernel version | Notes | 
|---|---|---|---|---|---|---|
| CPU | HiSilicon Kirin 990 |  | - | - | 4.19.71-46-kr990 |  | 
| NAND | SKHynix HN8T15BZGKX016 |  | - | ufshcd | 4.19.71-46-kr990 |  | 
| SATA Controller | ASMedia Technology Inc. ASM1064 Serial ATA Controller |  | 1b21:1064:2116:2116 / 01-06-01 | ahci | 4.19.71-46-kr990 |  | 
| SSD | Dell-EMC/Micron MTFDDAK480TDS |  | - | ahci,sd | 4.19.71-46-kr990 |  | 
| DVD-RW | LiteOn DU-8AESH |  | - | ahci,sr | 4.19.71-46-kr990 |  | 
| USB Controller | Renesas Electronics Corp. uPD720202 USB 3.0 Host Controller |  | 1912:0015 / 0c-03-30 | - | 4.19.71-46-kr990 |  | 
| Internal USB hub | Genesys Logic, Inc. Hub |  | 05e3:0610 / 09-00-00 | - | 4.19.71-46-kr990 |  | 
| Internal USB hub | VIA Labs, Inc. VL820 Hub |  | 2109:0820 / 09-00-00 | - | 4.19.71-46-kr990 |  | 
| LAN | Realtek RTL8153 Gigabit Ethernet Adapter |  | 0bda:8153 / ff-ff-00 | r8152\_n | 4.19.71-46-kr990 | Onboard, but on USB Bus | 
| WLAN | HiSilicon Hi1103CPC |  | ? | plat\_1105,wifi\_1105 | 4.19.71-46-kr990 |  | 
| Bluetooth | HiSilicon Hi1103CPC |  | ? | plat\_1105 | 4.19.71-46-kr990 |  | 
| Audio | HiSilicon Hi6405 |  | ? | - | 4.19.71-46-kr990 |  | 
| GPU | Mali-G76 |  | ? | - | 4.19.71-46-kr990 | Cannot get EGL & WayLand HW Accel working | 
| UEFI | Byosoft UEFI v1.00.71 |  | - | - | 4.19.71-46-kr990 | Cannot properly register efivars | 

### Accessories

| Device | Make/model | Status | Bus ID | Kernel driver(s) | Kernel version | Notes | 
|---|---|---|---|---|---|---|
| Keyboard | Holtek Semiconductor, Inc. Gaming Keyboard |  | 04d9:0024 | usbhid | 4.19.71-46-kr990 | Random branded mechanical keyboard | 
| Keyboard | SayoDevice 2x6F RGB |  | 8089:0008 | usbhid | 4.19.71-46-kr990 | 4x6 programmable keypad | 
| Mouse | Dell MS116 Optical Mouse |  | 413c:301a | usbhid | 4.19.71-46-kr990 |  | 

## Prototypes

There are prototype units being sold on second-hand markets. They could be identified by such identities:

- The front bezel is golden (which is the same as a W510), rather than pure black
- Sticker on the side with product name `XXXXXXXX`
- BIOS and EC version \< 1.00

The hardware of prototype and retail units are mostly identical. Most of the content in this document also applies to prototypes (unless explicitly pointed out), but they could run into strange issues that retail units won't have.

## Installation

### Firmware

The UEFI environment vaguely follows UEFI 2.7 standard. DeviceTree is passed from the UEFI environment. ACPI is not used.

#### Boot Sequence

The boot sequence of this device is closer to an SBC rather than ARM SystemReady compliant desktop computers/servers.

1. The first stage BootROM loads the payload from the UFS storage module, which is the UEFI environment as second stage bootloader.
  - The detailed process is similar to an Android phone that cannot find the OS, then boots into Recovery mode which is actually the UEFI ?
2. The second stage bootloader loads UEFI executables (GRUB etc.) as third stage bootloader
3. The third stage bootloader loads Linux.

#### Optional: Upgrading the Bootloader


Upgrading the Bootloader, both first stage and second stage (officially referenced as HiSilicon Firmware and BIOS) may improve the stability of the device.

Upgrade files could be downloaded from [Support Page](https://bsupport.huawei.com/cn/product/qingyun-w515/).

##### Upgrading the First Stage Bootloader

A command in NeoKylin/Uniontech UOS could be used to check the current first stage bootloader version:

`user $``hwfirmware -v`
current hisi Version: 2.1.203.51

If the first stage version of the firmware is below 2.1.203.27, upgrade to this version first. Otherwise, the newer firmware version may not install due to changed validation algorithms.

Upgrade files are installed the same way as a Debian binary package file. On NeoKylin or Uniontech UOS, this could be done by just double-clicking on the file. On other Linux distros this could be done by command:

`root #``apt install path/to/package.deb`
After the installation of the package, a popup will show up on the bottom right of the desktop. Click the blue start button and the device will reboot and start upgrading.

##### Upgrading the Second Stage Bootloader (UEFI)

Upgrade files are executable files. Run the following command to install:

`root #``chmod 777 path/to/executable.sh && path/to/executable.sh 1`
Upgrade applies once the script has successfully finished.

### Partitioning

This unit doesn't need a "hard disk" by common means to function. Instead, it has a 256/512GB UFS protocol NAND storage module underneath the CPU fan, which functions both as firmware and storage device.

The UFS storage module is emulated into 3 logical devices:

`root #``fdisk -l`
Disk /dev/sda: 4 MiB, 4194304 bytes, 1024 sectors
Disk model: HN8T15BZGKX016  
Units: sectors of 1 \* 4096 = 4096 bytes
Sector size (logical/physical): 4096 bytes / 4096 bytes
I/O size (minimum/optimal): 524288 bytes / 524288 bytes
Disk /dev/sdb: 4 MiB, 4194304 bytes, 1024 sectors
Disk model: HN8T15BZGKX016  
Units: sectors of 1 \* 4096 = 4096 bytes
Sector size (logical/physical): 4096 bytes / 4096 bytes
I/O size (minimum/optimal): 524288 bytes / 524288 bytes
GPT PMBR size mismatch (327680 != 327679) will be corrected by write.
Disk /dev/sdc: 1.26 GiB, 1342177280 bytes, 327680 sectors
Disk model: HN8T15BZGKX016  
Units: sectors of 1 \* 4096 = 4096 bytes
Sector size (logical/physical): 4096 bytes / 4096 bytes
I/O size (minimum/optimal): 524288 bytes / 524288 bytes
Disklabel type: gpt
Disk identifier: F9F221FF-A8D4-5F0E-9746-594869AEC34E
Device      Start    End Sectors  Size Type
/dev/sdc1     128    255     128  512K Linux filesystem
/dev/sdc2     256    383     128  512K Linux filesystem
(...)
/dev/sdc32 271360 271871     512    2M Linux filesystem
Disk /dev/sdd: 237.1 GiB, 254577475584 bytes, 62152704 sectors
Disk model: HN8T15BZGKX016  
Units: sectors of 1 \* 4096 = 4096 bytes
Sector size (logical/physical): 4096 bytes / 4096 bytes
I/O size (minimum/optimal): 524288 bytes / 524288 bytes
Disklabel type: gpt
Disk identifier: 1B7CF424-F98E-4EFB-A37A-0CE833407518

- `/dev/sda` and `/dev/sdb` are the first stage bootloader that **should not be modified in any ways.**
- `/dev/sdc` is the second stage bootloader that **should not be modified in any ways.**
- `/dev/sdd` is the safe part to modify. The OS should be installed here.

Procedures of partitioning SATA disks are trivial.

### Kernel

This device seems cannot boot mainline Linux kernel. The kernel of NeoKylin is used here as an alternative.

Till the last edit of this article, NeoKylin only provides kernel 4.19 for this device and it seems to be based on the kernel for Android.

#### Using Binary NeoKylin Kernel

A NeoKylin binary kernel could be acquired from either the LiveCD or full installation of NeoKylin.

Assuming Gentoo rootfs has been mounted to `/mnt/gentoo`.

Copying the kernel and modules from NeoKylin to Gentoo:

`root #``cp /boot/vmlinuz-4.19.71-46-kr990 /mnt/gentoo/boot/vmlinuz-4.19.71-46-kr990``root #``cp -r /lib/modules/4.19.71-46-kr990 /mnt/gentoo/lib/modules/4.19.71-46-kr990`
Then reference to [Handbook:AMD64/Installation/Base#Chrooting](https://wiki.gentoo.org/wiki/Handbook:AMD64/Installation/Base#Chrooting) to chroot into the Gentoo rootfs.

Regenerate initramfs:

`root #``dracut --kver 4.19.71-46-kr990 && grub-mkconfig -o /boot/grub/grub.cfg`
#### Alternative: Configure and Compile NeoKylin Kernel from Source

NeoKylin kernel source code could either be acquired from apt in a full installation of NeoKylin, or downloaded directly from NeoKylin apt repository.

To get the kernel source code in NeoKylin:

`root #``apt install linux-source`
Assuming the Gentoo rootfs has been mounted, extracting the tarball into the corresponding location:

`root #``tar -xf /usr/src/linux-source-4.19.71.tar.bz2 -C /mnt/gentoo/usr/src --verbose`
Copying the default configuration file into the corresponding location. This file is the one being used to compile the kernel in NeoKylin:

`root #``cp /boot/config-4.19.71-46-kr990 /mnt/gentoo/usr/src/linux-source-4.19.71/.config`
Reference to [Handbook:AMD64/Installation/Base#Chrooting](https://wiki.gentoo.org/wiki/Handbook:AMD64/Installation/Base#Chrooting) to chroot into the Gentoo rootfs.

Compiling the kernel requires GCC 9.5.0. On ARM64, this package has to be unmasked first:

**`/etc/portage/package.unmask`**

**Unmasking GCC 9.5.0**

Then emerging GCC 9.5.0:

`root #``emerge --ask sys-devel/gcc:9.5.0`
Enabling GCC 9.5.0:

`root #``eselect gcc set aarch64-unknown-linux-gnu-9.5.0`
Finally, reference to [Handbook:AMD64/Installation/Kernel](https://wiki.gentoo.org/wiki/Handbook:AMD64/Installation/Kernel) to compile and install the kernel.

##### Kernel Configuration

Most of the configurations required are already set in the default configuration, besides some debloating.

**Disable NeoKylin Specific Security Mechanisms**

Make sure all the framebuffer supports are disabled except `Simple framebuffer support` has been enabled (including `EFI-based Framebuffer Support`), otherwise the system will boot without any display:

**Framebuffer Supports**

### Bootloader

Due to UEFI environment oddities, GRUB may have trouble installing :

`root #``grub-install --efi-directory=/boot/efi`
Installing for arm64-efi platform.
Could not prepare Boot variable: Invalid argument
grub-install: error: efibootmgr failed to register the boot entry: Input/output error.

This is due to [efibootmgr](https://wiki.gentoo.org/wiki/Efibootmgr) unable to write boot entries. Adding `--no-nvram` would solve the issue:

`root #``grub-install --efi-directory=/boot/efi --no-nvram`
As the UEFI is so shabby that it doesn't support multiple hard disk boot entries, even multiple boot entries in the same hard disk, users that would like to boot from both the UFS storage module and SATA disks would have to :

- Put GRUB in the EFI partition of the UFS storage module and adding the Gentoo Linux boot entry to it.
- Put GRUB in the EFI partition of the UFS storage module and adding another GRUB executable in the EFI partition in the SATA disk to chainload it, which would then boot Gentoo.
- Boot from another boot manager in the CD-ROM or USB drive then either loads Gentoo in the SATA disk directly, or chainloads the GRUB in the SATA disk which would then boot Gentoo.

### Emerge

#### GPU

As the 4.19 kernel doesn't support Panfrost DRI, the graphics can only work in software rendering mode.

VIDEO\_CARDS extended use flag does not need to be set. The procedures of setting up X11-based window managers/desktop environments are trivial. Wayland-based window managers/desktop environments cannot function properly.

## Configuration

### Vendor Specific Routines

Some components require vendor-specific binaries and scripts to function. A full installation of NeoKylin could provide these components.

Assuming the Gentoo rootfs has been mounted to `/mnt/gentoo`, execute the following command to copy them into place:

`root #``cp -r /vendor /mnt/gentoo`
#### WLAN

Execute the initialization script `1103start.sh` for the wireless adapter to start functioning. This script will load the corresponding kernel modules.

`root #``/vendor/1103start.sh`
Mainline version of [NetworkManager](https://wiki.gentoo.org/wiki/NetworkManager) and [wpa\_supplicant](https://wiki.gentoo.org/wiki/Wpa_supplicant) have trouble with this wireless adapter that would cause themselves and programs relying on them getting stuck for minutes, if not indefinitely until forcefully stopped after changing/disconnecting from APs. Problem seems resides in power management.

Instead, use [iwd](https://wiki.gentoo.org/wiki/Iwd) in standalone mode.

[dhcpcd](https://wiki.gentoo.org/wiki/Dhcpcd) might have to be introduced to provide DHCP for the WLAN adapter:

`root #``emerge --ask net-misc/dhcpcd`
Then setting up the correspond USE flags:

**`/etc/portage/package.use/iwd`**

**`/etc/portage/package.use/networkmanager`**

Reinstall these packages:

`root #``emerge -atv --deep --newuse net-misc/networkmanager net-wireless/iwd`
Reconfiguring services:

`root #``systemctl daemon-reload && systemctl restart NetworkManager && systemctl enable --now iwd`
Then use iwd to manage wireless connections.

#### Bluetooth

Bluetooth is also provided by HiSilicon Hi1103CPC and has to be initialized using `1103start.sh` first.

After kernel modules are loaded by `1103start.sh`, it has to be further initialized by a customized version of hciattach. This could be acquired from a full installation of NeoKylin/Uniontech UOS:

`user $``cp /usr/bin/hciattach /mnt/gentoo/vendor/hciattach`
Then initialize the bluetooth adapter:

`root #``/vendor/hciattach -n /dev/hwbt hisi`
Finally, reference to [Bluetooth](https://wiki.gentoo.org/wiki/Bluetooth) to set up userland tools.

### Audio

PulseAudio has to have `daemon` use flag enabled:

**`/etc/portage/package.use/pulseaudio`**

(Re)installing PulseAudio:

`root #``emerge --ask media-sound/pulseaudio`
Then copy these files from NeoKylin to Gentoo:

`root #``cd /etc/pulse && cp -f client.conf daemon.conf default.pa pow-exponent.ini system.pa /mnt/gentoo/etc/pulse`
Finally, enabling the PulseAudio service reference to [PulseAudio#Running](https://wiki.gentoo.org/wiki/PulseAudio#Running).
