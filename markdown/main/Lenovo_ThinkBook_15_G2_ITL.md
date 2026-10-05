<!-- source: https://wiki.gentoo.org/wiki/Lenovo_ThinkBook_15_G2_ITL | group: Gentoo Wiki (Main) | wiki-title: Lenovo ThinkBook 15 G2 ITL -->
---
title: Lenovo ThinkBook 15 G2 ITL
url: https://wiki.gentoo.org/wiki/Lenovo_ThinkBook_15_G2_ITL
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2024-04-28"
fingerprint: "7f109d45ffa23169"
license: CC BY-SA 4.0
---

# Lenovo ThinkBook 15 G2 ITL

From Gentoo Wiki

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**Resources**

## Hardware

### Standard

| Device | Make/model | Status | Vendor ID / Product ID | Kernel driver(s) | Kernel version | Notes | 
|---|---|---|---|---|---|---|
| CPU | N/A |  | N/A | N/A | 5.17.0 |  | 
| GPU | Intel Corporation TigerLake-LP GT2 \[Iris Xe Graphics\] |  | N/A | i915 | 5.17.0 |  | 
| Sound | Intel Corporation Tiger Lake-LP Smart Sound Technology Audio Controller |  | N/A | snd\_hda\_intel snd\_sof\_pci\_intel\_tgl | 5.17.0 | Requires configuration. | 
| Wi-Fi | N/A |  | N/A | iwlwifi | 5.17.0 |  | 
| Fingerprint scanner | Elan Microelectronics Corp. ELAN:Fingerprint |  | 04f3:0c4b | N/A | 5.17.0 | No driver available. | 

`root #``lspci -k`
0000:00:00.0 Host bridge: Intel Corporation 11th Gen Core Processor Host Bridge/DRAM Registers (rev 01)
	Subsystem: Lenovo 11th Gen Core Processor Host Bridge/DRAM Registers
0000:00:02.0 VGA compatible controller: Intel Corporation TigerLake-LP GT2 \[Iris Xe Graphics\] (rev 01)
	Subsystem: Lenovo TigerLake-LP GT2 \[Iris Xe Graphics\]
	Kernel driver in use: i915
	Kernel modules: i915
0000:00:04.0 Signal processing controller: Intel Corporation TigerLake-LP Dynamic Tuning Processor Participant (rev 01)
	Subsystem: Lenovo TigerLake-LP Dynamic Tuning Processor Participant
	Kernel driver in use: proc\_thermal
	Kernel modules: processor\_thermal\_device\_pci\_legacy
0000:00:06.0 System peripheral: Intel Corporation Device 09ab
	Subsystem: COMPAL Electronics Inc Device 012d
0000:00:07.0 PCI bridge: Intel Corporation Tiger Lake-LP Thunderbolt 4 PCI Express Root Port #0 (rev 01)
	Kernel driver in use: pcieport
0000:00:0d.0 USB controller: Intel Corporation Tiger Lake-LP Thunderbolt 4 USB Controller (rev 01)
	Subsystem: Intel Corporation Tiger Lake-LP Thunderbolt 4 USB Controller
	Kernel driver in use: xhci\_hcd
	Kernel modules: xhci\_pci
0000:00:0d.2 USB controller: Intel Corporation Tiger Lake-LP Thunderbolt 4 NHI #0 (rev 01)
	Subsystem: Device 2222:1111
	Kernel driver in use: thunderbolt
	Kernel modules: thunderbolt
0000:00:0e.0 RAID bus controller: Intel Corporation Volume Management Device NVMe RAID Controller
	Subsystem: COMPAL Electronics Inc Volume Management Device NVMe RAID Controller
	Kernel driver in use: vmd
	Kernel modules: vmd
0000:00:14.0 USB controller: Intel Corporation Tiger Lake-LP USB 3.2 Gen 2x1 xHCI Host Controller (rev 20)
	Subsystem: Lenovo Tiger Lake-LP USB 3.2 Gen 2x1 xHCI Host Controller
	Kernel driver in use: xhci\_hcd
	Kernel modules: xhci\_pci
0000:00:14.2 RAM memory: Intel Corporation Tiger Lake-LP Shared SRAM (rev 20)
	Subsystem: Lenovo Tiger Lake-LP Shared SRAM
0000:00:14.3 Network controller: Intel Corporation Wi-Fi 6 AX201 (rev 20)
	Subsystem: Intel Corporation Wi-Fi 6 AX201
	Kernel driver in use: iwlwifi
	Kernel modules: iwlwifi
0000:00:15.0 Serial bus controller: Intel Corporation Tiger Lake-LP Serial IO I2C Controller #0 (rev 20)
	Subsystem: Lenovo Tiger Lake-LP Serial IO I2C Controller
	Kernel driver in use: intel-lpss
	Kernel modules: intel\_lpss\_pci
0000:00:15.3 Serial bus controller: Intel Corporation Tiger Lake-LP Serial IO I2C Controller #3 (rev 20)
	Subsystem: Lenovo Tiger Lake-LP Serial IO I2C Controller
	Kernel driver in use: intel-lpss
	Kernel modules: intel\_lpss\_pci
0000:00:16.0 Communication controller: Intel Corporation Tiger Lake-LP Management Engine Interface (rev 20)
	Subsystem: Lenovo Tiger Lake-LP Management Engine Interface
	Kernel driver in use: mei\_me
	Kernel modules: mei\_me
