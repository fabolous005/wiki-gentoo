<!-- source: https://wiki.gentoo.org/wiki/Citrix_Workspace | group: Gentoo Wiki (Main) | wiki-title: Citrix Workspace -->
---
title: Citrix Workspace
url: https://wiki.gentoo.org/wiki/Citrix_Workspace
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-07-20"
fingerprint: da5e94708d4ecacb
license: CC BY-SA 4.0
---

# Citrix Workspace

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

Citrix Workspace App (previously known as Citrix Receiver and ICA Client) is the app used to connect to Citrix Presentation Servers and display remote content (along with more advanced functionality like redirecting local printers, etc.)

## Installation

`root #``emerge --ask net-misc/icaclient`
### Configuration

Users that wish to redirect peripherals must be in the `usb` group.

## Troubleshooting

If Citrix doesn't present with any output, try running it manually.

`user $``wfica ~/Downloads/WGVuQXBwNy5BY3JvYmF0IFJlYWRlciBEQw--.ica`
