<!-- source: https://wiki.gentoo.org/wiki/Dell_Inspiron_3501 | group: Gentoo Wiki (Main) | wiki-title: Dell Inspiron 3501 -->
---
title: Dell Inspiron 3501
url: https://wiki.gentoo.org/wiki/Dell_Inspiron_3501
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2024-06-16"
fingerprint: a7d93c0f51be31a8
license: CC BY-SA 4.0
---

# Dell Inspiron 3501

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

This article focus on getting all default hardware working with this model

## Hardware

### Standard

| Device | Make/model | Status | Vendor ID / Product ID | Kernel driver(s) | Firmware | Kernel version | Notes | 
|---|---|---|---|---|---|---|---|
| CPU | [Intel(R) Core(TM) i3-10005G1 CPU @ 1.20GHz](https://www.intel.com/content/www/us/en/products/sku/196588/intel-core-i31005g1-processor-4m-cache-up-to-3-40-ghz/specifications.html) |  | N/A | N/A | N/A | 5.15.69 | Different CPU options are available for this laptop. | 
| GPU | [Intel® UHD Graphics](https://git.kernel.org/pub/scm/linux/kernel/git/firmware/linux-firmware.git/tree/i915) Iris Plus Graphics G1 (Ice Lake) |  | 8086:8a56 | [i915](https://wiki.gentoo.org/wiki/Intel) | icl\_dmc icl\_guc icl\_huc | 5.15.69 | Intel Corporation Iris Plus Graphics G1 (Ice Lake) | 
| RAM | RAM Module(s) 4GB SODIMM |  | N/A | N/A | N/A | 5.15.69 | Two DIMM slots. Max memory 16GB. | 
| Hard Disk |  |  |  | ahci [NVMe](https://wiki.gentoo.org/wiki/NVMe) | N/A | 5.15.69 |  | 
| [Wifi](https://wiki.gentoo.org/wiki/Wifi) | Qualcomm Atheros QCA9377 802.11ac Wireless Network Adapter |  |  | [ath10k](https://cateee.net/lkddb/web-lkddb/ATH10K.html) | [ath10k](https://git.kernel.org/pub/scm/linux/kernel/git/firmware/linux-firmware.git/tree/ath10k/QCA9377/hw1.0) | 5.15.69 |  | 
| Sound | Realtek ALC3204 |  | N/A | snd\_hda\_intel snd\_hda\_codec\_realtek | N/A | 5.15.69 | N/A | 
| HDMI Sound | Intel Corporation Ice Lake-LP Smart Sound Technology Audio Controller |  |  | snd\_hda\_intel snd\_hda\_codec\_hdmi | N/A | 5.15.69 | N/A | 
| [Touchpad](https://wiki.gentoo.org#Touchpad) | DELL [0A2B:00 06CB:CDD6](https://linux-hardware.org/?id=ps/2:06cb-cdd6-dell0a2b-00-06cb-cdd6-mouse) Touchpad |  |  | intel-lpss i2c-hid |  | 5.15.69 |  | 

## Installation

### Firmware

Due to errata in the processor, it is advised to install and keep the CPU microcode up-to-date. See intel [intel microcode](https://wiki.gentoo.org/wiki/Intel_microcode).

#### Graphics

Systems using Skylake, Broxton, or newer [Intel graphics](https://wiki.gentoo.org/wiki/Intel) will need additional [firmware](https://wiki.gentoo.org/wiki/Linux_firmware) from the [sys-kernel/linux-firmware](https://packages.gentoo.org/packages/sys-kernel/linux-firmware) package:

`root #``emerge --ask sys-kernel/linux-firmware`
Alternatively, the blobs can be directly downloaded from the [Linux repository](https://git.kernel.org/pub/scm/linux/kernel/git/firmware/linux-firmware.git/tree/i915) and put into /lib/firmware

##### DMC firmware

**D**isplay **M**icro**c**ontroller firmware provides support for advanced graphics low-power idle states.

##### GuC/HuC firmware

**G**raphics **µC**ontroller firmware offloads functions from the host driver. **H**EVC/H.265 **µC**ontroller firmware improves hardware acceleration of media decoding.

`root #``cp icl_guc_70.1.1.bin /var/lib/i915/`
**GPU firmware**

**Graphics (Linux 5.15)**

#### Ethernet

Install the [sys-kernel/linux-firmware](https://packages.gentoo.org/packages/sys-kernel/linux-firmware) package:

`root #``emerge --ask sys-kernel/linux-firmware`
Alternatively, the blob can be directly downloaded from the [Linux repository](https://git.kernel.org/pub/scm/linux/kernel/git/firmware/linux-firmware.git/tree/rtl_nic/rtl8106e-1.fw) and copied to /lib/firmware/rtl\_nic.

#### Wi-Fi

Install the [sys-kernel/linux-firmware](https://packages.gentoo.org/packages/sys-kernel/linux-firmware) package:

`root #``emerge --ask sys-kernel/linux-firmware`
Alternatively, the blobs can be directly downloaded from the [Linux repository](https://git.kernel.org/pub/scm/linux/kernel/git/firmware/linux-firmware.git/tree/ath10k/QCA9377/hw1.0).

### Kernel

#### GPU firmware

**GPU firmware**

#### CPU

**CPU**

#### Hard disk

#### Wi-Fi and Ethernet

**Wi-Fi**

#### Sound

snd-hda-intel driver is used . To force sof driver with kernel parameter options snd-intel-dspcfg dsp\_driver=3 but didnt see noticeable difference

**Sound**

#### Multi-function driver

**Multi-function driver**

#### Power management

#### Touchpad

kernel says DELL0A2B i2c\_designware which used hid-multitouch driver

**Touchpad**
