<!-- source: https://wiki.gentoo.org/wiki/Iwlwifi | group: Gentoo Wiki (Main) | wiki-title: Iwlwifi -->
---
title: iwlwifi
url: https://wiki.gentoo.org/wiki/Iwlwifi
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-10-01"
fingerprint: b596506b49a221a9
license: CC BY-SA 4.0
---

# iwlwifi

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)


**iwlwifi** is the wireless driver for [Intel's current wireless chips](https://wireless.wiki.kernel.org/en/users/drivers/iwlwifi#introduction). A card-specific firmware and support from the kernel's wifi stack are required for proper functioning.

To make it work, some kernel configuration is needed; the driver supports 802.11a/b/g/n/ac (depending on the device), so IEEE 802.11 should be enabled.

Activate at least [cfg80211](https://wireless.wiki.kernel.org/en/developers/documentation/cfg80211) (`CONFIG_CFG80211`) and [mac80211](https://wireless.wiki.kernel.org/en/developers/documentation/mac80211) (`CONFIG_MAC80211`). For details see the [IEEE 802.11 section of Wi-Fi artice](https://wiki.gentoo.org/wiki/Wi-Fi#IEEE_802.11).

Use this driver for Intel's [current wireless chips](https://wireless.wiki.kernel.org/en/users/drivers/iwlwifi#supported_devices); set it as a module (`<M>`) as shown here. The correct *DVM* or *MVM* firmware option according to the **Module** column of the [firmware table](https://web.archive.org/web/20190305085430/https://wireless.wiki.kernel.org/en/users/Drivers/iwlwifi) is also needed.

**Options for Linux Kernel 4.19**

After rebuilding and rebooting the kernel, the selected options can be verified as follows:

`user $``zgrep 'IWLWIFI\|IWLDVM\|IWLMVM' /proc/config.gz`
Additional [firmware](https://wiki.gentoo.org/wiki/Linux_firmware) for the individual device is needed as listed in [this table](https://www.intel.com/content/www/us/en/support/articles/000005511/wireless.html); contemporary firmware is always available in the [sys-kernel/linux-firmware](https://packages.gentoo.org/packages/sys-kernel/linux-firmware) package. If it isn't in linux-firmware, it might be found in device-specific [sys-firmware/iwlxxxx-\*ucode](https://packages.gentoo.org/packages/search?q=sys-firmware%2Fiwl) packages.

Upstream Intel instructions recommend<sup>[\[1\]](https://wiki.gentoo.org#cite_note-1)</sup> adding all iwlwifi ucode to the kernel image; this is recommended for convenience, though it will bloat the kernel slightly.

`root #``emerge --ask sys-kernel/linux-firmware`
In case the driver is built into the kernel (`<*>`) instead as a module (`<M>`), also the firmware needs to be built [into the kernel](https://wiki.gentoo.org/wiki/Kernel_Modules#About_loadable_kernel_modules).

**linux-4.19**

In this case, replace `iwlwifi-xxxx.ucode` with the exact firmware name;  some attention seems to be needed for `FW_LOADER_USER_HELPER_FALLBACK`.

The [savedconfig](https://wiki.gentoo.org/wiki/Savedconfig)[Linux firmware](https://wiki.gentoo.org/wiki/Linux_firmware) to avoid unneeded stuff in /lib/firmware/.

For example, the 
[Intel® Centrino® Advanced-N 6205](https://wireless.wiki.kernel.org/en/users/drivers/iwlwifi?s%5b%5d=N%206205#firmware) needs **iwlwifi-6000g2a-ucode,** while anything else can be commented out or deleted.

**`/etc/portage/savedconfig/sys-kernel/linux-firmware`**

**Ensure the version number is removed**

```
iwlwifi-6000g2a-5.ucode
iwlwifi-6000g2a-6.ucode
```
To prevent losing these settings on the next firmware update, the version number must be removed:

`user $``cd /etc/portage/savedconfig/sys-kernel``root #``mv linux-firmware{-20200316,}` ### Latest firmware version supporting device

Below is an incomplete list of the latest known firmware blobs supporting a given chipset. Device names are retrieved using lspci.

| Device name | Firmware blob filename | Firmware version | 
|---|---|---|
| Intel Corporation Wi-Fi 6 AX200 (rev 1a) | iwlwifi-cc-a0-77.ucode | `77.ad46c98b.0` | 



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

modprobe should return nothing:

`root #``modprobe iwlwifi`
Most information about the driver module can be obtained via modinfo iwlwifi:

`user $``modinfo iwlwifi`
lspci should display `iwlwifi` for both `Kernel driver in use:` and `Kernel modules:`.

`root #``lspci -nnkv | sed -n '/Network/,/^$/p'````
03:00.0 Network controller [0280]: Intel Corporation Centrino Advanced-N 6205 [Taylor Peak] [8086:0082] (rev 34)
        Subsystem: Intel Corporation Centrino Advanced-N 6205 AGN [8086:1321]
        Flags: bus master, fast devsel, latency 0, IRQ 33
        Memory at f7d00000 (64-bit, non-prefetchable) [size=8K]
        Capabilities: [c8] Power Management version 3
        Capabilities: [d0] MSI: Enable+ Count=1/1 Maskable- 64bit+
        Capabilities: [e0] Express Endpoint, MSI 00
        Capabilities: [100] Advanced Error Reporting
        Capabilities: [140] Device Serial Number confidential
        Kernel driver in use: iwlwifi
        Kernel modules: iwlwifi
```
The `xx:xx.x` identifier will be useful for grepping specific information from dmesg.

Check the output of dmesg. Replace `03:00.0` with the identifier from [lspci](https://wiki.gentoo.org/wiki/Iwlwifi#lspci) and `wlp` with the [network interface name](https://wiki.gentoo.org/wiki/Iwlwifi#Network_device_names).

`user $``dmesg | grep -i -E '03:00.0|wlp|iwl|80211'`
\[    0.251986\] pci 0000:03:00.0: \[8086:0082\] type 00 class 0x028000
\[    0.252146\] pci 0000:03:00.0: reg 0x10: \[mem 0xf7d00000-0xf7d01fff 64bit\]
\[    0.252863\] pci 0000:03:00.0: PME# supported from D0 D3hot D3cold
\[    3.621978\] cfg80211: Loading compiled-in X.509 certificates for regulatory database
\[    3.629362\] cfg80211: Loaded X.509 cert 'sforshee: 00b28ddf47aef9cea7'
\[    3.634986\] iwlwifi 0000:03:00.0: enabling device (0100 -> 0102)
\[    3.635111\] iwlwifi 0000:03:00.0: can't disable ASPM; OS doesn't have ASPM control
\[    3.644480\] iwlwifi 0000:03:00.0: loaded firmware version 18.168.6.1 op\_mode iwldvm
\[    3.659269\] iwlwifi 0000:03:00.0: CONFIG\_IWLWIFI\_DEBUG disabled
\[    3.659270\] iwlwifi 0000:03:00.0: CONFIG\_IWLWIFI\_DEBUGFS disabled
\[    3.659271\] iwlwifi 0000:03:00.0: CONFIG\_IWLWIFI\_DEVICE\_TRACING enabled
\[    3.659273\] iwlwifi 0000:03:00.0: Detected Intel(R) Centrino(R) Advanced-N 6205 AGN, REV=0xB0
\[    3.694543\] ieee80211 phy0: Selected rate control algorithm 'iwl-agn-rs'
\[    3.695812\] iwlwifi 0000:03:00.0 wlp3s0: renamed from wlan0
\[    5.060307\] iwlwifi 0000:03:00.0: Radio type=0x1-0x2-0x0
\[    5.352853\] iwlwifi 0000:03:00.0: Radio type=0x1-0x2-0x0
\[    5.431804\] IPv6: ADDRCONF(NETDEV\_UP): wlp3s0: link is not ready
\[    8.908518\] wlp3s0: authenticate with \<my WLAN AP>
\[    8.912238\] wlp3s0: send auth to \<my WLAN AP> (try 1/3)
\[    9.016437\] wlp3s0: send auth to \<my WLAN AP> (try 2/3)
\[    9.120455\] wlp3s0: send auth to \<my WLAN AP> (try 3/3)
\[    9.125773\] wlp3s0: authenticated
\[    9.126019\] wlp3s0: waiting for beacon from \<my WLAN AP>
\[    9.148418\] wlp3s0: associate with \<my WLAN AP> (try 1/3)
\[    9.191232\] wlp3s0: RX AssocResp from \<my WLAN AP> (capab=0x1431 status=0 aid=2)
\[    9.211860\] wlp3s0: associated
\[    9.242532\] IPv6: ADDRCONF(NETDEV\_CHANGE): wlp3s0: link becomes ready
\[    9.249856\] wlp3s0: Limiting TX power to 20 (20 - 0) dBm as advertised by \<my WLAN AP>

Check if the correct kernel is loaded. This can be accomplished as follows (depends on the [IKCONFIG feature](https://wiki.gentoo.org/wiki/Kernel/IKCONFIG_support)):

`user $``zgrep CONFIG_IWL /proc/config.gz`
- For systems using udev or systemd, it is imperative to configure the kernel to load binary blobs; in this case, the wireless card's firmware must be loaded. More information on configuring the kernel in this manner can be found in the following thread on the Gentoo forums: [FW\_LOADER\_USER\_HELPER\_FALLBACK](https://forums.gentoo.org/viewtopic-t-1001638.html).

- Intel Corporation Wireless 8260 (rev 3a) [can't access the RSA semaphore it is write protected](https://forums.gentoo.org/viewtopic-t-1040894-highlight-.html)
- [Intel Wireless-AC 9560 iwlwifi not working kernel 5.4.0](https://forums.gentoo.org/viewtopic-t-1104736.html)
- [Linux kernel 5.6.0 iwlwifi bug](https://www.mpagano.com/blog/?p=291)
- Intel Corporation Wi-Fi 6 AX200 (rev 1a). If getting dmesg message **pci\_enable\_msi failed - -38** and the card shows **Input/output** error in spite of the correct firmware being loaded, then try enabling the *Message Signaled Interrupts (MSI and MSI-X)* kernel option (*CONFIG\_PCI\_MSI*)

If it is possible to connect to an access point but not to the Internet, it may be worth trying to disable 802.11n and/or enable software encryption; to do so, pass the parameter(s) `iwlwifi.11n_disable=1` or `iwlwifi.11n_disable=8` and/or `iwlwifi.swcrypto=1` to the kernel. In order to pass the arguments automatically upon module load, the following file should be created:

**`/etc/modprobe.d/iwlwifi.conf`**

**Disabling 802.11n, enabling software crypto**

```
 iwlwifi 11n_disable=1 swcrypto=1
```
On some wireless cards (e.g. Intel® Wi-Fi 6 AX201), under heavy load, the network crashes with a similar error:

`root #``dmesg`
\[  367.411551\] iwlwifi 0000:00:14.3: reached 20 old SN frames from 77:77:77:77:77:77 on queue 2, stopping BA session on TID 3

As a workaround, RX aggregation should be disabled using the kernel parameter `iwlwifi.11n_disable=4`:

**`/etc/modprobe.d/iwlwifi.conf`**

**Disabling RX aggregation**

```
 iwlwifi 11n_disable=4
```
`root #``dmesg | grep iwlwifi`
(... redacted ...)
\[ 5711.326985\] iwlwifi 0000:28:00.0: Microcode SW error detected. Restarting 0x0.
\[ 5711.326987\] iwlwifi 0000:28:00.0: Start IWL Error Log Dump:
\[ 5711.326990\] iwlwifi 0000:28:00.0: Transport status: 0x0000004A, valid: 6
\[ 5711.326992\] iwlwifi 0000:28:00.0: Loaded firmware version: 71.058653f6.0 ty-a0-gf-a0-71.ucode
\[ 5711.326993\] iwlwifi 0000:28:00.0: 0x00000071 | NMI\_INTERRUPT\_UMAC\_FATAL    
\[ 5711.326995\] iwlwifi 0000:28:00.0: 0x00008210 | trm\_hw\_status0
\[ 5711.326998\] iwlwifi 0000:28:00.0: 0x00000000 | trm\_hw\_status1
\[ 5711.327001\] iwlwifi 0000:28:00.0: 0x004DAEA2 | branchlink2
\[ 5711.327003\] iwlwifi 0000:28:00.0: 0x004D9974 | interruptlink1
\[ 5711.327004\] iwlwifi 0000:28:00.0: 0x004D9974 | interruptlink2
\[ 5711.327006\] iwlwifi 0000:28:00.0: 0x0000C314 | data1
\[ 5711.327008\] iwlwifi 0000:28:00.0: 0x00000010 | data2
\[ 5711.327009\] iwlwifi 0000:28:00.0: 0x00000000 | data3
(... redacted ...)
\[ 5711.329587\] iwlwifi 0000:28:00.0: ieee80211 phy0: Hardware restart was requested

This indicates that the WiFi adapter's micro-controller has encountered a severe error, causing it to restart; consequences include network dropouts and/or severe slowdowns even after reconnecting to the AP. The root cause might be difficult to ascertain (platform's own radio noise, buggy firmware, etc.), however one of the very first things to try, even if the power management has been disabled for the iwlwifi module, is to prevent the WiFi adapter PCIe link from going into power-save mode; this is accomplished by changing the `power_scheme` value used by the iwlmvm module to **1** (active):

**`/etc/modprobe.d/iwlmvm.conf`**

**Changing power\_scheme to 'active'**

Amongst additional countermeasures suggested on [https://wireless.wiki.kernel.org/en/users/drivers/iwlwifi](https://wireless.wiki.kernel.org/en/users/drivers/iwlwifi), disabling 40 MHz channels usage on the 2.4GHz band might also help:

**`/etc/modprobe.d/cfg80211.conf`**

**Turning off 40 MHz channels usage (2.4 Ghz band)**

- [Handbook:AMD64/Networking/Wireless](https://wiki.gentoo.org/wiki/Handbook:AMD64/Networking/Wireless)
- [Wifi](https://wiki.gentoo.org/wiki/Wifi) — describes the setup of a [Wi-Fi](https://en.wikipedia.org/wiki/Wi-Fi) (wireless) network device.
- [Wpa\_supplicant](https://wiki.gentoo.org/wiki/Wpa_supplicant) — an app for [Wi-Fi](https://wiki.gentoo.org/wiki/Wi-Fi) authentication
- [Network management using DHCPCD](https://wiki.gentoo.org/wiki/Network_management_using_DHCPCD) — explains how to use dhcpcd for complete network stack management.
- [Netifrc](https://wiki.gentoo.org/wiki/Netifrc) — Gentoo's default framework for configuring and [managing network](https://wiki.gentoo.org/wiki/Network_management) interfaces on systems running [OpenRC](https://wiki.gentoo.org/wiki/OpenRC).
- [Troubleshooting](https://wiki.gentoo.org/wiki/Troubleshooting) — provide users with a set of techniques and tools to troubleshoot and fix problems with their Gentoo setups.
