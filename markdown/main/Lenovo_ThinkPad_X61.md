<!-- source: https://wiki.gentoo.org/wiki/Lenovo_ThinkPad_X61 | group: Gentoo Wiki (Main) | wiki-title: Lenovo ThinkPad X61 -->
---
title: Lenovo ThinkPad X61
url: https://wiki.gentoo.org/wiki/Lenovo_ThinkPad_X61
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2024-04-28"
fingerprint: "6f58bd07dbaa126d"
license: CC BY-SA 4.0
---

# Lenovo ThinkPad X61

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

### Hardware configuration

- Intel Core 2 Duo CPU T7250 (2.0GHz)
- 4GB DDR2 667Mhz RAM (extended from 1GB)
- 120GB SSD SATA2 (changed from HDD 100GB 7200rpm)
- Intel Graphics Media Accelerator X3100 (Intel GM965)
- 12.1" TFT display with 1024x768 resolution
- Active Protection System (APS, not applicated for SSD)
- Intel HD Audio
- Intel Gigabit Ethernet 10/100/1000 PCI-Express
- Atheros Wireless AR2425 Mini-PCI Express Adapter
- ThinkPad 56K Modem (not used)
- Trusted Computing Group TPM (not used)
- SD Card slot
- CardBus slot (PC Card Type 2, not tested)
- Firewire 400 (IEEE1394a, not tested)
- Broadcom Corp. BCM2045B (BDC-2) Bluetooth
- Mono speaker and microphone
- 4-cell battery with 2-3 hours life (model:42T4505, battery voltage:14.4V, power:38Wh, capacity:2600mAh, color:black, dimension:209.55x59.95x23.59mm, weight:228.3g)
- Weight is \~1.6kg


Printout of lspci -nn:

`root #``lspci -nn`
00:00.0 Host bridge \[0600\]: Intel Corporation Mobile PM965/GM965/GL960 Memory Controller Hub \[8086:2a00\] (rev 0c)
00:02.0 VGA compatible controller \[0300\]: Intel Corporation Mobile GM965/GL960 Integrated Graphics Controller (primary) \[8086:2a02\] (rev 0c)
00:02.1 Display controller \[0380\]: Intel Corporation Mobile GM965/GL960 Integrated Graphics Controller (secondary) \[8086:2a03\] (rev 0c)
00:19.0 Ethernet controller \[0200\]: Intel Corporation 82566MM Gigabit Network Connection \[8086:1049\] (rev 03)
00:1a.0 USB controller \[0c03\]: Intel Corporation 82801H (ICH8 Family) USB UHCI Controller #4 \[8086:2834\] (rev 03)
00:1a.1 USB controller \[0c03\]: Intel Corporation 82801H (ICH8 Family) USB UHCI Controller #5 \[8086:2835\] (rev 03)
00:1a.7 USB controller \[0c03\]: Intel Corporation 82801H (ICH8 Family) USB2 EHCI Controller #2 \[8086:283a\] (rev 03)
00:1b.0 Audio device \[0403\]: Intel Corporation 82801H (ICH8 Family) HD Audio Controller \[8086:284b\] (rev 03)
00:1c.0 PCI bridge \[0604\]: Intel Corporation 82801H (ICH8 Family) PCI Express Port 1 \[8086:283f\] (rev 03)
00:1c.1 PCI bridge \[0604\]: Intel Corporation 82801H (ICH8 Family) PCI Express Port 2 \[8086:2841\] (rev 03)
00:1d.0 USB controller \[0c03\]: Intel Corporation 82801H (ICH8 Family) USB UHCI Controller #1 \[8086:2830\] (rev 03)
00:1d.1 USB controller \[0c03\]: Intel Corporation 82801H (ICH8 Family) USB UHCI Controller #2 \[8086:2831\] (rev 03)
00:1d.7 USB controller \[0c03\]: Intel Corporation 82801H (ICH8 Family) USB2 EHCI Controller #1 \[8086:2836\] (rev 03)
00:1e.0 PCI bridge \[0604\]: Intel Corporation 82801 Mobile PCI Bridge \[8086:2448\] (rev f3)
00:1f.0 ISA bridge \[0601\]: Intel Corporation 82801HEM (ICH8M-E) LPC Interface Controller \[8086:2811\] (rev 03)
00:1f.1 IDE interface \[0101\]: Intel Corporation 82801HM/HEM (ICH8M/ICH8M-E) IDE Controller \[8086:2850\] (rev 03)
00:1f.2 SATA controller \[0106\]: Intel Corporation 82801HM/HEM (ICH8M/ICH8M-E) SATA Controller \[AHCI mode\] \[8086:2829\] (rev 03)
00:1f.3 SMBus \[0c05\]: Intel Corporation 82801H (ICH8 Family) SMBus Controller \[8086:283e\] (rev 03)
03:00.0 Ethernet controller \[0200\]: Qualcomm Atheros AR242x / AR542x Wireless Network Adapter (PCI-Express) \[168c:001c\] (rev 01)
05:00.0 CardBus bridge \[0607\]: Ricoh Co Ltd RL5c476 II \[1180:0476\] (rev ba)
05:00.1 FireWire (IEEE 1394) \[0c00\]: Ricoh Co Ltd R5C832 IEEE 1394 Controller \[1180:0832\] (rev 04)
05:00.2 SD Host controller \[0805\]: Ricoh Co Ltd R5C822 SD/SDIO/MMC/MS/MSPro Host Adapter \[1180:0822\] (rev 21)

