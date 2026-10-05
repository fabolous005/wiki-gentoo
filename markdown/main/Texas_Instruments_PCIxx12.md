<!-- source: https://wiki.gentoo.org/wiki/Texas_Instruments_PCIxx12 | group: Gentoo Wiki (Main) | wiki-title: Texas Instruments PCIxx12 -->
---
title: Texas Instruments PCIxx12
url: https://wiki.gentoo.org/wiki/Texas_Instruments_PCIxx12
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2024-04-10"
fingerprint: ed12b8ffa5bec46c
license: CC BY-SA 4.0
---

# Texas Instruments PCIxx12

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)





## Hardware detection

`root #``lspci -nnkv````
02:06.0 CardBus bridge [0607]: Texas Instruments PCIxx12 Cardbus Controller [104c:8039]
        Subsystem: Hewlett-Packard Company PCIxx12 Cardbus Controller [103c:30aa]
        Flags: bus master, medium devsel, latency 168, IRQ 18
        Memory at e8100000 (32-bit, non-prefetchable) [size=4K]
        Bus: primary=02, secondary=03, subordinate=06, sec-latency=176
        Memory window 0: ec000000-efffffff (prefetchable)
        Memory window 1: f0000000-f3ffffff
        I/O window 0: 00008000-000080ff
        I/O window 1: 00008400-000084ff
        16-bit legacy interface ports at 0001
        Capabilities: [a0] Power Management version 2
        Kernel driver in use: yenta_cardbus
02:06.1 FireWire (IEEE 1394) [0c00]: Texas Instruments PCIxx12 OHCI Compliant IEEE 1394 Host Controller [104c:803a] (prog-if 10 [OHCI])
        Subsystem: Hewlett-Packard Company PCIxx12 OHCI Compliant IEEE 1394 Host Controller [103c:30aa]
        Flags: bus master, medium devsel, latency 64, IRQ 19
        Memory at e8101000 (32-bit, non-prefetchable) [size=2K]
        Memory at e8104000 (32-bit, non-prefetchable) [size=16K]
        Capabilities: [44] Power Management version 2
        Kernel driver in use: firewire_ohci
        Kernel modules: firewire_ohci
02:06.2 Mass storage controller [0180]: Texas Instruments 5-in-1 Multimedia Card Reader (SD/MMC/MS/MS PRO/xD) [104c:803b]
        Subsystem: Hewlett-Packard Company 5-in-1 Multimedia Card Reader (SD/MMC/MS/MS PRO/xD) [103c:30aa]
        Flags: bus master, medium devsel, latency 64, IRQ 19
        Memory at e8108000 (32-bit, non-prefetchable) [size=4K]
        Capabilities: [44] Power Management version 2
        Kernel driver in use: tifm_7xx1
02:06.3 SD Host controller [0805]: Texas Instruments PCIxx12 SDA Standard Compliant SD Host Controller [104c:803c]
        Subsystem: Hewlett-Packard Company PCIxx12 SDA Standard Compliant SD Host Controller [103c:30aa]
        Flags: bus master, medium devsel, latency 64, IRQ 22
        Memory at e8109000 (32-bit, non-prefetchable) [size=256]
        Capabilities: [80] Power Management version 2
        Kernel driver in use: sdhci-pci
02:06.4 Communication controller [0780]: Texas Instruments PCIxx12 GemCore based SmartCard controller [104c:803d]
        Subsystem: Hewlett-Packard Company nc6310 [103c:30aa]
        Flags: medium devsel, IRQ 10
        Memory at e810a000 (32-bit, non-prefetchable) [size=4K]
        Memory at e810b000 (32-bit, non-prefetchable) [size=4K]
        Capabilities: [44] Power Management version 2
```
`root #``lshw````
  *-core
     *-pci
        *-pci:3
             description: PCI bridge
             product: 82801 Mobile PCI Bridge
             vendor: Intel Corporation
             physical id: 1e
             bus info: pci@0000:00:1e.0
             version: e1
             width: 32 bits
             clock: 33MHz
             capabilities: pci subtractive_decode bus_master cap_list
             resources: ioport:8000(size=4096) memory:e8100000-e83fffff
           *-pcmcia
                description: CardBus bridge
                product: PCIxx12 Cardbus Controller
                vendor: Texas Instruments
                physical id: 6
                bus info: pci@0000:02:06.0
                version: 00
                width: 32 bits
                clock: 33MHz
                capabilities: pcmcia bus_master cap_list
                configuration: driver=yenta_cardbus latency=176 maxlatency=5 mingnt=192
                resources: irq:18 memory:e8100000-e8100fff ioport:8000(size=256) ioport:8400(size=256) memory:ec000000-efffffff memory:f0000000-f3ffffff
           *-firewire
                description: FireWire (IEEE 1394)
                product: PCIxx12 OHCI Compliant IEEE 1394 Host Controller
                vendor: Texas Instruments
                physical id: 6.1
                bus info: pci@0000:02:06.1
                version: 00
                width: 32 bits
                clock: 33MHz
                capabilities: pm ohci bus_master cap_list
                configuration: driver=firewire_ohci latency=64 maxlatency=4 mingnt=2
                resources: irq:19 memory:e8101000-e81017ff memory:e8104000-e8107fff
           *-storage
                description: Mass storage controller
                product: 5-in-1 Multimedia Card Reader (SD/MMC/MS/MS PRO/xD)
                vendor: Texas Instruments
                physical id: 6.2
                bus info: pci@0000:02:06.2
                version: 00
                width: 32 bits
                clock: 33MHz
                capabilities: storage pm bus_master cap_list
                configuration: driver=tifm_7xx1 latency=64 maxlatency=4 mingnt=7
                resources: irq:19 memory:e8108000-e8108fff
           *-generic
                description: SD Host controller
                product: PCIxx12 SDA Standard Compliant SD Host Controller
                vendor: Texas Instruments
                physical id: 6.3
                bus info: pci@0000:02:06.3
                version: 00
                width: 32 bits
                clock: 33MHz
                capabilities: pm bus_master cap_list
                configuration: driver=sdhci-pci latency=64 maxlatency=4 mingnt=7
                resources: irq:22 memory:e8109000-e81090ff
           *-communication UNCLAIMED
                description: Communication controller
                product: PCIxx12 GemCore based SmartCard controller
                vendor: Texas Instruments
                physical id: 6.4
                bus info: pci@0000:02:06.4
                version: 00
                width: 32 bits
                clock: 33MHz
                capabilities: pm cap_list
                configuration: latency=0
                resources: memory:e810a000-e810afff memory:e810b000-e810bfff
```
## Device drivers

