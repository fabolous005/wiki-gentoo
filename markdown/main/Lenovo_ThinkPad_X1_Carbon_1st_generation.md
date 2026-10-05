<!-- source: https://wiki.gentoo.org/wiki/Lenovo_ThinkPad_X1_Carbon_1st_generation | group: Gentoo Wiki (Main) | wiki-title: Lenovo ThinkPad X1 Carbon 1st generation -->
---
title: Lenovo ThinkPad X1 Carbon 1st generation
url: https://wiki.gentoo.org/wiki/Lenovo_ThinkPad_X1_Carbon_1st_generation
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2024-04-28"
fingerprint: "2762dc69bb2003b"
license: CC BY-SA 4.0
---

# Lenovo ThinkPad X1 Carbon 1st generation

From Gentoo Wiki

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**Resources**

## Hardware

### Standard

| Device | Make/model | Status | Bus ID | Kernel driver(s) | Kernel version | Notes | 
|---|---|---|---|---|---|---|
| CPU | [Core i5-3317U](https://ark.intel.com/products/65707/Intel-Core-i5-3317U-Processor-3M-Cache-up-to-2_60-GHz) |  | N/A | N/A |  | [Haswell](<https://en.wikipedia.org/wiki/Haswell_(microarchitecture)>) | 
|  | [Core i5-3337U](https://ark.intel.com/products/72055/Intel-Core-i5-3337U-Processor-3M-Cache-up-to-2_70-GHzU) |  | N/A | N/A |  | [Haswell](<https://en.wikipedia.org/wiki/Haswell_(microarchitecture)>) | 
|  | [Core i5-3427U](https://ark.intel.com/products/64903/Intel-Core-i5-3427U-Processor-3M-Cache-up-to-2_80-GHz) |  | N/A | N/A | 4.14 | [Haswell](<https://en.wikipedia.org/wiki/Haswell_(microarchitecture)>) | 
|  | [Core i7-3667U](https://ark.intel.com/products/64898/Intel-Core-i7-3667U-Processor-4M-Cache-up-to-3_20-GHz) |  | N/A | N/A |  | [Haswell](<https://en.wikipedia.org/wiki/Haswell_(microarchitecture)>) | 
| Integrated Graphics | [HD Graphics 4000](https://wiki.gentoo.org/wiki/Intel) |  | 0000:0002 | i915 | 4.14 | VIDEO\_CARDS i965 | 
| Audio |  |  | 0000:001b | snd\_hda\_intel | 4.14 |  | 
| USB 3.0 |  |  | 0000:0014 | xhci\_hcd | 4.14 |  | 
| [Management Engine Interface](https://www.kernel.org/doc/Documentation/misc-devices/mei/mei.txt) |  |  | 0000:0016 | mei\_me | 4.14 |  | 
| PCI |  |  | 0000:001c | pcieport | 4.14 |  | 
| USB 2.0 |  |  | 0000:001d | ehci-pci | 4.14 |  | 
| ISA |  |  | 0000:001f | lpc\_ich | 4.14 |  | 
| SATA |  |  | 0000:001f.2 | ahci | 4.14 |  | 
| SMBus |  |  | 0000:001f.3 | i801\_smbus | 4.14 |  | 
| WiFi | Intel 6205 |  | 0003:0000 | [iwlwifi](https://wiki.gentoo.org/wiki/Iwlwifi) | 4.14 | MVM firmware | 
| Bluetooth | Intel 6205 |  | 0003:0000 |  | 4.14 | Bluetooth 4.0 | 
| TPM | STM |  | N/A | tpm\_tis | 4.14 | TPM 1.2 | 

### Accessories

| Device | Make/model | Status | Bus ID | Kernel driver(s) | Kernel version | Notes |  | 
|---|---|---|---|---|---|---|---|
| Fingerprint Scanner |  |  | N/A | N/A |  | Supported by fprint |  | 
| Multi-touch screen |  |  | N/A | N/A |  |  |  | 
| Memory card reader |  |  | 0002:0000 | rtsx\_pci | 4.14 | SD, MMC, SDHC, SDXC |  | 
| Webcam |  |  | N/A | uvcvideo | 4.14 | 720p; usb 5986:0266 |  | 

### ACPI / Power Management

| Function | Works | Notes | 
|---|---|---|
| CPU frequency scaling |  | Driven by intel\_pstate | 
| GPU Powersaving (RC6) |  |  | 
| SATA Power Management (ALPM) |  |  | 
| Suspend to RAM |  |  | 
| Suspend to disk (hibernate) |  |  | 
| Backlight control |  | Driven by acpi\_video. | 
| Keyboard backlight control |  |  | 

## Installation

### Firmware

#### Intel Wireless 6205

See [Detailed article](https://wiki.gentoo.org/wiki/Iwlwifi).

### Emerge

#### Battery thresholds

`root #``emerge --ask app-laptop/tpacpi-bat`
## Configuration

### Portage

FILE **`/etc/portage/make.conf`**