Printout of lspci -kk:

`root #``lspci -kk`
00:00.0 Host bridge: Intel Corporation Mobile PM965/GM965/GL960 Memory Controller Hub (rev 0c)
	Subsystem: Lenovo ThinkPad T61/R61
	Kernel driver in use: agpgart-intel
00:02.0 VGA compatible controller: Intel Corporation Mobile GM965/GL960 Integrated Graphics Controller (primary) (rev 0c)
	Subsystem: Lenovo ThinkPad T61/R61
	Kernel driver in use: i915
00:02.1 Display controller: Intel Corporation Mobile GM965/GL960 Integrated Graphics Controller (secondary) (rev 0c)
	Subsystem: Lenovo ThinkPad T61/R61
00:19.0 Ethernet controller: Intel Corporation 82566MM Gigabit Network Connection (rev 03)
	Subsystem: Lenovo Device 20de
	Kernel driver in use: e1000e
00:1a.0 USB controller: Intel Corporation 82801H (ICH8 Family) USB UHCI Controller #4 (rev 03)
	Subsystem: Lenovo ThinkPad T61/R61
	Kernel driver in use: uhci\_hcd
00:1a.1 USB controller: Intel Corporation 82801H (ICH8 Family) USB UHCI Controller #5 (rev 03)
	Subsystem: Lenovo Thinkpad T61/R61
	Kernel driver in use: uhci\_hcd
00:1a.7 USB controller: Intel Corporation 82801H (ICH8 Family) USB2 EHCI Controller #2 (rev 03)
	Subsystem: Lenovo ThinkPad T61/R61
	Kernel driver in use: ehci-pci
00:1b.0 Audio device: Intel Corporation 82801H (ICH8 Family) HD Audio Controller (rev 03)
	Subsystem: Lenovo ThinkPad T61/R61
	Kernel driver in use: snd\_hda\_intel
	Kernel modules: snd\_hda\_intel
00:1c.0 PCI bridge: Intel Corporation 82801H (ICH8 Family) PCI Express Port 1 (rev 03)
	Kernel driver in use: pcieport
	Kernel modules: shpchp
00:1c.1 PCI bridge: Intel Corporation 82801H (ICH8 Family) PCI Express Port 2 (rev 03)
	Kernel driver in use: pcieport
	Kernel modules: shpchp
00:1d.0 USB controller: Intel Corporation 82801H (ICH8 Family) USB UHCI Controller #1 (rev 03)
	Subsystem: Lenovo ThinkPad T61/R61
	Kernel driver in use: uhci\_hcd
00:1d.1 USB controller: Intel Corporation 82801H (ICH8 Family) USB UHCI Controller #2 (rev 03)
	Subsystem: Lenovo ThinkPad T61/R61
	Kernel driver in use: uhci\_hcd
00:1d.7 USB controller: Intel Corporation 82801H (ICH8 Family) USB2 EHCI Controller #1 (rev 03)
	Subsystem: Lenovo ThinkPad T61/R61
	Kernel driver in use: ehci-pci
00:1e.0 PCI bridge: Intel Corporation 82801 Mobile PCI Bridge (rev f3)
00:1f.0 ISA bridge: Intel Corporation 82801HEM (ICH8M-E) LPC Interface Controller (rev 03)
	Subsystem: Lenovo ThinkPad T61/R61
	Kernel driver in use: lpc\_ich
00:1f.1 IDE interface: Intel Corporation 82801HM/HEM (ICH8M/ICH8M-E) IDE Controller (rev 03)
	Subsystem: Lenovo ThinkPad T61/R61
00:1f.2 SATA controller: Intel Corporation 82801HM/HEM (ICH8M/ICH8M-E) SATA Controller \[AHCI mode\] (rev 03)
	Subsystem: Lenovo ThinkPad T61/R61
	Kernel driver in use: ahci
00:1f.3 SMBus: Intel Corporation 82801H (ICH8 Family) SMBus Controller (rev 03)
	Subsystem: Lenovo ThinkPad T61/R61
	Kernel driver in use: i801\_smbus
03:00.0 Ethernet controller: Qualcomm Atheros AR242x / AR542x Wireless Network Adapter (PCI-Express) (rev 01)
	Subsystem: Qualcomm Atheros Device 0034
	Kernel driver in use: ath5k
05:00.0 CardBus bridge: Ricoh Co Ltd RL5c476 II (rev ba)
	Subsystem: Lenovo ThinkPad R61
	Kernel driver in use: yenta\_cardbus
	Kernel modules: yenta\_socket
