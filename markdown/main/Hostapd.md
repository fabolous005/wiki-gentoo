<!-- source: https://wiki.gentoo.org/wiki/Hostapd | group: Gentoo Wiki (Main) | wiki-title: Hostapd -->
---
title: Hostapd
url: https://wiki.gentoo.org/wiki/Hostapd
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-07-14"
fingerprint: cebba87d78c419c1
license: CC BY-SA 4.0
---

# Hostapd

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**Hostapd** (**Host a**ccess **p**oint **d**aemon) is a user space software access point capable of turning normal network interface cards into access points and authentication servers. The current version supports Linux (Host AP, madwifi, mac80211-based drivers) and FreeBSD (net80211).[\[1\]](https://wiki.gentoo.org#cite_note-1)

## Scope of this document

Hostapd can do a lot of things, but only its most basic aspects will be covered in this article.

## Requirement

A WiFi card with AP mode support is needed:

`root #``iw list | grep "Supported interface modes" -A 8````
        Supported interface modes:
                 * IBSS
                 * managed
                 * AP
                 * AP/VLAN
                 * WDS
                 * monitor
                 * P2P-client
                 * P2P-GO
```
`iw` is part of the [net-wireless/iw](https://packages.gentoo.org/packages/net-wireless/iw) package.

## WiFi Technology

A brief reminder of the technology involved.

### 802.11

| Technology | Frequency Band | Year | Max Speed | notes | 
|---|---|---|---|---|
| 802.11a | 5GHz | 1999 | 54Mbps | obsolete | 
| 802.11b | 2.4GHz | 1999 | 11Mbps | obsolete | 
| 802.11g | 2.4GHz | 2003 | 54Mbps | becoming obsolete | 
| 802.11n | 2.4GHz or 5GHz | 2009 | 150Mbps | can use multiple streams to increase speed (if both client and AP have more than one antenna) | 
| 802.11ac | 5GHz | 2013 | 867Mbps | can use multiple streams | 
| 802.11ax | 2.4GHz or 5GHz | 2019 | 1201Mbps | can use multiple streams, supports higher clients density | 

### Frequency Bands

| Frequency | 802.11 | Channels | 
|---|---|---|
| 2.4GHz | b/g/n/ax | up to 14, depends on the country | 
| 5GHz | a/n/ac/ax | up to 37, depends on the country | 

### Access Point

- An AP is like a wireless switch;
- An AP can only use one band at a time: 2.4GHz OR 5GHz, a so-called "dual-band AP" is just one AP at 2.4GHz and another at 5GHz;
- An AP using the 2.4GHz band can be b, g, n and ax at the same time (if the hardware supports it);
- An AP using the 5GHz band can be a, n, ac and ax at the same time (if the hardware supports it);
- An AP can have multiple SSIDs, making it look like multiple APs, but all will share the same band AND channel.

## Capabilities of Hostapd

### What it can do

- Create an AP;
- Create multiple APs on the same card (if the card supports it, usually up to 8);
- Create one AP on one card and another AP on a second card, all within a single instance of Hostapd;
- Use 2.4GHz and 5GHz at the same time on the same card. This requires a card with two radios though, which is pretty rare (but hostapd supports it) - if the card creates two wlanX interfaces, you might be lucky;

### What it cannot do

- Create multiple APs on different channels on the same card. Multiple APs on the same card will share the same channel;
- Create a dual-band AP, even with two cards. But it can create two APs with the same SSID;
- Assign IPs to the devices connecting to the AP, a dhcp server is needed for that;
- Assign an IP to the AP itself, it is not hostapd's job to do that;

## IP, DHCP, and Routing

Hostapd only creates wireless Ethernet switches, it does not know about the IP protocol or routing.

### IP of the AP

An AP's interface really is just an Ethernet interface:

**`/etc/conf.d/net`**

**Sample network configuration for an AP**

```
(...)
modules_wlan0="!wireless"       # by default, wireless interfaces are assumed to be clients, not APs
config_wlan0="192.168.42.1/24"  # the AP's IP and network
```
`root #````
ln -s net.lo /etc/init.d/net.wlan0
```
`root #````
rc-update add net.wlan0 default
```
### DHCP

A DHCP server listening on the AP's interface will provide the AP's clients with IPs.

### Routing

Nothing special about routing an AP, it behaves exactly like an Ethernet interface.

## Sample configurations

### 802.11b/g/n with WPA2-PSK and CCMP

A simple but secure AP with maximal compatibility for current hardware:

**`/etc/hostapd/hostapd.conf`**

```
# the interface used by the AP
interface=wlan0
# "g" simply means 2.4GHz band
hw_mode=g
# the channel to use
channel=10
# limit the frequencies used to those allowed in the country
ieee80211d=1
# the country code
country_code=FR
# 802.11n support
ieee80211n=1
# QoS support, also required for full speed on 802.11n/ac/ax
wmm_enabled=1
# the name of the AP
ssid=somename
# 1=wpa, 2=wep, 3=both
auth_algs=1
# WPA2 only
wpa=2
wpa_key_mgmt=WPA-PSK
rsn_pairwise=CCMP
wpa_passphrase=somepassword
```
### 802.11a/n/ac with WPA2-PSK and CCMP

A simple but secure AP for recent hardware:

**`/etc/hostapd/hostapd.conf`**

```
interface=wlan0
# "a" simply means 5GHz
hw_mode=a
# the channel to use, 0 means the AP will search for the channel with the least interferences (ACS)
channel=0
ieee80211d=1
country_code=FR
ieee80211n=1
# 802.11ac support
ieee80211ac=1         
wmm_enabled=1
ssid=somename
auth_algs=1
wpa=2
wpa_key_mgmt=WPA-PSK
rsn_pairwise=CCMP
wpa_passphrase=somepassword
```
### 802.11b/g/n triple AP

Three APs on the same card, one with WPA2, one with WPA1, one without encryption.

Hostapd automatically creates new interfaces for the extra APs:

**`/etc/hostapd/hostapd.conf`**

```
interface=wlan0
hw_mode=g
channel=10
ieee80211d=1
country_code=FR
ieee80211n=1
wmm_enabled=1
# First AP
ssid=test1
auth_algs=1
wpa=2
wpa_key_mgmt=WPA-PSK
rsn_pairwise=CCMP
wpa_passphrase=somepassword
# Second AP
# the name of the new interface hostapd will create to handle this AP 
bss=wlan1
ssid=test2
auth_algs=1
wpa=1
wpa_key_mgmt=WPA-PSK
wpa_passphrase=someotherpassword
# Third AP
# the name of the new interface hostapd will create to handle this AP 
bss=wlan2
ssid=test3
# since there is no encryption defined, none will be used
```
## Proper use of the 5GHz band

Depending on where you live, using the 5GHz band for an AP has limitations:

- some channels are forbidden
- some channels are for indoor use only
- some channels cannot be used without first listening to make sure they are not already used by something else (no-IR, a.k.a: no initiate radiation)
- some channels require [DFS](https://en.wikipedia.org/wiki/Dynamic_Frequency_Selection) to be used (Dynamic Frequency Selection, to prevent interferences with radars)
- some channels require [TPC](https://en.wikipedia.org/wiki/IEEE_802.11h-2003#Spectrum_and_Transmit_Power_Management_Extensions) to be used (Transmit Power Control, to limit interferences)


The problem is that [each country has its own rules](<https://en.wikipedia.org/wiki/List_of_WLAN_channels#5_GHz_(802.11a/h/j/n/ac/ax)>) and those rules are complex and regularly changing.

The package [net-wireless/wireless-regdb](https://packages.gentoo.org/packages/net-wireless/wireless-regdb) maintains a regulatory database, for each country, of what channels can be used and with what limitations.

To use the database, you either need to make the database directly available to the kernel, as you would with a firmware (the files are: /lib/firmware/regulatory.db and /lib/firmware/regulatory.db.p7s)

These limitations are somewhat recent and only implemented in 802.11n/ac/ax devices. Old devices which ignore these limitations may break the law.



### Firmwares/drivers

Some firmwares will refuse to work as APs even though they can work as clients.

Some drivers do not implement the required checks (DFS, no-IR, etc) and will also refuse to create APs on most or even all channels.

Currently only Atheros drivers ([ath9k](https://wireless.wiki.kernel.org/en/users/drivers/ath9k), [ath10k](https://wireless.wiki.kernel.org/en/users/drivers/ath10k)) are know to properly support AP mode in the 5GHz band.

Most notably, the intel driver [iwlwifi](https://wireless.wiki.kernel.org/en/users/drivers/iwlwifi) only has good AP mode support for the 2.4GHz band, AP mode in the 5GHz band is either disabled or crippled.

## Troubleshooting

### Invalid BSSID mask

When using virtual APs, this type of error may be encountered:

By default each virtual AP is automatically given a unique MAC address by hostapd, this is calculated by simply adding 1 to the previous MAC address used.
If your base interface has a MAC address of *01:02:03:04:05:06*, the first virtual AP will get *01:02:03:04:05:07*, the second virtual AP will get *01:02:03:04:05:08*, etc ...

But hostapd wants all those MAC addresses to match a mask (e.g., *ff:ff:ff:ff:ff:fc*).
And it wants the interface's MAC address to be the first in this block.

Obviously, a lot of luck is required to have an interface's MAC address already matching these conditions.

There are 2 solutions to this problem:

1. Change the interface's MAC address to something matching the rules. The simplest way is to replace the last digit with 0, because it will always be the first address of the block.
2. Even simpler is to set the **bssid** field for the virtual APs in hostapd's configuration. Any MAC address will work because hostapd no longer enforces the mask rule when this field is set. This may depend on the hardware capabilities, if it doesn't work: go back to the first solution.

### "PREV\_AUTH\_NOT\_VALID" and/or "invalid MIC in msg 2/4 of 4-Way Handshake"

If you are unable to connect to the AP and wpa\_supplicant returns to following error:

And/or if you get the following error from hostapd:

Make sure you did not use quotes to set the passphrase in hostapd.conf

The wpa\_passphrase option takes *everything* atfer the '='. 
If there are quotes, they are assumed to be part of the passphrase

**`/etc/hostapd/hostapd.conf`**

```
# wrong
wpa_passphrase="your secret phrase"
# right
wpa_passphrase=your secret phrase
```
## External resources

- [Similar page from the Arch Linux Wiki](https://wiki.archlinux.org/index.php/software_access_point)
- [Status of ACS support in Linux drivers](https://wireless.wiki.kernel.org/en/users/documentation/acs#hostapd_setup), a good sign of proper 5GHz support
