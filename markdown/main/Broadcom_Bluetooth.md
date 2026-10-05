<!-- source: https://wiki.gentoo.org/wiki/Broadcom_Bluetooth | group: Gentoo Wiki (Main) | wiki-title: Broadcom Bluetooth -->
---
title: Broadcom Bluetooth
url: https://wiki.gentoo.org/wiki/Broadcom_Bluetooth
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2023-04-18"
fingerprint: "5c20f21d52bb1540"
license: CC BY-SA 4.0
---

# Broadcom Bluetooth

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

This article details setup for Broadcom Bluetooth 4.x devices mostly based on BCM20702, BCM4354, and BCM4356 chipsets. These bluetooth chipsets are also used in various devices including USB-dongles, hybrid WIFI+Bluetooth embedded chipsets, etc.

## Hardware

Mostly complete list of supported devices can be [found upstream](https://github.com/winterheart/broadcom-bt-firmware/blob/master/DEVICES.md).

| Device | Make/model | Status | Vendor ID / Product ID | Kernel driver(s) | Kernel version | Notes | 
|---|---|---|---|---|---|---|
| USB Dongle | Asus BT-400 USB |  | `0b05:17cb` | btbcm | 4.2 | Requires firmware | 
| USB Dongle | Targus ACB75AU |  | `0a5c:21e8` | btbcm | 3.4+ | Requires firmware brcm/BCM20702A1-0a5c-21e8.hcd | 

## Security considerations

Recently several vulnerabilities have been discovered in the Bluetooth stack such as [CVE-2018-5383](https://www.kb.cert.org/vuls/id/304725/), [CVE-2019-9506](https://www.kb.cert.org/vuls/id/918987/) (KNOB), [CVE-2020-10135](https://www.kb.cert.org/vuls/id/647177/) (BIAS) and others. Since Broadcom has stopped active support for its consumer devices, systems utilizing this software may be subject to security risks. It is wise to assess the risk before moving forward, since the repository maintainer cannot provide security fixes.

## Installation

### Kernel

Broadcom Bluetooth devices require the `btbcm` kernel module, which can be built with these kernel options:

**Broadcom Bluetooth support**

### Firmware

Mostly Broadcom Bluetooth stack requires external firmware, supplied with Windows drivers. This can be verified by using following commands:

`user $``dmesg | grep -i bluetooth`
Bluetooth: hci1: BCM: chip id 63
Bluetooth: hci1: BCM20702A
Bluetooth: hci1: BCM20702A1 (001.002.014) build 0000
bluetooth hci1: Direct firmware load for brcm/BCM20702A1-0b05-17cb.hcd failed with error -2
Bluetooth: hci1: BCM: Patch brcm/BCM20702A1-0b05-17cb.hcd not found

Luckily, the [sys-firmware/broadcom-bt-firmware](https://packages.gentoo.org/packages/sys-firmware/broadcom-bt-firmware) package can be used to install the most recent firmware files for Broadcom Bluetooth:

`root #``emerge --ask sys-firmware/broadcom-bt-firmware`
After installation, reinsert Bluetooth device or reboot system for applying firmware. After the reboot the output should look something like the following:

`user $``dmesg | grep -i bluetooth`
Bluetooth: hci1: BCM: chip id 63
Bluetooth: hci1: BCM20702A
Bluetooth: hci1: BCM20702A1 (001.002.014) build 0000
Bluetooth: hci1: BCM20702A1 (001.002.014) build 1467
Bluetooth: hci1: Broadcom Bluetooth Device

## Configuration

After enabling the kernel options and installing firmware, proceed to the [Bluetooth](https://wiki.gentoo.org/wiki/Bluetooth) article.

## Troubleshooting

### Kernel requires BCM.hcd or BCM\<CHIPSET>.hcd

Some VID/PIDs not yet defined in kernel driver so btbcm cannot properly identify the device:

`user $``dmesg | grep -i bluetooth`
Bluetooth: hci1: BCM: chip id 63
Bluetooth: hci1: BCM20702A
Bluetooth: hci1: BCM20702A1 (001.002.014) build 0000
bluetooth hci1: Direct firmware load for brcm/BCM.hcd failed with error -2
Bluetooth: hci1: BCM: Patch brcm/BCM.hcd not found

In this case VID/PID will be manually retrieved via the lspci or lsusb commands:

`user $``lsusb`
...
Bus 003 Device 005: ID 0b05:17cb ASUSTek Computer, Inc. Broadcom BCM20702A0 Bluetooth
...

Here the VID/PID - `0b05:17cb`. Next, check the [Devices list](https://github.com/winterheart/broadcom-bt-firmware/blob/master/DEVICES.md) and choose firmware as appropriate. After that just copy firmware file into name that requires kernel:

`root #````
cd /lib/firmware/brcm
```
`root #````
cp BCM20702A1-0b05-17cb.hcd BCM.hcd
```
After that reinsert device or reboot system.

### After installing firmware device still won't work

Some Bluetooth controller (for example, BCM4354 and BCM4356) are integrated to WiFi chipset (this can be BCM43XX 802.11ac Wireless Network Adapter or just simple generic Broadcom PCIE Wireless). These devices requires two kinds of firmware - first for WiFi, and second for Bluetooth. Without WiFi firmware Bluetooth will not initialize and will not work properly. Firmware for WiFi already included to kernel, but some additional work may be necessary to [place correct NVRAM](https://wireless.wiki.kernel.org/en/users/drivers/brcm80211#broadcom_brcmfmac_driver).

Here example how it can looks (note about brcm/brcmfmac4356-pcie.txt loading - this is the customized NVRAM):

`user $``dmesg`
usbcore: registered new interface driver brcmfmac
brcmfmac 0000:02:00.0: firmware: direct-loading firmware brcm/brcmfmac4356-pcie.bin
brcmfmac 0000:02:00.0: firmware: direct-loading firmware brcm/brcmfmac4356-pcie.txt
Bluetooth: hci0: BCM: chip id 101
Bluetooth: hci0: N360-11
Bluetooth: hci0: BCM4354A2 (001.003.015) build 0000
bluetooth hci0: firmware: direct-loading firmware brcm/BCM4354A2-13d3-3485.hcd

## See also

- [Bluetooth](https://wiki.gentoo.org/wiki/Bluetooth) — describes the configuration and usage of Bluetooth controllers and devices.
