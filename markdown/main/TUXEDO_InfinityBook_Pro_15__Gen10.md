<!-- source: https://wiki.gentoo.org/wiki/TUXEDO_InfinityBook_Pro_15_(Gen10) | group: Gentoo Wiki (Main) | wiki-title: TUXEDO InfinityBook Pro 15 (Gen10) -->
---
title: TUXEDO InfinityBook Pro 15 (Gen10)
url: https://wiki.gentoo.org/wiki/TUXEDO_InfinityBook_Pro_15_(Gen10)
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-05-08"
fingerprint: "9f541f7338bebd85"
license: CC BY-SA 4.0
---

# TUXEDO InfinityBook Pro 15 (Gen10)

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**Resources**


The TUXEDO InfinityBook Pro 15 (Gen10) a configurable Linux notebook from 2025.

## Hardware

| Device | Make/model | Status | Kernel version | Note | 
|---|---|---|---|---|
| APU | AMD Ryzen AI 9 HX 370 |  | 6.18.7 | Depends on chosen configuration. | 
| Video | Radeon 890M |  | 6.18.7 | Depends on chosen configuration. | 
| NPU | XDNA 2 NPU |  | 7.0 | Depends on chosen configuration. | 
| Keyboard | QWERTZ |  | 6.18.7 | should work with every other layout | 
| WiFi | AMD RZ616 / Mediatek MT7922A22M: |  | 6.18.7 | Intel Wi-Fi 6 AX210 available too. | 
| Ethernet | Motorcomm YT6801 |  | 7.0 | Basic functionality | 

## Installation

### make.conf

**`/etc/portage/package.use/00video`**

```
 VIDEO_CARDS: -* amdgpu radeonsi
```
## Kernel

Running kernels below 6.17 is not recommended by the vendor, who also reports other issues when not running its kernel and OS. [\[1\]](https://wiki.gentoo.org#cite_note-tuxedo_linux_faq-1)

#### Processor

Read [this](https://wiki.gentoo.org/wiki/Ryzen#Kernel) article for the processor.

#### TUXEDO's driver modules

Read the [TUXEDO Software](https://wiki.gentoo.org/wiki/TUXEDO_Software) article for further instructions. In general, install **tuxedo-drivers** and **tuxedo-control-center-bin**.

#### Alternative fan control

See [https://github.com/timohubois/tuxedo-infinitybook-gen10-fan](https://github.com/timohubois/tuxedo-infinitybook-gen10-fan) for an alternative approach towards fan-control avoiding tuxedo-control-center.

## Keyboard

Install the tuxedo-drivers. Backlight can be controlled in **/sys/devices/platform/tuxedo\_keyboard/leds/white:kbd\_backlight** or tuxedo-control-center.

## NPU

The NPU was succesfully tested using the upstream kernel drivers and Lemonade server and FastFlowLM. [User:Lockal/AMDXDNA](https://wiki.gentoo.org/wiki/User:Lockal/AMDXDNA) was a helfpul resource.

## YT6801

Starting with kernel 7.0, there is a dwmac\_motorcomm module, so Ethernet works out of the box now. Features like WoL, RSS and LED control do not appear part of this <sup>[\[2\]](https://wiki.gentoo.org#cite_note-yt6801_upstream-2)</sup>. Previously, the official  driver could be built from <sup>[\[3\]](https://wiki.gentoo.org#cite_note-yt6801_vendor-3)</sup> or TUXEDO's fork [https://gitlab.com/tuxedocomputers/development/packages/tuxedo-yt6801](https://gitlab.com/tuxedocomputers/development/packages/tuxedo-yt6801).
