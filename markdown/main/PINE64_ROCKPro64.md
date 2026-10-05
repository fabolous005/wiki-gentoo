<!-- source: https://wiki.gentoo.org/wiki/PINE64_ROCKPro64 | group: Gentoo Wiki (Main) | wiki-title: PINE64 ROCKPro64 -->
---
title: PINE64 ROCKPro64
url: https://wiki.gentoo.org/wiki/PINE64_ROCKPro64
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2024-04-12"
fingerprint: "3f73bc9047cd2950"
license: CC BY-SA 4.0
---

# PINE64 ROCKPro64

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

The PINE64 ROCKPro64 is a Rockchip RK3399 (ARMv8-A, Cortex-A72/A53 big.LITTLE) based, exceptionally libre software friendly SBC. It supports booting and even 3D hardware acceleration without any proprietary or closed source firmware blobs. The vendor focuses solely on hardware so there's no official software support. However, a [vibrant and open community](https://wiki.pine64.org/wiki/Main_Page#Community_and_Support) that provides extraordinary libre software support gathered around their products.

## Hardware

|  | Make/model | Notes | 
|---|---|---|
| Board | RockPro64 | Filename of device tree binary (board rev 2.1): rk3399-rockpro64.dtb | 
| SoC | Rockchip RK3399 |  | 
| RAM | 4GB | 2GB version available, LPDDR4 | 
| Firmware | [U-Boot](https://www.denx.de/wiki/U-Boot), [Trusted Firmware A](https://developer.trustedfirmware.org/project/profile/1/) (ATF)[\[1\]](https://wiki.gentoo.org#cite_note-1) | FLOSS, works without any proprietary blobs <sup>[\[2\]](https://wiki.gentoo.org#cite_note-2)</sup> | 
| Boot media | eMMC, SD Card, USB, PXE | Enabling boot from USB in U-Boot might have [issues](https://wiki.gentoo.org/wiki/PINE64_ROCKPro64#Troubleshooting). | 

### SoC

| Component | Make/model | Status | Kernel driver(s) | Kernel version | Notes | 
|---|---|---|---|---|---|
| CPU | 2 x ARM Cortex-A72 (ARMv8-A) 4 x ARM Cortex-A53 (ARMv8-A) |  |  | 5.10 | big.LITTLE | 
| GPU | 4 x Mali-T860 |  | panfrost, rockchip\_drm, drm\_fbdev\_emulation, rockchip\_iommu | 5.10 | Panfrost is needed for hardware 3D acceleration, doesn't yet support OpenGL 3.30 ( [media-libs/mesa](https://packages.gentoo.org/packages/media-libs/mesa) 20.x). | 
| HDMI | Synopsys Designware IP |  | rockchip\_dw\_hdmi | 5.10 |  | 
| MMC | Synopsys Designware IP |  | mmc\_dw\_rockchip, pwrseq\_emmc | 5.10 |  | 
| SDHCI | Arasan SDHCI |  | mmc\_sdhci\_of\_arasan | 5.10 |  | 
| Ethernet MAC |  |  | dwmac\_rockchip | 5.10 | 1 GBit | 
| USB Type-C | Fairchild FUSB302 |  | typec\_fusb302 | 5.10 | PD, alternate mode DP | 
| USB-A 3.0 |  |  | xhci\_platform | 5.10 |  | 
| USB 2.0 |  |  | ehci\_platform | 5.10 |  | 
| DMA engine | PL330 |  | pl330\_dma | 5.10 |  | 
| HDMI audio | Synopsis Designware IP |  | soc\_rockchip\_i2s, drm\_dw\_hdmi\_i2s\_audio, simple\_card | 5.10 |  | 

### Peripherals

| Component | Make/model | Status | Kernel driver(s) | Kernel version | Notes | 
|---|---|---|---|---|---|
| PMIC | Rockchip RK808 |  | rk808 | 5.10 | Power Management Integrated Circuit (Regulators, RTC, Clocking) <sup>[\[3\]](https://wiki.gentoo.org#cite_note-3)</sup> | 
| Ethernet PHY | Realtek RTL8211F |  | realtek\_phy | 5.10 | via RGMII | 
| Analog Audio |  |  | es8316, audio\_graph\_card | 5.10 |  | 

## GCC optimization

**`/etc/portage/make.conf`**

**RK3399 example**

```
COMMON_FLAGS="-march=armv8-a+crc+crypto -mtune=cortex-a72.cortex-a53 -mfix-cortex-a53-835769 -mfix-cortex-a53-843419"
CFLAGS="${COMMON_FLAGS}"
CXXFLAGS="${COMMON_FLAGS}"
```
## Installing the bootloader

To build the U-Boot bootloader from source, see [PINE64\_ROCKPro64/Installing\_U-Boot](https://wiki.gentoo.org/wiki/PINE64_ROCKPro64/Installing_U-Boot).

## Installing Gentoo

Consult [PINE64 ROCKPro64/Installing Gentoo](https://wiki.gentoo.org/wiki/PINE64_ROCKPro64/Installing_Gentoo) for instructions on how to install Gentoo on the Pine64 ROCKPro64.

## Issues

Depending on board revision and U-Boot version setting `CONFIG_USE_PREBOOT=y` (which enables boot from USB) in U-Boot might cause the boot process to get stuck at `Booting using the fdt blob at 0x1f00000` even when booting from SDXC or eMMC.[\[4\]](https://wiki.gentoo.org#cite_note-4)

## External resources

## References

1. [↑](https://wiki.gentoo.org#cite_ref-1) [ARM Trusted Firmware](https://developer.arm.com/documentation/dui0928/e/firmware/application-processor--ap--firmware/arm-trusted-firmware), [arm Developer](https://developer.arm.com/). Retrieved on January 25th, 2021
2. [↑](https://wiki.gentoo.org#cite_ref-2) [Blobless boot with RockPro64](https://stikonas.eu/wordpress/2019/09/15/blobless-boot-with-rockpro64/), [Andrius Štikonas](https://stikonas.eu). Retrieved on January 25th, 2021
3. [↑](https://wiki.gentoo.org#cite_ref-3) Fuzhou Rockchip Electronics Co., Ltd., [RK808 Datasheet V0.8 (PDF)](https://files.pine64.org/doc/datasheet/rockpro64/RK808%20datasheet%20V0.8.pdf), [PINE64](http://www.pine64.org). Retrieved on January 25th, 2021
4. [↑](https://wiki.gentoo.org#cite_ref-4) [Unstable boot on 2GB and 4GB RockPros with 2020.10](https://gitlab.manjaro.org/manjaro-arm/packages/core/uboot-rockpro64/-/issues/4), [All things related to Manjaro-ARM](https://gitlab.manjaro.org/manjaro-arm). Retrieved on January 25th, 2021
