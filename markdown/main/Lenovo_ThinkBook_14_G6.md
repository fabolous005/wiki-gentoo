<!-- source: https://wiki.gentoo.org/wiki/Lenovo_ThinkBook_14_G6 | group: Gentoo Wiki (Main) | wiki-title: Lenovo ThinkBook 14 G6 -->
---
title: Lenovo ThinkBook 14 G6
url: https://wiki.gentoo.org/wiki/Lenovo_ThinkBook_14_G6
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-10-07"
fingerprint: f0e092fd9a21628
license: CC BY-SA 4.0
---

# Lenovo ThinkBook 14 G6

From Gentoo Wiki

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

## Hardware

| Device | Make/model | Status | Kernel driver(s) | Kernel version | Notes | 
|---|---|---|---|---|---|
| CPU | Intel i3/i5/i7 | Works | N/A | 6.18.52 |  | 
| GPU | Intel Corporation Iris Xe Graphics | Works | i915 xe | 6.18.52 | Firmware required. | 
| Audio speakers and jack (3.5mm) port | Intel Corporation Raptor Lake-P/U/H cAVS | Works | snd\_soc\_avs snd\_sof\_pci\_intel\_tgl snd\_hda\_intel | 6.18.52 | Requires configuration and firmware. | 
| Wi-Fi and Bluetooth | Intel Corporation Raptor Lake PCH CNVi WiFi | Works | iwlwifi wl | 6.18.52 | Firmware required. | 
| Ethernet (RJ-45) port | Intel Corporation Ethernet Connection (23) I219-V | Works | e1000e | 6.18.52 | Firmware required. Bluetooth not tested. | 
| Trusted Platform Module (TPM) 2.0 | Intel Corporation Raptor Lake LPC/eSPI Controller | Works | N/A | 6.18.52 |  | 
| Gaussian & Neural Accelerator | Intel Corporation GNA Scoring Accelerator module | Borked | intel\_gna | 6.18.52 | No driver in gentoo-sources. | 
| Fingerprint scanner | Elan Microelectronics Corp. ELAN:Fingerprint | Borked | N/A | 6.18.52 | No driver available. | 



## Keys

All keys are working.

- Fn + Esc - FnLock
- Fn + Space - keyboard backlight
- Fn + B - Ctrl+Break
- Fn + P - Pause
- Fn + N - Unknown key
- Fn + R - Unknown key
- Fn + M - TouchpadToggle
- Fn + S - Alt+SysRq
- Fn + K - SckrLk
- Fn + Right Ctrl - right mouse click
- Fn + i - left mouse click


At boot:

- Fn + F12 Boot Menu (for temporary boot device selection)
- Fn + F2 UEFI/BIOS