0000:00:17.0 System peripheral: Intel Corporation Device 09ab
	Subsystem: COMPAL Electronics Inc Device 012d
0000:00:1d.0 PCI bridge: Intel Corporation Tiger Lake-LP PCI Express Root Port #9 (rev 20)
	Kernel driver in use: pcieport
0000:00:1d.1 PCI bridge: Intel Corporation Device a0b1 (rev 20)
	Kernel driver in use: pcieport
0000:00:1f.0 ISA bridge: Intel Corporation Tiger Lake-LP LPC Controller (rev 20)
	Subsystem: Lenovo Tiger Lake-LP LPC Controller
0000:00:1f.3 Multimedia audio controller: Intel Corporation Tiger Lake-LP Smart Sound Technology Audio Controller (rev 20)
	Subsystem: Lenovo Tiger Lake-LP Smart Sound Technology Audio Controller
	Kernel driver in use: snd\_hda\_intel
	Kernel modules: snd\_hda\_intel, snd\_sof\_pci\_intel\_tgl
0000:00:1f.4 SMBus: Intel Corporation Tiger Lake-LP SMBus Controller (rev 20)
	Subsystem: Lenovo Tiger Lake-LP SMBus Controller
	Kernel driver in use: i801\_smbus
	Kernel modules: i2c\_i801
0000:00:1f.5 Serial bus controller: Intel Corporation Tiger Lake-LP SPI Controller (rev 20)
	Subsystem: Lenovo Tiger Lake-LP SPI Controller
0000:2b:00.0 Ethernet controller: Realtek Semiconductor Co., Ltd. RTL8111/8168/8411 PCI Express Gigabit Ethernet Controller (rev 15)
	Subsystem: Lenovo RTL8111/8168/8411 PCI Express Gigabit Ethernet Controller
	Kernel driver in use: r8169
0000:2c:00.0 SD Host controller: O2 Micro, Inc. SD/MMC Card Reader Controller (rev 01)
	Subsystem: Lenovo SD/MMC Card Reader Controller
	Kernel driver in use: sdhci-pci
	Kernel modules: sdhci\_pci
10000:e0:06.0 PCI bridge: Intel Corporation 11th Gen Core Processor PCIe Controller (rev 01)
	Kernel driver in use: pcieport
10000:e0:17.0 SATA controller: Intel Corporation Device a0d3 (rev 20)
	Subsystem: Lenovo Device 381e
	Kernel driver in use: ahci
	Kernel modules: ahci
10000:e1:00.0 Non-Volatile memory controller: Samsung Electronics Co Ltd NVMe SSD Controller 980
	Subsystem: Samsung Electronics Co Ltd NVMe SSD Controller 980
	Kernel driver in use: nvme
	Kernel modules: nvme

`root #``lsusb -tv````
/:  Bus 04.Port 1: Dev 1, Class=root_hub, Driver=xhci_hcd/4p, 10000M
    ID 1d6b:0003 Linux Foundation 3.0 root hub
/:  Bus 03.Port 1: Dev 1, Class=root_hub, Driver=xhci_hcd/12p, 480M
    ID 1d6b:0002 Linux Foundation 2.0 root hub
    |__ Port 7: Dev 3, If 0, Class=Video, Driver=, 480M
        ID 13d3:56ff IMC Networks 
    |__ Port 7: Dev 3, If 1, Class=Video, Driver=, 480M
        ID 13d3:56ff IMC Networks 
    |__ Port 9: Dev 4, If 0, Class=Vendor Specific Class, Driver=, 12M
        ID 04f3:0c4b Elan Microelectronics Corp. 
    |__ Port 10: Dev 5, If 0, Class=Wireless, Driver=btusb, 12M
        ID 8087:0aaa Intel Corp. Bluetooth 9460/9560 Jefferson Peak (JfP)
    |__ Port 10: Dev 5, If 1, Class=Wireless, Driver=btusb, 12M
        ID 8087:0aaa Intel Corp. Bluetooth 9460/9560 Jefferson Peak (JfP)
/:  Bus 02.Port 1: Dev 1, Class=root_hub, Driver=xhci_hcd/4p, 10000M
    ID 1d6b:0003 Linux Foundation 3.0 root hub
/:  Bus 01.Port 1: Dev 1, Class=root_hub, Driver=xhci_hcd/1p, 480M
    ID 1d6b:0002 Linux Foundation 2.0 root hub
```
## Installation

### Kernel

Third-party provided [.config (5.17.0)](https://github.com/sergeygalkin/cookbook/blob/master/gentoo/kernel_config/Lenovo_ThinkBook_15_G2_ITL/kernel_5.17.0_config) can be used as a template.

## Configuration

### Sound

FILE **`/etc/modprobe.d/alsa.conf`**

### Xorg

#### External monitors

`root #``xrandr`
Screen 0: minimum 8 x 8, current 1920 x 1080, maximum 32767 x 32767
eDP1 connected primary 1920x1080+0+0 (normal left inverted right x axis y axis) 340mm x 190mm
   1920x1080     60.05\*+
DP1 disconnected (normal left inverted right x axis y axis)
DP2 disconnected (normal left inverted right x axis y axis)
HDMI1 disconnected (normal left inverted right x axis y axis)
VIRTUAL1 disconnected (normal left inverted right x axis y axis)

DP1 and DP2 are Type-C ports.

Example:

`root #``xrandr --output DP2 --mode 3440x1440 --pos -800x-1440  --rotate normal``root #``xrandr --output eDP1 --primary`
