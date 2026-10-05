<!-- source: https://wiki.gentoo.org/wiki/Lenovo_ThinkPad_X1_Carbon_11th_generation | group: Gentoo Wiki (Main) | wiki-title: Lenovo ThinkPad X1 Carbon 11th generation -->
---
title: Lenovo ThinkPad X1 Carbon 11th generation
url: https://wiki.gentoo.org/wiki/Lenovo_ThinkPad_X1_Carbon_11th_generation
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2025-03-14"
fingerprint: "151cf40351a621f5"
license: CC BY-SA 4.0
---

# Lenovo ThinkPad X1 Carbon 11th generation

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**Resources**



## Hardware

### System Model Verfication

`root #``dmidecode -s system-version`
ThinkPad X1 Carbon Gen 11

### Standard

| Device | Make/model | Status | Vendor ID / Product ID | Kernel driver(s) | Kernel version | Notes | 
|---|---|---|---|---|---|---|
| CPU | 13th Gen Intel(R) Core(TM) i7-1370P |  | N/A | N/A | 6.7.7 |  | 
| Video card | Iris Xe Graphics |  | 8086:a7a0 | i915 | 6.7.7 |  | 
| Wireless | Intel Corporation Wireless-AC 9462 |  | 8086:0090 | iwlwifi | 6.7.7 |  | 
| Speakers | Lenovo Raptor Lake-P/U/H |  | 17aa:2315 | snd\_hda\_intel, snd\_sof\_pci\_intel\_tgl | 6.7.7 |  | 
| Microphone | Lenovo Raptor Lake-P/U/H |  | 17aa:2315 | snd\_hda\_intel, snd\_sof\_pci\_intel\_tgl | 6.7.7 |  | 

## Installation

### USEflags

| USEflag | Purpose | 
|---|---|
| `bluetooth` | Bluetooth for Linux | 
| `nvme` | nvme disk support and firmware update | 
| `synaptics` | Synaptics touchpad and fingerprint reader support und firmware update | 
| `thunderbolt` | Thunderbolt support and firwmare update | 
| `tpm` | Trusted Platform support and firmware update | 
| `uefi` | UEFI support and firmware update | 

## Troubleshooting

Troubleshooting specific hardware issues can be challenging, especially when dealing with complex components like the Intel MIPI camera, which is known for its compatibility issues on Linux.

### Intel MIPI Camera

Getting the Intel MIPI camera to work can be particularly tricky due to the lack of open-source drivers and specific firmware requirements. Users might experience difficulties in making the camera fully operational under Linux.

For advanced configurations, troubleshooting tips, and ongoing driver development efforts, refer to the Intel camera IPU6 GitHub repository. This resource provides valuable insights, community support, and potential workarounds for getting the Intel MIPI camera up and running.

External Resource: [Intel ipu6 Camera](https://github.com/intel/ipu6-drivers)

## See also
