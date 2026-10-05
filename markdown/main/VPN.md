<!-- source: https://wiki.gentoo.org/wiki/VPN | group: Gentoo Wiki (Main) | wiki-title: VPN -->
---
title: VPN
url: https://wiki.gentoo.org/wiki/VPN
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2025-02-01"
categories: ['p.g.o/categories/net-vpn']
fingerprint: "255eab03c0b0a087"
license: CC BY-SA 4.0
---

# VPN

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

This article provides a list of some VPN options available in Gentoo Linux.

## Available software

This is just a partial selection of packages available, see [p.g.o/categories/net-vpn](https://packages.gentoo.org/categories/net-vpn), or use [eix](https://wiki.gentoo.org/wiki/Eix) (eix --category net-vpn), for more packages from the Gentoo repository.

| Name | Package | Description | 
|---|---|---|
| [Hamachi](https://wiki.gentoo.org/wiki/Hamachi) | [net-vpn/logmein-hamachi](https://packages.gentoo.org/packages/net-vpn/logmein-hamachi) | Cross platform VPN tunneling engine. | 
| [I2P](https://wiki.gentoo.org/wiki/I2P) | [net-vpn/i2p](https://packages.gentoo.org/packages/net-vpn/i2p) | Invisible Internet Project, an anonymous network. Similar to Tor, I2P is internal, focusing on providing anonymous services within the network. | 
| [libreswan](https://libreswan.org/) | [net-vpn/libreswan](https://packages.gentoo.org/packages/net-vpn/libreswan) | IPsec implementation for Linux, fork of Openswan. | 
| [OpenVPN](https://wiki.gentoo.org/wiki/OpenVPN) | [net-vpn/openvpn](https://packages.gentoo.org/packages/net-vpn/openvpn) | Open Virtual Private Network that enables the creation of secure point-to-point or site-to-site connections. | 
| [strongSwan](https://www.strongswan.org/) | [net-vpn/strongswan](https://packages.gentoo.org/packages/net-vpn/strongswan) | IPsec-based VPN solution, supporting IKEv1/IKEv2 and MOBIKE. | 
| [Tinc](https://wiki.gentoo.org/wiki/Tinc) | [net-vpn/tinc](https://packages.gentoo.org/packages/net-vpn/tinc) | VPN daemon using tunneling and encryption. | 
| [Tor](https://wiki.gentoo.org/wiki/Tor) | [net-vpn/tor](https://packages.gentoo.org/packages/net-vpn/tor) | Tor is an onion routing Internet anonymity system. | 
| [vpnc](https://wiki.gentoo.org/wiki/Vpnc) | [net-vpn/vpnc](https://packages.gentoo.org/packages/net-vpn/vpnc) | VPNC is a IPsec (Cisco/Juniper) VPN concentrator client to manage secure connections. | 
| [WireGuard](https://wiki.gentoo.org/wiki/WireGuard) | [net-vpn/wireguard-tools](https://packages.gentoo.org/packages/net-vpn/wireguard-tools) | Modern, simple, and secure VPN that utilizes state-of-the-art cryptography. | 

## Utility

[net-vpn/vopono](https://packages.gentoo.org/packages/net-vpn/vopono) is a tool to run applications through VPN tunnels via temporary network namespaces. It allows one to run only multiple applications through different VPNs simultaneously, whilst keeping the main connection as normal. It also has a killswitch for openVPN and Wireguard.
