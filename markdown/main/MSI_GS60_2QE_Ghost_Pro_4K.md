<!-- source: https://wiki.gentoo.org/wiki/MSI_GS60_2QE_Ghost_Pro_4K | group: Gentoo Wiki (Main) | wiki-title: MSI GS60 2QE Ghost Pro 4K -->
---
title: MSI GS60 2QE Ghost Pro 4K
url: https://wiki.gentoo.org/wiki/MSI_GS60_2QE_Ghost_Pro_4K
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2024-04-28"
fingerprint: "7f528f2ddba8d52a"
license: CC BY-SA 4.0
---

# MSI GS60 2QE Ghost Pro 4K

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)



## Laptop Specifics

MSI GS60 2QE Ghost Pro 4k - 079

- Intel® Core™ i7-4710HQ 2.5GHz to 3.5GHz w/ Turbo Boost 6MB Cache
- Intel® HM87
- 15.6" WQHD+ 4K IPS 3840x2160 (16:9)
- GeForce® GTX 970M 6GB GDDR5
- SteelSeries Programmable Full Color Backlit Keyboard
- Dynaudio Premium Speakers & Subwoofer
- 16GB DDR3L 1600MHz
- 128GB M.2 SSD x 2 RAID 0 + 1TB HDD (7200RPM)
- LANKiller™ E2200 Game Networking
- WLANKiller™ N1525 Wireless-AC with Bluetooth
- Card ReaderSD (XC/HC)
- 1080p FHD Webcam
- HDMI 1.4 x 1, mDP x 1
- Battery Pack 6 Cell
- Mic-in x 1, Headphone-out x 1
- 15.35" (L) x 10.47" (W) x 0.78" (H)
- 4.2 lbs



### lspci

`root #``lspci`
00:00.0 Host bridge: Intel Corporation Xeon E3-1200 v3/4th Gen Core Processor DRAM Controller (rev 06)
00:01.0 PCI bridge: Intel Corporation Xeon E3-1200 v3/4th Gen Core Processor PCI Express x16 Controller (rev 06)
00:02.0 VGA compatible controller: Intel Corporation 4th Gen Core Processor Integrated Graphics Controller (rev 06)
00:03.0 Audio device: Intel Corporation Xeon E3-1200 v3/4th Gen Core Processor HD Audio Controller (rev 06)
00:14.0 USB controller: Intel Corporation 8 Series/C220 Series Chipset Family USB xHCI (rev 05)
00:16.0 Communication controller: Intel Corporation 8 Series/C220 Series Chipset Family MEI Controller #1 (rev 04)
00:1a.0 USB controller: Intel Corporation 8 Series/C220 Series Chipset Family USB EHCI #2 (rev 05)
00:1b.0 Audio device: Intel Corporation 8 Series/C220 Series Chipset High Definition Audio Controller (rev 05)
00:1c.0 PCI bridge: Intel Corporation 8 Series/C220 Series Chipset Family PCI Express Root Port #1 (rev d5)
00:1c.2 PCI bridge: Intel Corporation 8 Series/C220 Series Chipset Family PCI Express Root Port #3 (rev d5)
00:1c.3 PCI bridge: Intel Corporation 8 Series/C220 Series Chipset Family PCI Express Root Port #4 (rev d5)
00:1c.4 PCI bridge: Intel Corporation 8 Series/C220 Series Chipset Family PCI Express Root Port #5 (rev d5)
00:1d.0 USB controller: Intel Corporation 8 Series/C220 Series Chipset Family USB EHCI #1 (rev 05)
00:1f.0 ISA bridge: Intel Corporation HM87 Express LPC Controller (rev 05)
00:1f.2 RAID bus controller: Intel Corporation 82801 Mobile SATA Controller \[RAID mode\] (rev 05)
00:1f.3 SMBus: Intel Corporation 8 Series/C220 Series Chipset Family SMBus Controller (rev 05)
01:00.0 3D controller: NVIDIA Corporation GM204M \[GeForce GTX 970M\] (rev ff)
03:00.0 Unassigned class \[ff00\]: Realtek Semiconductor Co., Ltd. RTS5249 PCI Express Card Reader (rev 01)
04:00.0 Ethernet controller: Qualcomm Atheros Killer E220x Gigabit Ethernet Controller (rev 13)
05:00.0 Network controller: Qualcomm Atheros QCA6174 802.11ac Wireless Network Adapter (rev 20)

### lsusb

`root #``lsusb`
Bus 002 Device 003: ID 1770:ff00  
Bus 002 Device 002: ID 8087:8000 Intel Corp. 
Bus 002 Device 001: ID 1d6b:0002 Linux Foundation 2.0 root hub
Bus 001 Device 004: ID 5986:055c Acer, Inc 
Bus 001 Device 003: ID 0cf3:3004 Atheros Communications, Inc. AR3012 Bluetooth 4.0
Bus 001 Device 002: ID 8087:8008 Intel Corp. 
Bus 001 Device 001: ID 1d6b:0002 Linux Foundation 2.0 root hub
Bus 004 Device 001: ID 1d6b:0003 Linux Foundation 3.0 root hub
Bus 003 Device 001: ID 1d6b:0002 Linux Foundation 2.0 root hub

