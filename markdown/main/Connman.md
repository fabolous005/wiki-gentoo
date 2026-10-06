<!-- source: https://wiki.gentoo.org/wiki/Connman | group: Gentoo Wiki (Main) | wiki-title: Connman -->
---
title: ConnMan
url: https://wiki.gentoo.org/wiki/Connman
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2022-05-03"
fingerprint: fd01c37d51aa38c3
license: CC BY-SA 4.0
---

# ConnMan

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**ConnMan** (short for **Conn**ection **Man**ager) is a [network management](https://wiki.gentoo.org/wiki/Network_management) service created by [Intel](https://wiki.gentoo.org/wiki/Category:Intel), built with [embedded](https://wiki.gentoo.org/wiki/Embedded_systems) use cases in mind. It is shipped by default on some desktop and mobile distributions however, such as [Sailfish OS](https://en.wikipedia.org/wiki/Sailfish_OS).

## Installation

### USE flags


### USE flags for
            [net-misc/connman](https://packages.gentoo.org/packages/net-misc/connman)
            
            Provides a daemon for managing internet connections

| [+ethernet](https://packages.gentoo.org/useflags/+ethernet) | Enable ethernet support. | 
| [+nftables](https://packages.gentoo.org/useflags/+nftables) | Use net-firewall/nftables as firewall. | 
| [+wifi](https://packages.gentoo.org/useflags/+wifi) | Enable wireless network functions | 
| [bluetooth](https://packages.gentoo.org/useflags/bluetooth) | Enable Bluetooth Support | 
| [debug](https://packages.gentoo.org/useflags/debug) | Enable extra debug codepaths, like asserts and extra output. If you want to get meaningful backtraces see https://wiki.gentoo.org/wiki/Project:Quality\_Assurance/Backtraces | 
| [doc](https://packages.gentoo.org/useflags/doc) | Add extra documentation (API, Javadoc, etc). It is recommended to enable per package instead of globally | 
| [examples](https://packages.gentoo.org/useflags/examples) | Install examples, usually source code | 
| [iptables](https://packages.gentoo.org/useflags/iptables) | Use net-firewall/iptables as firewall. | 
| [iwd](https://packages.gentoo.org/useflags/iwd) | Enable wifi support via net-wireless/iwd | 
| [l2tp](https://packages.gentoo.org/useflags/l2tp) | Use net-dialup/xl2tpd for L2TP VPN support. | 
| [networkmanager](https://packages.gentoo.org/useflags/networkmanager) | Enable net-misc/networkmanager support | 
| [ofono](https://packages.gentoo.org/useflags/ofono) | Use net-misc/ofono for telephony support. | 
| [openconnect](https://packages.gentoo.org/useflags/openconnect) | Use net-vpn/openconnect for VPN support. | 
| [openvpn](https://packages.gentoo.org/useflags/openvpn) | Use net-vpn/openvpn for openvpn support. | 
| [policykit](https://packages.gentoo.org/useflags/policykit) | Enable PolicyKit (polkit) authentication support | 
| [pptp](https://packages.gentoo.org/useflags/pptp) | Use net-dialup/pptpclient for PPTP VPN support. | 
| [tools](https://packages.gentoo.org/useflags/tools) | Enable testing tools. | 
| [vpnc](https://packages.gentoo.org/useflags/vpnc) | Use net-vpn/vpnc for cisco VPN support. | 
| [wireguard](https://packages.gentoo.org/useflags/wireguard) | Enable WireGuard VPN support. | 
| [wispr](https://packages.gentoo.org/useflags/wispr) | Enable support for WISPr hotspot logins. | 

Make sure to enable the proper USE flags for planned use. For mobile data support, make sure to enable the `ofono` USE flag.

### Emerge

`root #``emerge --ask net-misc/connman`
## Configuration

### Services

#### OpenRC

To enable ConnMan at boot:

`root #``rc-update add connman default`
To start ConnMan now:

`root #``rc-service connman start`
#### systemd

To enable ConnMan at boot and start it now:

`root #``systemctl enable --now connman`
## Usage

connmanctl is an interactive CLI application that comes installed with ConnMan. Make sure `connmand` has been started, either manually or through the init system, or ConnMan will be unable to function.

connmanctl can be launched with no arguments to enter interactive mode, or a command can be specified after it (useful for scripting!)

### Connecting to a network

#### Listing available technologies

`user $````
connmanctl technologies
```
Should list the available networking technologies for ConnMan, such as `/net/connman/technology/wifi` (WiFi support). If desired technology isn't listed, make sure it was set in ConnMan's USE flags, and that support for the network interface is built into the device's kernel.

#### Powering on the selected technology interface

`user $````
connmanctl enable wifi
```
Replace `wifi` with a network interface technology as desired.

#### Scanning wireless networks

`user $````
connmanctl scan wifi
```
"Scan completed for wifi" should return when scanning is done.

#### Showing available services

`user $````
connmanctl services
```
This outputs all available services, no matter if WiFi, Ethernet, or other. If the device is connected to the network, `*AO` is in the leftmost column, followed by the SSID in the next, and then followed by a unique identifier. This unique identifier is needed to connect to the network.

#### Enabling connecting to an encrypted network

`user $````
connmanctl agent on
```
#### Configuring a VPN

Make sure connman was emerged with the necessary USE flag ("openvpn" for example).

In the case of openVPN, a working openvpn configuration can easily be used by editing /var/lib/connman-vpn/myvpnname.config and adding

**`/var/lib/connman-vpn/myvpnname.config`**

```
[provider_openvpn]
Type = OpenVPN
Name = myVPNname
Host = openvpn.mydomain.com
Domain = openvpn.mydomain.com
OpenVPN.ConfigFile = /etc/openvpn/myvpnname.conf
```
Some connman GUI (see below) or Enlightenment's built in connman client can be used to connect to the VPN ("connmanctl connect" does NOT seem to work). It will show up as the string provided by "Name" in the above config file.

The connection is established as it should, but connman always sets the default route over the first service.

To not route the whole traffic over the VPN connection (or the openVPN server is only configured to route private networks), the order of the connections has to be changed (this does not seem to be documented anywhere!):

`user $``connmanctl`
Use "move-after" or "move-before" to push the VPN down after local uplink.

#### Connect to a network

`user $``connmanctl connect` *your_unique_identifier*
Fill in the unique identifier from what is after network name from `services`. If connecting to a wired or unsecured network, no further action is needed. If connecting to a network with a passphrase, enter it when prompted. **Note: Double check password! ConnMan can often be annoying and not re-prompt for incorrect passwords.**

#### Testing a connection

To reveal the connection to the Local Area Network (LAN):

`user $````
connmanctl state
```
And of course to test connection to the internet:

`user $````
ping gentoo.org
```
## Graphical front-ends

### CMST

**CMST** is a Qt graphical front-end for ConnMan, and comes with a system tray icon. See [net-misc/cmst](https://packages.gentoo.org/packages/net-misc/cmst).

### connman-gtk

This is the GTK counterpart. The project (in portage) does not seem to be active anymore, but there is [https://github.com/debiangamer/connman-gtk](https://github.com/debiangamer/connman-gtk) which has a few improvements.

### EConnMan

**EConnMan** is the ConnMan UI that's part of the Enlightenment desktop environment. See [net-misc/econnman](https://packages.gentoo.org/packages/net-misc/econnman).

This is deprecated and seems abandoned.

The package is not necessary for using connman, connecting to a network or entering a wifi passphrase. This all works out of the box. Connecting to a VPN is also supported, but advanced configuration options are not available through the UI. Use connmanctl or config files (as stated above for VPN connections) to configure these connections.

## Troubleshooting

### ConnMan is throwing some cryptic error I can't find a reference to online!

It's most likely lack of kernel support for the device's WiFi card. Make sure if built as a module it's loaded and that dmesg isn't complaining about a load error.

It is possible to try using Gentoo's distribution kernel to see if that gets the network card going. If so, run lsmod with and without the distribution kernel and diff the results to view the kernel drivers that were in use with the distribution kernel that weren't with whichever was running originally.

### How to use ConnMan along with NetworkManager/dhcpcd?

**`/etc/connman/main.conf`**

```
NetworkInterfaceBlacklist = usb,wlan
```
Obviously, change the interface names to those that are desired to be to blacklistd with ConnMan. This allows another service to manage those connections, while utilizing ConnMan for other network interfaces (ie, ofono, [bluetooth](https://wiki.gentoo.org/wiki/Bluetooth)). If connected via [SSH](https://wiki.gentoo.org/wiki/SSH), this should be done first to prevent ConnMan closing the SSH tunnel.

### Error: no carrier

This can mean that there isn't proper firmware support for the technology. However, this error can also be caused by improper permissions. Try adding the user to the wheel and network groups, or run connmanctl as root.

## See also

- [Dhcpcd](https://wiki.gentoo.org/wiki/Dhcpcd) — a popular DHCP client capable of handling both IPv4 and IPv6 configuration.
- [Netifrc](https://wiki.gentoo.org/wiki/Netifrc) — Gentoo's default framework for configuring and [managing network](https://wiki.gentoo.org/wiki/Network_management) interfaces on systems running [OpenRC](https://wiki.gentoo.org/wiki/OpenRC).
- [NetworkManager](https://wiki.gentoo.org/wiki/NetworkManager) — a [network management software](https://wiki.gentoo.org/wiki/Network_management) for [Ethernet](https://wiki.gentoo.org/wiki/Ethernet), [WiFi](https://wiki.gentoo.org/wiki/Wifi), DSL, dialup, VPN, [WiMAX](https://wiki.gentoo.org/wiki/WiMAX), and mobile broadband network connections
