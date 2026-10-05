<!-- source: https://wiki.gentoo.org/wiki/Mullvad_VPN | group: Gentoo Wiki (Main) | wiki-title: Mullvad VPN -->
---
title: Mullvad VPN
url: https://wiki.gentoo.org/wiki/Mullvad_VPN
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2025-06-14"
fingerprint: e610e7d7293113c0
license: CC BY-SA 4.0
---

# Mullvad VPN

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**Mullvad VPN** is a no-log, open-source VPN.

## Requirements

Notice: this section is currently WIP.

### Kernel

### Netfilter

"mullvad" netfilter table must be set

## Installation

Mullvad is available on the [GURU](https://wiki.gentoo.org/wiki/Project:GURU/Information_for_End_Users). Please follow those steps to enable the GURU if it is not already enabled.

### Emerge

`root #``emerge --ask net-vpn/mullvadvpn-app`
## Usage

### Enabling the service

Installing Mullvad provides both a [daemon](https://wiki.gentoo.org/index.php?title=Daemon&action=edit&redlink=1) and the app (client) itself. In order to use Mullvad, first enable the daemon, and then start the app.

#### OpenRC

`root #``rc-update add mullvad-daemon default``root #``rc-service mullvad-daemon start`
#### systemd

`root #``systemctl enable mullvad-daemon``root #``systemctl start mullvad-daemon`
### Starting the client

Run the installed client using the `mullvad-vpn` command:

`user $``mullvad-vpn`
