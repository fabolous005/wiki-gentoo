<!-- source: https://wiki.gentoo.org/wiki/Tailscale | group: Gentoo Wiki (Main) | wiki-title: Tailscale -->
---
title: Tailscale
url: https://wiki.gentoo.org/wiki/Tailscale
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2025-11-26"
fingerprint: a7b160552d904bc7
license: CC BY-SA 4.0
---

# Tailscale

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

Tailscale facilitates remote access between devices and services across complex network boundaries such as [CGNAT](https://en.wikipedia.org/wiki/CGNAT).

Tailscale requires either [iptables](https://wiki.gentoo.org/wiki/Iptables) or [nftables](https://wiki.gentoo.org/wiki/Nftables) to be present. Ensure one or the other is installed before continuing.



**Enable`CONFIG_TUN` in the kernel**

Tailscale does not support any `USE` flags.

Merge the package:

`root #``emerge --ask net-vpn/tailscale`
Setting up Tailscale requires an account. Tailscale does not store passwords, but instead relies on third-party single sign-on (SSO) providers such as Google, GitHub or OpenID connect. Users can sign up at [login.tailscale.com](https://login.tailscale.com/).

Add the Tailscale daemon to the default runlevel:

`root #````
rc-service tailscale start
```
`root #````
rc-update add tailscale default
```
Enable the Tailscale daemon:

`root #````
systemctl start tailscaled.service
```
`root #````
systemctl enable tailscaled.service
```
`user $``tailscale --help````
USAGE
 The easiest, most secure way to use WireGuard.
USAGE
  tailscale [flags] <subcommand> [command flags]
For help on subcommands, add --help after: "tailscale status --help".
This CLI is still under active development. Commands and flags will
change in the future.
SUBCOMMANDS
  up          Connect to Tailscale, logging in if needed
  down        Disconnect from Tailscale
  set         Change specified preferences
  login       Log in to a Tailscale account
  logout      Disconnect from Tailscale and expire current node key
  switch      Switch to a different Tailscale account
  configure   Configure the host to enable more Tailscale features
  syspolicy   Diagnose the MDM and system policy configuration
  netcheck    Print an analysis of local network conditions
  ip          Show Tailscale IP addresses
  dns         Diagnose the internal DNS forwarder
  status      Show state of tailscaled and its connections
  metrics     Show Tailscale metrics
  ping        Ping a host at the Tailscale layer, see how it routed
  nc          Connect to a port on a host, connected to stdin/stdout
  ssh         SSH to a Tailscale machine
  funnel      Serve content and local servers on the internet
  serve       Serve content and local servers on your tailnet
  version     Print Tailscale version
  web         Run a web server for controlling Tailscale
  file        Send or receive files
  bugreport   Print a shareable identifier to help diagnose issues
  cert        Get TLS certs
  lock        Manage tailnet lock
  licenses    Get open source license information
  exit-node   Show machines on your tailnet configured as exit nodes
  update      Update Tailscale to the latest/different version
  whois       Show the machine and user associated with a Tailscale IP (v4 or v6)
  drive       Share a directory with your tailnet
  completion  Shell tab-completion scripts
FLAGS
  --socket value
        path to tailscaled socket (default /var/run/tailscale/tailscaled.sock)
```
Once the service is running, enable the VPN and follow the instructions:

`root #``tailscale up`
Unmerge the package:

`root #``emerge --ask --depclean --verbose net-vpn/tailscale`
- [WireGuard](https://wiki.gentoo.org/wiki/WireGuard) — a modern, simple, and secure VPN that utilizes state-of-the-art cryptography.
- [net-vpn/headscale](https://packages.gentoo.org/packages/net-vpn/headscale) — third-party self-hosted implementation of Tailscale control center.

- [Manage permissions (ACLs)](https://tailscale.com/kb/1018/acls) — used to restrict traffic between devices in both directions, optionally based on port number.
- [Subnet routers and traffic relay nodes](https://tailscale.com/kb/1019/subnets) — allows access to devices that can't run Tailscale on a local network.
- [Exit Nodes (route all traffic)](https://tailscale.com/kb/1103/exit-nodes) — routes all traffic through one device, similarly to popular VPN services. By default, Tailscale acts as an overlay network and does not route internet traffic.
- [Setting up a server on your Tailscale network](https://tailscale.com/kb/1245/set-up-servers) — provides instructions for setting up a server and limiting access to it.
- [Tailscale Funnel](https://tailscale.com/kb/1223/funnel) — exposes a service to the internet.
