<!-- source: https://wiki.gentoo.org/wiki/Lenovo_ThinkPad_E485 | group: Gentoo Wiki (Main) | wiki-title: Lenovo ThinkPad E485 -->
---
title: Lenovo ThinkPad E485
url: https://wiki.gentoo.org/wiki/Lenovo_ThinkPad_E485
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2024-12-20"
fingerprint: "4d06872f82b651de"
license: CC BY-SA 4.0
---

# Lenovo ThinkPad E485

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**Resources**

The Lenovo ThinkPad E485 is a business laptop aimed at SMB. It is one of the early laptops with an AMD Ryzen CPU with integrated graphics. The laptop has a solid build, a good keyboard, and a nice 14" 1920x1080 display. It is possible to have both NMVe and an SSD or spinning hard disk. It is cheaper than most other ThinkPads, but lacks some features like a fingerprint scanner or a backlit keyboard.

## Hardware

| Device | Make/model | Status | Vendor ID / Product ID | Kernel driver(s) | Kernel version | Notes | 
|---|---|---|---|---|---|---|
| CPU | [AMD Ryzen 5 2500U](http://www.amd.com/en/products/apu/amd-ryzen-5-2500u) |  | N/A | N/A | 5.4.30 |  | 
| Video | AMD Radeon Vega 8 |  | 05:00.0 | amdgpu | 5.4.30 | Integrated graphics | 
| Audio | AMD Raven/Raven2/Fenghuang HDMI/DP Audio Controller |  | 05:00.1 | snd\_hda\_intel | 5.4.40 |  | 
| Audio | AMD Family 17h (Models 10h-1fh) HD Audio Controller |  | 05:00.1 | snd\_hda\_intel | 5.4.40 |  | 
| NVME controller | Samsung SM961/PM961 |  | 01:00.0 | nvme | 5.4.30 |  | 
| SATA controller | AMD FCH SATA Controller |  | 06:00.0 | ahci | 5.4.30 |  | 
| SD/MMC Card Reader | O2 Micro |  | 03:00.0 | sdhci\_pci | 5.4.30 |  | 
| USB | AMD Raven USB 3.1 |  | 05:00.3/4 | xhci\_hcd | 5.4.30 |  | 
| Ethernet | Realtek RTL8111GUS |  | 02:00.0 | r8169 | 5.4.30 | Gigabit LAN 10/100/1000 Mb/s | 
| Wireless LAN | Qualcomm Atheros QCA9377 |  | 04.00.0 | ath10k\_pci | 5.4.30 | 802.11ac | 
| SMBus | AMD FCH SMBus Controller (rev 61) |  | 00:14.0 | piix4\_smbus, i2c\_piix4, sp5100\_tco | 5.4.30 |  | 
| Camera | Chicony, ID 04f2:b604 |  | USB Bus 003 Device 003 | uvcvideo | 5.4.30 | 720p | 
| Bluetooth | Qualcomm Atheros Bluetooth Device ID 0cf3:e500 |  | USB Bus 003 Device 002 | btusb | 5.4.30 |  | 

## Installation

The installation is per the [Gentoo AMD64 handbook](https://wiki.gentoo.org/wiki/Handbook:AMD64).

Some specific configuration options are:

- If [sys-kernel/gentoo-sources](https://packages.gentoo.org/packages/sys-kernel/gentoo-sources) is emerged with the "experimental" USE flag, then it is possible to set the processor family to "AMD Zen"
- Enable the kernel modules for encryption using SSE3 and AES instructions, see [Iwd](https://wiki.gentoo.org/wiki/Iwd) for an example of the options to enable
- [AMDGPU graphics](https://wiki.gentoo.org/wiki/AMDGPU), configuring the drivers for the Vega family
- [USB](https://wiki.gentoo.org/wiki/USB/Guide) support, configuring xHCI, EHCI
- Audio according to [ALSA](https://wiki.gentoo.org/wiki/ALSA) and optionally [PulseAudio](https://wiki.gentoo.org/wiki/PulseAudio). Make sure to enable build Realtek HD-audio codec support.

## Kernel configuration

### CPU

- Microarchitecture: Zen
- Processor core: Raven Ridge

Enable AMD specific kernel options, load the required microcode, and enable the crypto instructions.



**CPU**

### PCI Express

**PCI Express**



### Storage controllers

Configure SATA and NMVe drivers as built-in. The memory card driver can be built in as module.



**Storage controllers**

### Video

**Video**

Note that the AMD GPU also needs firmware, which will be loaded automatically if the driver is built as a module.

`root #``dmesg | grep firmware | grep drm`
\[    4.046891\] \[drm\] Found VCN firmware Version ENC: 1.9 DEC: 1 VEP: 0 Revision: 28
\[    4.046906\] \[drm\] PSP loading VCN firmware

### Sound

**Sound**



### USB

**USB**



### Network

Configure support for both ethernet and wifi



**Network**

### Camera

**Camera**



### Sensors

Sensors are provided through:

- CPU
- GPU
- Thinkpad



**Sensors**

## Portage make.conf

Many options of Gentoo are set in /etc/portage/make.conf. Some of the key aspects are:

**`/etc/portage/make.conf`**

```
CFLAGS="-march=znver1 -O2 -pipe
CXXFLAGS="${CFLAGS}"
```
**`/etc/portage/package.use/00cpu-flags`**

```
 CPU_FLAGS_X86: aes avx avx2 f16c fma3 mmx mmxext pclmul popcnt sha sse sse2 sse3 sse4_1 sse4_2 sse4a ssse3
```
**`/etc/portage/package.use/00video`**

```
 VIDEO_CARDS: -* amdgpu radeonsi
```
