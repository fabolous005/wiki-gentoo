<!-- source: https://wiki.gentoo.org/wiki/Lenovo_ThinkPad_T540p | group: Gentoo Wiki (Main) | wiki-title: Lenovo ThinkPad T540p -->
---
title: Lenovo ThinkPad T540p
url: https://wiki.gentoo.org/wiki/Lenovo_ThinkPad_T540p
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2024-04-28"
fingerprint: "7f529f87fba23069"
license: CC BY-SA 4.0
---

# Lenovo ThinkPad T540p

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**Resources**

## Hardware

`root #``lscpu`
Architecture:          x86\_64
CPU op-mode(s):        32-bit, 64-bit
Byte Order:            Little Endian
CPU(s):                8
On-line CPU(s) list:   0-7
Thread(s) per core:    2
Core(s) per socket:    4
Socket(s):             1
NUMA node(s):          1
Vendor ID:             GenuineIntel
CPU family:            6
Model:                 60
Model name:            Intel(R) Core(TM) i7-4710MQ CPU @ 2.50GHz
Stepping:              3
CPU MHz:               800.292
CPU max MHz:           3500.0000
CPU min MHz:           800.0000
BogoMIPS:              4990.30
Virtualization:        VT-x
L1d cache:             32K
L1i cache:             32K
L2 cache:              256K
L3 cache:              6144K
NUMA node0 CPU(s):     0-7

