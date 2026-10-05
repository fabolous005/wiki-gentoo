<!-- source: https://wiki.gentoo.org/wiki/Clevo_P650HS-G | group: Gentoo Wiki (Main) | wiki-title: Clevo P650HS-G -->
---
title: Clevo P650HS-G
url: https://wiki.gentoo.org/wiki/Clevo_P650HS-G
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2024-04-28"
fingerprint: bd08a3225dba9b4d
license: CC BY-SA 4.0
---

# Clevo P650HS-G

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

This is a Laptop model made by OEM manufacturer Clevo and sold under many different brands as various brand-specific models.

## Hardware

### Standard

| Device | Make/model | Status | Vendor ID / Product ID | Kernel driver(s) | Kernel version | Notes | 
|---|---|---|---|---|---|---|
| CPU | Intel(R) Core(TM) i7-7820hk / i7-7700HQ |  | N/A | N/A | 4.14.16 | Thermal throttles under heavy load. | 
| Video card 0 | NVIDIA Corporation GP104M \[GeForce GTX 1070 Mobile\] |  | 10de:1be1 | nouveau / nvidia | 4.14.16 | Can use Nouveau or Nvidia when integrated graphics is turned off. Can only use Nvidia with [Bumblebee](https://wiki.gentoo.org/wiki/NVIDIA/Bumblebee) when in hybrid mode. Can be passed using KVM and VFIO to virtual machines running Linux with Nouveau. | 
| Video card 1 | Intel Gen9.5 Integrated Graphics \[HD Graphics 630\] |  | 8086:591b | i915 | 4.14.16 | Can be turned off in bios. | 

## Installation

The dual graphics cards setup would cause problems during installation. To install the system, either

- turn integrated graphics off in UEFI mode (switch to DISCRETE mode in bios) and then boot into UEFI enabled live media (X works with the current live DVD). Integrated graphics can be properly setup and turned back on for later use.

or

- boot into live media in bios mode for installation (integrated graphics may still need to be turned off to boot properly into a live environment).

### General graphics kernel options for hybrid mode

- To set up the system for use in hybrid mode, kernel options should be set up properly for Intel graphics. Refer to [Intel](https://wiki.gentoo.org/wiki/Intel)
- To prevent freeze, leave all frame buffer devices unselected.
- To use [Bumblebee](https://wiki.gentoo.org/wiki/NVIDIA/Bumblebee), Nouveau should not be selected.

Here is a list of options that should work for kernel 4.14.16

**Enable support for these hardware drivers**

- Turn on integrated graphics by switching to MSHYBRID mode in bios and the system should boot without freezing if initramfs and grub is configured properly. To use X, extra setups are needed.

## Configuration

### Xorg

To use X in Hybrid mode, you might need to specify modesetting driver options. Example:

**`/etc/X11/xorg.conf.d/modesetting.conf`**

**xorg conf example for hybrid mode**

### Bumblebee

To use the Nvidia card, set up [Bumblebee](https://wiki.gentoo.org/wiki/NVIDIA/Bumblebee).

### VGA passthrough

The Nvidia card can function properly when passed suing VFIO and KVM to linux virtual machines using Nouveau (extra setup required). However, as Nouveau support for Nvidia's 10-series card is currently severely limited[\[1\]](https://www.phoronix.com/scan.php?page=article&item=nouveau-pascal-3d), being able to do so does not provide much benefit other than the ability to use external displays. The possibility of using proprietary Nvidia drivers in VM (including running Windows) is yet to be explored.

### Keyboard backlight

[Clevo-xsm-wmi](https://bitbucket.org/tuxedocomputers/clevo-xsm-wmi) can be used for keyboard backlight control.
