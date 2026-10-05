<!-- source: https://wiki.gentoo.org/wiki/Qualcomm_Atheros_QCA6174 | group: Gentoo Wiki (Main) | wiki-title: Qualcomm Atheros QCA6174 -->
---
title: Qualcomm Atheros QCA6174
url: https://wiki.gentoo.org/wiki/Qualcomm_Atheros_QCA6174
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2025-07-05"
fingerprint: "1543d179e09233a9"
license: CC BY-SA 4.0
---

# Qualcomm Atheros QCA6174

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

This article is **archived (obsolete)**. Contents are surely incorrect for current usage, and are intended for historical reference only.

TLDR:

**Do not use this article!**

The [Qualcomm Atheros](https://en.wikipedia.org/wiki/Qualcomm_Atheros) QCA6174 is a [802.11ac](https://en.wikipedia.org/wiki/IEEE_802.11ac) Wireless Network Adapter which is used in some laptops.

To make it work it needs the kernel driver and firmware.

## Kernel

### Precondition

Activate at least [cfg80211](https://wireless.wiki.kernel.org/en/developers/documentation/cfg80211) (`CONFIG_CFG80211`) and [mac80211](https://wireless.wiki.kernel.org/en/developers/documentation/mac80211) (`CONFIG_MAC80211`).

**linux-4.19 example**

Minstrel and its 802.11n support is a [rate control algorithm](https://wireless.wiki.kernel.org/en/developers/Documentation/mac80211/RateControl/minstrel). Some wireless drivers might require it enabled.

### Kernel driver

Enable  Atheros 802.11ac wireless cards support (`CONFIG_ATH10K`) and Atheros ath10k PCI support (`CONFIG_ATH10K_PCI`) as modules `<M>`:

**Wireless configuration, linux-4.8**

Set it as a module `<M>` as shown here. After changes on kernel configuration do not forget to [rebuild the kernel](https://wiki.gentoo.org/wiki/Kernel/Rebuild).

After rebuilding the kernel and reboot the selected options could be verified (needs [Kernel/IKCONFIG support](https://wiki.gentoo.org/wiki/Kernel/IKCONFIG_support)) like

`user $``zgrep 'ATH10K' /proc/config.gz`
lspci should then display the adapter like:

`root #``lspci -nnkv````
02:00.0 Network controller [0280]: Qualcomm Atheros QCA6174 802.11ac Wireless Network Adapter [168c:003e] (rev 32)
        Subsystem: Bigfoot Networks, Inc. QCA6174 802.11ac Wireless Network Adapter [1a56:1535]
        Flags: bus master, fast devsel, latency 0, IRQ 141
        Memory at ed200000 (64-bit, non-prefetchable) [size=2M]
        Capabilities: [40] Power Management version 3
        Capabilities: [50] MSI: Enable+ Count=8/8 Maskable+ 64bit-
        Capabilities: [70] Express Endpoint, MSI 00
        Capabilities: [100] Advanced Error Reporting
        Capabilities: [148] Virtual Channel
        Capabilities: [168] Device Serial Number 00-00-00-00-00-00-00-00
        Capabilities: [178] Latency Tolerance Reporting
        Capabilities: [180] L1 PM Substates
        Kernel driver in use: ath10k_pci
        Kernel modules: ath10k_pci 
```
## Firmware

The Qualcomm Atheros Killer N1525 Wireless-AC requires Linux firmware files.

`root #``emerge sys-kernel/linux-firmware`
Find out more on [GitHub kvalo/ath10k-firmware](https://github.com/kvalo/ath10k-firmware/pull/3).



## Testing

After a reboot with the new kernel or after loading the modules, the device can be checked for availability by using following methods:

- Using the [/sys file system](https://wiki.gentoo.org/wiki/Wifi/Testing#.2Fsys_file_system)
- Using the [ip command](https://wiki.gentoo.org/wiki/Wifi/Testing#ip_command)
- Using the [ifconfig command](https://wiki.gentoo.org/wiki/Wifi/Testing#ifconfig_command)
- Using the [iw command](https://wiki.gentoo.org/wiki/Wifi/Testing#iw_command)

Get the device name by listing the /sys/class/net directory contents using ls -al or the tree command (provided by the [app-text/tree](https://packages.gentoo.org/packages/app-text/tree) package):

`user $``tree /sys/class/net`
/sys/class/net/
├── enp2s14 -> ../../devices/pci0000:00/0000:00:1e.0/0000:02:0e.0/net/enp2s14
├── lo -> ../../devices/virtual/net/lo
├── sit0 -> ../../devices/virtual/net/sit0
└── wlp8s0 -> ../../devices/pci0000:00/0000:00:1c.0/0000:08:00.0/net/wlp8s0

To obtain the device name and verify that the wireless card is detected, execute the following [ip command](https://wiki.gentoo.org/wiki/Iproute2):

`user $``ip addr`
3: wlan0:   ...

A network card can be activated as follows:

`root #``ip link set wlan0 up`
The ifconfig command is provided through the [sys-apps/net-tools](https://packages.gentoo.org/packages/sys-apps/net-tools) package. Use ifconfig -a to list all detected network cards, even those that are not enabled/active yet:

`user $``ifconfig -a`
wlan0     ...

A network card can be activated as follows:

`root #``ifconfig -v wlan0 up`
SIOCSIFFLAGS: Operation not possible due to RF-kill
WARNING: at least one error occurred. (-1)

In this example, enabling the wireless card failed as a radio frequency kill state is set (usually to reduce power consumption and not connect by accident to a wireless network).

If the wireless network card driver supports the nl80211 stack, then the iw command as offered by the [net-wireless/iw](https://packages.gentoo.org/packages/net-wireless/iw) package can show the detected wireless cards:

`root #``iw dev`
phy#0
	Interface wlan0
		ifindex 4
		type managed

## See also

## External resources

- [https://gist.github.com/harrykipper/d1bedb234c4af0692f7ccd33329a02d7](https://gist.github.com/harrykipper/d1bedb234c4af0692f7ccd33329a02d7) - Unofficial patch to increase TX rate to more moderate speeds.
- [wireless.wiki.kernel.org](https://wireless.wiki.kernel.org/en/users/drivers/ath10k/firmware?s%5b%5d=qca6174)
- [Official upstream ath10k-firmware by Kalle Valo on GitHub](https://github.com/kvalo/ath10k-firmware/)
- [Kernel drivers on cateee.net](https://www.startpage.com/do/search?query=%22168c003e%22+site%3Acateee.net)
- [patchwork.kernel.org](https://patchwork.kernel.org/project/ath10k/list/) — Patchwork ath10k project mail archive.
