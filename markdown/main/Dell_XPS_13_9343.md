<!-- source: https://wiki.gentoo.org/wiki/Dell_XPS_13_9343 | group: Gentoo Wiki (Main) | wiki-title: Dell XPS 13 9343 -->
---
title: Dell XPS 13 9343
url: https://wiki.gentoo.org/wiki/Dell_XPS_13_9343
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2024-12-20"
fingerprint: c203132998aa1d93
license: CC BY-SA 4.0
---

# Dell XPS 13 9343

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

## Hardware specs

All variants of this model come with 5th-gen Intel processors and have an option of a touchscreen with HD+ display.

- [i3-5010U](https://ark.intel.com/products/84697/Intel-Core-i3-5010U-Processor-3M-Cache-2_10-GHz), [i5-5200U](https://ark.intel.com/products/85212/Intel-Core-i5-5200U-Processor-3M-Cache-up-to-2_70-GHz), [i7-5500U](https://ark.intel.com/products/85214/Intel-Core-i7-5500U-Processor-4M-Cache-up-to-3_00-GHz) or [i7-5600U](https://ark.intel.com/de/products/85215/Intel-Core-i7-5600U-Processor-4M-Cache-up-to-3_20-GHz) processor
- 13.3" LCD screen (3200x1800 touchscreen display or 1920x1080without touchscreen)
- Broadwell-U Integrated Graphics
- 4-8 GB DDR3 1600MHz RAM
- 128-512 GB Samsung SSD
- 2x USB 3 ports
- mini-DisplayPort output
- Broadcom DW1560 802.11a/b/g/n/ac + Bluetooth 4.0 *or* Intel Dual Band Wireless-AC 7265 802.11a/b/g/n/ac + Bluetooth 4.0

## make.conf

**`/etc/portage/make.conf`**

```
CFLAGS="-O2 -pipe -march=core-avx2 -mabm -madx -mavx256-split-unaligned-load -mavx256-split-unaligned-store -mprfchw -mrdseed"
CXXFLAGS="${CFLAGS}"
MAKEOPTS="-j4"
```
**`/etc/portage/package.use/00video`**

```
 VIDEO_CARDS: -* intel i96
```
**`/etc/portage/package.use/00input`**

```
 INPUT_DEVICES: evdev synaptics
```
**`/etc/portage/package.use/00cpu-flags`**

```
 CPU_FLAGS_X86: aes avx avx2 fma3 mmx mmxext popcnt sse sse2 sse3 sse4_1 sse4_2 ssse3
```
## Hardware Config

### USB

The two USB 3 ports require the xHCI driver to function with USB 3 devices and the EHCI driver to function with non-USB 3 devices. Any removable USB drive will also require USB Mass storage support.

**USB configuration, 4.0.5-gentoo**

### Wireless

The Broadcom BCM4352 wireless adapter requires the use of the official Broadcom driver. This driver is proprietary and requires that several kernel options be (un)set before installation:

**Configuration for broadcom-sta, 4.0.5-gentoo**

After compiling the new kernel, emerge broadcom-sta:

`root #``emerge --ask net-wireless/broadcom-sta`
Reboot to the new kernel and make sure the new Broadcom module is loaded

`root #``modprobe wl`
If all goes well, you should have a working wireless interface

`root #``iw list````
Wiphy phy0
        max # scan SSIDs: 1
        max scan IEs length: 0 bytes
        Retry short limit: 7
        Retry long limit: 4
        Coverage class: 0 (up to 0m)
        Supported Ciphers:
                * WEP40 (00-0f-ac:1)
                * WEP104 (00-0f-ac:5)
                * TKIP (00-0f-ac:2)
                * CCMP (00-0f-ac:4)
                * CMAC (00-0f-ac:6)
        Available Antennas: TX 0 RX 0
        Supported interface modes:
                 * IBSS
                 * managed
```
The configuration for the [Intel adapter](https://wiki.gentoo.org/wiki/Iwlwifi) is similar, but no other package than linux-firmware needs to be installed:

**Configuration for Intel, 4.2.0-gentoo-r1**

### Bluetooth

Much like the Broadcom wireless chipset, the Broadcom BCM2045A0 Bluetooth chipset also requires proprietary firmware. If you are booting the kernel using [EFI stub](https://wiki.gentoo.org/wiki/EFI_stub), you will need to compile the kernel bluetooth options as modules:

**Configuration for Intel, 4.5.3-gentoo**

If you compile the Bluetooth components into the kernel, the firmware is not available at the time the kernel loads, the firmware is not loaded and the Bluetooth system is not initialized. Compiling as a module ensures that the firmware is available at module autoload, and the system is initialized successfully. See [Broadcom Bluetooth](https://wiki.gentoo.org/wiki/Broadcom_Bluetooth) for additional info.

Remember to set the appropriate Bluetooth USE flags.

### ALSA sound

The built-in sound card has two different components: Standard audio output and audio output using the HDMI port. It is necessary to configure ALSA so that these are loaded in the proper order.

**Intel HD Audio, 4.0.5-gentoo**

**`/etc/modprobe.d/alsa.conf`**

### Synaptics Touchpad

The touchpad runs off of an I2C bus and needs some special kernel drivers to be installed:

**i2c touchpad, 4.0.5-gentoo**

### MMC/SD Card Reader

The MMC/SD slot is located on the right side of the laptop, next to the right USB port. It is relatively easy to use with the proper kernel config.

**Realtek PCI-E SD/MMC card interface, 4.0.5-gentoo**

**Realtek PCI-E SD/MMC card interface, 4.5.3-gentoo**

## Issues

### Loss of horizontal sync when switching TTYs

A bug is present in current kernel versions that results in horizontal sync loss on Broadwell machines when switching TTYs. [\[1\]](https://bugzilla.kernel.org/show_bug.cgi?id=95741) To fix it, you can add `i915.enable_ips=0` to your kernel command-line as a workaround.

### GPU hang/freeze4 with external display

An issue appears present on QHD display models that may result in GPU hangs/freezing when manipulating the displays with xrandr on kernels 4.0 and up. Ensure i915.preliminary\_hw\_support=0 is set as this appears to alleviate this issue.

### Display Blanking randomly

This issue appears to be related to high resolutions on both the internal display as well as 4k external displays. Preliminary testing indicates that the i915.preliminary\_hw\_support=0 option being set significantly reduces or removes the issue.

## See also

- [Intel](https://wiki.gentoo.org/wiki/Intel) - For configuring the Intel integrated graphics card