### lsmod

`root #``lsmod`
mmc\_block              25887  2
rtsx\_pci\_sdmmc         10079  0
rtsx\_pci\_ms             5010  0
mmc\_core               86950  2 mmc\_block,rtsx\_pci\_sdmmc
memstick                6160  1 rtsx\_pci\_ms
rtsx\_pci               28841  2 rtsx\_pci\_ms,rtsx\_pci\_sdmmc
mfd\_core                3177  1 rtsx\_pci
ccm                     7641  2
uvcvideo               72059  0
videobuf2\_vmalloc       4648  1 uvcvideo
videobuf2\_memops        1783  1 videobuf2\_vmalloc
videobuf2\_core         33987  1 uvcvideo
v4l2\_common             3358  1 videobuf2\_core
videodev              126553  3 uvcvideo,v4l2\_common,videobuf2\_core
ath3k                   7837  0
btusb                  27652  0
btrtl                   3824  1 btusb
btbcm                   5170  1 btusb
btintel                 1313  1 btusb
bluetooth             315253  5 ath3k,btbcm,btrtl,btusb,btintel
bbswitch                4528  0
snd\_hda\_codec\_hdmi     36001  1
snd\_hda\_codec\_realtek    54779  1
snd\_hda\_codec\_generic    53728  1 snd\_hda\_codec\_realtek
x86\_pkg\_temp\_thermal     4519  0
kvm\_intel             146790  0
ath10k\_pci             26573  0
ath10k\_core           172016  1 ath10k\_pci
kvm                   404662  1 kvm\_intel
msi\_wmi                 2537  0
sparse\_keymap           2818  1 msi\_wmi
ath                    18475  1 ath10k\_core
joydev                  9207  0
mac80211              480391  1 ath10k\_core
snd\_hda\_intel          21359  10
snd\_hda\_codec          88562  4 snd\_hda\_codec\_realtek,snd\_hda\_codec\_hdmi,snd\_hda\_codec\_generic,snd\_hda\_intel
snd\_hwdep               5690  1 snd\_hda\_codec
snd\_hda\_core           36195  5 snd\_hda\_codec\_realtek,snd\_hda\_codec\_hdmi,snd\_hda\_codec\_generic,snd\_hda\_codec,snd\_hda\_intel
cfg80211              426882  3 ath,mac80211,ath10k\_core
snd\_pcm                77395  4 snd\_hda\_codec\_hdmi,snd\_hda\_codec,snd\_hda\_intel,snd\_hda\_core
rfkill                 14015  4 cfg80211,bluetooth
alx                    27327  0
mdio                    2951  1 alx
snd\_timer              17993  1 snd\_pcm
snd                    56013  28 snd\_hda\_codec\_realtek,snd\_hwdep,snd\_timer,snd\_hda\_codec\_hdmi,snd\_pcm,snd\_hda\_codec\_generic,snd\_hda\_codec,snd\_hda\_intel
soundcore               4906  1 snd
xhci\_pci                3635  0
wmi                     7571  1 msi\_wmi
efivarfs                5355  1
atl1c                  32826  0
fuse                   74793  1
usbhid                 35186  0
xhci\_hcd               99925  1 xhci\_pci
ohci\_hcd               26846  0
uhci\_hcd               21091  0
usb\_storage            47023  0
ehci\_pci                3695  0
ehci\_hcd               40198  1 ehci\_pci
usbcore               156973  11 ath3k,btusb,uhci\_hcd,uvcvideo,usb\_storage,ohci\_hcd,ehci\_hcd,ehci\_pci,usbhid,xhci\_hcd,xhci\_pci
usb\_common              1544  1 usbcore

## Hardware Info

### Atheros Killer E220x Gigabit Ethernet Controller

**Ethernet: Atheros Killer E220x Gigabit Ethernet Controller**

### Realtek RTS5249 PCI Express Card Reader

**Card Reader: Realtek Semiconductor Co., Ltd. RTS5249 PCI Express Card Reader**

### Bluetooth

The Bluetooth doesn't work with default kernel configuration. Below changes are needed for the bluetooth to function.

#### Modify existing btusb.c file

`root #``nano /usr/src/linux/drivers/bluetooth/btusb.c`
**`/usr/src/linux/drivers/bluetooth/btusb.c`**

Rebuild the kernel after the change.

### SteelSeries Programmable Full Color Backlit Keyboard

Control the LED colors of the SteelSeries programmable full color backlit keyboard.

`root #``emerge -av net-libs/nodejs`
Clone and install the utility:

`user $``cd msi-keyboard``user $``npm install`
Run the example:

`user $``cd examples``user $``node sample.js`
