<!-- source: https://wiki.gentoo.org/wiki/Acer_eMachines_eM250 | group: Gentoo Wiki (Main) | wiki-title: Acer eMachines eM250 -->
---
title: Acer eMachines eM250
url: https://wiki.gentoo.org/wiki/Acer_eMachines_eM250
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2024-12-20"
fingerprint: "6f588f27efaaf36b"
license: CC BY-SA 4.0
---

# Acer eMachines eM250

From Gentoo Wiki

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)



![](https://wiki.gentoo.org/images/thumb/0/0f/Acer_eM250.jpg/300px-Acer_eM250.jpg)

## Software

### GCC

`root #``gcc-config -l`
i686-pc-linux-gnu-4.7.2 \*

### Portage

FILE **`/etc/portage/make.conf`**

```
CHOST="i686-pc-linux-gnu"
CFLAGS="-O2 -march=native -fomit-frame-pointer -pipe"
CXXFLAGS="${CFLAGS}"
```
FILE **`/etc/portage/package.use/00input`**

```
 INPUT_DEVICES: vdev synaptic
```
FILE **`/etc/portage/package.use/00video`**

```
 VIDEO_CARDS: -* intel fbdev vesa
```
### Kernel

FILE **`/etc/modules-load.d/drivers.conf`**

#### CPU

KERNEL **Intel Atom N270**

#### Disk

KERNEL **SATA**

#### Video

KERNEL **Graphics**

#### Sound

KERNEL **Sound**

#### Network

KERNEL **Ethernet**

KERNEL **WIFI**

#### Webcam

KERNEL **Webcam**

#### Other

## Hardware

`root #``lspci`
00:00.0 Host bridge: Intel Corporation Mobile 945GSE Express Memory Controller Hub (rev 03)
00:02.0 VGA compatible controller: Intel Corporation Mobile 945GSE Express Integrated Graphics Controller (rev 03)
00:02.1 Display controller: Intel Corporation Mobile 945GM/GMS/GME, 943/940GML Express Integrated Graphics Controller (rev 03)
00:1b.0 Audio device: Intel Corporation NM10/ICH7 Family High Definition Audio Controller (rev 02)
00:1c.0 PCI bridge: Intel Corporation NM10/ICH7 Family PCI Express Port 1 (rev 02)
00:1c.1 PCI bridge: Intel Corporation NM10/ICH7 Family PCI Express Port 2 (rev 02)
00:1c.2 PCI bridge: Intel Corporation NM10/ICH7 Family PCI Express Port 3 (rev 02)
00:1c.3 PCI bridge: Intel Corporation NM10/ICH7 Family PCI Express Port 4 (rev 02)
00:1d.0 USB controller: Intel Corporation NM10/ICH7 Family USB UHCI Controller #1 (rev 02)
00:1d.1 USB controller: Intel Corporation NM10/ICH7 Family USB UHCI Controller #2 (rev 02)
00:1d.2 USB controller: Intel Corporation NM10/ICH7 Family USB UHCI Controller #3 (rev 02)
00:1d.3 USB controller: Intel Corporation NM10/ICH7 Family USB UHCI Controller #4 (rev 02)
00:1d.7 USB controller: Intel Corporation NM10/ICH7 Family USB2 EHCI Controller (rev 02)
00:1e.0 PCI bridge: Intel Corporation 82801 Mobile PCI Bridge (rev e2)
00:1f.0 ISA bridge: Intel Corporation 82801GBM (ICH7-M) LPC Interface Bridge (rev 02)
00:1f.2 SATA controller: Intel Corporation 82801GBM/GHM (ICH7-M Family) SATA Controller \[AHCI mode\] (rev 02)
00:1f.3 SMBus: Intel Corporation NM10/ICH7 Family SMBus Controller (rev 02)
01:00.0 Network controller: Broadcom Corporation BCM4312 802.11b/g LP-PHY (rev 01)
03:00.0 Ethernet controller: Qualcomm Atheros AR8132 Fast Ethernet (rev c0)

### BIOS

### CPU

The CPU is an Intel® Atom™ processor N270 (512 KB L2 cache, 1.60 GHz) with 2 cores.

`user $``cat /proc/cpuinfo`
processor	: 0
vendor\_id	: GenuineIntel
cpu family	: 6
model		: 28
model name	: Intel(R) Atom(TM) CPU N270   @ 1.60GHz
stepping	: 2
microcode	: 0x212
cpu MHz		: 1600.000
cache size	: 512 KB
physical id	: 0
siblings	: 1
core id		: 0
cpu cores	: 1
apicid		: 0
initial apicid	: 0
fdiv\_bug	: no
hlt\_bug		: no
f00f\_bug	: no
coma\_bug	: no
fpu		: yes
fpu\_exception	: yes
cpuid level	: 10
wp		: yes
flags		: fpu vme de pse tsc msr pae mce cx8 apic sep mtrr pge mca cmov pat clflush dts acpi mmx fxsr sse sse2 ss ht tm pbe constant\_tsc arch\_perfmon pebs bts aperfmperf pni dtes64 monitor ds\_cpl est tm2 ssse3 xtpr pdcm movbe lahf\_lm dtherm
bogomips	: 3191.98
clflush size	: 64
cache\_alignment	: 64
address sizes	: 32 bits physical, 32 bits virtual
power management:
processor	: 1
vendor\_id	: GenuineIntel
cpu family	: 6
model		: 28
model name	: Intel(R) Atom(TM) CPU N270   @ 1.60GHz
stepping	: 2
microcode	: 0x212
cpu MHz		: 1600.000
cache size	: 512 KB
physical id	: 0
siblings	: 1
core id		: 0
cpu cores	: 0
apicid		: 1
initial apicid	: 1
fdiv\_bug	: no
hlt\_bug		: no
f00f\_bug	: no
coma\_bug	: no
fpu		: yes
fpu\_exception	: yes
cpuid level	: 10
wp		: yes
flags		: fpu vme de pse tsc msr pae mce cx8 apic sep mtrr pge mca cmov pat clflush dts acpi mmx fxsr sse sse2 ss ht tm pbe constant\_tsc arch\_perfmon pebs bts aperfmperf pni dtes64 monitor ds\_cpl est tm2 ssse3 xtpr pdcm movbe lahf\_lm dtherm
bogomips	: 3191.98
clflush size	: 64
cache\_alignment	: 64
address sizes	: 32 bits physical, 32 bits virtual
power management:

`user $``lscpu`
Architecture :        i686
Mode(s) opératoire(s) des processeurs : 32-bit
Boutisme :            Little Endian
Processeur(s) :       2
Liste de processeur(s) en ligne : 0,1
Thread(s) par cœur : 2
Cœur(s) par socket : 0
Socket(s) :           2
Identifiant constructeur : GenuineIntel
Famille de processeur : 6
Modèle :             28
Nom de modèle :      Intel(R) Atom(TM) CPU N270   @ 1.60GHz
Révision :           2
Vitesse du processeur en MHz : 1600.000
BogoMIPS :            3191.98
Cache L1d :           24K
Cache L1i :           32K
Cache L2 :            512K

### Memory

- 1 GB of DDR2 533MHz memory

### Graphics

This Netbook has a 10 inch screen

`root #``lspci -k -s 00:02.0`
00:02.0 VGA compatible controller: Intel Corporation Mobile 945GSE Express Integrated Graphics Controller (rev 03)
	Subsystem: Acer Incorporated \[ALI\] Device 022f
	Kernel driver in use: i915
	Kernel modules: i915

#### Frame Buffer

### Ethernet

`root #``lspci -k -s 03:00.0`
03:00.0 Ethernet controller: Qualcomm Atheros AR8132 Fast Ethernet (rev c0)
	Subsystem: Acer Incorporated \[ALI\] Device 022f
	Kernel driver in use: atl1c
	Kernel modules: atl1c

### ACPI

### Wireless

- 802.11b/g Wi-Fi CERTIFIED® wireless LAN card

`root #``lspci -k -s 01:00.0`
01:00.0 Network controller: Broadcom Corporation BCM4312 802.11b/g LP-PHY (rev 01)
	Subsystem: Foxconn International, Inc. T77H106.00 Wireless Half-size Mini PCIe Card
	Kernel driver in use: b43-pci-bridge
	Kernel modules: ssb

`root #``grep b43 /proc/modules`
b43 136216 0 - Live 0xf8283000
mac80211 326274 1 b43, Live 0xf81ea000
cfg80211 297999 2 b43,mac80211, Live 0xf810b000
ssb 28693 1 b43, Live 0xf860f000

### Sound

`root #``lspci -k -s 00:1b.0`
00:1b.0 Audio device: Intel Corporation NM10/ICH7 Family High Definition Audio Controller (rev 02)
	Subsystem: Acer Incorporated \[ALI\] Device 022f
	Kernel driver in use: snd\_hda\_intel
	Kernel modules: snd\_hda\_intel

### USB

#### 2.0

`root #``lspci -k -s 00:1d.7`
00:1d.7 USB controller: Intel Corporation NM10/ICH7 Family USB2 EHCI Controller (rev 02)
	Subsystem: Acer Incorporated \[ALI\] Device 022f
	Kernel driver in use: ehci-pci
	Kernel modules: ehci\_pci

### SATA

`root #``lspci -k -s 00:1f.2`
00:1f.2 SATA controller: Intel Corporation 82801GBM/GHM (ICH7-M Family) SATA Controller \[AHCI mode\] (rev 02)
	Subsystem: Acer Incorporated \[ALI\] Device 022f
	Kernel driver in use: ahci

### Webcam

`root #``lsusb -s 001:003`
Bus 001 Device 003: ID 0c45:62c0 Microdia Sonix USB 2.0 Camera