`root #``lspci -nnk````
00:00.0 Host bridge [0600]: Intel Corporation Xeon E3-1200 v3/4th Gen Core Processor DRAM Controller [8086:0c04] (rev 06)
        Subsystem: Lenovo Xeon E3-1200 v3/4th Gen Core Processor DRAM Controller [17aa:2210]
00:01.0 PCI bridge [0604]: Intel Corporation Xeon E3-1200 v3/4th Gen Core Processor PCI Express x16 Controller [8086:0c01] (rev 06)
        Kernel driver in use: pcieport
00:02.0 VGA compatible controller [0300]: Intel Corporation 4th Gen Core Processor Integrated Graphics Controller [8086:0416] (rev 06)
        Subsystem: Lenovo 4th Gen Core Processor Integrated Graphics Controller [17aa:221e]
        Kernel driver in use: i915
        Kernel modules: i915
00:03.0 Audio device [0403]: Intel Corporation Xeon E3-1200 v3/4th Gen Core Processor HD Audio Controller [8086:0c0c] (rev 06)
        Subsystem: Lenovo Xeon E3-1200 v3/4th Gen Core Processor HD Audio Controller [17aa:2210]
        Kernel driver in use: snd_hda_intel
        Kernel modules: snd_hda_intel
00:14.0 USB controller [0c03]: Intel Corporation 8 Series/C220 Series Chipset Family USB xHCI [8086:8c31] (rev 04)
        Subsystem: Lenovo 8 Series/C220 Series Chipset Family USB xHCI [17aa:2210]
        Kernel driver in use: xhci_hcd
        Kernel modules: xhci_pci
00:16.0 Communication controller [0780]: Intel Corporation 8 Series/C220 Series Chipset Family MEI Controller #1 [8086:8c3a] (rev 04)
        Subsystem: Lenovo 8 Series/C220 Series Chipset Family MEI Controller [17aa:2210]
        Kernel driver in use: mei_me
        Kernel modules: mei_me
00:19.0 Ethernet controller [0200]: Intel Corporation Ethernet Connection I217-LM [8086:153a] (rev 04)
        Subsystem: Lenovo Ethernet Connection I217-LM [17aa:2210]
        Kernel driver in use: e1000e
        Kernel modules: e1000e
00:1b.0 Audio device [0403]: Intel Corporation 8 Series/C220 Series Chipset High Definition Audio Controller [8086:8c20] (rev 04)
        Subsystem: Lenovo 8 Series/C220 Series Chipset High Definition Audio Controller [17aa:2210]
        Kernel driver in use: snd_hda_intel
        Kernel modules: snd_hda_intel
00:1c.0 PCI bridge [0604]: Intel Corporation 8 Series/C220 Series Chipset Family PCI Express Root Port #1 [8086:8c10] (rev d4)
        Kernel driver in use: pcieport
00:1c.1 PCI bridge [0604]: Intel Corporation 8 Series/C220 Series Chipset Family PCI Express Root Port #2 [8086:8c12] (rev d4)
        Kernel driver in use: pcieport
00:1c.2 PCI bridge [0604]: Intel Corporation 8 Series/C220 Series Chipset Family PCI Express Root Port #3 [8086:8c14] (rev d4)
        Kernel driver in use: pcieport
00:1d.0 USB controller [0c03]: Intel Corporation 8 Series/C220 Series Chipset Family USB EHCI #1 [8086:8c26] (rev 04)
        Subsystem: Lenovo 8 Series/C220 Series Chipset Family USB EHCI [17aa:2210]
        Kernel driver in use: ehci-pci
        Kernel modules: ehci_pci
00:1f.0 ISA bridge [0601]: Intel Corporation QM87 Express LPC Controller [8086:8c4f] (rev 04)
        Subsystem: Lenovo QM87 Express LPC Controller [17aa:2210]
        Kernel driver in use: lpc_ich
        Kernel modules: lpc_ich
00:1f.2 SATA controller [0106]: Intel Corporation 8 Series/C220 Series Chipset Family 6-port SATA Controller 1 [AHCI mode] [8086:8c03] (rev 04)
        Subsystem: Lenovo 8 Series/C220 Series Chipset Family 6-port SATA Controller 1 [AHCI mode] [17aa:2210]
        Kernel driver in use: ahci
        Kernel modules: ahci
00:1f.3 SMBus [0c05]: Intel Corporation 8 Series/C220 Series Chipset Family SMBus Controller [8086:8c22] (rev 04)
        Subsystem: Lenovo 8 Series/C220 Series Chipset Family SMBus Controller [17aa:2210]
        Kernel driver in use: i801_smbus
        Kernel modules: i2c_i801
01:00.0 VGA compatible controller [0300]: NVIDIA Corporation GK208M [GeForce GT 730M] [10de:1290] (rev a1)
        Subsystem: Lenovo GK208M [GeForce GT 730M] [17aa:221e]
        Kernel driver in use: nvidia
        Kernel modules: nouveau, nvidia_drm, nvidia
03:00.0 Unassigned class [ff00]: Realtek Semiconductor Co., Ltd. RTS5227 PCI Express Card Reader [10ec:5227] (rev 01)
        Subsystem: Lenovo RTS5227 PCI Express Card Reader [17aa:2210]
        Kernel driver in use: rtsx_pci
        Kernel modules: rtsx_pci
04:00.0 Network controller [0280]: Intel Corporation Wireless 7260 [8086:08b2] (rev 83)
        Subsystem: Intel Corporation Dual Band Wireless-AC 7260 [8086:c270]
        Kernel driver in use: iwlwifi
        Kernel modules: iwlwifi
```
`root #``lsusb`
Bus 002 Device 002: ID 8087:8000 Intel Corp. 
Bus 002 Device 001: ID 1d6b:0002 Linux Foundation 2.0 root hub
Bus 001 Device 002: ID 8087:8008 Intel Corp. 
Bus 001 Device 001: ID 1d6b:0002 Linux Foundation 2.0 root hub
Bus 004 Device 001: ID 1d6b:0003 Linux Foundation 3.0 root hub
Bus 003 Device 003: ID 058f:9540 Alcor Micro Corp. AU9540 Smartcard Reader
Bus 003 Device 005: ID 04f2:b39a Chicony Electronics Co., Ltd 
Bus 003 Device 006: ID 046d:c048 Logitech, Inc. G9 Laser Mouse
Bus 003 Device 001: ID 1d6b:0002 Linux Foundation 2.0 root hub

## Installation

### Firmware

The Wi-Fi module requires external firmware (hardware specific package [sys-firmware/iwl7260-ucode](https://packages.gentoo.org/packages/sys-firmware/iwl7260-ucode) or the general package [sys-kernel/linux-firmware](https://packages.gentoo.org/packages/sys-kernel/linux-firmware)):

`root #``emerge --ask sys-firmware/iwl7260-ucode`
or

`root #``emerge --ask sys-kernel/linux-firmware`
### Kernel

**Wi-Fi**

**TPM**

## Troubleshooting

### Suspend/Resume does not work

A common problem can be encountered if the TPM chip has been activated in UEFI/BIOS. If the kernel lacks the necessary drivers the system freezes when attempting to resume from a suspend. As a solution, enable TPM in the kernel.
