<!-- source: https://wiki.gentoo.org/wiki/Apple_MacBook_Pro_15-inch_(2017,_Intel,_Four_Thunderbolt_3_Ports) | group: Gentoo Wiki (Main) | wiki-title: Apple MacBook Pro 15-inch (2017, Intel, Four Thunderbolt 3 Ports) -->
---
title: Apple MacBook Pro 15-inch (2017, Intel, Four Thunderbolt 3 Ports)
url: https://wiki.gentoo.org/wiki/Apple_MacBook_Pro_15-inch_(2017,_Intel,_Four_Thunderbolt_3_Ports)
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-10-01"
fingerprint: b5c1e81111ae1114
license: CC BY-SA 4.0
---

# Apple MacBook Pro 15-inch (2017, Intel, Four Thunderbolt 3 Ports)

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)



## Introduction

The **15-inch MacBook Pro (2017)** is an Intel-based laptop using a 7th-generation Intel Core (AMD64) processor, AMD Radeon Pro discrete graphics, Apple T1 hardware, and four Thunderbolt 3 ports.  Model number is A1707.

This page details both:

- External booting Gentoo using an USB stick/drive.
- Installing Gentoo on internal SDD drive



### Hardware

MacBook Pro 15.4" 2017 model A1707 has the following components:

| Component | Hardware | 
|---|---|
| CPU | Intel Core i7 Kaby Lake, typically i7-7700HQ, i7-7820HQ, or i7-7920HQ | 
| Memory | 16 GiB soldered LPDDR3-2133 | 
| Internal storage | Apple PCIe/NVMe SSD, soldered | 
| Integrated graphics | Intel HD Graphics 630 | 
| Discrete graphics | AMD Radeon Pro 555 2 GiB or Radeon Pro 560 4 GiB | 
| Display | 15.4-inch Retina, 2880×1800 | 
| Wi-Fi | Broadcom BCM43602, 802.11ac | 
| Bluetooth | Bluetooth 4.2 (has Bluetooth Classic, Bluetooth A2DP); no support for the following: BlueZ, LE audio (low-energy), BLE 2M PHY, LE Coded PHY / Long Range, Extended Advertising, Direction Finding / AoA/AoD, Periodic Advertising, LE Isochronous Channels, Auracast); rejects products with LE-Audio-only. | 
| Audio | Cirrus Logic CS8409 | 
| Keyboard/trackpad | Apple SPI devices | 
| Touch Bar | Apple T1/iBridge | 
| Touch ID | Apple T1 | 
| Webcam | 720p FaceTime HD camera | 
| Thunderbolt | 4 × Thunderbolt 3 / USB-C | 
| Headphone | 3.5 mm audio jack | 

## External Booting Gentoo

### Requirements

To boot Gentoo on a A1707 from an external drive, have the following items:

- Firmware password, if any
- USB stick/flash-drive, 256GB minimum



### USB preparation

