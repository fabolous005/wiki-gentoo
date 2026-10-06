<!-- source: https://wiki.gentoo.org/wiki/Clevo_N850HK1 | group: Gentoo Wiki (Main) | wiki-title: Clevo N850HK1 -->
---
title: Clevo N850HK1
url: https://wiki.gentoo.org/wiki/Clevo_N850HK1
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2024-04-28"
fingerprint: "8f009b5d8faaae3b"
license: CC BY-SA 4.0
---

# Clevo N850HK1

From Gentoo Wiki

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)


The N850HK1 is a Laptop model made by the Taiwanese OEM manufacturer Clevo sold under many different names and brands.

The particular variant described below is distributed under the name of G1050Ti-15PL22 by Dream Machines.

## Hardware

### Standard

| Device | Make/model | Status | Vendor ID / Product ID | Kernel driver(s) | Kernel version | Notes | 
|---|---|---|---|---|---|---|
| CPU | Intel(R) Core(TM) i7-7700HQ | Works | N/A | N/A | 4.14.65 | Thermal throttling under heavy load. | 
| Video card 0 | NVIDIA Corporation GP107M \[GeForce GTX 1050 Ti Mobile\] | Works | 10de:1c8c | nvidia | 4.14.65 | Nouveau, Bumblebee, Primusrun have not been tested. | 
| Video card 1 | Intel Corporation Device | Works | 8086:591b | ? | 4.14.65 | Only tested briefly before installing the NVIDIA driver. | 
| Audio | Intel Corporation CM238 HD Audio Controller | Works | 8086:a171 | snd\_hda\_intel | 4.14.65 | Needs some configuration to work properly. | 
| Ethernet | Realtek Semiconductor Co., Ltd. RTL8111/8168/8411 PCI Express Gigabit Ethernet Controller | Works | 10ec:8168 | r8169 | 4.14.65 | The interface is named *enp3s0f1*. | 
| WiFi | Intel Corporation Wireless 8265 / 8275 | Works | 8086:24fd | iwlwifi | 4.14.65 |  | 
| Fingerprint reader | EgisTec ES603 | Unusable | 1c7a:0603 | fprint | 4.14.65 | Scanning fingers rarely works. Crashes fingerprint-gui after enrolling the first finger. | 

## Installation

A standard installation procedure results in a working system.

### Kernel

This is recommended for all systems using snd\_hda\_intel:

KERNEL

```
Device Drivers  --->
    <*> Sound card support  --->
        <*> Advanced Linux Sound Architecture  --->
            (2048) Pre-allocated buffer size for HD-audio driver
```
This may be required if you plan to use [nvidia-drivers](https://wiki.gentoo.org/wiki/NVIDIA/nvidia-drivers):
You may encounter the following error during emerging:

`root #``emerge nvidia-drivers`
Disable *CONFIG\_DEBUG\_MUTEXES* in the kernel's .config file to fix this.

## Troubleshooting

### Screen tearing with binary NVIDIA drivers

FILE **`/etc/X11/xorg.conf`****Fixing screen tearing**

```
Section "ServerLayout"
	Identifier "layout"
	Screen 0 "nvidia"
	Inactive "intel"
EndSection
Section "Device"
	Identifier "nvidia"
	Driver "nvidia"
	BusID "01:00:0"
	Option "RegistryDwords" "EnableBrightnessControl=1"
EndSection
Section "Screen"
	Identifier "nvidia"
	Device "nvidia"
	Option "AllowEmptyInitialConfiguration"
EndSection
Section "Device"
	Identifier "intel"
	Driver "modesetting"
	BusID "00:02:0"
	Option "AccelMethod" "none"
EndSection
Section "Screen"
	Identifier "intel"
	Device "intel"
EndSection
```
### Sound crackling & broken audio hotplugging

FILE **`/etc/modprobe.d/alsa.conf`****(append this, don't replace the whole file)**

```
options snd-hda-intel model=headset-mic
options snd-hda-intel probe_mask=1 # You can also try probe_mask=8 if this fails
```
