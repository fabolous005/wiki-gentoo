<!-- source: https://wiki.gentoo.org/wiki/Libre_Computer_Renegade | group: Gentoo Wiki (Main) | wiki-title: Libre Computer Renegade -->
---
title: Libre Computer Renegade
url: https://wiki.gentoo.org/wiki/Libre_Computer_Renegade
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2024-04-15"
fingerprint: "1960900a15ef9d46"
license: CC BY-SA 4.0
---

# Libre Computer Renegade

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

The Libre Computer Renegade (also known under the label Firefly ROC-RK3328-CC) is a Rockchip RK3328 (ARMv8-A, Cortex-A53) based single board computer. Its performance especially with regard to native compilation is rather limited (compile time of firefox-67: ca. 10h). However, Rockchip has been known for its relatively good open source support compared to other system on chip (SoC) manufacturers, especially since the [ASUS Chromebook C201](https://wiki.gentoo.org/wiki/ASUS_Chromebook_C201) which is supported by [Libreboot](https://libreboot.org). But despite Libre Computer claiming intentions to liberate their products, too, there are no signs yet that they will deliver on their promise.

## Hardware

|  | Make/model | Notes | 
|---|---|---|
| Board | Firefly ROC-RK3328-CC | filename of device tree binary: rk3328-roc-cc.dtb | 
| SoC | Rockchip RK3328 |  | 
| RAM | 4GB Samsung K4A8G165WB-BCRC | 1GB / 2GB versions available, DDR4 | 
| Firmware | proprietary (miniloader), [Trusted Firmware A](https://developer.trustedfirmware.org/project/profile/1/) (ATF)[\[1\]](https://wiki.gentoo.org#cite_note-1) |  | 
| Boot media | eMMC or SDXC | eMMC 5.0 supported; to boot from eMMC SDXC must be removed and vice versa | 

### SoC

| Component | Make/model | Status | Kernel driver(s) | Kernel version | Notes | 
|---|---|---|---|---|---|
| CPU | 4 x ARM Cortex-A53 (ARMv8-A) |  |  | 5.2 | current max @1,3GHz, specified max @1,4GHz | 
| GPU | 2G + 2P Mali-450 |  | Lima | 5.2 | [info](https://gitlab.freedesktop.org/lima/web/wikis/home), [code](https://gitlab.freedesktop.org/mesa), specified max @500MHz | 
| Ethernet MAC | STMicroelectronics (Synopsys DW IP) |  | stmmac (rk\_gmac-dwmac) | 4.20 | 1 GBit, external PHY (see [below](https://wiki.gentoo.org/wiki/Libre_Computer_Renegade#Peripherals)) | 
| USB 3.0 |  |  |  | 5.2 | 1 port | 
| USB 2.0 |  |  |  | 4.20 | 1 of the 2 ports works | 
| HDMI |  |  | dwhdmi | 4.20 | xorg-server via fbdev works | 
| Analog Audio | I2S |  | N/A | 5.2 |  | 

### Peripherals

| Component | Make/model | Status | Kernel driver(s) | Kernel version | Notes | 
|---|---|---|---|---|---|
| Ethernet PHY | Realtek RTL8211E |  |  | 4.20 | 1 GBit, connected to MAC via RGMII <sup>[\[2\]](https://wiki.gentoo.org#cite_note-2)</sup> | 
| PMIC | Rockchip RK805 |  | rk808 | 4.20 | Power Management Integrated Circuit (Regulators, RTC, Clocking) <sup>[\[3\]](https://wiki.gentoo.org#cite_note-3)</sup> | 

## Installing Gentoo

Consult [Libre Computer Renegade/Installing Gentoo](https://wiki.gentoo.org/wiki/Libre_Computer_Renegade/Installing_Gentoo) for instructions on how to install Gentoo on the Libre Computer Renegade.

## Software development kit

The parts of Firefly's Linux SDK that are essential to get started with the Libre Computer Renegade can be obtained and installed as follows<sup>[\[4\]](https://wiki.gentoo.org#cite_note-4)</sup>:

First, assemble the DDR init binary rk3328\_loader\_ddr786\_v1.06.243.bin needed for flashing to the eMMC:

`user $````
./build/mk-uboot.sh roc-rk3328-cc
```
Then install rkdeveloptool:

`user $````
./build/mk-uboot.sh roc-rk3328-cc
```
`user $````
cd rkdeveloptool
```
`user $````
autoreconf -i
```
`user $````
./configure
```
`user $````
make
```
`root #````
make install
```
## Maskrom Mode

To be able to flash to an eMMC installed on the Libre Computer Renegade the latter needs to be forced into the so called Maskrom Mode<sup>[\[5\]](https://wiki.gentoo.org#cite_note-5)</sup>:

- Pull all the USB cables (including micro USB cable and male to male USB cable) out of the board to keep the board powered off and if applicable pull out the SD card
- Use a male to male USB cable to connect the host system and the USB OTG port (lower one of double-decker, white) of the target device, for pictures see the [ROC-RK3328-CC Manual](https://roc-rk3328-cc.readthedocs.io/en/latest/flash_emmc.html#maskrom-mode)
- Connect the eMMC CLK and GND pads with metal tweezers while plugging in the micro USB cable to power on the board. Wait about 1 second before breaking the connection of the two pads

## Flashing to eMMC

With rkdeveloptool data (in the following example the INPUT\_FILE) can be flashed to an arbitrary offset (0x0 in the example) on an eMMC installed on the Libe Computer Renegade<sup>[\[6\]](https://wiki.gentoo.org#cite_note-6)</sup>:

`root #````
rkdeveloptool db      out/u-boot/rk3328_loader_ddr786_v1.06.243.bin
```
`root #````
rkdeveloptool wl 0x0  INPUT_FILE
```
`root #````
rkdeveloptool rd
```
## Recovery

If a kernel upgrade goes wrong and a backup is at hand the boot partition can be restored by forcing the device into Maskrom Mode and flashing the backup boot partition to the eMMC:

`root #````
rkdeveloptool db         out/uboot/rk3328_loader_ddr786_v1.06.243.bin
```
`root #````
rkdeveloptool wl 0x8000  /PATH/TO/ARBITRARY_BACKUP_LOCATION/rk3328-roc-cc-boot.img
```
`root #````
rkdeveloptool rd
```
## Recommended partition layout with offsets

|  | Purpose <sup>[\[7\]](https://wiki.gentoo.org#cite_note-7)</sup> | Start (Bytes) | End (Bytes) | Size (Bytes) | Partition Offset | 
|---|---|---|---|---|---|
| Partition 1 | preloader (miniloader or U-Boot SPL) | 32768 | 4128767 | 4096000 | 0x40 | 
| Partition 2 | U-Boot | 8388608 | 12582911 | 4194304 | 0x4000 | 
| Partition 3 | trusted OS (ATF) | 12582912 | 16777215 | 4194304 | 0x6000 | 
| Partition 4 | boot partition | 16777216 | 134217727 | 117440512 | 0x8000 | 
| Partition 5 | root partition | 134217728 | arbitrary | depends | 0x40000 | 

## References

1. [↑](https://wiki.gentoo.org#cite_ref-1) [ARM Trusted Firmware](http://infocenter.arm.com/help/index.jsp?topic=/com.arm.doc.dui0928e/CJHIDGJF.html), [ARM Information Center](http://infocenter.arm.com). Retrieved on June 21st, 2019
2. [↑](https://wiki.gentoo.org#cite_ref-2) Fuzhou Rockchip Electronics Co., Ltd., [RK3328 Datasheet Rev1.1 (PDF)](http://opensource.rock-chips.com/images/d/d7/Rockchip_RK3328_Datasheet_V1.1-20170309.pdf), [RK3328 - Rockchip open source Document](http://opensource.rock-chips.com/wiki_RK3328). Retrieved on June 28th, 2019
3. [↑](https://wiki.gentoo.org#cite_ref-3) Fuzhou Rockchip Electronics Co., Ltd., [RK805 Datasheet Rev1.1 (PDF)](http://files.pine64.org/doc/rock64/Rockchip_RK805_Datasheet_V1.1%C2%A020160921.pdf), [PINE64](http://www.pine64.org). Retrieved on June 28th, 2019
4. [↑](https://wiki.gentoo.org#cite_ref-4) Firefly Team, [Compiling linux firmware](https://roc-rk3328-cc.readthedocs.io/en/latest/linux_compile_firmware.html), [ROC-RK3328-CC Manual](https://roc-rk3328-cc.readthedocs.io/en/latest). Retrieved on May 31st, 2019
5. [↑](https://wiki.gentoo.org#cite_ref-5) Firefly Team, [Flashing to the eMMC - Maskrom Mode](https://roc-rk3328-cc.readthedocs.io/en/latest/flash_emmc.html#maskrom-mode), [ROC-RK3328-CC Manual](https://roc-rk3328-cc.readthedocs.io/en/latest). Retrieved on June 1st, 2019
6. [↑](https://wiki.gentoo.org#cite_ref-6) Firefly Team, [Flashing to the eMMC - rkdeveloptool](https://roc-rk3328-cc.readthedocs.io/en/latest/flash_emmc.html#rkdeveloptool), [ROC-RK3328-CC Manual](https://roc-rk3328-cc.readthedocs.io/en/latest). Retrieved on June 1st, 2019
7. [↑](https://wiki.gentoo.org#cite_ref-7) [Default storage map](http://opensource.rock-chips.com/wiki_Partitions), [Rockchip wiki](http://opensource.rock-chips.com/wiki_Main_Page). Retrieved on June 1st, 2019
