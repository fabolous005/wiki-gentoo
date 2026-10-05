<!-- source: https://wiki.gentoo.org/wiki/Bluetooth/hciconfig | group: Gentoo Wiki (Main) | wiki-title: Bluetooth/hciconfig -->
---
title: Bluetooth/hciconfig
url: https://wiki.gentoo.org/wiki/Bluetooth/hciconfig
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2025-12-31"
fingerprint: "5cd960c0a932025"
license: CC BY-SA 4.0
---

# Bluetooth/hciconfig

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

Display controller information:

`root #``hciconfig -a````
hci0:   Type: BR/EDR  Bus: USB
        BD Address: 00:02:72:2F:A9:33  ACL MTU: 1021:8  SCO MTU: 64:1
        UP RUNNING PSCAN 
        RX bytes:1166 acl:0 sco:0 events:43 errors:0
        TX bytes:960 acl:0 sco:0 commands:43 errors:0
        Features: 0xbf 0xfe 0xcf 0xfe 0xdb 0xff 0x7b 0x87
        Packet type: DM1 DM3 DM5 DH1 DH3 DH5 HV1 HV2 HV3 
        Link policy: RSWITCH SNIFF 
        Link mode: SLAVE ACCEPT 
        Name: 'BlueZ 5.21'
        Class: 0x000104
        Service Classes: Unspecified
        Device Class: Computer, Desktop workstation
        HCI Version: 4.0 (0x6)  Revision: 0x1000
        LMP Version: 4.0 (0x6)  Subversion: 0x220e
        Manufacturer: Broadcom Corporation (15)
```
Where `hci0` is the name of the controller, and `UP` (3rd line) indicates that the controller is enabled.

Open and initialize HCI device:

`root #``hciconfig  -a <hci0> up`
Close HCI device:

`root #``hciconfig  -a <hci0> down`
Reset HCI device:

`root #``hciconfig  -a <hci0> reset`
Reset statistic counters:

`root #``hciconfig  -a <hci0> rstat`
Enable Authentication:

`root #``hciconfig  -a <hci0> auth`
Disable Authentication:

`root #``hciconfig  -a <hci0> noauth`
Enable Encryption:

`root #``hciconfig  -a <hci0> encrypt`
Disable Encryption:

`root #``hciconfig  -a <hci0> noencrypt`
Enable Page and Inquiry scan:

`root #``hciconfig  -a <hci0> piscan`
Disable scan:

`root #``hciconfig  -a <hci0> noscan`
Enable Inquiry scan:

`root #``hciconfig  -a <hci0>  iscan`
Enable Page scan:

`root #``hciconfig  -a <hci0> pscan`
Get/Set default packet type:

`root #``hciconfig  -a <hci0> ptype [type]`
Get/Set default link mode:

`root #``hciconfig  -a <hci0> lm [mode]`
Get/Set default link policy:

`root #``hciconfig  -a <hci0> lp [policy]`
Get/Set local name:

`root #``hciconfig  -a <hci0> name [name]`
Get/Set class of device:

`root #``hciconfig  -a <hci0> class [class]`
Get/Set voice setting:

`root #``hciconfig  -a <hci0> voice [voice]`
Get/Set inquiry access code:

`root #``hciconfig  -a <hci0> iac [iac]`
Get/Set inquiry transmit power level:

`root #``hciconfig  -a <hci0> nqtpl [level]`
Get/Set inquiry mode:

`root #``hciconfig  -a <hci0> inqmode [mode]`
Get/Set inquiry data:

`root #``hciconfig  -a <hci0> inqdata [data]`
Get/Set inquiry scan type:

`root #``hciconfig  -a <hci0> inqtype [type]`
Get/Set inquiry scan window and interval:

`root #``hciconfig  -a <hci0> inqparms [win:int]`
Get/Set page scan window and interval:

`root #``hciconfig  -a <hci0> pageparms [win:int]`
Get/Set page timeout:

`root #``hciconfig  -a <hci0> ageto [to]`
Get/Set AFH mode:

`root #``hciconfig  -a <hci0> afhmode [mode]`
Get/Set Simple Pairing Mode:

`root #``hciconfig  -a <hci0> sspmode [mode]`
Set ACL MTU and number of packets:

`root #``hciconfig  -a <hci0> clmtu <mtu:pkt>`
Set SCO MTU and number of packets:

`root #``hciconfig  -a <hci0> scomtu <mtu:pkt>`
Delete link key from the device:

`root #``hciconfig  -a <hci0> delkey <bdaddr>`
Get local OOB data:

`root #``hciconfig  -a <hci0> oobdata`
Display supported commands:

`root #``hciconfig  -a <hci0> commands`
Display device features:

`root #``hciconfig  -a <hci0> features`
Display version information:

`root #``hciconfig  -a <hci0> version`
Display revision information:

`root #``hciconfig  -a <hci0> revision`
Add a device to the blacklist:

`root #``hciconfig  -a <hci0> block  <bdaddr>`
Remove a device from the blacklist:

`root #``hciconfig  -a <hci0> unblock <bdaddr>`
Set LE Random Address:

`root #``hciconfig  -a <hci0> lerandaddr <bdaddr>`
Enable LE advertising:

`root #``hciconfig  -a <hci0> leadv [type]`
Show all commands device has support for:

`root #``hciconfig  get commands`
## See also

- [Bluetooth](https://wiki.gentoo.org/wiki/Bluetooth) — describes the configuration and usage of Bluetooth controllers and devices.