Create the [USB containing Gentoo ISO image](https://wiki.gentoo.org/wiki/Install_Gentoo_on_a_bootable_USB_stick), then:

- Shut the Mac down.
- Insert the USB.
- Power it on.
- Immediately hold Option (⌥).
- Select the first EFI Boot.


Apple documents Option as the key for invoking the Startup Manager on Intel Macs.

### Boot up

To make display more readable other than tiny but readable 1mm font on a 2880x1880, insert the following kernel bootline from custom GRUB menu editor:

But small font during Linux kernel boot-up can be enlarged. To make font larger and easier to read on 2880x1800 screen during GRUB boot up, append this bootline option into the kernel bootup sequence:

- fbcon=font:TER12x24

to the bootline.

## Installation

Below are the installation of Gentoo on the MacBook Pro (model A1707).

### Requirements

To install Gentoo on a A1707, have the following items:

- External USB stick/drive; flash, HDD, or SDD drive
  - for installation-only, minimum 16GB
  - for external booting into a desktop GUI mode, minimum 256GB, larger the better
- Apple Account ID password, if macOS is installed
- Apple firmware password, if any
- A/C power

### Preparation

Certain steps must be done before installing any non-Apple macOS, especially MacBook Pro using T1 Security chipset (later uses T2s):

Test the MacBook Pro with Gentoo OS before installation.

#### Phase 1 — Before you erase anything

- Back up your personal data.
- Copy important files to a USB drive (Apple Time Machine is fine)
- If there's anything you might need later, always assume the Linux installation will destroy it.
- Make sure you know your correct Apple Account password (not the screen login password, your Apple ID Account)
- Under System Preferences
  - Date/Time is current and is in correct timezone (or else Apple Login TLS/SSL HTTP will failed SILENTLY)
  - Turn on Wake on Network Access (aka Wake-on-LAN) (under System Preferences->Battery->Power Adapter. If changed, reboot then do next step.
  - In System preferences->Apple ID->iCloud
  - In Find My Mac, (scroll-down to find that) then Options.
    - Turn off Find My Network (do this first before the next)
    - Turn off Find My Mac
  - Turn off all apps' access to iCloud.

- Remove any firmware password if one exists.

#### Phase 2 — Preserve the Apple EFI/T1 firmware

This is the big important one.

T1 isn't simply a conventional piece of hardware that Linux can initialize from scratch. Community Linux/T1 documentation indicates that important T1 firmware is stored in the Apple EFI partition. If that firmware is removed, the T1 can show up as an Apple device in Recovery Mode instead of functioning normally.

Therefore:

Do not select an installer option equivalent to:

- “Erase disk and install Gentoo"

if that means the installer will completely repartition/format the entire SSD, including the existing EFI System Partition.

Do preserve and keep the existing EFI System Partition, instead, during Linux installation,

#### Phase 3 — Create & Testing the Linux USB

Create the USB containing Gentoo ISO image, then:

- Shut the Mac down.
- Insert the USB.
- Power it on.
- Immediately hold Option (⌥).
- Select the first EFI Boot.

Apple documents Option as the key for invoking the Startup Manager on Intel Macs.

#### Phase 4 — Don't install immediately

Spend some time checking:

- keyboard
- trackpad
- display
- brightness
- USB
- Wi-Fi
- Bluetooth
- webcam
- speakers/headphones
- Touch Bar
- sleep/wake

This is particularly important on the 2017 Touch Bar MacBook Pro.

There is active community work for T1 Macs, but hardware support isn't equivalent to a normal PC. Current documentation describes additional kernel/driver work for audio, Touch Bar, Wi-Fi, camera, etc.

## Hardware checklist

The following checklist ensures that its hardware components are working.

### Firmware

The Apple firmware can boot a standard EFI-compatible Gentoo installation.

Hold `Option` while powering on the machine and select the EFI boot entry for the Gentoo installation medium.

An external USB keyboard is useful during installation because the internal keyboard and trackpad require Apple-specific support.

A wired Ethernet adapter is also recommended when configuring the system for the first time.

### Partitioning

Use GPT partitioning with an EFI System Partition.

For example:

`root #``sgdisk --zap-all /dev/nvme0n1`
Create at least:

| Partition | Suggested size | Filesystem | Purpose | 
|---|---|---|---|
| EFI System Partition | 512 MiB | FAT32 | UEFI boot files | 
| Root | Remaining space | ext4, XFS, Btrfs, etc. | Gentoo system | 

### CPU

The 2017 A1707 uses Intel **Kaby Lake** processors.

For a single-machine installation, an appropriate Kaby Lake target may be selected in /etc/portage/make.conf.

**`/etc/portage/make.conf`**

```
 
COMMON_FLAGS="-O2 -pipe -march=skylake"
```
GCC does not provide a separate kaby-lake for -march target; -march=skylake is appropriate for Kaby Lake and enables the instruction-set extensions supported by the processor.

### Graphics

The machine contains both Intel and AMD graphics.

Enable:

**`/etc/portage/make.conf`**

```
 
VIDEO_CARDS="intel radeonsi"
```
The kernel should provide support for both i915 and amdgpu.

To enlarge tiny font during GRUB, change GRUB\_GFXMODE=auto into:

**`/etc/default/grub`**

**GRUB configuration file**

then run

`root #``update-grub`
### Kernel

When rebuilding the kernel, the following kernel Kconfig functionality is relevant:



| Kernel option | Purpose | 
|---|---|
| CONFIG\_EFI | EFI support | 
| CONFIG\_EFI\_STUB | EFI stub kernel | 
| CONFIG\_NVME\_CORE | NVMe support | 
| CONFIG\_BLK\_DEV\_NVME | NVMe block device | 
| CONFIG\_USB\_XHCI\_HCD | USB 3 / Thunderbolt USB controllers | 
| CONFIG\_THUNDERBOLT | Thunderbolt support | 
| CONFIG\_I2C | I2C support | 
| CONFIG\_SPI | SPI support | 
| CONFIG\_DRM\_I915 | Intel graphics | 
| CONFIG\_DRM\_AMDGPU | AMD graphics | 
| CONFIG\_BRCMFMAC | Broadcom Wi-Fi | 
| CONFIG\_SND\_HDA\_INTEL | Intel HDA audio framework | 
| CONFIG\_HID | HID devices | 
| CONFIG\_INPUT | Input subsystem | 

The exact kernel configuration depends on the installed Gentoo kernel and the desired hardware configuration.

### Apple T1

The 2017 MacBook Pro contains the Apple **T1** chip and its associated iBridge device.

The T1 provides functionality for hardware including the Touch Bar and Touch ID.

The T1 does not expose the same PCIe interfaces as the T2-equipped MacBook models.

#### Keyboard and trackpad

The internal keyboard and trackpad communicate through Apple's SPI interface.

Linux provides support through the Apple SPI input drivers, including applespi.

The Broadcom/Apple trackpad may additionally appear through bcm5974.

Check the kernel log and input devices before installing an external driver:

`root #``dmesg -T | grep -Ei 'apple|spi|bcm5974|keyboard|trackpad'``user $``cat /proc/bus/input/devices`
#### Touch Bar

The Touch Bar is connected through the Apple T1/iBridge subsystem.

Support has historically required out-of-tree software, and available implementations have changed over time.

Avoid embedding a particular DKMS repository or kernel version into the main hardware page unless it is required for the currently supported Gentoo kernel.

#### Touch ID

Touch ID depends on the Apple T1 secure hardware.

Touch ID functionality is not generally available to Linux desktop applications.

### Wi-Fi

The A1707 uses the Broadcom BCM43602 wireless controller.

The Linux driver is brcmfmac.

Check the detected device with:

`user $``lspci | grep -i -E 'network``user $``wireless'`
and:

`user $``lspci -nn`
The required firmware must be available to brcmfmac.

Set the regulatory domain appropriately for the machine's operating location.

For example:

`root #``iw reg set US`
A persistent regulatory-domain configuration should be preferred over running the command manually at every boot.

### Bluetooth

Bluetooth functionality is provided by the Apple/Broadcom Bluetooth hardware connected through USB.

Verify detection with:

`user $``lsusb`
and:

`user $``dmesg -T | grep -i bluetooth`
The standard Linux Bluetooth stack can be used once the controller and firmware are operational.

### Audio

The internal audio controller uses the Cirrus Logic **CS8409** codec.

Generic snd\_hda\_intel support may detect the controller while failing to provide complete functionality on this MacBook Pro.

Apple-specific CS8409 support may therefore be required.

Check detection with:

`user $``lspci -nn | grep -i audio`
and:

`user $``aplay -l`
### Webcam

The built-in FaceTime HD camera is connected through USB.

Check whether the camera is detected:

`user $``lsusb`
and:

`user $``v4l2-ctl --list-devices`
Install [media-video/v4l-utils](https://packages.gentoo.org/packages/media-video/v4l-utils) if v4l2-ctl is unavailable.

Camera support depends on the firmware and kernel/userspace support available for the particular hardware revision.

### Thunderbolt and USB

The machine provides four Thunderbolt 3 / USB-C ports.

Thunderbolt support should be enabled in the kernel with:

thunderbolt

Verify the controller with:

`user $``lspci | grep -i thunderbolt`
and inspect the Thunderbolt subsystem with:

`user $``boltctl`
if [sys-apps/bolt](https://packages.gentoo.org/packages/sys-apps/bolt) is installed.

USB devices can be inspected with:

`user $``lsusb`
### Graphics and power management

Both GPUs are visible to Linux.

Check them with:

`user $``lspci | grep -Ei 'vga|3d|display'`
The Intel GPU uses i915 and the AMD GPU uses amdgpu.

Power management and GPU switching are particularly model- and kernel-dependent.

#### Backlight

Display brightness should normally be exposed through the kernel backlight subsystem.

Check:

`user $``ls /sys/class/backlight/`
If no usable backlight interface is present, inspect:

`user $``dmesg -T | grep -i backlight`
### Battery and power management

Battery information is normally exposed through the kernel power-supply subsystem.

`user $``ls /sys/class/power_supply/``user $``upower -d`
Install [sys-power/upower](https://packages.gentoo.org/packages/sys-power/upower) when desktop power-management integration is required.

The Apple SMC/T1 hardware may expose less functionality than equivalent PC hardware.

### Suspend and hibernate

Suspend support is kernel- and firmware-dependent.

Test suspend only after basic device operation has been verified.

`root #``systemctl suspend`
If resume fails, inspect the previous boot:

`root #``journalctl -b -1 -k`
Avoid copying suspend scripts containing fixed PCI addresses from another MacBook model.

PCI bus numbering can vary between firmware versions, kernel versions, and hardware configurations.

### Hardware detection

The following commands are useful when diagnosing hardware support:

`user $``lspci -nn``user $``lsusb``user $``lsmod``user $``dmesg -T``user $``journalctl -b -k``user $``inxi -Fxxx`
Install [sys-apps/inxi](https://packages.gentoo.org/packages/sys-apps/inxi) when a compact hardware report is useful.

## Troubleshooting

### Internal keyboard or trackpad does not work

Check that the Apple SPI support is present:

`user $``lsmod | grep -E 'applespi|bcm5974'`
Inspect the kernel log:

`user $``dmesg -T | grep -Ei 'apple|spi|bcm5974'`
An external USB keyboard can be used while debugging.

### Wi-Fi controller is detected but cannot connect

Check:

1. The brcmfmac module is loaded.

1. Required firmware is installed.

1. The regulatory domain is correct.

1. The BCM43602 firmware/NVRAM configuration is appropriate for Apple hardware.

1. NetworkManager or another network manager is not overriding the intended configuration.

`user $``dmesg -T | grep -i brcm``user $``iw reg get`
### Audio controller is detected but no sound is available

Check:

`user $``aplay -l``user $``dmesg -T | grep -Ei 'snd|hda|cs8409'`
The CS8409 codec may require Apple-specific driver support.

### Touch Bar is not detected

Check the Apple iBridge device:

`user $``lsusb -nn`
and:

`user $``dmesg -T | grep -Ei 'apple|ibridge|touchbar|t1'`
Touch Bar support is separate from keyboard and trackpad support.

### Suspend fails

Inspect:

`user $``journalctl -b -1 -k`
and:

`user $``cat /sys/power/mem_sleep`
Avoid model-specific scripts from older MacBook documentation unless their device paths have been verified against the current machine.

## External resources

- [MacBook Pro (15-inch, 2017) technical specifications](https://support.apple.com/en-us/111949) — Apple
- [MacBook Pro 2017 Linux Guide](https://github.com/moabdrabou/macbook-pro-2017-linux-guide)
- [snd\_hda\_macbookpro](https://github.com/davidjo/snd_hda_macbookpro) — Apple CS8409 audio driver
- [MacBook Pro Linux documentation](https://github.com/Dunedan/mbp-2016-linux)
- [T1 Touch Bar support](https://github.com/AJ-dev-i60/t1-touchbar)
