<!-- source: https://wiki.gentoo.org/wiki/Embedded_Handbook/Boards/Pine64/QuartzPro64 | group: Gentoo Wiki (Main) | wiki-title: Embedded Handbook/Boards/Pine64/QuartzPro64 -->
---
title: Embedded Handbook/Boards/Pine64/QuartzPro64
url: https://wiki.gentoo.org/wiki/Embedded_Handbook/Boards/Pine64/QuartzPro64
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2024-04-15"
fingerprint: "6c67bcfa47cd39d3"
license: CC BY-SA 4.0
---

# Embedded Handbook/Boards/Pine64/QuartzPro64

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

This is a VERY early page for the QuartzPro64 development board. The primary purpose at this time is to create a path to the install guide for Gentoo; the hardware specs on this page are largely inaccurate having been "cut and pasted" from the RockPro64 page which was used as a template.

The PINE64 QuartzPro64 is a Rockchip RK3588 (ARMv8-A, Cortex-A76/A55 big.LITTLE) based, exceptionally libre software friendly SBC. It supports booting and even 3D hardware acceleration without any proprietary or closed source firmware blobs. The vendor focuses solely on hardware so there's no official software support. However, a [vibrant and open community](https://wiki.pine64.org/wiki/Main_Page#Community_and_Support) that provides extraordinary libre software support gathered around their products.

## Hardware

|  | Make/model | Notes | 
|---|---|---|
| Board | QuartzPro64 | Filename of device tree binary (development board): rk3588-quartzpro64.dtb | 
| SoC | Rockchip RK3588 |  | 
| RAM | 16GB |  | 
| Firmware | [U-Boot](https://www.denx.de/wiki/U-Boot), [Trusted Firmware A](https://developer.trustedfirmware.org/project/profile/1/) (ATF)[\[1\]](https://wiki.gentoo.org#cite_note-1) | FLOSS, works without any proprietary blobs <sup>[\[2\]](https://wiki.gentoo.org#cite_note-2)</sup> | 
| Boot media | eMMC, SD Card, USB, PXE | Enabling boot from USB in U-Boot might have [issues](https://wiki.gentoo.org/wiki/PINE64_ROCKPro64#Troubleshooting). | 

### SoC

| Component | Make/model | Status | Kernel driver(s) | Kernel version | Notes | 
|---|---|---|---|---|---|
| CPU | 4 x ARM Cortex-A76@2.4Ghz (ARMv8-A) 4 x ARM Cortex-A55@1.8Ghz (ARMv8-A) |  |  | 6.1 | big.LITTLE | 
| GPU | 4 x Mali-G610MC4 (4x Valhalla cores) |  | panfrost, rockchip\_drm, drm\_fbdev\_emulation, rockchip\_iommu | 6.1 |  | 
| HDMI | Synopsys Designware IP |  | rockchip\_dw\_hdmi | 6.1 |  | 
| MMC | Synopsys Designware IP |  | mmc\_dw\_rockchip, pwrseq\_emmc | 6.1 |  | 
| SDHCI | Arasan SDHCI |  | mmc\_sdhci\_of\_arasan | 6.1 |  | 
| Ethernet MAC |  |  | dwmac\_rockchip | 6.1 | 1 GBit | 
| USB Type-C | Fairchild FUSB302 |  | typec\_fusb302 | 6.1 | PD, alternate mode DP | 
| USB-A 3.0 |  |  | xhci\_platform | 6.1 |  | 
| USB 2.0 |  |  | ehci\_platform | 6.1 |  | 
| DMA engine | PL330 |  | pl330\_dma | 6.1 |  | 
| HDMI audio | Synopsis Designware IP |  | soc\_rockchip\_i2s, drm\_dw\_hdmi\_i2s\_audio, simple\_card | 6.1 |  | 

### Peripherals

| Component | Make/model | Status | Kernel driver(s) | Kernel version | Notes | 
|---|---|---|---|---|---|
| PMIC | Rockchip RK806 |  | rk806 | 6.1 | Power Management Integrated Circuit (Regulators, RTC, Clocking) | 
| Ethernet PHY | Realtek RTL8211F |  | realtek\_phy | 6.1 | via RGMII | 
| Analog Audio |  |  | es8388, audio\_graph\_card | 6.1 |  | 

## General Hardware

- RK3588 SoC (8 cores: 4x A76@2.4GHz + 4x A55@1.8GHz)
- Mali G610MC4 GPU (4x Valhalla cores)
- 16 GB LPDDR4X (SK hynix)
- 64 GB eMMC (soldered)
- 1x USB-C (with video-alt mode)
- 1x USB-C (FTDI debug UART)
- 1x USB 3.0
- 2x USB 2.0
- 1x HDMI in
- 2x HDMI out
- 1x PCIe 3.0 x4 slot (open-ended)
- 1x SD / TF card slot
- 3x SATA ports
- 2x Gigabit ethernet
- 1x WiFi & BT Module
- 2x SMA Antenna
- 2x MIPI DPHY
- 1x MIPI D/C PHY
- 1x MIPI CSI
- 1x Fan header
- 1x RTC battery socket
- 1x MIC (soldered)
- 1x audio output 3.5mm jack
- Power in via DC 12V

## GCC optimization

**`/etc/portage/make.conf`**

**RK33588 example**

```
COMMON_FLAGS="-O2 -pipe"
CFLAGS="${COMMON_FLAGS}"
CXXFLAGS="${COMMON_FLAGS}"
```
## Installing the bootloader

To build the U-Boot bootloader from source, see [PINE64\_QuartzPro64/Installing\_U-Boot](https://wiki.gentoo.org/wiki/PINE64_QuartzPro64/Installing_U-Boot).

## Installing Gentoo

Consult [PINE64 QuartzPro64/Installing Gentoo](https://wiki.gentoo.org/wiki/PINE64_QuartzPro64/Installing_Gentoo) for instructions on how to install Gentoo on the Pine64 QuartzPro64.

## Issues

Depending on board revision and U-Boot version setting `CONFIG_USE_PREBOOT=y` (which enables boot from USB) in U-Boot might cause the boot process to get stuck at `Booting using the fdt blob at 0x1f00000` even when booting from SDXC or eMMC.[\[3\]](https://wiki.gentoo.org#cite_note-3)

## External resources

## References

1. [↑](https://wiki.gentoo.org#cite_ref-1) [ARM Trusted Firmware](https://developer.arm.com/documentation/dui0928/e/firmware/application-processor--ap--firmware/arm-trusted-firmware), [arm Developer](https://developer.arm.com/). Retrieved on January 25th, 2021
2. [↑](https://wiki.gentoo.org#cite_ref-2) [Blobless boot with RockPro64](https://stikonas.eu/wordpress/2019/09/15/blobless-boot-with-rockpro64/), [Andrius Štikonas](https://stikonas.eu). Retrieved on January 25th, 2021
3. [↑](https://wiki.gentoo.org#cite_ref-3) [Unstable boot on 2GB and 4GB RockPros with 2020.10](https://gitlab.manjaro.org/manjaro-arm/packages/core/uboot-rockpro64/-/issues/4), [All things related to Manjaro-ARM](https://gitlab.manjaro.org/manjaro-arm). Retrieved on January 25th, 2021
