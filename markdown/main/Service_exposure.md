<!-- source: https://wiki.gentoo.org/wiki/Service_exposure | group: Gentoo Wiki (Main) | wiki-title: Service exposure -->
---
title: Service exposure
url: https://wiki.gentoo.org/wiki/Service_exposure
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2025-10-09"
fingerprint: "77e92351fd01f9c5"
license: CC BY-SA 4.0
---

# Service exposure

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

This article introduces several ways to expose local services to devices on other networks.

Each device on the internet has a unique [IPv4](https://en.wikipedia.org/wiki/IPv4) address. Because IPv4 addresses can only address a maximum of about 4.4 billion devices, some internet service providers (ISPs) place [NAT](https://en.wikipedia.org/wiki/NAT) gateways between their customers' devices and the internet, in order to hide multiple devices behind one IPv4 address. In some cases, these NAT gateways run firewalls that prevent outside devices from establishing connections with devices on the ISPs' networks.[\[1\]](https://wiki.gentoo.org#cite_note-1)

Before NAT, enabling port forwarding was all that was needed to expose a service to the internet. With NAT, this is no longer a solution.

| Name | Package | Homepage | Description | 
|---|---|---|---|
| [Tailscale](https://wiki.gentoo.org/wiki/Tailscale) | [net-vpn/tailscale](https://packages.gentoo.org/packages/net-vpn/tailscale) | [https://tailscale.com/](https://tailscale.com/) | A VPN. Offers a free plan with no bandwidth restrictions; no private server needed. Offers fast speeds across all but the most complex network boundaries. Can expose one service per device to the entire internet. | 
| ZeroTier | [net-misc/zerotier](https://packages.gentoo.org/packages/net-misc/zerotier) | [https://zerotier.com/](https://zerotier.com/) | Similar to Tailscale. No option to expose services to the internet. | 
| [Wireguard](https://wiki.gentoo.org/wiki/Wireguard) | [net-vpn/wireguard-tools](https://packages.gentoo.org/packages/net-vpn/wireguard-tools) | [https://wireguard.com/](https://wireguard.com/) | Self-hosted; a private server is needed. Offers fast speeds, with no traffic flowing through the private server in some cases. | 

- [SSH tunneling](https://en.wikipedia.org/wiki/Tunneling_protocol#Secure_Shell_tunneling) — using a server on the internet to relay encrypted traffic.
- [How NAT traversal works](https://tailscale.com/blog/how-nat-traversal-works) — explains different NAT setups and how Tailscale bypasses them.
