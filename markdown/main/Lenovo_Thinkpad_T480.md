<!-- source: https://wiki.gentoo.org/wiki/Lenovo_Thinkpad_T480 | group: Gentoo Wiki (Main) | wiki-title: Lenovo Thinkpad T480 -->
---
title: Lenovo Thinkpad T480
url: https://wiki.gentoo.org/wiki/Lenovo_Thinkpad_T480
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2025-12-12"
fingerprint: "2f5a8d26f722116b"
license: CC BY-SA 4.0
---

# Lenovo Thinkpad T480

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**Resources**

Lenovo Thinkpad T480 is a business notebook based on 7th/8th generation Intel® Core™ processor.

## Hardware

### Standard

| Device | Make/model | Status | Vendor ID / Product ID | Kernel driver(s) | Kernel version | Notes | 
|---|---|---|---|---|---|---|
| CPU | Intel i5-7200U, i5-7300U, i3-8130U, i5-8250U, i5-8350U, i7-8550U, or i7-8650U |  | N/A | N/A | 6.12.58 |  | 
| RAM | max 64GiB DDR4 2400MHz (2 SO-DIMM) |  | N/A | N/A | 6.12.58 |  | 
| GPU | integrated Intel HD/UHD Graphics 620 |  | N/A | [intel](https://wiki.gentoo.org/wiki/Intel) | 6.12.58 |  | 
| Discrete GPU (optional) | NVIDIA GeForce MX150, 2GB GDDR5 memory |  | N/A | N/A | 6.12.58 | x11-drivers/nvidia-drivers 580.95.05 | 
| Display | 14.0"; 16:9; HD 1366x768 TN 220 nits, FHD 1920 x 1080 IPS 250 nits, or WQHD 2560x1440 IPS 300 nits |  | N/A | N/A | 6.12.58 | multitouch option for FHD | 
| Storage | M.2 PCIe-NVMe, 512GB/1TB |  | N/A | [nvme](https://wiki.gentoo.org/wiki/NVMe) | 6.6.6 | Sandisk Corp WD Black 2018/SN750 / PC SN720 NVMe SSD | 
| Ethernet | Intel Ethernet Connection I219-V |  | N/A | e1000e | 6.12.58 | Intel Corporation Ethernet Connection (4) I219-V (rev 21) | 
| Wi-Fi | Intel Dual Band Wireless-AC 8265, Wi-Fi 2x2 802.11ac |  | N/A | [iwlwifi](https://wiki.gentoo.org/wiki/Iwlwifi) | 6.12.58 | Intel Corporation Dual Band Wireless-AC 8265 \[Windstorm Peak\] | 
| Bluetooth | Intel Bluetooth 4.1 |  | 8087:0a2b | [btusb](https://wiki.gentoo.org/wiki/Bluetooth) | 6.12.58 |  | 
| Mobile Broadband | Intel XMM 7262 (Fibocom L830-EB) or Intel XMM 7360 (Fibocom L850-GL) |  | N/A | N/A | N/A |  | 
| Sound | HD Audio, Realtek ALC3287 codec |  | N/A | snd\_hda\_intel | 6.12.58 |  | 
| Webcam | IMC Networks Integrated Camera |  | 13d3:56a6 | uvcvideo | 6.12.58 | HD720p | 
| Keyboard | Backlit |  | N/A | N/A | 6.12.58 |  | 
| Touchpad | Synaptics TM3276-022 |  | N/A | [libinput](https://wiki.gentoo.org/wiki/Libinput) | 6.12.58 |  | 
| Fingerprint Reader | Synaptics, Inc. Metallica MIS Touch Fingerprint Reader |  | 06cb:009a | N/A | N/A |  | 
| Smartcard Reader | Alcor Micro Corp. AU9540 Smartcard Reader |  | 058f:9540 | sdhci-pci | 6.6.6 |  | 
| SD Card Reader | Realtek Semiconductor Corp. Card Reader |  | 0bda:0316 | usb-storage | 6.12.58 |  | 

CPU and its features:

`root #``lscpu`
Architecture:                       x86\_64
CPU op-mode(s):                     32-bit, 64-bit
Address sizes:                      39 bits physical, 48 bits virtual
Byte Order:                         Little Endian
CPU(s):                             8
On-line CPU(s) list:                0-7
Vendor ID:                          GenuineIntel
Model name:                         Intel(R) Core(TM) i7-8550U CPU @ 1.80GHz
CPU family:                         6
Model:                              142
Thread(s) per core:                 2
Core(s) per socket:                 4
Socket(s):                          1
Stepping:                           10
CPU(s) scaling MHz:                 20%
CPU max MHz:                        4000.0000
CPU min MHz:                        400.0000
BogoMIPS:                           3999.93
Flags:                              fpu vme de pse tsc msr pae mce cx8 apic sep mtrr pge mca cmov pat pse36 clflush dts acpi mmx fxsr sse sse2 ss ht tm pbe syscall nx pdpe1gb rdtscp lm constant\_tsc art arch\_perfmon pebs bts rep\_good nopl xtopology nonstop\_tsc cpuid aperfmperf pni pclmulqdq dtes64 monitor ds\_cpl vmx est tm2 ssse3 sdbg fma cx16 xtpr pdcm pcid sse4\_1 sse4\_2 x2apic movbe popcnt tsc\_deadline\_timer aes xsave avx f16c rdrand lahf\_lm abm 3dnowprefetch cpuid\_fault epb pti ssbd ibrs ibpb stibp tpr\_shadow flexpriority ept vpid ept\_ad fsgsbase tsc\_adjust bmi1 avx2 smep bmi2 erms invpcid mpx rdseed adx smap clflushopt intel\_pt xsaveopt xsavec xgetbv1 xsaves dtherm ida arat pln pts hwp hwp\_notify hwp\_act\_window hwp\_epp vnmi md\_clear flush\_l1d arch\_capabilities

List of PCI devices:

`root #``lspci -k`
00:00.0 Host bridge: Intel Corporation Xeon E3-1200 v6/7th Gen Core Processor Host Bridge/DRAM Registers (rev 08)
	Subsystem: Lenovo ThinkPad T480
	Kernel driver in use: skl\_uncore
00:02.0 VGA compatible controller: Intel Corporation UHD Graphics 620 (rev 07)
	Subsystem: Lenovo UHD Graphics 620
	Kernel driver in use: i915
00:04.0 Signal processing controller: Intel Corporation Xeon E3-1200 v5/E3-1500 v5/6th Gen Core Processor Thermal Subsystem (rev 08)
	Subsystem: Lenovo ThinkPad T480
	Kernel driver in use: proc\_thermal
	Kernel modules: processor\_thermal\_device\_pci\_legacy
00:08.0 System peripheral: Intel Corporation Xeon E3-1200 v5/v6 / E3-1500 v5 / 6th/7th/8th Gen Core Processor Gaussian Mixture Model
	Subsystem: Lenovo ThinkPad T480
00:14.0 USB controller: Intel Corporation Sunrise Point-LP USB 3.0 xHCI Controller (rev 21)
	Subsystem: Lenovo ThinkPad T480
	Kernel driver in use: xhci\_hcd
	Kernel modules: xhci\_pci
00:14.2 Signal processing controller: Intel Corporation Sunrise Point-LP Thermal subsystem (rev 21)
	Subsystem: Lenovo ThinkPad T480
	Kernel driver in use: intel\_pch\_thermal
	Kernel modules: intel\_pch\_thermal
00:15.0 Signal processing controller: Intel Corporation Sunrise Point-LP Serial IO I2C Controller #0 (rev 21)
	Subsystem: Lenovo ThinkPad T480
00:16.0 Communication controller: Intel Corporation Sunrise Point-LP CSME HECI #1 (rev 21)
	Subsystem: Lenovo ThinkPad T480
00:1c.0 PCI bridge: Intel Corporation Sunrise Point-LP PCI Express Root Port #1 (rev f1)
	Subsystem: Lenovo Sunrise Point-LP PCI Express Root Port
	Kernel driver in use: pcieport
00:1c.6 PCI bridge: Intel Corporation Sunrise Point-LP PCI Express Root Port #7 (rev f1)
	Subsystem: Lenovo Sunrise Point-LP PCI Express Root Port
	Kernel driver in use: pcieport
00:1d.0 PCI bridge: Intel Corporation Sunrise Point-LP PCI Express Root Port #9 (rev f1)
	Subsystem: Lenovo Sunrise Point-LP PCI Express Root Port
	Kernel driver in use: pcieport
00:1d.2 PCI bridge: Intel Corporation Sunrise Point-LP PCI Express Root Port #11 (rev f1)
	Subsystem: Lenovo Sunrise Point-LP PCI Express Root Port
	Kernel driver in use: pcieport
00:1f.0 ISA bridge: Intel Corporation Sunrise Point LPC/eSPI Controller (rev 21)
	Subsystem: Lenovo ThinkPad T480
00:1f.2 Memory controller: Intel Corporation Sunrise Point-LP PMC (rev 21)
	Subsystem: Lenovo ThinkPad T480
00:1f.3 Audio device: Intel Corporation Sunrise Point-LP HD Audio (rev 21)
	Subsystem: Lenovo ThinkPad T480
	Kernel driver in use: snd\_hda\_intel
00:1f.4 SMBus: Intel Corporation Sunrise Point-LP SMBus (rev 21)
	Subsystem: Lenovo ThinkPad T480
	Kernel driver in use: i801\_smbus
00:1f.6 Ethernet controller: Intel Corporation Ethernet Connection (4) I219-V (rev 21)
	Subsystem: Lenovo ThinkPad T480
	Kernel driver in use: e1000e
03:00.0 Network controller: Intel Corporation Wireless 8265 / 8275 (rev 78)
	Subsystem: Intel Corporation Dual Band Wireless-AC 8265 \[Windstorm Peak\]
	Kernel driver in use: iwlwifi
	Kernel modules: iwlwifi
04:00.0 PCI bridge: Intel Corporation JHL6240 Thunderbolt 3 Bridge (Low Power) \[Alpine Ridge LP 2016\] (rev 01)
	Subsystem: Device 2222:1111
	Kernel driver in use: pcieport
05:00.0 PCI bridge: Intel Corporation JHL6240 Thunderbolt 3 Bridge (Low Power) \[Alpine Ridge LP 2016\] (rev 01)
	Subsystem: Device 2222:1111
	Kernel driver in use: pcieport
05:01.0 PCI bridge: Intel Corporation JHL6240 Thunderbolt 3 Bridge (Low Power) \[Alpine Ridge LP 2016\] (rev 01)
	Subsystem: Device 2222:1111
	Kernel driver in use: pcieport
05:02.0 PCI bridge: Intel Corporation JHL6240 Thunderbolt 3 Bridge (Low Power) \[Alpine Ridge LP 2016\] (rev 01)
	Subsystem: Device 2222:1111
	Kernel driver in use: pcieport
06:00.0 System peripheral: Intel Corporation JHL6240 Thunderbolt 3 NHI (Low Power) \[Alpine Ridge LP 2016\] (rev 01)
	Subsystem: Device 2222:1111
3c:00.0 USB controller: Intel Corporation JHL6240 Thunderbolt 3 USB 3.1 Controller (Low Power) \[Alpine Ridge LP 2016\] (rev 01)
	Subsystem: Device 2222:1111
	Kernel driver in use: xhci\_hcd
	Kernel modules: xhci\_pci
3d:00.0 Non-Volatile memory controller: Sandisk Corp SanDisk Extreme Pro / WD Black 2018/SN750/PC SN720 NVMe SSD
	Subsystem: Sandisk Corp WD Black 2018/SN750 / PC SN720 NVMe SSD
	Kernel driver in use: nvme

List of USB devices:

`root #``lsusb`
Bus 004 Device 001: ID 1d6b:0003 Linux Foundation 3.0 root hub
Bus 003 Device 001: ID 1d6b:0002 Linux Foundation 2.0 root hub
Bus 002 Device 002: ID 0bda:0316 Realtek Semiconductor Corp. Card Reader
Bus 002 Device 001: ID 1d6b:0003 Linux Foundation 3.0 root hub
Bus 001 Device 005: ID 06cb:009a Synaptics, Inc. Metallica MIS Touch Fingerprint Reader
Bus 001 Device 004: ID 13d3:56a6 IMC Networks Integrated Camera
Bus 001 Device 003: ID 8087:0a2b Intel Corp. Bluetooth wireless interface
Bus 001 Device 002: ID 058f:9540 Alcor Micro Corp. AU9540 Smartcard Reader
Bus 001 Device 001: ID 1d6b:0002 Linux Foundation 2.0 root hub

List the state of both batteries:

`root #``upower -i /org/freedesktop/UPower/devices/battery_BAT0``root #``upower -i /org/freedesktop/UPower/devices/battery_BAT1`
## Installation

### Firmware

`root #``emerge --ask sys-firmware/intel-microcode`
In order for the SoC and wireless to work properly, it is necessary to install the proper firmware files:

`root #``emerge --ask sys-kernel/linux-firmware`
### Kernel

#### Processor

**Kernel 6.6.6 (gentoo-sources)**

Using [sys-kernel/gentoo-sources](https://packages.gentoo.org/packages/sys-kernel/gentoo-sources) with the [experimental](https://packages.gentoo.org/useflags/experimental) [USE flag will have additional Processor family options made available:](https://wiki.gentoo.org/wiki/USE_flag)

**Kernel 6.6.6 (gentoo-sources)**

or simply autodetecting the processor options by the compiler:

**Kernel 6.6.6 (gentoo-sources)**

#### Drivers

**Kernel 6.6.6 (gentoo-sources)**

## Configuration

### Portage

**`/etc/portage/make.conf`**

```
# Intel(R) Core(TM) i7-8550U CPU @ 1.80GHz
MAKEOPTS="-j8"
CPU_FLAGS_X86="aes avx avx2 f16c fma3 mmx mmxext pclmul popcnt sse sse2 sse3 sse4_1 sse4_2 sse4a ssse3"
# drivers
#
INPUT_DEVICES="libinput"
VIDEO_CARDS="intel"
```
### libinput

**`/etc/X11/xorg.conf.d/40-libinput.conf`**

**Configure touchpad**

```
Section "InputClass"
     Identifier "libinput touchpad catchall"
     MatchIsTouchpad "on"
     MatchDevicePath "/dev/input/event*"
     Option "Tapping" "True" # Touchpad tapping enabled
     Option "TappingDrag" "False" # Never drag                                                                         
     Option "TransformationMatrix" "0.96 0 0 0 0.96 0 0 0 1" # Slight accel
     Driver "libinput"
EndSection
```
### Display backlight

To control the display backlight install [sys-power/acpilight](https://packages.gentoo.org/packages/sys-power/acpilight):

`root #``emerge --ask sys-power/acpilight`
Make the desired users a part of the `video` group to set the display brightness.

### Keyboard backlight

The keyboard backlight is working out of the box (with a keyboard actually supporting the backlight), and can be controlled by using the `Fn`+`Space` keys without any further adjustments. There are 3 predefined steps of the keyboard backlight. `0`, `50` and `100`.

To display the current setting of keyboard backlight use the `-ctrl tpacpi::kbd_backlight` command line option for xbacklight.

To show the available adjustment steps use following command, it will show `3` steps:

`user $``xbacklight -ctrl tpacpi::kbd_backlight -get-steps`
To display the current setting use following command:

`user $``xbacklight -ctrl tpacpi::kbd_backlight -getf`
## Troubleshooting

### Processor throttles under load

Lenovo Thinkpad T480 is one of the devices hit by CPU package power limit bug. The bug causes the affected processors to throttle its frequency down to the base line (\~1.8GHz) during a multi-core load (e.g. kernel compilation).[\[1\]](https://wiki.gentoo.org#cite_note-1)

There is a simple utility correcting the power limit configuration - [sys-power/throttled](https://packages.gentoo.org/packages/sys-power/throttled):

`root #``emerge --ask sys-power/throttled`
