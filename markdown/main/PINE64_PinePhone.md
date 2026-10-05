<!-- source: https://wiki.gentoo.org/wiki/PINE64_PinePhone | group: Gentoo Wiki (Main) | wiki-title: PINE64 PinePhone -->
---
title: PINE64 PinePhone
url: https://wiki.gentoo.org/wiki/PINE64_PinePhone
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-08-31"
fingerprint: e784da5e57b72d01
license: CC BY-SA 4.0
---

# PINE64 PinePhone

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

The **Pinephone** is a cheap, generic, ARM64 smartphone produced with the goal of supporting user-modifiable operating systems and hardware. It uses an Allwinner "sunxi" A64 processor, a Quectel EG-25G Modem, and can boot from either microSD (removable storage) or eMMC (internal storage). It comes in a couple variants that don't really affect the installation process.

## Initial chroot and disk setup

### Option 1: Using another operating system as installation medium

This is the easiest option for installing on the internal eMMC storage, although it takes a long time to compile everything.

This method works by installing another OS on the PinePhone that the user can ssh into, then using that to partition the flash, unpack stage3, and chroot, etc. Gentoo could be either installed on eMMC by installing the other operating system on microSD, or installed on the microSD, by first installing another OS on the eMMC ([https://wiki.pine64.org/index.php/PinePhone#Flashing\_eMMC\_using\_Jumpdrive](https://wiki.pine64.org/index.php/PinePhone#Flashing_eMMC_using_Jumpdrive)).

The author currently have problems with mounting and formatting eMMC with postmarketOS (seems it has to do with the way that filesystems are mounted by it's initramfs), but the unofficial Fedora Linux port works fine. postmarketOS has a nice ssh-over-USB feature ([https://wiki.postmarketos.org/wiki/SSH](https://wiki.postmarketos.org/wiki/SSH)).

### Option 2: cross-compilation from another Gentoo system

This is the easiest option for installing on microSD.

The eMMC can be exposed via USB by installing jumpdrive on a microSD ([https://wiki.pine64.org/index.php/PinePhone#Flashing\_eMMC\_using\_Jumpdrive](https://wiki.pine64.org/index.php/PinePhone#Flashing_eMMC_using_Jumpdrive)).

## Compiling the kernel

The new mainline kernels include the sun50i-a64-pinephone-1.X.dtb file (where 1.X is the hardware version), which is needed for the phone's bootloader. As of December 2020, the latest stable gentoo-sources doesn't have it yet so add "sys-kernel/gentoo-sources \~arm64" to /etc/portage/package.accept\_keywords to get a kernel that supports it.

`root #````
emerge gentoo-sources
```
`root #````
cd /usr/src/linux
```
`root #````
make menuconfig # (or some pinephone_defconfig)
```
`root #````
make -j4
```
`root #````
make modules_install
```
`root #````
make install
```
`root #````
make dtbs
```
`root #````
make dtbs_install
```
## Installing the bootloader

### Option 1: p-boot

p-boot is a bootloader designed specifically for the PinePhone<sup>[\[1\]](https://wiki.gentoo.org#cite_note-1)</sup>. It has a menu that lets the user select different kernels at boot time, like GRUB does. It also allows booting into FEL mode<sup>[\[2\]](https://wiki.gentoo.org#cite_note-2)</sup>.

#### Building from source

To compile p-boot, the Ninja Build system and a secondary compiler (arm-none-eabi-gcc) is needed, which could be acquired with crossdev as so:

`root #``emerge -av git ninja crossdev``root #``crossdev --target arm-none-eabi``root #``cd p-boot/build``root #``ninja`
then the firmware has to be built, which requires aarch64-linux-musl-gcc and or1k-linux-musl-gcc compilers.

`root #``crossdev --target aarch64-linux-musl``root #``crossdev --target or1k-linux-musl``root #``cd ../fw`
### Option 2: U-Boot

U-Boot is a a bootloader for embedded devices. It's supported by the SoC manufacturer, and works on the pine64 which has the same processor.

## Installing a user interface

### Option 1: SwayWM with custom configuration

Sway is a Wayland compositor built to be identical to configure as i3wm. Unlike window managers such as i3, the user can use the mouse, therefore it would work well on a touchscreen. The user could increase the border size to make windows easier to drag and use window scaling to make things larger. The user just needs to replace dmenu with a touchscreen alternative, make an on-screen keyboard for wayland, could set those to the volume buttons respectively and then set swaylock to `Power`. A way to drag windows between virtual workspaces with the touchscreen is needed. This is slowed down by the phone's processor and the lack of touchscreen controls. it takes too long to scroll with the volume buttons for a good user experience.

The user can use [wf-osk](https://github.com/WayfireWM/wf-osk) as an on screen keyboard.

`Volume Up`, `Volume Down`, and `Power` buttons could be bindsym with XF86XK\_AudioRaiseVolume, XF86XK\_AudioLowerVolume, and XF86XK\_PowerOff repectively.

### Option 2: phosh

phosh is a port of GNOME for phone usage.

phosh can be installed from the official guru repository.

Enable the repository and sync:

`root #``eselect repository enable guru``root #``emerge --sync`
Then, merge the phosh package:

`root #``emerge phosh-base/phosh-shell`
## Wifi/Bluetooth

uses RTL8723BS/RTL8723CS

## GPU

the Allwinner A64 has a mali 400 GPU, which is supported by the lima driver<sup>[\[3\]](https://wiki.gentoo.org#cite_note-3)</sup>. To enable support for lima, add `VIDEO_CARDS: -* lima` to /etc/portage/package.use.

The main way programs can take advantage the GPU on this hardware is through OpenGL. Specifically, it supports OpenGL ES 2, so consider using "gles2" and "gles2-only" alongside other already configured useflags. Don't enable the "opengl" useflag, as this seems to override the "gles2-only" useflag on packages like media-libs/mesa.

**`/etc/portage/make.conf`**

**Recommended graphics options**

```
USE="gles2 gles2-only"
```
**`/etc/portage/package.use/00video`**

```
 VIDEO_CARDS: -* lima
```
The GPU frequency could also be adjusted. [\[4\]](https://wiki.gentoo.org#cite_note-4)

## Modem

The current user needs to be in the plugdev group in order to manipulate texts

## See also

- [PINE64 ROCKPro64](https://wiki.gentoo.org/wiki/PINE64_ROCKPro64) — a Rockchip RK3399 (ARMv8-A, Cortex-A72/A53 big.LITTLE) based, exceptionally libre software friendly SBC.
