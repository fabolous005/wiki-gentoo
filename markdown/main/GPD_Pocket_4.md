<!-- source: https://wiki.gentoo.org/wiki/GPD_Pocket_4 | group: Gentoo Wiki (Main) | wiki-title: GPD Pocket 4 -->
---
title: GPD Pocket 4
url: https://wiki.gentoo.org/wiki/GPD_Pocket_4
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2025-06-29"
fingerprint: "7552897d996213c2"
license: CC BY-SA 4.0
---

# GPD Pocket 4

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**GPD Pocket 4** is an 8" screen laptop from GPD Corporation.

## Hardware

### Standard

| Device | Make/model | Status | Vendor ID / Product ID | Kernel driver(s) | Kernel version | Notes | 
|---|---|---|---|---|---|---|
| CPU | AMD Ryzen™ AI 9 HX 370 |  | N/A | N/A | 6.14.9 |  | 
| Video card | AMD Radeon™ 890M |  | 1002:150e | amdgpu | 6.14.9 |  | 
| Ethernet controller | Realtek Semiconductor Co., Ltd. RTL8125 2.5GbE Controller |  | 10ec:8125 | r8169 | 6.14.9 |  | 
| Wireless network controller | Intel Corporation Wi-Fi 6E(802.11ax) AX210/AX1675 |  | 8086:2725 | iwlwifi | 6.14.9 |  | 

## LTE broadband

GPD offers Quectel EC25 LTE modem as an optional module. By default, the modem *fcc-lock* state and needs to be unlocked in order to connect to any network.

Default unlocking by ModemManager can be enabled by linking the following file:

`root #``ln -s ../../../usr/share/ModemManager/fcc-unlock.available.d/2c7c /etc/ModemManager/fcc-unlock.d/`
