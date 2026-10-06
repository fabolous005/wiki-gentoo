<!-- source: https://wiki.gentoo.org/wiki/Acer_Travelmate_8172 | group: Gentoo Wiki (Main) | wiki-title: Acer Travelmate 8172 -->
---
title: Acer Travelmate 8172
url: https://wiki.gentoo.org/wiki/Acer_Travelmate_8172
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2024-04-28"
fingerprint: "7f4e0965d3a6d32a"
license: CC BY-SA 4.0
---

# Acer Travelmate 8172

From Gentoo Wiki

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

The information in this article is probably **outdated**. You can help the Gentoo community by verifying and [updating this article](https://wiki.gentoo.org/index.php?title=Acer_Travelmate_8172&action=edit).

This is an article about running Gentoo on an Acer TravelMate 8172 series laptop.

## Hardware

### Standard

| Device | Make/model | Status | Vendor ID / Product ID | Kernel driver(s) | Kernel version | Notes | 
|---|---|---|---|---|---|---|
| CPU | Intel® Core™ i3-330UM | Works | N/A | N/A | 2.6.39 |  | 
| GPU | Intel Corporation Arrandale Integrated Graphics Controller (rev 02) | Works | N/A | N/A | 2.6.39 |  | 
| HDD | N/A | Works | N/A | ahci | 2.6.39 |  | 
| Ethernet | Broadcom Corporation NetXtreme BCM57760 Gigabit Ethernet PCIe (rev 01) | Works | N/A | N/A | 2.6.39 |  | 
| Wi-Fi | Broadcom Corporation Device 4357 (rev 01) | Works | N/A | brcm80211 | 2.6.39 |  | 
| Sound | Intel Corporation Ibex Peak High Definition Audio (rev 05) | Works | N/A | snd\_hda\_codec\_conexant snd\_hda\_intel | 2.6.39 |  | 
| SD card reader | N/A | Works | N/A | sdhci | 2.6.39 |  | 
| Webcam | N/A | Works | N/A | uvcvideo | 2.6.39 |  | 
| Fingerprint reader | N/A | Not tested | N/A | N/A | N/A |  | 

### Detailed information

`root #``lspci`
00:00.0 Host bridge: Intel Corporation Arrandale DRAM Controller (rev 02)
00:02.0 VGA compatible controller: Intel Corporation Arrandale Integrated Graphics Controller (rev 02)
00:16.0 Communication controller: Intel Corporation Ibex Peak HECI Controller (rev 06)
00:1a.0 USB Controller: Intel Corporation Ibex Peak USB2 Enhanced Host Controller (rev 05)
00:1b.0 Audio device: Intel Corporation Ibex Peak High Definition Audio (rev 05)
00:1c.0 PCI bridge: Intel Corporation Ibex Peak PCI Express Root Port 1 (rev 05)
00:1c.1 PCI bridge: Intel Corporation Ibex Peak PCI Express Root Port 2 (rev 05)
00:1c.2 PCI bridge: Intel Corporation Ibex Peak PCI Express Root Port 3 (rev 05)
00:1d.0 USB Controller: Intel Corporation Ibex Peak USB2 Enhanced Host Controller (rev 05)
00:1e.0 PCI bridge: Intel Corporation 82801 Mobile PCI Bridge (rev a5)
00:1f.0 ISA bridge: Intel Corporation Ibex Peak LPC Interface Controller (rev 05)
00:1f.2 SATA controller: Intel Corporation Ibex Peak 4 port SATA AHCI Controller (rev 05)
00:1f.3 SMBus: Intel Corporation Ibex Peak SMBus Controller (rev 05)
00:1f.6 Signal processing controller: Intel Corporation Ibex Peak Thermal Subsystem (rev 05)
01:00.0 Network controller: Broadcom Corporation Device 4357 (rev 01)
03:00.0 Ethernet controller: Broadcom Corporation NetXtreme BCM57760 Gigabit Ethernet PCIe (rev 01)
ff:00.0 Host bridge: Intel Corporation QuickPath Architecture Generic Non-core Registers (rev 05)
ff:00.1 Host bridge: Intel Corporation QuickPath Architecture System Address Decoder (rev 05)
ff:02.0 Host bridge: Intel Corporation QPI Link 0 (rev 05)
ff:02.1 Host bridge: Intel Corporation QPI Physical 0 (rev 05)
ff:02.2 Host bridge: Intel Corporation Device 2d12 (rev 05)
ff:02.3 Host bridge: Intel Corporation Device 2d13 (rev 05)

`root #``lsusb`
Bus 002 Device 005: ID 0bda:0138 Realtek Semiconductor Corp. 
Bus 002 Device 002: ID 8087:0020  
Bus 002 Device 001: ID 1d6b:0002 Linux Foundation 2.0 root hub
Bus 001 Device 005: ID 0489:e010 Foxconn / Hon Hai 
Bus 001 Device 004: ID 08ff:168c AuthenTec, Inc. 
Bus 001 Device 003: ID 0402:9665 ALi Corp. 
Bus 001 Device 002: ID 8087:0020  
Bus 001 Device 001: ID 1d6b:0002 Linux Foundation 2.0 root hub

`root #``lsmod`
leho@travelmate \~ $ lsmod | sort
ac                      1632  0 
acer\_wmi               13531  0 
arc4                     958  2 
battery                 4386  0 
bluetooth              40786  1 btusb
brcm80211             560283  0 
broadcom                4722  0 
btusb                   8128  0 
cfg80211               84944  2 brcm80211,mac80211
cifs                  189381  2 
coretemp                3822  0 
fan                     1738  0 
i2c\_i801                5488  0 
intel\_ips               6695  0 
iTCO\_wdt                8341  0 
libphy                 11577  2 broadcom,tg3
mac80211              130876  1 brcm80211
md4                     2665  0 
pcspkr                  1199  0 
processor              20540  0 
psmouse                29722  0 
rfkill                 10464  3 bluetooth,cfg80211,acer\_wmi
rtc\_cmos                6822  0 
rtc\_core               10241  1 rtc\_cmos
rtc\_lib                 1434  1 rtc\_core
snd                    32871  10 snd\_pcm\_oss,snd\_mixer\_oss,snd\_seq\_oss,snd\_seq,snd\_seq\_device,snd\_hda\_codec\_conexant,snd\_hda\_intel,snd\_hda\_codec,snd\_pcm,snd\_timer
snd\_hda\_codec          45259  2 snd\_hda\_codec\_conexant,snd\_hda\_intel
snd\_hda\_codec\_conexant    29810  1 
snd\_hda\_intel          15331  0 
snd\_mixer\_oss          10119  1 snd\_pcm\_oss
snd\_page\_alloc          4925  2 snd\_hda\_intel,snd\_pcm
snd\_pcm                42465  3 snd\_pcm\_oss,snd\_hda\_intel,snd\_hda\_codec
snd\_pcm\_oss            25723  0 
snd\_seq                33123  5 snd\_seq\_dummy,snd\_seq\_oss,snd\_seq\_midi\_event
snd\_seq\_device          3685  3 snd\_seq\_dummy,snd\_seq\_oss,snd\_seq
snd\_seq\_dummy            907  0 
snd\_seq\_midi\_event      3636  1 snd\_seq\_oss
snd\_seq\_oss            19436  0 
snd\_timer              12082  2 snd\_seq,snd\_pcm
sparse\_keymap           1892  1 acer\_wmi
tg3                    99008  0 
thermal                 6050  0 
uvcvideo               45608  0 
videodev               45609  1 uvcvideo
wmi                     5926  1 acer\_wmi

`user $``cat /proc/cpuinfo`
processor	: 0
vendor\_id	: GenuineIntel
cpu family	: 6
model		: 37
model name	: Intel(R) Core(TM) i3 CPU       U 330  @ 1.20GHz
stepping	: 5
cpu MHz		: 1199.882
cache size	: 3072 KB
physical id	: 0
siblings	: 4
core id		: 0
cpu cores	: 2
apicid		: 0
initial apicid	: 0
fdiv\_bug	: no
hlt\_bug		: no
f00f\_bug	: no
coma\_bug	: no
fpu		: yes
fpu\_exception	: yes
cpuid level	: 11
wp		: yes
flags		: fpu vme de pse tsc msr pae mce cx8 apic sep mtrr pge mca cmov pat pse36 clflush dts acpi mmx fxsr sse sse2 ss ht tm pbe nx rdtscp lm constant\_tsc arch\_perfmon pebs bts xtopology nonstop\_tsc aperfmperf pni dtes64 monitor ds\_cpl vmx est tm2 ssse3 cx16 xtpr pdcm sse4\_1 sse4\_2 popcnt lahf\_lm arat dts tpr\_shadow vnmi flexpriority ept vpid
bogomips	: 2402.00
clflush size	: 64
cache\_alignment	: 64
address sizes	: 36 bits physical, 48 bits virtual
power management:
processor	: 1
vendor\_id	: GenuineIntel
cpu family	: 6
model		: 37
model name	: Intel(R) Core(TM) i3 CPU       U 330  @ 1.20GHz
stepping	: 5
cpu MHz		: 1199.882
cache size	: 3072 KB
physical id	: 0
siblings	: 4
core id		: 0
cpu cores	: 2
apicid		: 1
initial apicid	: 1
fdiv\_bug	: no
hlt\_bug		: no
f00f\_bug	: no
coma\_bug	: no
fpu		: yes
fpu\_exception	: yes
cpuid level	: 11
wp		: yes
flags		: fpu vme de pse tsc msr pae mce cx8 apic sep mtrr pge mca cmov pat pse36 clflush dts acpi mmx fxsr sse sse2 ss ht tm pbe nx rdtscp lm constant\_tsc arch\_perfmon pebs bts xtopology nonstop\_tsc aperfmperf pni dtes64 monitor ds\_cpl vmx est tm2 ssse3 cx16 xtpr pdcm sse4\_1 sse4\_2 popcnt lahf\_lm arat dts tpr\_shadow vnmi flexpriority ept vpid
bogomips	: 2402.01
clflush size	: 64
cache\_alignment	: 64
address sizes	: 36 bits physical, 48 bits virtual
power management:
processor	: 2
vendor\_id	: GenuineIntel
cpu family	: 6
model		: 37
model name	: Intel(R) Core(TM) i3 CPU       U 330  @ 1.20GHz
stepping	: 5
cpu MHz		: 1199.882
cache size	: 3072 KB
physical id	: 0
siblings	: 4
core id		: 2
cpu cores	: 2
apicid		: 4
initial apicid	: 4
fdiv\_bug	: no
hlt\_bug		: no
f00f\_bug	: no
coma\_bug	: no
fpu		: yes
fpu\_exception	: yes
cpuid level	: 11
wp		: yes
flags		: fpu vme de pse tsc msr pae mce cx8 apic sep mtrr pge mca cmov pat pse36 clflush dts acpi mmx fxsr sse sse2 ss ht tm pbe nx rdtscp lm constant\_tsc arch\_perfmon pebs bts xtopology nonstop\_tsc aperfmperf pni dtes64 monitor ds\_cpl vmx est tm2 ssse3 cx16 xtpr pdcm sse4\_1 sse4\_2 popcnt lahf\_lm arat dts tpr\_shadow vnmi flexpriority ept vpid
bogomips	: 2402.02
clflush size	: 64
cache\_alignment	: 64
address sizes	: 36 bits physical, 48 bits virtual
power management:
processor	: 3
vendor\_id	: GenuineIntel
cpu family	: 6
model		: 37
model name	: Intel(R) Core(TM) i3 CPU       U 330  @ 1.20GHz
stepping	: 5
cpu MHz		: 1199.882
cache size	: 3072 KB
physical id	: 0
siblings	: 4
core id		: 2
cpu cores	: 2
apicid		: 5
initial apicid	: 5
fdiv\_bug	: no
hlt\_bug		: no
f00f\_bug	: no
coma\_bug	: no
fpu		: yes
fpu\_exception	: yes
cpuid level	: 11
wp		: yes
flags		: fpu vme de pse tsc msr pae mce cx8 apic sep mtrr pge mca cmov pat pse36 clflush dts acpi mmx fxsr sse sse2 ss ht tm pbe nx rdtscp lm constant\_tsc arch\_perfmon pebs bts xtopology nonstop\_tsc aperfmperf pni dtes64 monitor ds\_cpl vmx est tm2 ssse3 cx16 xtpr pdcm sse4\_1 sse4\_2 popcnt lahf\_lm arat dts tpr\_shadow vnmi flexpriority ept vpid
bogomips	: 2402.03
clflush size	: 64
cache\_alignment	: 64
address sizes	: 36 bits physical, 48 bits virtual
power management:

## Laptop Specifications

Hardware specs may vary. These are the specs for the model Acer TravelMate 8172-33U3G32nkk:

- Intel Core i3-330UM 1.2GHz 3MB cache
- 3GB DDR2 RAM (2 slots)
- Intel HD Graphics (on-CPU)
- Integrated HDA Conexant Audio
- 11.6in TFT LCD Screen (Widescreen), 1366x768 WXGA
- 320GB 2.5in SATA Hard Disk
- 3x USB 2.0 ports
- VGA output
- Broadcom tg3 Gigabit ethernet
- Broadcom brcm80211 Wifi 4357 abgn
- 5in1 Card Reader
- Dock connector

## Installation

### Firmware

The wireless network card requires external firmware:

`root #``emerge --ask sys-kernel/linux-firmware`
### Kernel

KERNEL **Ethernet (kernel v. 2.6.38)**

```
Device Drivers  --->
    [*] Network device support  --->
        [*]   Ethernet (1000 Mbit)  --->
            <*>   Broadcom Tigon3 support
```
KERNEL **Wi-Fi, (kernel v. 2.6.38)**

```
[*] Networking support  --->
    [*]   Wireless  --->
        <*>   Generic IEEE 802.11 Networking Stack (mac80211)
Device Drivers  --->
    [*] Staging drivers  --->
        Broadcom IEEE802.11n WLAN drivers  --->
            Broadcom IEEE802.11n driver style (Broadcom IEEE802.11n PCIe SoftMAC WLAN driver)  --->
                (X) Broadcom IEEE802.11n PCIe SoftMAC WLAN driver
                ( ) Broadcom IEEE802.11n embedded FullMAC WLAN driver
```