05:00.1 FireWire (IEEE 1394): Ricoh Co Ltd R5C832 IEEE 1394 Controller (rev 04)
	Subsystem: Lenovo ThinkPad R61
	Kernel driver in use: firewire\_ohci
05:00.2 SD Host controller: Ricoh Co Ltd R5C822 SD/SDIO/MMC/MS/MSPro Host Adapter (rev 21)
	Subsystem: Lenovo ThinkPad W500
	Kernel driver in use: sdhci-pci

Printout of lsusb (no external devices connected):

`root #``lsusb` Bus 002 Device 001: ID 1d6b:0002 Linux Foundation 2.0 root hub
Bus 006 Device 001: ID 1d6b:0001 Linux Foundation 1.1 root hub
Bus 005 Device 001: ID 1d6b:0001 Linux Foundation 1.1 root hub
Bus 001 Device 001: ID 1d6b:0002 Linux Foundation 2.0 root hub
Bus 004 Device 001: ID 1d6b:0001 Linux Foundation 1.1 root hub
Bus 003 Device 002: ID 0a5c:2110 Broadcom Corp. BCM2045B (BDC-2) \[Bluetooth Controller\]
Bus 003 Device 001: ID 1d6b:0001 Linux Foundation 1.1 root hub

Printout of lsmod (builtin devices, no external devices connected):

`root #``lsmod`
Module                  Size  Used by
snd\_hda\_codec\_analog     7673  1 
snd\_hda\_codec\_generic    39119  1 snd\_hda\_codec\_analog
microcode               7558  0 
snd\_hda\_intel          16879  3 
snd\_hda\_controller     13921  1 snd\_hda\_intel
snd\_hda\_codec          68019  4 snd\_hda\_codec\_generic,snd\_hda\_intel,snd\_hda\_controller,snd\_hda\_codec\_analog
snd\_hwdep               5373  1 snd\_hda\_codec
shpchp                 21721  0 
snd\_pcm                62839  3 snd\_hda\_codec,snd\_hda\_intel,snd\_hda\_controller
snd\_timer              15326  1 snd\_pcm
thinkpad\_acpi          54157  0 
nvram                   5119  1 thinkpad\_acpi
snd                    48241  13 snd\_hwdep,snd\_timer,snd\_pcm,snd\_hda\_codec\_generic,snd\_hda\_codec,snd\_hda\_intel,thinkpad\_acpi
acpi\_cpufreq            5890  1 
xt\_multiport            1526  16 
xt\_conntrack            2793  21 
iptable\_filter          1304  1 
iptable\_mangle          1400  0 
iptable\_nat             2406  0 
nf\_conntrack\_ipv4      10876  22 
nf\_defrag\_ipv4          1267  1 nf\_conntrack\_ipv4
nf\_nat\_ipv4             3016  1 iptable\_nat
nf\_nat                 10154  2 nf\_nat\_ipv4,iptable\_nat
nf\_conntrack           59005  5 nf\_nat,nf\_nat\_ipv4,xt\_conntrack,iptable\_nat,nf\_conntrack\_ipv4
xts                     2727  0 
gf128mul                5242  1 xts
aes\_x86\_64              7263  0 
cbc                     2456  0 
sha512\_generic          4728  0 
sha1\_generic            1830  0 
iscsi\_tcp               7852  0 
libiscsi\_tcp           10570  1 iscsi\_tcp
libiscsi               31451  2 libiscsi\_tcp,iscsi\_tcp
scsi\_transport\_iscsi    60988  2 iscsi\_tcp,libiscsi
nfs                    98770  0 
lockd                  52398  1 nfs
sunrpc                151788  2 nfs,lockd
sx8                    10844  0 
mptsas                 32858  0 
scsi\_transport\_sas     21333  1 mptsas
mptfc                  10188  0 
scsi\_transport\_fc      39365  1 mptfc
mptspi                 11202  0 
scsi\_transport\_spi     17275  1 mptspi
mptscsih               16081  3 mptfc,mptsas,mptspi
mptbase                54327  4 mptfc,mptsas,mptspi,mptscsih
BusLogic               19039  0 
sg                     21817  0 
pcmcia                 29243  0 
yenta\_socket           18633  0 
pcmcia\_rsrc             7048  1 yenta\_socket
pcmcia\_core            10792  3 pcmcia,pcmcia\_rsrc,yenta\_socket

### Patching Linux kernel to get Atheros WIFI \[168c:001c\] LED works

cd /usr/src/linux

patch -p0 \< ath5kled-patch

### Kernel config

Example of kernel configuration:
[X61-kernel-config](https://raw.githubusercontent.com/serge123/atheros-ar242x-ar542x-wifi-laptop-led-support/master/x61-kernel-config)

### Updating BIOS to get SATA2 interface with increased speed

Official BIOS release has a limited SATA2 interface speed - 1.5 Gb/s.
