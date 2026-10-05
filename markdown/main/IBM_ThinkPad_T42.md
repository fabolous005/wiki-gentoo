<!-- source: https://wiki.gentoo.org/wiki/IBM_ThinkPad_T42 | group: Gentoo Wiki (Main) | wiki-title: IBM ThinkPad T42 -->
---
title: IBM ThinkPad T42
url: https://wiki.gentoo.org/wiki/IBM_ThinkPad_T42
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2024-12-20"
fingerprint: "6f4b9c057fa2126f"
license: CC BY-SA 4.0
---

# IBM ThinkPad T42

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

It is an old IBM ThinkPad T42, at the year of writing of this article it is 12 years old. It's a good piece of hardware, good quality, great keyboard. Overall a very good laptop. Nowadays it is working as a metronome, sequencer and small synthesizer.

## Hardware

Hardware list with corresponding kernel modules and its names:

`root #``lspci -k````
0:00.0 Host bridge: Intel Corporation 82855PM Processor to I/O Controller (rev 03)
        Subsystem: IBM Thinkpad T40 series
00:01.0 PCI bridge: Intel Corporation 82855PM Processor to AGP Controller (rev 03)
00:1d.0 USB controller: Intel Corporation 82801DB/DBL/DBM (ICH4/ICH4-L/ICH4-M) USB UHCI Controller #1 (rev 01)
        Subsystem: IBM ThinkPad
        Kernel driver in use: uhci_hcd
00:1d.1 USB controller: Intel Corporation 82801DB/DBL/DBM (ICH4/ICH4-L/ICH4-M) USB UHCI Controller #2 (rev 01)
        Subsystem: IBM ThinkPad
        Kernel driver in use: uhci_hcd
00:1d.2 USB controller: Intel Corporation 82801DB/DBL/DBM (ICH4/ICH4-L/ICH4-M) USB UHCI Controller #3 (rev 01)
        Subsystem: IBM ThinkPad
        Kernel driver in use: uhci_hcd
00:1d.7 USB controller: Intel Corporation 82801DB/DBM (ICH4/ICH4-M) USB2 EHCI Controller (rev 01)
        Subsystem: IBM ThinkPad
        Kernel driver in use: ehci-pci
00:1e.0 PCI bridge: Intel Corporation 82801 Mobile PCI Bridge (rev 81)
00:1f.0 ISA bridge: Intel Corporation 82801DBM (ICH4-M) LPC Interface Bridge (rev 01)
00:1f.1 IDE interface: Intel Corporation 82801DBM (ICH4-M) IDE Controller (rev 01)
        Subsystem: IBM ThinkPad
        Kernel driver in use: ata_piix
00:1f.3 SMBus: Intel Corporation 82801DB/DBL/DBM (ICH4/ICH4-L/ICH4-M) SMBus Controller (rev 01)
        Subsystem: IBM ThinkPad
        Kernel driver in use: i801_smbus
00:1f.5 Multimedia audio controller: Intel Corporation 82801DB/DBL/DBM (ICH4/ICH4-L/ICH4-M) AC'97 Audio Controller (rev 01)
        Subsystem: IBM ThinkPad T4x Series
        Kernel driver in use: snd_intel8x0
00:1f.6 Modem: Intel Corporation 82801DB/DBL/DBM (ICH4/ICH4-L/ICH4-M) AC'97 Modem Controller (rev 01)
        Subsystem: IBM ThinkPad R50e
        Kernel driver in use: snd_intel8x0m
01:00.0 VGA compatible controller: Advanced Micro Devices, Inc. [AMD/ATI] RV200/M7 [Mobility Radeon 7500]
        Subsystem: IBM ThinkPad T4x Series
        Kernel driver in use: radeonfb
02:00.0 CardBus bridge: Texas Instruments PCI4520 PC card Cardbus Controller (rev 01)
        Subsystem: IBM ThinkPad
        Kernel driver in use: yenta_cardbus
02:00.1 CardBus bridge: Texas Instruments PCI4520 PC card Cardbus Controller (rev 01)
        Subsystem: IBM ThinkPad
        Kernel driver in use: yenta_cardbus
02:01.0 Ethernet controller: Intel Corporation 82540EP Gigabit Ethernet Controller (Mobile) (rev 03)
        Subsystem: IBM Thinkpad
        Kernel driver in use: e1000
02:02.0 Network controller: Intel Corporation PRO/Wireless 2915ABG [Calexico2] Network Connection (rev 05)
        Subsystem: Intel Corporation PRO/Wireless 2915ABG [Calexico2] Network Connection
        Kernel driver in use: ipw2200
        Kernel modules: ipw2200
```
## CPU

`user $``cat /proc/cpuinfo`
processor       : 0
vendor\_id       : GenuineIntel
cpu family      : 6
model           : 13
model name      : Intel(R) Pentium(R) M processor 1.70GHz
stepping        : 6
microcode       : 0x18
cpu MHz         : 1700.000
cache size      : 2048 KB
physical id     : 0
siblings        : 1
core id         : 0
cpu cores       : 1
apicid          : 0
initial apicid  : 0
fdiv\_bug        : no
f00f\_bug        : no
coma\_bug        : no
fpu             : yes
fpu\_exception   : yes
cpuid level     : 2
wp              : yes
flags           : fpu vme de pse tsc msr mce cx8 sep mtrr pge mca cmov clflush dts acpi mmx fxsr sse sse2 ss tm pbe bts est tm2
bugs            :
bogomips        : 3388.71
clflush size    : 64
cache\_alignment : 64
address sizes   : 32 bits physical, 32 bits virtual
power management:

`user $``lscpu`
Architecture:          i686
CPU op-mode(s):        32-bit
Byte Order:            Little Endian
CPU(s):                1
On-line CPU(s) list:   0
Thread(s) per core:    1
Core(s) per socket:    1
Socket(s):             1
Vendor ID:             GenuineIntel
CPU family:            6
Model:                 13
Model name:            Intel(R) Pentium(R) M processor 1.70GHz
Stepping:              6
CPU MHz:               1700.000
CPU max MHz:           1700.0000
CPU min MHz:           600.0000
BogoMIPS:              3388.71

## Synaptics

**Synaptics PS/2 mouse protocol extension**

### Emerge

`root #``emerge --ask x11-drivers/xf86-input-synaptics`
### Configuration

**`/etc/X11/xorg.conf.d/50-synaptics.conf`**

**Synaptics enhanced configuration**

## Wired NIC

**Intel(R) PRO/1000 Gigabit Ethernet support**

## Wireless NIC

Needed to be build as a module (`<M>`).

**Intel PRO/Wireless 2200BG and 2915ABG Network Connection**

### Emerge

Wireless NIC kernel firmware:

`root #``emerge --ask sys-firmware/ipw2200-firmware`
## Configuration

### package.use

**`/etc/portage/make.conf`**

```
CHOST="i686-pc-linux-gnu"
CFLAGS="-O2 -march=pentium-m -pipe -fomit-frame-pointer"
CXXFLAGS="${CFLAGS}"
MAKEOPTS="-j2"
```
**`/etc/portage/package.use/00video`**

```
 VIDEO_CARDS: -* radeon intel i915
```
**`/etc/portage/package.use/00input`**

```
 INPUT_DEVICES: evdev synaptics
```
**`/etc/portage/package.use/00cpu-flags`**

```
 CPU_FLAGS_X86: mmx mmext sse sse2
```