### pcmcia - PCIxx12 Cardbus Controller

**linux-4.1.8 - PCIxx12 Cardbus Controller**

See also the [PC-Card](https://wiki.gentoo.org/wiki/PC-Card) article and [Handbook: Get PCMCIA working](https://wiki.gentoo.org/wiki/Handbook:Parts/Installation/System#Optional:_Get_PCMCIA_working).

### firewire - PCIxx12 OHCI Compliant IEEE 1394 Host Controller

For kernel config and more details see the [FireWire](https://wiki.gentoo.org/wiki/FireWire) article.

### storage - 5-in-1 Multimedia Card Reader (SD/MMC/MS/MS PRO/xD)

**linux-4.1.8 - 5-in-1 Multimedia Card Reader (SD/MMC/MS/MS PRO/xD)**

### generic - PCIxx12 SDA Standard Compliant SD Host Controller

**linux-4.1.8 - PCIxx12 SDA Standard Compliant SD Host Controller**

## Card detection

Media inserted in the card reader do not go in /dev/disk/ but will be presented as mspblk0p1 in case of [MemoryStick](https://en.wikipedia.org/wiki/Memory_Stick):

`user $` `dmesg`
\[ 4344.728058\] tifm\_core: MemoryStick card detected in socket 0:0
\[ 4344.884195\] memstick0: switching to 4-bit parallel mode
\[ 4344.886722\]  mspblk0: p1

`user $``lsblk -o +fstype,label,uuid,partuuid`
NAME        MAJ:MIN RM   SIZE RO TYPE MOUNTPOINT FSTYPE   LABEL           UUID                                 PARTUUID
mspblk0     254:0    0 958.5M  0 tape                                                                          
└─mspblk0p1 254:1    0   957M  0 part            vfat     1WD67HU8N6F

And as mmcblk0p1 in case of SDHC Card:

`user $` `dmesg`
\[ 5427.236057\] tifm\_core: MMC/SD card detected in socket 0:1
\[ 5427.501725\] mmc1: new SDHC card at address e624
\[ 5427.502529\] mmcblk0: mmc1:e624 SU16G 14.8 GiB 
\[ 5427.509956\]  mmcblk0: p1

`user $``lsblk -o +fstype,label,uuid,partuuid`
NAME        MAJ:MIN RM   SIZE RO TYPE MOUNTPOINT FSTYPE   LABEL           UUID                                 PARTUUID
mmcblk0     179:0    0  14.9G  0 disk                                                                          
└─mmcblk0p1 179:1    0  14.9G  0 part            vfat                     30CB-DBD7

For handling of inserted media, see also the [Removable media](https://wiki.gentoo.org/wiki/Removable_media) article.
