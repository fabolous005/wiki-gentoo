<!-- source: https://wiki.gentoo.org/wiki/AC1300_Wireless_Adapters | group: Gentoo Wiki (Main) | wiki-title: AC1300 Wireless Adapters -->
---
title: AC1300 Wireless Adapters
url: https://wiki.gentoo.org/wiki/AC1300_Wireless_Adapters
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2025-04-21"
fingerprint: "1ea4a9f8d0f57885"
license: CC BY-SA 4.0
---

# AC1300 Wireless Adapters

From Gentoo Wiki

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

## Drivers for Realtek's chipsets

- "RTW88" built-in kernel (require firmware) [https://github.com/lwfinger/rtw88](https://github.com/lwfinger/rtw88)
- "88x2bu-20210702" for RTL8822BU (USB) [https://github.com/morrownr/88x2bu-20210702](https://github.com/morrownr/88x2bu-20210702)
- "rtl88x2ce-dkms" for RTL8822CE (PCIE) [https://github.com/juanro49/rtl88x2ce-dkms](https://github.com/juanro49/rtl88x2ce-dkms)

## Kernel configuration

KERNEL

```
Device Drivers --->
  - Generic Driver Options
    - Firmware loader
      - Build named firmware blobs into the kernel binary. Add those two files:
        - regulatory.db regulatory.db.p7s
[*] Networking support  --->
  - Networking options
    - [*] Packet socket
  - [*] Wireless  --->
    - <*>   cfg80211 - wireless configurtion API
    - <*>   Generic IEEE 802.11 Networking Stack (mac80211)
  - <*>   RF switch subsystem support ---> 
Cryptographic API --->
   - Accelerated Cryptographic Algorithms for CPU (x86)  --->
     - <*> Ciphers: AES, modes: ECB, CBC, CTS, CTR, XTR, XTS, GCM (AES-NI)
```
## Troubleshooting

### "wlan0: Failed to initialize driver interface"

Make sure that these options are enabled:

KERNEL

```
- [*] Networking support  --->
  - Networking options
    - [*] Packet socket
```
