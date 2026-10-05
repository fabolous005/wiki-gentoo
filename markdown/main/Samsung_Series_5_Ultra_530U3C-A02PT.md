<!-- source: https://wiki.gentoo.org/wiki/Samsung_Series_5_Ultra_530U3C-A02PT | group: Gentoo Wiki (Main) | wiki-title: Samsung Series 5 Ultra 530U3C-A02PT -->
---
title: Samsung Series 5 Ultra 530U3C-A02PT
url: https://wiki.gentoo.org/wiki/Samsung_Series_5_Ultra_530U3C-A02PT
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2024-12-20"
fingerprint: "7f1e3f1dc3ba7cea"
license: CC BY-SA 4.0
---

# Samsung Series 5 Ultra 530U3C-A02PT

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)


This is an article about running Gentoo on a **Samsung Series 5 Ultra 530U3C-A02PT**.

- Product page (in Portuguese): [https://www.samsung.com/pt/consumer/monitor-peripherals-printer/notebook/ultra-portable/NP530U3C-A02PT](https://www.samsung.com/pt/consumer/monitor-peripherals-printer/notebook/ultra-portable/NP530U3C-A02PT)

## Preamble / Special Requirements

The current setup was designed for a working laptop with a focus on performance ~~and security~~.
It features:

- Optimizations for a Solid State Disk (SSD)
- ~~Full disk encryption~~ *This requirement was dropped due to a lack of performance and increased complexity. May be enabled later by using Full Disk Encryption (dm-crypt/luks) or encrypted home.*

## Hardware

### Laptop Specifications

| Hardware Summary and Support Status |  |  |  |  | 
|---|---|---|---|---|
| Type | Device | Model | Support | Driver | 
|---|---|---|---|---|
| Processor | Processor | [Intel Core i5-3317U CPU @ 1.70GHz](https://ark.intel.com/products/65707) | Full | acpi-cpufreq | 
|  | Power Management | ACPI | Full | i2c-i801 | 
|  | PCI Express Bus | 7 Series/C210 Series Chipset Family SMBus Controller | Full | pcieport | 
| Storage | Hard Disk | 7 Series Chipset Family 6-port SATA Controller \[AHCI mode\] | Full | ahci | 
| Video Chipset | Discrete GPU | 3rd Gen Core processor Graphics Controller | Full | i915 | 
| Input | Keyboard | - | Full | evdev | 
|  | Touchpad | - | Partial | synaptics | 
| Network | Gigabit Ethernet | RTL8111/8168B PCI Express Gigabit Ethernet controller | Full | r8169 | 
|  | 802.11n Wifi | Centrino Advanced-N 6235 | Full | iwlwifi | 
| Sound | HD Audio | 7 Series/C210 Series Chipset Family High Definition Audio Controller | Full | snd\_hda\_intel | 
| Peripheral | USB 3.0 | 7 Series/C210 Series Chipset Family USB xHCI Host Controller | Full | xhci | 
|  | USB 2.0 | 7 Series/C210 Series Chipset Family USB Enhanced Host Controller | Full | ehci\_hcd | 
|  | Webcam | WebCam SC-13HDL12131N | Full | uvcvideo | 
|  | Brightness Sensor | - | ? |  | 
|  | Info LEDs | - | ? |  | 

### SSD Specifications

The SSD is a 120GB Intel® Solid-State Drive 330 Series.

- Capacity: 120 GB
- Components:
  - Intel® 25nm NAND Flash Memory
  - Multi-Level Cell (MLC)
- Form Factor: 2.5-inch
- Thickness: 9.5 mm
- Weight: Up to 80 grams
- SATA 6Gb/s Bandwidth Performance (Iometer Queue Depth 32)
  - **Sustained Sequential Read: 500 MB/s**
  - **Sustained Sequential Write: 450 MB/**
- Read and Write IOPS1(Iometer Queue Depth 32)
  - Random 4 KB Reads: 42,000 IOPS
  - Random 4 KB Writes: 52,000 IOPS
- Compatibility
  - Intel® SSD Toolbox with Intel® SSD Optimizer
  - Intel® Data Migration Software
  - Intel® Rapid Storage Technology
  - Intel® 6 Series Express Chipsets (with SATA 6Gb/s)
  - SATA Revision 3.0
  - ACS-2 (ATA/ATAPI Command Set 2)
  - Limited SMART ATA feature set
  - Native Command Queuing (NCQ) command set
  - Data Set Management Command **Trim** attribute
- Power Management
  - 5 V SATA Supply Rail— SATA Link Power Management (LPM)
- Power
  - Active (MobileMark\* 2007 Workload): 850 mW (TYP)
  - Idle: 600 mW (TYP)
- Temperature
  - Operating: 0o C to 70o C
  - Non-Operating: -55o C to 95o C

### cpuinfo

`root #``cat /proc/cpuinfo`
processor       : 0
vendor\_id       : GenuineIntel
cpu family      : 6
model           : 58
model name      : Intel(R) Core(TM) i5-3317U CPU @ 1.70GHz
stepping        : 9
microcode       : 0x12
cpu MHz         : 1701.000
cache size      : 3072 KB
physical id     : 0
siblings        : 4
core id         : 0
cpu cores       : 2
apicid          : 0
initial apicid  : 0
fpu             : yes
fpu\_exception   : yes
cpuid level     : 13
wp              : yes
flags           : fpu vme de pse tsc msr pae mce cx8 apic sep mtrr pge mca cmov pat pse36 clflush dts acpi mmx fxsr sse sse2 ss ht tm pbe syscall nx rdtscp lm constant\_tsc arch\_perfmon pebs bts rep\_good nopl xtopology nonstop\_tsc aperfmperf pni pclmulqdq dtes64 monitor ds\_cpl vmx est tm2 ssse3 cx16 xtpr pdcm pcid sse4\_1 sse4\_2 x2apic popcnt tsc\_deadline\_timer aes xsave avx f16c rdrand lahf\_lm ida arat epb xsaveopt pln pts dtherm tpr\_shadow vnmi flexpriority ept vpid fsgsbase smep erms
bogomips        : 3393.62
clflush size    : 64
cache\_alignment : 64
address sizes   : 36 bits physical, 48 bits virtual
power management:

### lspci

`root #``lspci -nn`
00:00.0 Host bridge \[0600\]: Intel Corporation 3rd Gen Core processor DRAM Controller \[8086:0154\] (rev 09)
00:02.0 VGA compatible controller \[0300\]: Intel Corporation 3rd Gen Core processor Graphics Controller \[8086:0166\] (rev 09)
00:14.0 USB controller \[0c03\]: Intel Corporation 7 Series/C210 Series Chipset Family USB xHCI Host Controller \[8086:1e31\] (rev 04)
00:16.0 Communication controller \[0780\]: Intel Corporation 7 Series/C210 Series Chipset Family MEI Controller #1 \[8086:1e3a\] (rev 04)
00:1a.0 USB controller \[0c03\]: Intel Corporation 7 Series/C210 Series Chipset Family USB Enhanced Host Controller #2 \[8086:1e2d\] (rev 04)
00:1b.0 Audio device \[0403\]: Intel Corporation 7 Series/C210 Series Chipset Family High Definition Audio Controller \[8086:1e20\] (rev 04)
00:1c.0 PCI bridge \[0604\]: Intel Corporation 7 Series/C210 Series Chipset Family PCI Express Root Port 1 \[8086:1e10\] (rev c4)
00:1c.3 PCI bridge \[0604\]: Intel Corporation 7 Series/C210 Series Chipset Family PCI Express Root Port 4 \[8086:1e16\] (rev c4)
00:1d.0 USB controller \[0c03\]: Intel Corporation 7 Series/C210 Series Chipset Family USB Enhanced Host Controller #1 \[8086:1e26\] (rev 04)
00:1f.0 ISA bridge \[0601\]: Intel Corporation HM76 Express Chipset LPC Controller \[8086:1e59\] (rev 04)
00:1f.2 SATA controller \[0106\]: Intel Corporation 7 Series Chipset Family 6-port SATA Controller \[AHCI mode\] \[8086:1e03\] (rev 04)
00:1f.3 SMBus \[0c05\]: Intel Corporation 7 Series/C210 Series Chipset Family SMBus Controller \[8086:1e22\] (rev 04)
01:00.0 Network controller \[0280\]: Intel Corporation Centrino Advanced-N 6235 \[8086:088e\] (rev 24)
02:00.0 Ethernet controller \[0200\]: Realtek Semiconductor Co., Ltd. RTL8111/8168B PCI Express Gigabit Ethernet controller \[10ec:8168\] (rev 06)

### lsusb

`root #``lsusb`
Bus 001 Device 002: ID 8087:0024 Intel Corp. Integrated Rate Matching Hub
Bus 002 Device 002: ID 8087:0024 Intel Corp. Integrated Rate Matching Hub
Bus 003 Device 003: ID 15ca:00c3 Textech International Ltd. Mini Optical Mouse
Bus 003 Device 002: ID 152d:2329 JMicron Technology Corp. / JMicron USA Technology Corp. JM20329 SATA Bridge
Bus 001 Device 001: ID 1d6b:0002 Linux Foundation 2.0 root hub
Bus 002 Device 001: ID 1d6b:0002 Linux Foundation 2.0 root hub
Bus 003 Device 001: ID 1d6b:0002 Linux Foundation 2.0 root hub
Bus 004 Device 001: ID 1d6b:0003 Linux Foundation 3.0 root hub
Bus 001 Device 003: ID 2232:1035  
Bus 002 Device 003: ID 8087:07da Intel Corp

## Installing Gentoo

### Booting the ~~CD~~ USB

This laptop doesn't include a CDROM drive so you must either install a LiveCD into a USB thumb drive (LiveUSB) or borrow an external CDROM drive. If you opt to use a USB stick [SystemRescueCD has a guide about creating a LiveUSB](https://www.system-rescue.org/Installing-SystemRescue-on-a-USB-memory-stick/) .

Now plug your LiveUSB, change BIOS boot order by pressing F2 and follow the instructions in the [Gentoo Linux AMD64 Handbook](https://wiki.gentoo.org/wiki/Handbook:AMD64). Come back to this guide when configuring the kernel or adjusting `make.conf`.

### Partitioning

Some remarks before launching GParted:

- I've replaced the original Hitachi 500GB HDD with a 120GB SSD.
- This laptop is bundled with an internal 24GB SanDisk SSD **ExpressCache**. ExpressCache is a software layer that caches HDD reads/writes with a small SSD boosting the OS (Windows 7) performance.
- I'll be using the HDD connected via USB when emerging packages because that's where `/var/tmp/portage` and `/usr/portage/distfiles` will reside.

#### Partition Alignment for SSD

You may already notice all the fuss around \[how important is to [properly align SSD partitions](http://www.velobit.com/storage-performance-blog/bid/120529/Partition-Alignment-Key-to-SSD-Performance). To achieve this the simply answer is: use a tool like GParted and select [MiB alignment](https://gparted.sourceforge.net/display-doc.php?name=help-manual#gparted-specify-partition-alignment) to start and end on precise mebibyte (1,048,576 byte) boundaries. Yan Li's [Easy way to align Linux partitions for SSD and myths debunked](https://elliotli.blogspot.pt/2010/12/easy-way-to-align-linux-partitions-for.html) corroborates the GParted statement.

#### Partitioning Scheme

Using the setup above we now have the following disk identifiers.

- **sda** - Intel 120GB SSD
- **sdb** - SanDisk 24GB SSD (ExpressCache)
- **sdc** - Hitachi 500GB HDD

| Disk | Partition | Size | Starts at | Description | Mount point | 
|---|---|---|---|---|---|
| **sda** | sda1 | 128 Mib | 1 MiB | Boot partition | /boot | 
|  | sda2 | ? GiB | ? MiB | Linux root partition | / | 
|  | sda3 | 40.00 GiB | ? MiB | Windows 7 (original install) | /mnt/windows | 
| **sdb** | sdb1 | ? GiB | ? GiB | Used by Diskeeper's ExpressCache |  | 
|  | sdb2 | ? GiB | ? MiB | Used by Intel Rapid Start Technology to allow Win7 to save its state |  | 
|  | sdb3 | 6.1 GiB | ? GiB | Linux swap. I've used the same size as RAM (plus 100MiB) to allow **hibernation**. |  | 

### Filesystems and fstab

**`/etc/fstab`**

**Static information about the filesystems**

### Kernel configuration

First install kernel sources of your liking. I'm using reliable `gentoo-sources`.

`root #``emerge gentoo-sources`
### make.conf

**`/etc/portage/make.conf`**

**Custom settings for Portage**

```
# Please consult /usr/share/portage/config/make.conf.example for a more
# detailed example.
CFLAGS="-march=native -O2 -pipe"
CXXFLAGS="${CFLAGS}"
MAKEOPTS="-j4"
```
**`/etc/portage/package.use/00localization`**

```
 LINGUAS: py
```
**`/etc/portage/package.use/00video`**

```
 VIDEO_CARDS: -* i915 intel vesa fbdev
```
**`/etc/portage/package.use/00input`**

```
 INPUT_DEVICES: evdev synaptics keyboard mouse
```
