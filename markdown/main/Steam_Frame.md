<!-- source: https://wiki.gentoo.org/wiki/Steam_Frame | group: Gentoo Wiki (Main) | wiki-title: Steam Frame -->
---
title: Steam Frame
url: https://wiki.gentoo.org/wiki/Steam_Frame
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-10-09"
fingerprint: "9c86bd3ade053a49"
license: CC BY-SA 4.0
---

# Steam Frame

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

This article is written for streaming PCVR content to a Steam Frame VR headset from the Steam client running on a Gentoo Linux PC using the included USB dongle.

## Installation

### Kernel

The USB dongle included with the Steam Frame uses a **Realtek RTL8832CU** Wi-Fi 6E chip.
Mainline support for this hardware is available from the `rtw89_8852cu` kernel module in **Linux 7.2 or later**. When not using the Steam Frame the USB dongle is a normal Wi-Fi adapter that can connect to other networks.

```
Device Drivers --->
  Network device support --->
    Wireless LAN --->
      [*]   Realtek devices --->
        <M>   Realtek 802.11ax wireless chips support --->
          <M>   Realtek 8852CU USB wireless network (Wi-Fi 6E) adapter
Networking support --->
  Wireless --->
    <M>   cfg80211 - wireless configuration API
```
### Emerge

[net-wireless/wireless-regdb](https://packages.gentoo.org/packages/net-wireless/wireless-regdb) needs to be installed so the Steam Frames USB dongle knows which 6 GHz frequencies it can use based on a Wi-Fi regulatory country code database.

`root #``emerge --ask net-wireless/wireless-regdb`
If you don't know your Wi-Fi regulatory country code already, make sure [app-arch/bzip2](https://packages.gentoo.org/packages/app-arch/bzip2) is installed so you can read the database with `bzgrep` in the configuration stage.

`root #``emerge --ask app-arch/bzip2`
Additionally [net-wireless/iw](https://packages.gentoo.org/packages/net-wireless/iw) is needed to check and set the Wi-Fi regulatory country code on Wi-Fi adapters.

`root #``emerge --ask net-wireless/iw`
### Steam

The Steam Frame is designed to stream from a Linux or Windows based PC using the platform native version of [Steam](https://wiki.gentoo.org/wiki/Steam) and SteamVR.

Using the steam-launcher ebuild from the [steam-overlay](https://github.com/anyc/steam-overlay) repository with the following useflags is recommended.

**`/etc/portage/package.use/steam-frame`**

```
games-util/steam-launcher +steamruntime +steamvr
```
For more infomation on how to install the Steam client on Gentoo using the steam-launcher ebuild see [Steam -> Installation -> Emerge (recommended)](https://wiki.gentoo.org/wiki/Steam#Emerge_.28recommended.29)

### SteamVR

With the Steam client installed, install SteamVR as you would any other game or software through the Steam Store.

#### Beta branch

Sometimes it might be useful to try the beta version of SteamVR to see if that resolves a bug or test a new feature from Valve. To switch to SteamVR Beta:

1. Select **SteamVR** from the Steam library
2. Select **Manage** (little gear '⚙' opposite the **LAUNCH** or **PLAY** button) then **Properties...** from the dropdown menu
3. In the properties menu select **Game Versions & Betas** from the left list then select **beta** from the **Selected Game Version** list
4. Close the Properties menu and note how the title of the application has changed to **SteamVR \[beta\]** in your Steam library
5. Begin downloading the update for **SteamVR \[beta\]** if it hasn't automatically started
6. To revert these changes repeat the steps above but select **Default Public Version** game version instead of **beta** in step 3

## Configuration

### Wi-Fi regulatory country code

The Steam Frames USB dongle might default to the global Wi-Fi regulatory country code when first plugged in. If the global region is not set to the correct country code then it might not be able to connect to the Steam Frame and/or maintain a connection. This is because the adapter does not know which 6 GHz frequency bands are available for it to safely use.

#### Check Wi-Fi regulatory country code

To check the current Wi-Fi regulatory country code of each Wi-Fi adapter on the system

`user $``iw reg get`
The output of the command will look somewhat like this:

global
country 00: DFS-UNSET
	(755 - 928 @ 2), (N/A, 20), (N/A), PASSIVE-SCAN
	(2402 - 2472 @ 40), (N/A, 20), (N/A)
	(2457 - 2482 @ 20), (N/A, 20), (N/A), AUTO-BW, PASSIVE-SCAN
	(2474 - 2494 @ 20), (N/A, 20), (N/A), NO-OFDM, PASSIVE-SCAN
	(5170 - 5250 @ 80), (N/A, 20), (N/A), AUTO-BW, PASSIVE-SCAN
	(5250 - 5330 @ 80), (N/A, 20), (0 ms), DFS, AUTO-BW, PASSIVE-SCAN
	(5490 - 5730 @ 160), (N/A, 20), (0 ms), DFS, PASSIVE-SCAN
	(5735 - 5835 @ 80), (N/A, 20), (N/A), PASSIVE-SCAN
	(57240 - 63720 @ 2160), (N/A, 0), (N/A)

If the regulatory country code is `00: DFS_UNSET` like above then the Steam Frames USB dongle might not work properly until it is set to another code.

#### Search for valid Wi-Fi regulatory country codes

The Wi-Fi regulatory country code are typically the same as a countries two letter top level domain. These are some common examples:

| Code | Country | 
|---|---|
| `00` | Undefined / Fallback | 
| `AU` | Australia | 
| `DE` | Germany | 
| `GB` | United Kingdom | 
| `JP` | Japan | 
| `PL` | Poland | 
| `TW` | Taiwan | 
| `US` | United States | 

To list all available country regions in the Wi-Fi regulatory country code database run the following command:

`user $``bzgrep country /usr/share/doc/wireless-regdb-*/db.txt.bz2`
#### Set the global Wi-Fi regulatory country code

##### On a running system

To set a new global regulatory country code on a system without restarting, run the following command.

`root #``iw reg set 00`
Replace `00` with your local Wi-Fi regulatory country code.

##### On boot

The most straightforward way to set the global regulatory country code on system startup is to specify the option to the `cfg80211` kernel module.

**`/etc/modprobe.d/cfg80211.conf`**

```
options cfg80211 ieee80211_regdom=00
```
Replace `00` with your local Wi-Fi regulatory country code.
