<!-- source: https://wiki.gentoo.org/wiki/Logitech_bolt | group: Gentoo Wiki (Main) | wiki-title: Logitech bolt -->
---
title: Logitech bolt
url: https://wiki.gentoo.org/wiki/Logitech_bolt
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2023-11-16"
fingerprint: b42e4dc270eb8840
license: CC BY-SA 4.0
---

# Logitech bolt

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

The Logitech Bolt is an encrypted wireless receiver dongle for Bolt enabled devices.

To quote Logitech:

It’s Logitech’s next-gen wireless technology — delivering a high-performance, secure wireless connection when compatible mice and keyboards are connected via Logi Bolt USB receiver.


## Installation

### Kernel

**Enable Logitech hid++**

### Install

[Solaar](https://pwr-solaar.github.io/Solaar/) is a replacement for the OEM Logitech software. It allows pairing of bolt devices to their bolt receiver and configuring bolt and unify receiver enabled devices.

`root #``emerge --ask app-misc/solaar`
### Udev Rules

**`/etc/udev/rules.d/42-logitech-unify-permissions.rules`**

`root #``udevadm control --reload-rules`
### Hardware list

| Device | Version | Solaar Compat | Works? | Reported by | Notes | 
|---|---|---|---|---|---|
| MX Keys Mini | V1 | Yes | Yes | [nathanlkoch](https://wiki.gentoo.org/wiki/User:Nathanlkoch) | Everything works | 

Logitech bolt receivers can pair up to six keyboards and mice each. Logitech bolt receivers are fully HID complaint and work in the bios and are paired on a hardware level. Great for people who multi boot and do not require to be repaired.

## Troubleshooting

### Disconnects

If you are experiencing disconnects on either the Bolt or Unify receiver after enabling hid++ in the kernel, it could be because you are using an unpowered USB hub. Adding the kernel extension appear to either draw more power or cause conflicts when operating though a hub. Try connecting directly to your computer.
