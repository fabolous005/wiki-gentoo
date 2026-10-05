<!-- source: https://wiki.gentoo.org/wiki/Encrypted_bootable_media_with_SecureBoot/GRUB/LUKS | group: Gentoo Wiki (Main) | wiki-title: Encrypted bootable media with SecureBoot/GRUB/LUKS -->
---
title: Encrypted bootable media with SecureBoot/GRUB/LUKS
url: https://wiki.gentoo.org/wiki/Encrypted_bootable_media_with_SecureBoot/GRUB/LUKS
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-03-25"
fingerprint: "9dc8c932a09cab54"
license: CC BY-SA 4.0
---

# Encrypted bootable media with SecureBoot/GRUB/LUKS

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

## Motivation

Often bootable media holds unencrypted next stage boot content like kernel, initramfs and other boot config.

Encrypting bootable media can enhance privacy, and prevent unauthorized access.

## Abstract

Overall boot process varies depending on the hardware platform being used.

This page focuses primarily on x86\_64 architecture with simplified boot process chain:

**Hardware -> [UEFI](https://wiki.gentoo.org/wiki/UEFI) -> [Unified kernel image](https://wiki.gentoo.org/wiki/Unified_kernel_image) (UKI) -> OS**

Per design boot stages hardware->UEFI and UEFI->UKI are never encrypted.

These boot stages can only protected by cryptographic signature like [Secure Boot](https://wiki.gentoo.org/wiki/Secure_Boot).

## Implementation

[GRUB](https://wiki.gentoo.org/wiki/GRUB) boot loader support encrypted next stage boot content.

As [UEFI](https://wiki.gentoo.org/wiki/UEFI) still needs unencypted next stage bootloader, [GRUB](https://wiki.gentoo.org/wiki/GRUB) boot loader have to be splited into two parts: unencrypted [GRUB](https://wiki.gentoo.org/wiki/GRUB) core and encrypted next boot stage content.

Unencrypted [GRUB](https://wiki.gentoo.org/wiki/GRUB) core can protected by cryptographic signature like [Secure Boot](https://wiki.gentoo.org/wiki/Secure_Boot) also.

## Installation

### Prepare bootable media

| Partition | Size | Purpose | Encrypted? | Comment | 
|---|---|---|---|---|
| 1 | 1 MiB | BIOS GRUB | unencrypted | not in use on UEFI platform (only needed on PC-BIOS platform) | 
| 2 | 1 GiB | UEFI ESP | unencrypted | GRUB core, UKI and firmware update blobs. | 
| 3 | 2 GiB | GRUB data | encrypted | GRUB content and next stage boot loaders | 

#### Create bootable media partition

`root #``export bootmedia=/dev/disk/by-id/usb-General_USB_Flash_Disk_01234567890-0:0`
Double check for empty media source:

`root #``parted "${bootmedia}" print``root #``parted --script "${bootmedia}" \````
    "mklabel gpt" \
    "mkpart 'BIOS Grub'  34s            1mib" \
    "mkpart 'EFI System' 1mib           $((1+1024))mib" \
    "mkpart 'Grub data'  $((1+1024))mib $((1+1024+2024))mib" \
    "set 1 bios_grub on" \
    "set 2 esp on" \
    "type 3 ca7d7ccb-63ed-4c53-861c-1742536059cc"
```
Partitions should look like:

`root #``parted "${bootmedia}" unit mib print`
...
Partition Table: gpt
Disk Flags: 
Number  Start    End      Size     File system  Name        Flags
 1      0.02MiB  1.00MiB  0.98MiB               BIOS Grub   bios\_grub
 2      1.00MiB  1025MiB  1024MiB               EFI System  boot, esp
 3      1025MiB  3049MiB  2024MiB               Grub data

#### Create bootable media filesystems

##### Create EFI System Partition (ESP) filesystem

`root #``mkfs.fat -v -F 32 -S 4096 -n "efi-bootfs" -i ef10b001 "${bootmedia}-part2"`
##### Create encrypted GRUB data filesystem

Prepare partition by filling with random data:

`root #``shred --verbose -n1 "${bootmedia}-part3"`
Create encrypted partition with LUKS2 container:

`root #````
cryptsetup luksFormat --type luks2 --cipher aes-xts-plain64 --key-size 512 --hash sha512 --pbkdf argon2id --pbkdf-force-iterations 10 --pbkdf-memory 524288 --pbkdf-parallel 4 --use-random "${bootmedia}-part3"
```
Open encrypted partition to create a filesystem on it:

`root #``cryptsetup open "${bootmedia}-part3" "${bootmedia##*/}-part3.luks"`
Create [GRUB](https://wiki.gentoo.org/wiki/GRUB) data filesystem:

`root #``mkfs.ext4 -e remount-ro -m 0 -L "grub-bootfs" /dev/mapper/"${bootmedia##*/}-part3.luks"`
### Install GRUB boot loader

Mount filesystems for [GRUB](https://wiki.gentoo.org/wiki/GRUB) boot loader installation:

`root #````
mkdir /mnt/bootmedia_efi
```
`root #````
mount -o noatime,dmask=022,fmask=133 "${bootmedia}-part2" /mnt/bootmedia_efi
```
`root #````
mkdir /mnt/bootmedia_grubdata
```
`root #````
mount -o noatime /dev/mapper/"${bootmedia##*/}-part3.luks" /mnt/bootmedia_grubdata
```
Enable [GRUB](https://wiki.gentoo.org/wiki/GRUB) encrypted data support:

`root #``echo GRUB_ENABLE_CRYPTODISK=y >> /etc/default/grub`
Install [GRUB](https://wiki.gentoo.org/wiki/GRUB) boot loader:

`root #````
GRUB_PREBOOT_MODULES="argon2 gcry_sha256 gcry_sha512"
```
`root #````
GRUB_ALLOWEDSECUREBOOT_MODULES="linux tpm"
```
`root #``grub-install --target=x86_64-efi --boot-directory=/mnt/bootmedia_grubdata --efi-directory=/mnt/bootmedia_efi --modules "${GRUB_PREBOOT_MODULES} ${GRUB_ALLOWEDSECUREBOOT_MODULES}" --recheck --removable --disable-shim-lock`
### (optional) sign GRUB boot loader

`root #````
sbsign --key "${SECUREBOOT_SIGN_KEY}" --cert "${SECUREBOOT_SIGN_CRT}" --output /mnt/bootmedia_efi/EFI/BOOT/BOOTX64.EFI /mnt/bootmedia_efi/EFI/BOOT/BOOTX64.EFI
```
`root #````
sbverify --list /mnt/bootmedia_efi/EFI/BOOT/BOOTX64.EFI
```
`root #``sbverify --cert "${SECUREBOOT_SIGN_CRT}" /mnt/bootmedia_efi/EFI/BOOT/BOOTX64.EFI`
Backup signed [GRUB](https://wiki.gentoo.org/wiki/GRUB) core in case generic BOOTX64.EFI will be overridden:

`root #````
mkdir /mnt/bootmedia_efi/EFI/grub/
```
`root #````
cp -a /mnt/bootmedia_efi/EFI/BOOT/BOOTX64.EFI /mnt/bootmedia_efi/EFI/grub/grubx64.efi
```
## Configuration

### Configure GRUB next stage boot loader

Install default [GRUB](https://wiki.gentoo.org/wiki/GRUB) config:

`root #``grub-mkconfig -o /mnt/bootmedia_grubdata/grub/grub.cfg`
Example custom config to boot kernel with initramfs from encrypted [GRUB](https://wiki.gentoo.org/wiki/GRUB) data partition.

**`/mnt/bootmedia_grubdata/grub/custom.cfg`**

```
 "host1 (menu)" {
    menuentry "host1 [curr]" {
        linux /linux/host1/kernel-curr
        initrd /linux/host1/initramfs.cpio.xz /linux/additional.cpio
    }
    menuentry "host1 [prev]" {
        linux /linux/host1/kernel-prev
        initrd /linux/host1/initramfs.cpio.xz /linux/additional.cpio
    }
}
```
Example custom config to boot SystemRescue ISO<sup>[\[1\]](https://wiki.gentoo.org#cite_note-1)</sup> from encrypted [GRUB](https://wiki.gentoo.org/wiki/GRUB) data partition.

**`/mnt/bootmedia_grubdata/grub/custom.cfg`**

```
 "SystemRescue (menu)" {
    set sysrcd_version='11.01'
    menuentry "Boot SystemRescue ${sysrcd_version}" {
        set gfxpayload=keep
        set isofile="/iso/systemrescue-${sysrcd_version}-amd64.iso"
        loopback loop ${isofile}
        # use cryptdevice= to open encrypted GRUB boot filesystem (grub-bootfs) again
        linux (loop)/sysresccd/boot/x86_64/vmlinuz cryptdevice=UUID=<GRUBDATA-LUKS-UUID>:grub-bootfs_crypt img_label=grub-bootfs img_loop=${isofile} archisobasedir=sysresccd iomem=relaxed ar_source=/dev/mapper/grub-bootfs_crypt
        initrd (loop)/sysresccd/boot/intel_ucode.img (loop)/sysresccd/boot/amd_ucode.img (loop)/sysresccd/boot/x86_64/sysresccd.img
    }
}
```
## Clear up

Close all bootmedia filesystems and LUKS container:

`root #````
umount /mnt/bootmedia_efi
```
`root #````
umount /mnt/bootmedia_grubdata
```
`root #````
cryptsetup close "${bootmedia##*/}-part3.luks"
```
`root #````
unset bootmedia
```
- [UEFI](https://wiki.gentoo.org/wiki/UEFI) — a firmware standard for boot ROM designed to provide a stable API for interacting with system hardware. On [x86](https://en.wikipedia.org/wiki/x86) it replaced the legacy [BIOS](https://wiki.gentoo.org/wiki/BIOS).
- [Secure Boot](https://wiki.gentoo.org/wiki/Secure_Boot) — an enhancement of the security of the pre-boot process of a [UEFI](https://wiki.gentoo.org/wiki/UEFI) system.
- [GRUB](https://wiki.gentoo.org/wiki/GRUB) — a multiboot secondary [bootloader](https://wiki.gentoo.org/wiki/Bootloader) capable of loading kernels from a variety of [filesystems](https://wiki.gentoo.org/wiki/Filesystem) on most system architectures.
- [Dm-crypt](https://wiki.gentoo.org/wiki/Dm-crypt) — a disk encryption system using the kernels crypto API framework and device mapper subsystem.
- [ext4](https://wiki.gentoo.org/wiki/Ext4) — an open source disk [filesystem](https://wiki.gentoo.org/wiki/Filesystem) and the most recent version of the extended series of filesystems.
- [Full Disk Encryption From Scratch](https://wiki.gentoo.org/wiki/Full_Disk_Encryption_From_Scratch) — a guide which covers the process of configuring a drive to be encrypted using LUKS and btrfs.
