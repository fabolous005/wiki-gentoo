<!-- source: https://wiki.gentoo.org/wiki/Lenovo_ThinkPad_T420s | group: Gentoo Wiki (Main) | wiki-title: Lenovo ThinkPad T420s -->
---
title: Lenovo ThinkPad T420s
url: https://wiki.gentoo.org/wiki/Lenovo_ThinkPad_T420s
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2024-12-22"
fingerprint: "7f529725dbaa5a2a"
license: CC BY-SA 4.0
---

# Lenovo ThinkPad T420s

From Gentoo Wiki

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

## Hardware

### Standard

| Device | Make/model | Status | Vendor ID / Product ID | Kernel driver(s) | Kernel version | Notes | 
|---|---|---|---|---|---|---|
| CPU | N/A |  | N/A | N/A | N/A |  | 
| GPU | Intel Corporation 2nd Generation Core Processor Family Integrated Graphics Controller |  | N/A | i915 | N/A |  | 
| Ethernet | Intel Corporation 82579LM Gigabit Network Connection |  | N/A | N/A | N/A |  | 
| Wi-Fi | Intel Corporation Centrino Ultimate-N 6300 |  | N/A | iwlwifi | N/A |  | 
| Card Reader | Ricoh Co Ltd MMC/SD Host Controller |  | N/A | N/A | N/A |  | 

`root #``lspci (partial output, recovered from the history)`
00:00.0 Host bridge: Intel Corporation 2nd Generation Core Processor Family DRAM Controller (rev 09)
00:02.0 VGA compatible controller: Intel Corporation 2nd Generation Core Processor Family Integrated Graphics Controller (rev 09)
00:16.0 Communication controller: Intel Corporation 6 Series/C200 Series Chipset Family MEI Controller #1 (rev 04)
00:16.3 Serial controller: Intel Corporation 6 Series/C200 Series Chipset Family KT Controller (rev 04)
00:19.0 Ethernet controller: Intel Corporation 82579LM Gigabit Network Connection (rev 04)
00:1a.0 USB controller: Intel Corporation 6 Series/C200 Series Chipset Family USB Enhanced Host Controller #2 (rev 04)
00:1b.0 Audio device: Intel Corporation 6 Series/C200 Series Chipset Family High Definition Audio Controller (rev 04)
00:1c.0 PCI bridge: Intel Corporation 6 Series/C200 Series Chipset Family PCI Express Root Port 1 (rev b4)
00:1c.1 PCI bridge: Intel Corporation 6 Series/C200 Series Chipset Family PCI Express Root Port 2 (rev b4)
00:1c.3 PCI bridge: Intel Corporation 6 Series/C200 Series Chipset Family PCI Express Root Port 4 (rev b4)
00:1c.4 PCI bridge: Intel Corporation 6 Series/C200 Series Chipset Family PCI Express Root Port 5 (rev b4)
00:1d.0 USB controller: Intel Corporation 6 Series/C200 Series Chipset Family USB Enhanced Host Controller #1 (rev 04)
00:1f.0 ISA bridge: Intel Corporation QM67 Express Chipset Family LPC Controller (rev 04)
00:1f.2 SATA controller: Intel Corporation 6 Series/C200 Series Chipset Family 6 port SATA AHCI Controller (rev 04)
00:1f.3 SMBus: Intel Corporation 6 Series/C200 Series Chipset Family SMBus Controller (rev 04)
03:00.0 Network controller: Intel Corporation Centrino Ultimate-N 6300 (rev 3e)
05:00.0 System peripheral: Ricoh Co Ltd MMC/SD Host Controller (rev 07)
0d:00.0 USB controller: NEC Corporation uPD720200 USB 3.0 Host Controller (rev 04)

## Installation

### Kernel

FILE **`.config`**

```
CONFIG_DRM_I915
CONFIG_DRM_I915_KMS
CONFIG_SERIAL_CORE
CONFIG_E1000E
CONFIG_USB_EHCI_PCI
CONFIG_SND_HDA_INTEL
CONFIG_USB_EHCI_PCI
CONFIG_SATA_AHCI
CONFIG_I2C_I801
CONFIG_IWLWIFI
CONFIG_MMC_SDHCI
CONFIG_MMC_SDHCI_PCI
```
### Emerge

FILE **`/etc/portage/package.use/00input`**

```
 INPUT_DEVICES: evdev synaptics
```
FILE **`/etc/portage/package.use/00video`**

```
 VIDEO_CARDS: -* intel i965
```
FILE **`/etc/portage/package.use/00cpu-flags`**

```
  CPU_FLAGS_X86: aes avx mmx mmxext popcnt sse sse2 sse3 sse4_1 sse4_2 ssse3
```
## Configuration

### Fan control

Fan control needs to be explicitly allowed:

FILE **`/etc/modprobe.d/thinkpad_acpi.conf`**

Turn off:

`root #``echo level 0 > /proc/acpi/ibm/fan`
Maximum measured speed:

`root #``echo level 7 > /proc/acpi/ibm/fan`
Automatic speed (default):

`root #``echo level auto > /proc/acpi/ibm/fan`
Maximum speed:

`root #``echo level disengaged > /proc/acpi/ibm/fan`
