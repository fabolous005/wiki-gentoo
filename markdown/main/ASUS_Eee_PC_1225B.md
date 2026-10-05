<!-- source: https://wiki.gentoo.org/wiki/ASUS_Eee_PC_1225B | group: Gentoo Wiki (Main) | wiki-title: ASUS Eee PC 1225B -->
---
title: ASUS Eee PC 1225B
url: https://wiki.gentoo.org/wiki/ASUS_Eee_PC_1225B
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-05-10"
fingerprint: df0588d4bd9eb3dd
license: CC BY-SA 4.0
---

# ASUS Eee PC 1225B

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

## Backlight support

Brightness LCD may changed by Fn+F5/Fn+F6. To support it you must config kernel

**Backlight**

This options also add other functional of your laptop.

## USB 3.0 support

USB 3.0 provides by ASMedia ASM1042 SuperSpeed Controller. Gentoo sources have drivers for it

**USB Support**

## 802.11 WiFi

WiFi is provided by Broadcom BCM4313 802.11bgn Wireless Network Adapter

**WiFi Support**

Minstrel and its 802.11n support is a rate control algorithm<sup>[\[1\]](https://wiki.gentoo.org#cite_note-1)</sup> should always be activated.[\[2\]](https://wiki.gentoo.org#cite_note-2)

## Bluetooth

Bluetooth is provided by Broadcom(?) chip.

**Bluetooth support**

## Ethernet

Ethernet is provided by a Realtek RTL8101E Fast Ethernet device.

**Ethernet Support**

You can compile ethernet driver as module, then you must check loading this module.

## Graphics with open-source radeon drivers

All the necessary firmware is provided with [sys-kernel/linux-firmware](https://packages.gentoo.org/packages/sys-kernel/linux-firmware) package (it is easy to emerge within a chrooted environment):

`root #``emerge --ask sys-kernel/linux-firmware`
Kernel parameters are well described in [the article about Radeon](https://wiki.gentoo.org/wiki/Radeon#Installation). However the choice of firmware isn't that obvious. As it is supposed [here](https://forums.gentoo.org/viewtopic-t-907980.html), APU is usually detected as PALM, while in fact it is closer to SUMO. The only way out is to make the driver a module and load after the root file system is mounted:

**Radeon drivers**

This lets kernel modesetting work properly.

### Hardware acceleration video

**2026-05-09**, the information in this section is probably

**outdated**. You can help the Gentoo community by verifying and

[updating this section](https://wiki.gentoo.org/index.php?title=ASUS_Eee_PC_1225B&action=edit).

Laptop has GPU Radeon HD6320. This card support some codecs for hardware acceleration:

- MPEG1
- MPEG2
- H264
- VC1
- MPEG4

The GPU can decode several videos without 100% load CPU.

For support hardware acceleration you must edit your install libs and player which support VDPAU technology. Enabling USE flag `vdpau` and update world.

**`/etc/portage/make.conf`**

```
USE="... vdpau ..."
```
Now you can manually setup the name of back-end driver with help of VDPAU\_DRIVER environment variable. To do that you need to add the following line to \~/.bashrc file (provided that Bash is the default shell of a user who is going to run graphical environment).

**`~/.bashrc`**

**Loged as a user who run graphical interface**

```
export VDPAU_DRIVER=r600
```
After installation, check vdpauinfo for get info

`user $``vdpauinfo`
display: :0   screen: 0
API version: 1
Information string: G3DVL VDPAU Driver Shared Library version 1.0
Video surface:
name   width height types
-------------------------------------------
420    16384 16384  NV12 YV12 
422    16384 16384  UYVY YUYV 
444    16384 16384  Y8U8V8A8 V8U8Y8A8 
Decoder capabilities:
name                        level macbs width height
----------------------------------------------------
MPEG1                           0  9216  2048  1152
MPEG2\_SIMPLE                    3  9216  2048  1152
MPEG2\_MAIN                      3  9216  2048  1152
H264\_BASELINE                  41  9216  2048  1152
H264\_MAIN                      41  9216  2048  1152
H264\_HIGH                      41  9216  2048  1152
VC1\_SIMPLE                     --- not supported ---
VC1\_MAIN                       --- not supported ---
VC1\_ADVANCED                    4  9216  2048  1152
MPEG4\_PART2\_SP                  3  9216  2048  1152
MPEG4\_PART2\_ASP                 5  9216  2048  1152
DIVX4\_QMOBILE                  --- not supported ---
DIVX4\_MOBILE                   --- not supported ---
DIVX4\_HOME\_THEATER             --- not supported ---
DIVX4\_HD\_1080P                 --- not supported ---
DIVX5\_QMOBILE                  --- not supported ---
DIVX5\_MOBILE                   --- not supported ---
DIVX5\_HOME\_THEATER             --- not supported ---
DIVX5\_HD\_1080P                 --- not supported ---
H264\_CONSTRAINED\_BASELINE      --- not supported ---
H264\_EXTENDED                  --- not supported ---
H264\_PROGRESSIVE\_HIGH          --- not supported ---
H264\_CONSTRAINED\_HIGH          --- not supported ---
H264\_HIGH\_444\_PREDICTIVE       --- not supported ---
HEVC\_MAIN                      --- not supported ---
HEVC\_MAIN\_10                   --- not supported ---
HEVC\_MAIN\_STILL                --- not supported ---
HEVC\_MAIN\_12                   --- not supported ---
HEVC\_MAIN\_444                  --- not supported ---
Output surface:
name              width height nat types
----------------------------------------------------
B8G8R8A8         16384 16384    y  NV12 YV12 UYVY YUYV Y8U8V8A8 V8U8Y8A8 A4I4 I4A4 A8I8 I8A8 
R8G8B8A8         16384 16384    y  NV12 YV12 UYVY YUYV Y8U8V8A8 V8U8Y8A8 A4I4 I4A4 A8I8 I8A8 
R10G10B10A2      16384 16384    y  NV12 YV12 UYVY YUYV Y8U8V8A8 V8U8Y8A8 A4I4 I4A4 A8I8 I8A8 
B10G10R10A2      16384 16384    y  NV12 YV12 UYVY YUYV Y8U8V8A8 V8U8Y8A8 A4I4 I4A4 A8I8 I8A8 
Bitmap surface:
name              width height
------------------------------
B8G8R8A8         16384 16384
R8G8B8A8         16384 16384
R10G10B10A2      16384 16384
B10G10R10A2      16384 16384
A8               16384 16384
Video mixer:
feature name                    sup
------------------------------------
DEINTERLACE\_TEMPORAL             y
DEINTERLACE\_TEMPORAL\_SPATIAL     -
INVERSE\_TELECINE                 -
NOISE\_REDUCTION                  y
SHARPNESS                        y
LUMA\_KEY                         -
HIGH QUALITY SCALING - L1        -
HIGH QUALITY SCALING - L2        -
HIGH QUALITY SCALING - L3        -
HIGH QUALITY SCALING - L4        -
HIGH QUALITY SCALING - L5        -
HIGH QUALITY SCALING - L6        -
HIGH QUALITY SCALING - L7        -
HIGH QUALITY SCALING - L8        -
HIGH QUALITY SCALING - L9        -
parameter name                  sup      min      max
-----------------------------------------------------
VIDEO\_SURFACE\_WIDTH              y        48     2048
VIDEO\_SURFACE\_HEIGHT             y        48     1152
CHROMA\_TYPE                      y  
LAYERS                           y         0        4
attribute name                  sup      min      max
-----------------------------------------------------
BACKGROUND\_COLOR                 y  
CSC\_MATRIX                       y  
NOISE\_REDUCTION\_LEVEL            y      0.00     1.00
SHARPNESS\_LEVEL                  y     -1.00     1.00
LUMA\_KEY\_MIN\_LUMA                y  
LUMA\_KEY\_MAX\_LUMA                y

VDPAU support:

#### VLC player

VLC have support vdpau. To use it you must enable USE flag `vdpau`. If you enable it to `/etc/portage/make.conf`, it not necessary.

In VLC go to settings `(Ctrl+P) --> Input / Codecs --> Hardware-accelerated decoding` choose `VDPAU video decoder`

When VLC play movie with hardware acceleration load CPU is 30-40%.

## Audio

### ALSA configuration

**Alsa configuration**

A good way to manage ALSA is to emerge alsa-utils:

`root #``emerge --ask alsa-utils`
To start alsa at boot time type:

`root #````
rc-update add alsasound boot
```
There is a problem that actually ALSA often sees two sound cards: HD-Audio Generic (that is a kind of a virtual device, i.e. it doesn't actually play music) and the required HDA ATI SB. The simplest way to check it is to launch

`user $``aplay -l`
\*\*\*\* List of PLAYBACK Hardware Devices \*\*\*\*
card 0: Generic \[HD-Audio Generic\], device 3: HDMI 0 \[HDMI 0\]
  Subdevices: 1/1
  Subdevice #0: subdevice #0
card 1: SB \[HDA ATI SB\], device 0: ALC269VB Analog \[ALC269VB Analog\]
  Suvdevices: 1/1
  Subdevice #0: subdevice #0

If HD-Audio Generic is the first number, it might be the default card. If it is the case, than you're likely to have no sound unless the default card is switched from 0 to 1.

**`/etc/asound.conf`**

## Memory Card Reader

Card reader is provided by Alcor Micro.

## Webcam

Webcam is supported with standart UVC

**Webcam Support**

## Tips and tricks

If the laptop stops responding to keyboard and touchpad, add some parameters to kernel

**`/etc/default/grub`**

Then, rebuild grub config

`root #``grub-mkconfig -o /boot/grub/grub.cfg`
Finally, reboot

## See aslo
