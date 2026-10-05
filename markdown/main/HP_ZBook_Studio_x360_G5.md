<!-- source: https://wiki.gentoo.org/wiki/HP_ZBook_Studio_x360_G5 | group: Gentoo Wiki (Main) | wiki-title: HP ZBook Studio x360 G5 -->
---
title: HP ZBook Studio x360 G5
url: https://wiki.gentoo.org/wiki/HP_ZBook_Studio_x360_G5
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2024-04-29"
fingerprint: bf570b5f5fa2a39d
license: CC BY-SA 4.0
---

# HP ZBook Studio x360 G5

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

# Linux Installation

## Kernel Configuration - the easy way

The easiest way to get a running Linux Kernel with support of all devices of the ZBook Studio x360 G5 is to use the config-file of the newest Gentoo-Live-CD or Gentoo-Live-USB-Stick and strip it down to what you really need. Afterwards it could be tweaked to achieve best performance or/and special demands.

## Kernel tweaking

### Kernel Type and Processor Family

Intel(R) Xeon(R) E-2186M CPU @ 2.90GHz

### Storage

Silicon Motion, Inc. Device 2262 (rev 03) (prog-if 02 \[NVM Express\])

For details see [NVMe](https://wiki.gentoo.org/wiki/NVMe)

### Video - Nouveau Driver vs. Nvidia proprietary driver

- Intel Corporation UHD Graphics 630 (Mobile) (prog-if 00 \[VGA controller\])

If there is no line "00:02.0 VGA compatible controller: Intel Corporation Coffee Lake-S GT2 \[UHD Graphics P630\]" in the lspci-listing you have to enable "Hybrid Graphics" in the advanced settings of the BIOS.

- NVIDIA Corporation GP107GLM \[Quadro P1000 Mobile\] (rev a1) (prog-if 00 \[VGA controller\])

Although there are actually some drawbacks on backlight control which are currently not fixed it's a good idea to use the Nvidia proprietary driver. It is more performant and causes less CPU workload. This is especially important for high-performance demanding applications.

#### Usage of the Nouveau driver

For details see [Intel](https://wiki.gentoo.org/wiki/Intel) and [nouveau & nvidia-drivers switching](https://wiki.gentoo.org/wiki/Nouveau_%26_nvidia-drivers_switching) articles.  Note that with models with discrete graphics, the HDMI port can only be used with the discrete graphics enabled.

Bumblebee is no longer required to use the HDMI port with hybrid graphics.  The earliest versions have not been verified with this laptop, but this will work starting with at least Linux 5.4, [x11-base/xorg-server](https://packages.gentoo.org/packages/x11-base/xorg-server) 1.20.8, and [x11-drivers/xf86-video-nouveau](https://packages.gentoo.org/packages/x11-drivers/xf86-video-nouveau) 1.0.16.

#### Usage of the Nvidia proprietary driver

To get more information about using the Nvidia proprietary driver please take a look at the  [Gentoo NVIDIA/nvidia-drivers](https://wiki.gentoo.org/wiki/Nvidia-drivers) wiki page.



#### Console Fonts on HiDPI displays (very small characters)

On modern displays with high DPI ("HiDPI"), e.g. UHD (3840x2160), the standard font will look very small. If you like to have a bigger (readable) font, Terminus can be used, which resembles a BIOS built-in textmode font.

To select this font in-kernel, `CONFIG_FONT_TER16x32` has to be enabled.

**Kernel compiled-in fonts**

### Wireless

Intel Corporation Wireless-AC 9560 \[Jefferson Peak\] (rev 10)

See the [iwlwifi](https://wiki.gentoo.org/wiki/Iwlwifi) article for more information as this laptop does suffer from not being able to connect to some access points without disabling 802.11n and enabling software crypto.

### Audio

Intel Corporation Cannon Lake PCH cAVS (rev 10) (prog-if 80)

### Bluetooth

Deeper information about the kernel configuration for bluetooth could be found in this  [Wiki-page](https://wiki.gentoo.org/wiki/Bluetooth#Kernel)

For details see [Bluetooth](https://wiki.gentoo.org/wiki/Bluetooth)

### Card Reader

Realtek Semiconductor Co., Ltd. RTS525A PCI Express Card Reader (rev 01)

### Touchpad / Trackpad

### Touchscreen

### Accelerometer

ST Microelectronics LIS3LV02DL Accelerometer

## Bootloader - using GRUB

Using GRUB for loading the operating system provides the same problem as mentioned above for the Linux console. The ZBook start in HiDPI resolution mode and the characters on the screen are very small and hard to read.

### Tweaking GRUB

To get a bigger font also for the grub-menue you can set the font to terminus-font

#### media-fonts/terminus-font

Emerge [media-fonts/terminus-font](https://packages.gentoo.org/packages/media-fonts/terminus-font) :

`root #``emerge --ask media-fonts/terminus-font`
Afterwards font-conversion is needed from .otb format to the .pf2 format. This format could then be used by grub.
For more information about the font configuration please read this  [WIKI-Page](https://wiki.gentoo.org/wiki/GRUB)

`root #``grub-mkfont -s 32 -o /boot/grub/fonts/terminus32b.pf2 /usr/share/fonts/terminus/ter-u32b.otb`
The font then has to be set as `GRUB_FONT` in `/etc/default/grub` in order to be used.

**`/etc/default/grub`**

**Framebuffer related settings**

```
# Use a custom font, converted using grub-mkfont utility
GRUB_FONT="/boot/grub/fonts/terminus32b.pf2"
```
Updating the GRUB configuration file `grub.cfg` will then activate the configuration with the new font.

`root #``grub-mkconfig -o /boot/grub/grub.cfg`
## "The sound of silence" - Thermal Subsystem and FAN control (optional)

- The HP ZBook Studio x360 G5 provides the following thermal subsystem (e.g. for the Intel(R) Xeon(R) E-2186M CPU):

`root #````
lspci 
```
....
00:04.0 Intel Corporation Xeon E3-1200 v5/E3-1500 v5/6th Gen Core Processor Thermal Subsystem (rev 07)

Without any precausions the "ZBook Studio x360 G5" causes some annoying fan noise although the CPU temperature is wide under an acceptabe thermal threshold. A solution for this problem is the usage of a combination of thermal monitoring and fan control on the operating system side.

### Thermal monitoring - lm\_sensors

The most widely used package for thermal monitoring ist lm-sensors. An extensive description of this package could be found on the  [Gentoo lm\_sensors WIKI page](https://wiki.gentoo.org/wiki/Lm_sensors)

### User space fan control application: nbfc-linux

A good tool for fan control is the user-space tool nbfc-linux which is a C-port of the "Hirschmann" application nbfc implemented under the usage of Mono.
The nbfc-linux tool can be found [here](https://github.com/nbfc-linux/nbfc-linux) at github.
