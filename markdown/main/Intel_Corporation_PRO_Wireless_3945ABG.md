<!-- source: https://wiki.gentoo.org/wiki/Intel_Corporation_PRO/Wireless_3945ABG | group: Gentoo Wiki (Main) | wiki-title: Intel Corporation PRO/Wireless 3945ABG -->
---
title: Intel Corporation PRO/Wireless 3945ABG
url: https://wiki.gentoo.org/wiki/Intel_Corporation_PRO/Wireless_3945ABG
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2025-07-05"
fingerprint: ff4fde7ff92ab109
license: CC BY-SA 4.0
---

# Intel Corporation PRO/Wireless 3945ABG

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

This guide describes how to configure Linux to be able to work with an **Intel Corporation PRO 3945ABG** wireless card. The wireless configuration itself is not part of this guide, please see [Wi-Fi](https://wiki.gentoo.org/wiki/Wi-Fi) for this.

## Kernel

### Hardware detection

First verify if the hardware is detected. A number of approaches can be used.

#### lspci

With lspci the PCI based hardware can be searched through:

`root #``lspci -nnkv | sed -n '/Network/,/^$/p' | head -n -1````
08:00.0 Network controller [0280]: Intel Corporation PRO/Wireless 3945ABG [Golan] Network Connection [8086:4222] (rev 02)
        Subsystem: Hewlett-Packard Company PRO/Wireless 3945ABG [Golan] Network Connection [103c:135c]
        Flags: bus master, fast devsel, latency 0, IRQ 16
        Memory at e8000000 (32-bit, non-prefetchable) [size=4K]
        Capabilities: [c8] Power Management version 2
        Capabilities: [d0] MSI: Enable- Count=1/1 Maskable- 64bit+
        Capabilities: [e0] Express Legacy Endpoint, MSI 00
        Kernel driver in use: iwl3945
        Kernel modules: iwl3945
```
#### lshw

With lshw all hardware information can be searched through:

`root #``lshw````
  *-core
     *-pci
        *-pci:0
           *-network
                description: Ethernet interface
                product: PRO/Wireless 3945ABG [Golan] Network Connection
                vendor: Intel Corporation
                physical id: 0
                bus info: pci@0000:08:00.0
                logical name: wlp8s0
                version: 02
                serial: 00:1b:77:b1:c8:8e
                width: 32 bits
                clock: 33MHz
                capabilities: pm msi pciexpress bus_master cap_list ethernet physical
                configuration: broadcast=yes driver=iwl3945 driverversion=4.1.8-gentoo firmware=15.32.2.9 ip=192.168.178.23 latency=0 link=yes multicast=yes
                resources: irq:16 memory:e8000000-e8000fff
```
### IEEE 802.11

Activate at least [cfg80211](https://wireless.wiki.kernel.org/en/developers/documentation/cfg80211) (`CONFIG_CFG80211`) and [mac80211](https://wireless.wiki.kernel.org/en/developers/documentation/mac80211) (`CONFIG_MAC80211`).

**linux-4.19 example**

Minstrel and its 802.11n support is a [rate control algorithm](https://wireless.wiki.kernel.org/en/developers/Documentation/mac80211/RateControl/minstrel). Some wireless drivers might require it enabled.

### iwl3945 device driver as module

Intel Corporation PRO/Wireless 3945ABG needs the [iwl3945 driver](https://git.kernel.org/cgit/linux/kernel/git/stable/linux-stable.git/tree/drivers/net/wireless/intel/iwlegacy/Kconfig) aka [iwlegacy](https://wireless.wiki.kernel.org/en/users/drivers/iwlegacy). Search (`/` in make menuconfig) shows where to find it:

**linux-4.1 search results**

Set it as a module `<M>` as shown here. After changes on kernel configuration do not forget to [rebuild the kernel](https://wiki.gentoo.org/wiki/Kernel/Rebuild).

**linux-4.1 with modular drivers**

The help (which can be obtained by hitting the `h` in make menuconfig) on the `Intel PRO/Wireless 3945ABG/BG ...` line will show more details about the driver:

### iwl3945 device driver built-in

In case the driver is built into the kernel (`<*>`) instead as a module (`<M>`), also the firmware needs to be built [into the kernel](https://wiki.gentoo.org/wiki/Kernel_Modules#Compile-in-kernel_modules_vs_Loadable_kernel_modules_.28LKMs.29).

**linux-4.1 with built-in drivers**

## Firmware

The [iwlwifi-3945-ucode-15.32.2.9.tgz](https://wireless.wiki.kernel.org/en/users/drivers/iwlegacy#firmware) firmware (or later) is needed. It is available through the [sys-kernel/linux-firmware](https://packages.gentoo.org/packages/sys-kernel/linux-firmware) package.

`root #``emerge --ask sys-kernel/linux-firmware`
## Check the setup

### Module loading

modprobe should not return any errors:

`root #``modprobe iwl3945`
More information about the driver module (such as the supported parameters) can be obtained by modinfo iwl3945:

`user $``modinfo iwl3945`
filename:       /lib/modules/4.1.15-gentoo-r1/kernel/drivers/net/wireless/iwlegacy/iwl3945.ko
firmware:       iwlwifi-3945-2.ucode
license:        GPL
author:         Copyright(c) 2003-2011 Intel Corporation \<ilw@linux.intel.com>
version:        in-tree:s
description:    Intel(R) PRO/Wireless 3945ABG/BG Network Connection driver for Linux
srcversion:     A279E9C3E47A7F92078CA7C
alias:          pci:v00008086d00004227sv\*sd\*bc\*sc\*i\*
alias:          pci:v00008086d00004222sv\*sd\*bc\*sc\*i\*
alias:          pci:v00008086d00004227sv\*sd00001014bc\*sc\*i\*
alias:          pci:v00008086d00004222sv\*sd00001044bc\*sc\*i\*
alias:          pci:v00008086d00004222sv\*sd00001034bc\*sc\*i\*
alias:          pci:v00008086d00004222sv\*sd00001005bc\*sc\*i\*
depends:        iwlegacy
intree:         Y
vermagic:       4.1.15-gentoo-r1 SMP mod\_unload 
parm:           antenna:select antenna (1=Main, 2=Aux, default 0 \[both\]) (int)
parm:           swcrypto:using software crypto (default 1 \[software\]) (int)
parm:           disable\_hw\_scan:disable hardware scanning (default 1) (int)
parm:           fw\_restart:restart firmware in case of error (int)

## Validation

### Dmesg

After reboot, grep the dmesg output for `08:00.0`, `iwl3945` and `wlp8s0`:

`user $``dmesg | grep -i '08:00.0\|iwl3945\|wlp8s0'`
\[    0.148048\] pci 0000:08:00.0: \[8086:4222\] type 00 class 0x028000
\[    0.148099\] pci 0000:08:00.0: reg 0x10: \[mem 0xe8000000-0xe8000fff\]
\[    0.148419\] pci 0000:08:00.0: PME# supported from D0 D3hot D3cold
\[    0.148518\] pci 0000:08:00.0: disabling ASPM on pre-1.1 PCIe device.  You can enable it with 'pcie\_aspm=force'
\[    7.198115\] iwl3945: Intel(R) PRO/Wireless 3945ABG/BG Network Connection driver for Linux, in-tree:s
\[    7.198118\] iwl3945: Copyright(c) 2003-2011 Intel Corporation
\[    7.198183\] iwl3945 0000:08:00.0: can't disable ASPM; OS doesn't have ASPM control
\[    7.252250\] iwl3945 0000:08:00.0: Tunable channels: 13 802.11bg, 23 802.11a channels
\[    7.252254\] iwl3945 0000:08:00.0: Detected Intel Wireless WiFi Link 3945ABG
\[    7.492257\] iwl3945 0000:08:00.0 wlp8s0: renamed from wlan0
\[    7.504225\] systemd-udevd\[278\]: renamed network interface wlan0 to wlp8s0
\[   15.828778\] iwl3945 0000:08:00.0: loaded firmware version 15.32.2.9

## See also

- [Wi-Fi](https://wiki.gentoo.org/wiki/Wi-Fi) — describes the setup of a [Wi-Fi](https://en.wikipedia.org/wiki/Wi-Fi) (wireless) network device.
- [wpa\_supplicant](https://wiki.gentoo.org/wiki/Wpa_supplicant) — an app for [Wi-Fi](https://wiki.gentoo.org/wiki/Wi-Fi) authentication
- [Network management](https://wiki.gentoo.org/wiki/Network_management) — describes possibilities for managing the network stack.
