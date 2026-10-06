<!-- source: https://wiki.gentoo.org/wiki/Dell_XPS_15_9570 | group: Gentoo Wiki (Main) | wiki-title: Dell XPS 15 9570 -->
---
title: Dell XPS 15 9570
url: https://wiki.gentoo.org/wiki/Dell_XPS_15_9570
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2024-04-29"
fingerprint: "3f4d8d275fa2106d"
license: CC BY-SA 4.0
---

# Dell XPS 15 9570

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

The Dell XPS 15 9570 is considered a "MacBook Pro killer". It has amazing performance for around half the cost. As with many laptops, there is some configuration require to make all the components work with Gentoo.

## Status

| **Device/Functionality** | **Status** |  | 
| Suspend | Works |  | 
| Hbernate | Works |  | 
| Intel Integrated Graphics | Works |  | 
| Discrete NVIDIA Graphics | Works |  | 
| Hybrid Graphics/NVIDIA Optimus | Works |  | 
| Wifi | Works |  | 
| Bluetooth | Works |  | 
| rfkill | Works |  | 
| Audio | Works |  | 
| Touchpad | Works |  | 
| Touchscreen | Works |  | 
| Webcam | Works |  | 
| Card Reader | Works |  | 
| Function/Multimedia Keys | Works |  | 
| Power management | Works (Remember to update BIOS) |  | 
| EFI Firmware Updates | Works |  | 
| Fingerprint Reader | Not working |  | 

## History

The Dell XPS 15 9570 is the updated version of the 9560 models. Released in May 2018, this model supports several choices in hardware configurations. Namely,

- The CPU can be one of the new Coffee Lake quad-core Core i5, hexa-core Core i7, or Core i9 processors.
- The graphics subsystem can consist of solely an Intel Integrated GPU or both an Intel Integrated GPU and NVIDIA GeForce GTX 1050Ti GPU.
- An optional fingerprint scanner integrated into the power button.
- There is a choice of touch FHD (1920x1080 @ 60.00 Hz), non-touch FHD (1920x1080 @ 60.00 Hz), touch UHD (3840x2160 @ 60.00 Hz), or non-touch UHD (3840x2160 @ 60.00 Hz) for the screen.
- The choice of a 2.5 SATA SSD or M.@ PCIe NVME SSD.

Additionally, the 9570's Thunderbolt 3 port now supports all four PCIe lanes, unlike the previous models, which only had support for two lanes.

## Hardware

### Standard

