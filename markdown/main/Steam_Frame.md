<!-- source: https://wiki.gentoo.org/wiki/Steam_Frame | group: Gentoo Wiki (Main) | wiki-title: Steam Frame -->
---
title: Steam Frame
url: https://wiki.gentoo.org/wiki/Steam_Frame
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-10-11"
fingerprint: "9490b43f4e01324b"
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

### Network

The Steam Frame contains two Wi-Fi radios. The first (`wlan0`) is used like a normal Wi-Fi adapter to allow the Steam Frame user to connect to their Wi-Fi while the second (`wlanap`) is setup as an access point (a.k.a hotspot) for the USB dongle to connect to.

When the USB dongle is connected to a typical Linux or Windows desktop running the Steam client, the USB dongle is detected and controlled directly by Steam. Steam will attempt to find and connect to a Steam Frame logged into the same Steam account.

In a typical home network this *should* make PCVR streaming to the Steam Frame a plug n' play experience.

#### Steam Frame access point

By default the Steam Frame access point credentials will be managed by Steam. The Steam managed SSID will be `frame_########` where `#` is a unique set of alpha-numeric characters. The Steam managed password will be 32 alpha-numeric characters.

The IPv4 address of the Steam Frame on its own access point is `10.35.78.1`.

##### Set custom SSID and password

The Steam Frame allows you to set custom credentials to join its access point instead of being managed by Steam. Doing so will allow you to easily connect 6 GHz Wi-Fi devices other than the Steam Frames included USB dongle. Multiple devices can connect to the Steam Frames access point at the same time.

Custom access point credentials could be useful if the required packages that allow the Steam client to automatically configure the USB dongle are not available or when the included USB dongle itself cannot be used for whatever reason.

To configure custom credentials for the Steam Frames access point:

1. While wearing the Steam Frame, select **Steam** (logo on the left of the the bottom bar in the overlay) then select **Steam Settings**
2. Select **Remote Play** from the left list then scroll down on the right until you find the **Wireless Streaming Adapter (Steam Frame)** section
3. Set **Connection Management** from **Steam** to **Custom**
4. Type the **Hotspot SSID** and **Hotspot Password** of your choice in the newly appeared options under **Connection Management**
  - If you would like to share the Steam Frames Internet connection with all devices connected to its access point then set **Hotspot Client Internet** to **NAT (via HMD Wi-Fi)**
5. Restart the Steam Frame to apply changes

##### Advanced Wi-Fi settings

The Steam Frame allows you to configure some advanced settings on its access point. Usually these settings can be left on the **Auto** but can benefit from a manual setting in some circumstances.

To configure Advanced Wi-Fi settings for the Steam Frames access point:

1. While wearing the Steam Frame, select **Steam** (logo on the left of the the bottom bar in the overlay) then select **Steam Settings**
2. Select **Remote Play** from the left list then scroll down on the right until you find the **Wireless Streaming Adapter (Steam Frame)** section
3. Set the **Show Advanced Wi-Fi Settings** switch to the right
  - **Status**, **Current Band** and **Current Channel** will show your the current settings of the access point
4. Set **Channel** and/or **Channel Width** to an option of your choice
5. Restart the Steam Frame to apply changes

#### PC firewall

In order for the Steam Frame to be used for PCVR streaming. Several ports that Steam and SteamVR use need to be opened. These ports cannot be reconfigured.

If you have not configured a defensive firewall on this system you can most likely skip this entire section.

The Steam Frame already allows every port it needs for PCVR streaming and Steam Remote Play.

##### Steam ports

If a firewall is setup on your Gentoo system then following incoming ports should be allowed on the PC running the Steam client.

| Port | Protocol | Description | Required for PCVR | 
|---|---|---|---|
| 10400 | UDP | Streaming VR | Yes | 
| 10401 | UDP | Streaming VR | Yes | 
| 27031-27036 | UDP | Streaming 2D | Yes | 
| 27036 | TCP | Streaming 2D | Yes | 
| 27040 | TCP | Game file transfer over LAN | No | 

##### Nftables example

If the PC is setup to use nftables as it's firewall with the default input policy set to drop, then a ruleset like this can be used to allow an exception for the required ports the Steam Frame needs to connect to the Steam client.

**`/etc/nftables/sets/steam.nft`**

**Typical ruleset for allowing Steam incoming LAN connections**

```
table inet filter {
	set STEAM_INCOMING_PORTS {
		type inet_proto . inet_service
		flags interval
		elements = {
			udp . 10400-10401, # Steam VR streaming ports (for Steam Frame)
			udp . 27031-27036, # Steam 2D streaming ports (a.k.a Remote Play)
			tcp . 27036,
			tcp . 27040, # Steam game file transfer (optional but useful)
		}
	}
	set STEAM_INCOMING_ADDRS {
		type ipv4_addr
		elements = {
			10.35.78.1, # The address of the Steam Frame when connected directly via USB Dongle
			# Add any additional ranges/addresses you want Steam to communicate with in your LAN here
		}
	}
}
```
The rulesets can be added with the following command

`root #``nft -f /etc/nftables/rules/sets/steam.nft`
With the rulesets loaded add them as a incoming filter to be accepted

`root #``nft add rule inet filter input ip saddr @STEAM_INCOMING_ADDRS meta l4proto . th dport @STEAM_INCOMING_PORTS accept`
Finally check that the firewall looks correct

`root #``nft list ruleset`
The output could look somewhat like this (depending on what other firewall rules existed before)
