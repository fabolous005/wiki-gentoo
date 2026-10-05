<!-- source: https://wiki.gentoo.org/wiki/Lenovo_Thinkpad_X250 | group: Gentoo Wiki (Main) | wiki-title: Lenovo Thinkpad X250 -->
---
title: Lenovo Thinkpad X250
url: https://wiki.gentoo.org/wiki/Lenovo_Thinkpad_X250
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2024-04-28"
fingerprint: "46562b6e83aa19b7"
license: CC BY-SA 4.0
---

# Lenovo Thinkpad X250

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**Resources**

## Hardware

### Standard

| Device | Make/model | Status | Bus ID | Kernel driver(s) | Kernel version | Notes | 
|---|---|---|---|---|---|---|
| CPU | [Core i3-5010U](http://ark.intel.com/products/84697/Intel-Core-i3-5010U-Processor-3M-Cache-2_10-GHz) |  | N/A | N/A | 4.3 | [Broadwell](<https://en.wikipedia.org/wiki/Broadwell_(microarchitecture)>) | 
|  | [Core i5-5200U](http://ark.intel.com/products/85212/Intel-Core-i5-5200U-Processor-3M-Cache-up-to-2_70-GHz) |  | N/A | N/A |  | [Broadwell](<https://en.wikipedia.org/wiki/Broadwell_(microarchitecture)>) | 
|  | [Core i5-5300U](http://ark.intel.com/products/85213/Intel-Core-i5-5300U-Processor-3M-Cache-up-to-2_90-GHz) |  | N/A | N/A |  | [Broadwell](<https://en.wikipedia.org/wiki/Broadwell_(microarchitecture)>) | 
|  | [Core i7-5600U](http://ark.intel.com/products/85215/Intel-Core-i7-5600U-Processor-4M-Cache-up-to-3_20-GHz) |  | N/A | N/A | 5.4 | [Broadwell](<https://en.wikipedia.org/wiki/Broadwell_(microarchitecture)>) | 
| Integrated Graphics | [HD Graphics 5500](https://wiki.gentoo.org/wiki/Intel) |  | 0000:0002 | i915 | 4.3 | VIDEO\_CARDS i965 | 
| Audio |  |  | 0000:0003 | snd\_hda\_intel | 4.3 |  | 
| USB 3.0 |  |  | 0000:0014 | xhci\_hcd | 4.3 |  | 
| [Management Engine Interface](https://www.kernel.org/doc/Documentation/misc-devices/mei/mei.txt) |  |  | 0000:0016 | mei\_me | 4.3 |  | 
| Ethernet |  |  | 0000:0019 | e1000e | 4.3 |  | 
| PCI |  |  | 0000:001c | pcieport | 4.3 |  | 
| USB 2.0 |  |  | 0000:001d | ehci-pci | 4.3 |  | 
| ISA |  |  | 0000:001f | lpc\_ich | 4.3 |  | 
| SATA |  |  | 0000:001f.2 | ahci | 4.3 |  | 
| SMBus |  |  | 0000:001f.3 | i801\_smbus | 4.3 |  | 
| WiFi | Intel 7265 |  | 0003:0000 | [iwlwifi](https://wiki.gentoo.org/wiki/Iwlwifi) | 4.3 | MVM firmware | 
| Bluetooth | Intel 7265 |  | 0003:0000 |  | 4.3 | Bluetooth 4.0 | 
| TPM | STM |  | N/A | tpm\_tis | 4.3 | TPM 1.2 | 

### Accessories

| Device | Make/model | Status | Bus ID | Kernel driver(s) | Kernel version | Notes | 
|---|---|---|---|---|---|---|
| Fingerprint Scanner |  |  | N/A | N/A |  | Supported by fprint | 
| Multi-touch screen |  |  | N/A | N/A |  |  | 
| Memory card reader |  |  | 0002:0000 | rtsx\_pci | 4.3 | SD, MMC, SDHC, SDXC | 
| [Smart card reader](https://wiki.gentoo.org/wiki/PCSC-Lite) | Alcor Micro AU9540 |  | N/A | ehci-pci | 4.3 |  | 
| Webcam |  |  | N/A | uvcvideo | 4.4 | 720p; usb 04ca:703c | 
| Dock | ThinkPad Ultra Dock |  | N/A | N/A |  |  | 
| Dock | ThinkPad Pro Dock |  | N/A | N/A |  | Tested: all ports (but not with more than one external display), (Un)Dock, Suspend-then-(un)dock | 
| Dock | ThinkPad Basic Dock |  | N/A | N/A |  |  | 

### ACPI / Power Management

| Function | Works | Notes | 
|---|---|---|
| CPU frequency scaling |  | Driven by intel\_pstate | 
| GPU Powersaving (RC6) |  | Support PC8+ with i915.allow\_pc8=1 kernel parameter | 
| SATA Power Management (ALPM) |  |  | 
| Suspend to RAM |  |  | 
| Suspend to disk (hibernate) |  | At first seems frozen, but start to react normally after a few seconds | 
| Backlight control |  | Driven by acpi\_video. | 
| Keyboard backlight control |  |  | 

## Installation

### Firmware

#### Intel Wireless 7265

See [Detailled article](https://wiki.gentoo.org/wiki/Iwlwifi).

### Emerge

#### Battery thresholds

`root #``emerge --ask app-laptop/tpacpi-bat`
Modify the init file in order to change the two batteries, else only one battery will be changed:

**`/etc/init.d/tpacpi-bat`**

See also: [https://github.com/dywisor/tlp-portage](https://github.com/dywisor/tlp-portage)

## Configuration

### Portage

**`/etc/portage/make.conf`**

### X.org

In order to have more readable screen and texts (particularly if you own a 1080p display), indicate to X.org physical size of the display:

**`/etc/X11/xorg.conf.d/90-monitor.conf`**

On 1080p displays, you'll get a DPI resolution of 177x176 dots per inch (default is 96x96).

## Troubleshooting

### Suspend/resume

In order to resume properly, your need to have a working TPM:

**Enable TPM support**

### Total Freeze Using Graphical Acceleration

If you except entire system freeze when using graphical acceleration, you should disable execlists in the kernel cmdline:

i915.enable\_execlists=0

Or just disable VT-d processor feature in BIOS.

When booting directly from UEFI (eg. without Grub), set the following kernel variables:

**Disabling execlists**