| Device | Make/model | Status | Kernel driver(s) | Kernel version | 
|---|---|---|---|---|
| CPU | Intel(R) Core(TM) i7-8750H CPU @ 4.10 GHz | Works |  | 4.20.0 | 
| Memory | 16GB DDR4-2400MHz | Works |  |  | 
| Hard disk | 512GB PCIe NVME Solid State Drive | Works | nvme |  | 
| Video card | NVIDIA Corporation GP107M GeForce GTX 1050 Ti Mobile (4GB GDDR5) | Works | nvidia, fbsimple (or efifb on a UEFI setup) | 4.20.0 | 
| Video card | Intel UHD Graphics 630 (Mobile) | Works | i915 (i965 in your portage.conf - I'll explain later) | 4.20.0 | 
| Wireless | Killer 1535 802.11ac 2x2 WiFi ( [Qualcomm Atheros QCA6174](https://wiki.gentoo.org/wiki/Qualcomm_Atheros_QCA6174)) (Can be switched out for Intel Wireless AC 9260 - more details later) | Works | ath10k\_core ath10k\_pci linux-firmware |  | 
| Touchscreen | ELAN Touchscreen | Works | usbhid hid\_multitouch | 4.15.4 | 
| Touchpad | Synaptics TouchPad | Works | mouse\_ps2\_synaptics\_smbus | 4.13.0 | 
| Bluetooth | Killer 1535 Bluetooth | Works | bluetooth btrtl btintel bnep btbcm rfcomm btusb linux-firmware | 4.15.4 | 
| USB 3.0 |  | Works | xhci\_hcd |  | 
| Thunderbolt 3 | 4 lanes of PCI Express Gen 3. Supports: Power In / Charging, PowerShare, 40Gbps Bi-Directional, 3.1 USB Gen 2 (10Gbps), VGA, HDMI, Ethernet and USB-A via Dell Adapter (Sold Separately) | Works | 4.20.0 |  | 
| SD Card Reader | SD, SDHC, SDXC | Works | ? |  | 
| Webcam | Widescreen HD (720p) | Works | uvc | 4.14.8 | 
| Microphone | Dual array digital microphones | Works | 4.20.0 |  | 
| Fingerprint reader | 138a:0091 Validity Sensors, Inc. | No | None |  | 

### LSPCI

Printout of lspci:

`root #``lspci -nnk````
00:00.0 Host bridge [0600]: Intel Corporation 8th Gen Core Processor Host Bridge/DRAM Registers [8086:3ec4] (rev 07)
        Subsystem: Dell 8th Gen Core Processor Host Bridge/DRAM Registers [1028:087c]
        Kernel driver in use: skl_uncore
00:01.0 PCI bridge [0604]: Intel Corporation Xeon E3-1200 v5/E3-1500 v5/6th Gen Core Processor PCIe Controller (x16) [8086:1901] (rev 07)
        Kernel driver in use: pcieport
00:02.0 VGA compatible controller [0300]: Intel Corporation UHD Graphics 630 (Mobile) [8086:3e9b]
        DeviceName:  Onboard IGD
        Subsystem: Dell UHD Graphics 630 (Mobile) [1028:087c]
        Kernel driver in use: i915
        Kernel modules: i915
00:04.0 Signal processing controller [1180]: Intel Corporation Xeon E3-1200 v5/E3-1500 v5/6th Gen Core Processor Thermal Subsystem [8086:1903] (rev 07)
        Subsystem: Dell Xeon E3-1200 v5/E3-1500 v5/6th Gen Core Processor Thermal Subsystem [1028:087c]
        Kernel driver in use: proc_thermal
        Kernel modules: processor_thermal_device
00:08.0 System peripheral [0880]: Intel Corporation Xeon E3-1200 v5/v6 / E3-1500 v5 / 6th/7th Gen Core Processor Gaussian Mixture Model [8086:1911]
        Subsystem: Dell Xeon E3-1200 v5/v6 / E3-1500 v5 / 6th/7th Gen Core Processor Gaussian Mixture Model [1028:087c]
00:12.0 Signal processing controller [1180]: Intel Corporation Cannon Lake PCH Thermal Controller [8086:a379] (rev 10)
        Subsystem: Dell Cannon Lake PCH Thermal Controller [1028:087c]
        Kernel driver in use: intel_pch_thermal
        Kernel modules: intel_pch_thermal
00:14.0 USB controller [0c03]: Intel Corporation Cannon Lake PCH USB 3.1 xHCI Host Controller [8086:a36d] (rev 10)
        Subsystem: Dell Cannon Lake PCH USB 3.1 xHCI Host Controller [1028:087c]
        Kernel driver in use: xhci_hcd
00:14.2 RAM memory [0500]: Intel Corporation Cannon Lake PCH Shared SRAM [8086:a36f] (rev 10)
        Subsystem: Dell Cannon Lake PCH Shared SRAM [1028:087c]
00:15.0 Serial bus controller [0c80]: Intel Corporation Device [8086:a368] (rev 10)
        Subsystem: Dell Device [1028:087c]
        Kernel driver in use: intel-lpss
        Kernel modules: intel_lpss_pci
00:15.1 Serial bus controller [0c80]: Intel Corporation Device [8086:a369] (rev 10)
        Subsystem: Dell Device [1028:087c]
        Kernel driver in use: intel-lpss
        Kernel modules: intel_lpss_pci
00:16.0 Communication controller [0780]: Intel Corporation Cannon Lake PCH HECI Controller [8086:a360] (rev 10)
        Subsystem: Dell Cannon Lake PCH HECI Controller [1028:087c]
        Kernel driver in use: mei_me
        Kernel modules: mei_me
00:17.0 SATA controller [0106]: Intel Corporation Device [8086:a353] (rev 10)
        Subsystem: Dell Device [1028:087c]
        Kernel driver in use: ahci
        Kernel modules: ahci
00:1b.0 PCI bridge [0604]: Intel Corporation Cannon Lake PCH PCI Express Root Port #17 [8086:a340] (rev f0)
        Kernel driver in use: pcieport
00:1c.0 PCI bridge [0604]: Intel Corporation Cannon Lake PCH PCI Express Root Port #1 [8086:a338] (rev f0)
        Kernel driver in use: pcieport
00:1c.4 PCI bridge [0604]: Intel Corporation Cannon Lake PCH PCI Express Root Port #5 [8086:a33c] (rev f0)
        Kernel driver in use: pcieport
00:1d.0 PCI bridge [0604]: Intel Corporation Cannon Lake PCH PCI Express Root Port #9 [8086:a330] (rev f0)
        Kernel driver in use: pcieport
00:1f.0 ISA bridge [0601]: Intel Corporation Device [8086:a30e] (rev 10)
        Subsystem: Dell Device [1028:087c]
00:1f.3 Audio device [0403]: Intel Corporation Cannon Lake PCH cAVS [8086:a348] (rev 10)
        Subsystem: Dell Cannon Lake PCH cAVS [1028:087c]
        Kernel driver in use: snd_hda_intel
        Kernel modules: snd_hda_intel
00:1f.4 SMBus [0c05]: Intel Corporation Cannon Lake PCH SMBus Controller [8086:a323] (rev 10)
        Subsystem: Dell Cannon Lake PCH SMBus Controller [1028:087c]
        Kernel driver in use: i801_smbus
        Kernel modules: i2c_i801
00:1f.5 Serial bus controller [0c80]: Intel Corporation Cannon Lake PCH SPI Controller [8086:a324] (rev 10)
        Subsystem: Dell Cannon Lake PCH SPI Controller [1028:087c]
01:00.0 3D controller [0302]: NVIDIA Corporation GP107M [GeForce GTX 1050 Ti Mobile] [10de:1c8c] (rev a1)
        Subsystem: Dell GP107M [GeForce GTX 1050 Ti Mobile] [1028:087c]
        Kernel driver in use: nvidia
        Kernel modules: nvidiafb, nouveau, nvidia_drm, nvidia
02:00.0 PCI bridge [0604]: Intel Corporation JHL6340 Thunderbolt 3 Bridge (C step) [Alpine Ridge 2C 2016] [8086:15da] (rev 02)
        Kernel driver in use: pcieport
03:00.0 PCI bridge [0604]: Intel Corporation JHL6340 Thunderbolt 3 Bridge (C step) [Alpine Ridge 2C 2016] [8086:15da] (rev 02)
        Kernel driver in use: pcieport
03:01.0 PCI bridge [0604]: Intel Corporation JHL6340 Thunderbolt 3 Bridge (C step) [Alpine Ridge 2C 2016] [8086:15da] (rev 02)
        Kernel driver in use: pcieport
03:02.0 PCI bridge [0604]: Intel Corporation JHL6340 Thunderbolt 3 Bridge (C step) [Alpine Ridge 2C 2016] [8086:15da] (rev 02)
        Kernel driver in use: pcieport
3a:00.0 USB controller [0c03]: Intel Corporation Device [8086:15db] (rev 02)
        Subsystem: Device [2222:1111]
        Kernel driver in use: xhci_hcd
3b:00.0 Network controller [0280]: Qualcomm Atheros QCA6174 802.11ac Wireless Network Adapter [168c:003e] (rev 32)
        Subsystem: Bigfoot Networks, Inc. QCA6174 802.11ac Wireless Network Adapter [1a56:1535]
Kernel driver in use: ath10k_pci
        Kernel modules: ath10k_pci
3c:00.0 Unassigned class [ff00]: Realtek Semiconductor Co., Ltd. RTS525A PCI Express Card Reader [10ec:525a] (rev 01)
        Subsystem: Dell RTS525A PCI Express Card Reader [1028:087c]
        Kernel driver in use: rtsx_pci
        Kernel modules: rtsx_pci
3d:00.0 Non-Volatile memory controller [0108]: Toshiba America Info Systems Device [1179:0116]
        Subsystem: Toshiba America Info Systems Device [1179:0001]
        Kernel driver in use: nvme
        Kernel modules: nvme
```
### Accessories

## Discussions, considerations, tips, and tricks

### Hybrid graphics/NVIDIA Optimus

No acpi\_osi hackery should be necessary anymore (tested with BIOS version 1.17.1 - BIOS can be updated through fwupd).

You have to use [bbswitch](https://wiki.gentoo.org/wiki/NVIDIA/Bumblebee) to turn off the discrete card when not in use, GTX 1050Ti is not new enough to support runtime D3 power management (that needs at least a Turing based GPU - GTX 1650 or newer).

### Power management

### Physical modifications

#### Wireless card

Remove the Killer Wireless-AC 1535 card and replace it with the Intel Wireless-AC 9260. According to [this forums discussion](https://www.snbforums.com/threads/review-of-intel-wireless-ac-9260-9560.44622/), the Intel card is vastly superior to the Killer card in every way.

The Intel Wireless-AC 9260 can be purchased [on Amazon.](https://www.amazon.com/Intel-Wireless-Ac-9260-2230-Gigabit/dp/B079QJQF4Y/ref=sr_1_2?keywords=intel+wireless+ac+9260&qid=1553136796&refinements=p_85%3A2470955011&rnid=2470954011&rps=1&s=gateway&sr=8-2)

A tutorial on replacing the WiFi card can be found [here.](https://www.youtube.com/watch?v=hAKpjfc2hs8)

#### Heat and CPU throttling

The 9570 has a serious issue with dissipating heat which sometimes leads to throttling [\[1\]](http://www.notebookcheck.net/Some-Dell-XPS-15-9570-laptops-may-have-a-BIOS-related-GPU-bug.355026.0.html)[\[2\]](http://www.dell.com/community/XPS/Dell-XPS-15-9570-i9-thermals-throttling-and-solutions/td-p/6116619)[\[3\]](https://www.reddit.com/r/Dell/comments/98cyam/power_limit_throttling_xps_15_9570/).

In most cases, this can be solved by updating the motherboard firmware with the latest images from [Dell's 9570 support page](https://www.dell.com/support/home/us/en/19/product-support/product/xps-15-9570-laptop/drivers). However, if the issue still persists, following [this tutorial](http://www.ultrabookreview.com/14875-fix-throttling-xps-15/) may produce better results.

## Troubleshooting

## See also

- [Xorg/Guide](https://wiki.gentoo.org/wiki/Xorg/Guide) — explains what Xorg is, how to install it, and the various configuration options.
- [Hprofile](https://wiki.gentoo.org/wiki/Hprofile) — an application that can be used to manage multiple profiles be it hardware or software.
- [NVIDIA](https://wiki.gentoo.org/wiki/NVIDIA) — a popular graphical chipset manufacturer.
- [NVIDIA/nvidia-drivers](https://wiki.gentoo.org/wiki/NVIDIA/nvidia-drivers) — The [x11-drivers/nvidia-drivers](https://packages.gentoo.org/packages/x11-drivers/nvidia-drivers) package contains the *proprietary* graphics driver for [NVIDIA](https://wiki.gentoo.org/wiki/NVIDIA) graphic cards.
- [NVIDIA/Optimus](https://wiki.gentoo.org/wiki/NVIDIA/Optimus) — a proprietary technology that seamlessly switches between two GPUs.
- [NVIDIA/Bumblebee](https://wiki.gentoo.org/wiki/NVIDIA/Bumblebee) — an open source implementation of [NVIDIA Optimus](https://wiki.gentoo.org/wiki/NVIDIA/Optimus).
- [Nouveau](https://wiki.gentoo.org/wiki/Nouveau) — an open source driver for [NVIDIA](https://wiki.gentoo.org/wiki/NVIDIA) graphic cards.
- [Nouveau & nvidia-drivers switching](https://wiki.gentoo.org/wiki/Nouveau_%26_nvidia-drivers_switching) — describes how to switch between [NVIDIA's binary driver](https://wiki.gentoo.org/wiki/NVIDIA/nvidia-drivers) and the open source [nouveau](https://wiki.gentoo.org/wiki/Nouveau) driver.

## External Resources

- [ArchWiki: Dell XPS 15 9570](https://wiki.archlinux.org/index.php/Dell_XPS_15_9570) - The arch wiki page on the Dell XPS 15 9570. For the time being this is the best place to look for general information.
