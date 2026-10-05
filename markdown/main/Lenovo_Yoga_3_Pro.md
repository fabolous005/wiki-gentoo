<!-- source: https://wiki.gentoo.org/wiki/Lenovo_Yoga_3_Pro | group: Gentoo Wiki (Main) | wiki-title: Lenovo Yoga 3 Pro -->
---
title: Lenovo Yoga 3 Pro
url: https://wiki.gentoo.org/wiki/Lenovo_Yoga_3_Pro
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2024-12-20"
fingerprint: "963cb85f5ba707d9"
license: CC BY-SA 4.0
---

# Lenovo Yoga 3 Pro

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**Resources**

# Hardware

## Laptop Specifications

| **Type** | **Device** | **Model** | **ID** | **Works?** | **Note** | 
| **Processor** | Intel® Core™ M | 5Y70, 5Y71, (Broadwell) | n/a |  |  | 
| **Graphics** | Intel® HD Graphics | 5300 | 8086:161e |  |  | 
| **Display** | 13.3" QHD+ LED (3200x1800) | LTN133YL03-L01 | n/a |  |  | 
| **Audio** | Intel HDA | Broadwell-U Audio Controller | 8086:160c |  | Needs snd\_hda\_intel | 
| **Network** | Wireless | Broadcom BCM4352 | 14e4:43b1 |  | Supported by the wl driver only | 
| **Network** | Bluetooth | Lenovo NGFF (4352 / 20702) | 0489:e07a |  | See [Broadcom Bluetooth](https://wiki.gentoo.org/wiki/Broadcom_Bluetooth) | 
| **Input** | Camera | Lenovo EasyCamera | 5986:0535 | ? | Not yet tested | 
| **Input** | Card Reader |  |  | ? | Not yet tested | 
| **Input** | Touchscreen |  |  |  |  | 
| **Input** | Touchpad |  |  |  |  | 



# Installation

## Booting

To get to the boot/BIOS menu, there is no key combination. Press the recessed button next to the power button instead; unintuitively called the "novo" button by the documentation. You also might have to disable FastStartup and Secure Boot temporarily in the bios menu to get it to boot from your USB device.

## HiDPI

The high DPI means that you have to tweak some settings or suffer through tiny nigh-unreadable text while installing. There are larger fonts available at */usr/share/consolefonts/*, which can be used by running e.g:

`user $``setfont ter-128b`
Later on when you get X11 running, it might think your screen is a lot larger then it really is, defaulting to 96 DPI. Setting the window size manually fixes this.

You generally want a DPI double or 50% larger than 96, as it makes bitmap GUI elements scale better. Still, you need software which takes the DPI into account - the default xterm XLFD font, for example, will still look tiny until you replace it with a non-bitmap font.

**`/etc/X11/xorg.conf.d/30-monitor.conf`**

```
 "Monitor"
    Identifier "<default monitor>"
    # We want a DPI of 192, i.e. half the size of what x11 thinks our screen is (846mm x 476mm).
    DisplaySize 423 238
EndSection
```
## Wireless

The only driver available that supports the card is the proprietary **wl** driver, available by installing **net-wireless/broadcom-sta**. If you don't have this on your installation drive, you might need to use an external USB NIC.

# Configuration

**`/etc/portage/make.conf`**

```
CFLAGS="-O2 -pipe -march=native"
```
**`/etc/portage/package.use/00input`**

```
 INPUT_DEVICES: evdev synaptics
```
**`/etc/portage/package.use/00video`**

```
 VIDEO_CARDS: -* intel i965
```
**`/etc/portage/package.use/00cpu-flags`**

```
  CPU_FLAGS_X86: aes avx avx2 fma3 mmx mmxext popcnt sse sse2 sse3 sse4_1 sse4_2 ssse3
```
## Kernel

**Audio**

**Wifi**

**Inputs**
