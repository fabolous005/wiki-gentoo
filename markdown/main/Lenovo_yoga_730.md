<!-- source: https://wiki.gentoo.org/wiki/Lenovo_yoga_730 | group: Gentoo Wiki (Main) | wiki-title: Lenovo yoga 730 -->
---
title: Lenovo yoga 730
url: https://wiki.gentoo.org/wiki/Lenovo_yoga_730
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2021-02-27"
fingerprint: "7f4c04b5d138026a"
license: CC BY-SA 4.0
---

# Lenovo yoga 730

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

## Hardware

intel gen8 i7 16gb ram nvidia gforce 1050 512GB nvm-e

i was not able to get the drives to be recognized when sata mode was set to RST in the bios. it would be nice that one didn't have to switch RST - AHCI in bios to get the stock windows installation to boot back and forth.

the touchpad is a i2c\_designware\_platform device. the following modules are necessary: 
intel\_lpss\_pci, i2c\_designware, pinctrl\_sunrisepoint, i2c\_hid.  To configure the touchpad as an *actual* touchpad with two-finger scrolling etc, then it is also necessary to enable the HID\_MULTITOUCH option in the kernel.  Without this, the touchpad may only be recognized as a pointer/mouse.  More information [at this forum post](https://forums.gentoo.org/viewtopic-t-1098936.html).

wifi driver for 4.14.83 is in Device Drivers --> Staging Drivers --> Realtek RTL8822BE Wireless Network Adapter

wifi driver for 5.4.38 is in Device Drivers --> Network device Support --> WIreless LAN --> Realtek 802.11ac wireless chip support --> Realtek 8822BE PCI wireless network adapter

todo:

sound
  touch screen and pen
  better tablet mode behavior

output of lspci

`root #``lspci`
00:00.0 Host bridge: Intel Corporation Device 5914 (rev 08)
00:02.0 VGA compatible controller: Intel Corporation Device 5917 (rev 07)
00:04.0 Signal processing controller: Intel Corporation Xeon E3-1200 v5/E3-1500 v5/6th Gen Core Processor Thermal Subsystem (rev 08)
00:14.0 USB controller: Intel Corporation Sunrise Point-LP USB 3.0 xHCI Controller (rev 21)
00:14.2 Signal processing controller: Intel Corporation Sunrise Point-LP Thermal subsystem (rev 21)
00:15.0 Signal processing controller: Intel Corporation Sunrise Point-LP Serial IO I2C Controller #0 (rev 21)
00:15.1 Signal processing controller: Intel Corporation Sunrise Point-LP Serial IO I2C Controller #1 (rev 21)
00:15.2 Signal processing controller: Intel Corporation Sunrise Point-LP Serial IO I2C Controller #2 (rev 21)
00:16.0 Communication controller: Intel Corporation Sunrise Point-LP CSME HECI #1 (rev 21)
00:1c.0 PCI bridge: Intel Corporation Sunrise Point-LP PCI Express Root Port #1 (rev f1)
00:1c.3 PCI bridge: Intel Corporation Device 9d13 (rev f1)
00:1c.4 PCI bridge: Intel Corporation Sunrise Point-LP PCI Express Root Port #5 (rev f1)
00:1d.0 PCI bridge: Intel Corporation Sunrise Point-LP PCI Express Root Port #9 (rev f1)
00:1e.0 Signal processing controller: Intel Corporation Sunrise Point-LP Serial IO UART Controller #0 (rev 21)
00:1f.0 ISA bridge: Intel Corporation Device 9d4e (rev 21)
00:1f.2 Memory controller: Intel Corporation Sunrise Point-LP PMC (rev 21)
00:1f.3 Audio device: Intel Corporation Sunrise Point-LP HD Audio (rev 21)
00:1f.4 SMBus: Intel Corporation Sunrise Point-LP SMBus (rev 21)
3a:00.0 Network controller: Realtek Semiconductor Co., Ltd. Device b822
3b:00.0 3D controller: NVIDIA Corporation GP107M \[GeForce GTX 1050 Mobile\] (rev a1)
3c:00.0 Non-Volatile memory controller: Samsung Electronics Co Ltd Device a808

`root #``lsusb`
Bus 002 Device 001: ID 1d6b:0003 Linux Foundation 3.0 root hub
Bus 001 Device 006: ID 0bda:b023 Realtek Semiconductor Corp. 
Bus 001 Device 005: ID 06cb:0081 Synaptics, Inc. 
Bus 001 Device 004: ID 13d3:56b2 IMC Networks 
Bus 001 Device 002: ID abcd:1234 Unknown 
Bus 001 Device 001: ID 1d6b:0002 Linux Foundation 2.0 root hub

output of lsmod on a fairly lean and working system:

`root #``lsmod`
Module                  Size  Used by
rfcomm                 36864  12
bnep                   20480  2
uvcvideo              106496  0
videobuf2\_vmalloc      16384  1 uvcvideo
videobuf2\_memops       16384  1 videobuf2\_vmalloc
videobuf2\_v4l2         24576  1 uvcvideo
btusb                  49152  0
btrtl                  16384  1 btusb
videodev              200704  2 videobuf2\_v4l2,uvcvideo
btbcm                  16384  1 btusb
btintel                20480  1 btusb
videobuf2\_common       49152  2 videobuf2\_v4l2,uvcvideo
bluetooth             425984  43 btrtl,btintel,btbcm,bnep,btusb,rfcomm
ecdh\_generic           16384  1 bluetooth
ecc                    28672  1 ecdh\_generic
nvidia\_drm             45056  0
mousedev               24576  0
bbswitch               16384  0
hid\_sensor\_custom      24576  0
wacom                 106496  0
hid\_sensor\_hub         20480  1 hid\_sensor\_custom
hid\_multitouch         28672  0
snd\_hda\_codec\_hdmi     57344  1
i2c\_designware\_platform    16384  0
i2c\_designware\_core    20480  1 i2c\_designware\_platform
intel\_rapl\_msr         20480  0
snd\_hda\_codec\_realtek   106496  1
wmi\_bmof               16384  0
snd\_hda\_codec\_generic    77824  1 snd\_hda\_codec\_realtek
nvidia\_modeset       1077248  1 nvidia\_drm
x86\_pkg\_temp\_thermal    20480  0
intel\_powerclamp       20480  0
snd\_hda\_intel          40960  6
snd\_intel\_nhlt         16384  1 snd\_hda\_intel
snd\_hda\_codec         122880  4 snd\_hda\_codec\_generic,snd\_hda\_codec\_hdmi,snd\_hda\_intel,snd\_hda\_codec\_realtek
snd\_hwdep              16384  1 snd\_hda\_codec
snd\_hda\_core           77824  5 snd\_hda\_codec\_generic,snd\_hda\_codec\_hdmi,snd\_hda\_intel,snd\_hda\_codec,snd\_hda\_codec\_realtek
snd\_pcm                98304  4 snd\_hda\_codec\_hdmi,snd\_hda\_intel,snd\_hda\_codec,snd\_hda\_core
rtwpci                 24576  0
i2c\_i801               28672  0
rtw88                 487424  1 rtwpci
processor\_thermal\_device    20480  0
intel\_rapl\_common      28672  2 intel\_rapl\_msr,processor\_thermal\_device
mei\_me                 40960  0
intel\_soc\_dts\_iosf     20480  1 processor\_thermal\_device
intel\_lpss\_pci         20480  0
mei                    77824  1 mei\_me
intel\_lpss             16384  1 intel\_lpss\_pci
mfd\_core               16384  2 hid\_sensor\_hub,intel\_lpss
intel\_pch\_thermal      16384  0
i2c\_hid                28672  0
ideapad\_laptop         24576  0
int3403\_thermal        16384  0
int340x\_thermal\_zone    16384  2 int3403\_thermal,processor\_thermal\_device
wmi                    24576  2 wmi\_bmof,ideapad\_laptop
acpi\_pad               20480  0
pinctrl\_sunrisepoint    28672  1
pinctrl\_intel          24576  1 pinctrl\_sunrisepoint
int3400\_thermal        16384  0
acpi\_thermal\_rel       16384  1 int3400\_thermal
nvidia              20185088  7 nvidia\_modeset
efivarfs               16384  1
configfs               36864  1
fuse                  118784  1
nfs                   266240  0
lockd                  90112  1 nfs
grace                  16384  1 lockd
sunrpc                344064  2 lockd,nfs

more to come
