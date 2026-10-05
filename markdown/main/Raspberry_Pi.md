<!-- source: https://wiki.gentoo.org/wiki/Raspberry_Pi | group: Gentoo Wiki (Main) | wiki-title: Raspberry Pi -->
---
title: Raspberry Pi
url: https://wiki.gentoo.org/wiki/Raspberry_Pi
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-07-30"
fingerprint: "1373628b5dd20836"
license: CC BY-SA 4.0
---

# Raspberry Pi

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

The **Raspberry Pi** is a series of small single-board computers.

These machines are well supported by Gentoo. There are two main ways of installing Gentoo on them:

- cross-compiling
- using Raspberry Pi OS, or the gentoo "Minimal Installation CD", as a system to provide a chroot. - Note: This method is more complex and less supported.



## Installation Instructions

There is no Gentoo ARM(64) Handbook in general since the hardware and therefore the installation process differs much more than installation for e.g. x86 hardware.

The below guide is the recommended for both 32and 64bit Raspberry Pis:

## RPi usage guides

A collection of guides of interesting projects to run on a Raspberry Pi:

## Hardware

| Model | CPU | Architecture | Stage3 | 
|---|---|---|---|
| Raspberry PI Zero | BCM2708 | ARM | [ARMv6j stage 3](http://gentoo.org/downloads/#arm) | 
| Raspberry PI (Original) | BCM2708 | ARM | [ARMv6j stage 3](http://gentoo.org/downloads/#arm) | 
| Raspberry PI Zero w | BCM2708 | ARM | [ARMv6j stage 3](http://gentoo.org/downloads/#arm) | 
| Raspberry PI 2b Before Ver 1.2 | BCM2709 | ARM | [ARMv7a stage 3](http://gentoo.org/downloads/#arm) | 
| Raspberry PI 2b Ver 1.2 | BCM2710 | ARM/ARM64 | [ARMv7a stage 3](http://gentoo.org/downloads/#arm) or [arm64 stage3](http://gentoo.org/downloads/#arm64) | 
| Raspberry PI 3b | BCM2710 | ARM/ARM64 | [ARMv7a stage 3](http://gentoo.org/downloads/#arm) or [arm64 stage3](http://gentoo.org/downloads/#arm64) | 
| Raspberry PI 3b+ | BCM2710 | ARM/ARM64 | [ARMv7a stage 3](http://gentoo.org/downloads/#arm) or [arm64 stage3](http://gentoo.org/downloads/#arm64) | 
| Raspberry PI Zero 2 | BCM2710 | ARM/ARM64 | [ARMv7a stage 3](http://gentoo.org/downloads/#arm) or [arm64 stage3](http://gentoo.org/downloads/#arm64) | 
| Raspberry PI Zero 2 w | BCM2710 | ARM/ARM64 | [ARMv7a stage 3](http://gentoo.org/downloads/#arm) or [arm64 stage3](http://gentoo.org/downloads/#arm64) | 
| Raspberry PI 4b | BCM2711 | ARM/ARM64 | [ARMv7a stage 3](http://gentoo.org/downloads/#arm) or [arm64 stage3](http://gentoo.org/downloads/#arm64) | 
| Raspberry PI CM4 | BCM2711 | ARM/ARM64 | [ARMv7a stage 3](http://gentoo.org/downloads/#arm) or [arm64 stage3](http://gentoo.org/downloads/#arm64) | 
| Raspberry PI 5 | BCM2712 | ARM/ARM64 | [ARMv7a stage 3](http://gentoo.org/downloads/#arm) or [arm64 stage3](http://gentoo.org/downloads/#arm64) | 
| Raspberry PI CM5 | BCM2712 | ARM/ARM64 | [ARMv7a stage 3](http://gentoo.org/downloads/#arm) or [arm64 stage3](http://gentoo.org/downloads/#arm64) | 
| Raspberry PI 6 | TBD | TBD |  | 

There are some ARM SoCs that support 64-bit but no 32-bit. Those weren't used in the Pis.

### WiFi 5Ghz DFS Channels

On the Raspberry Pi you may have issues being able to see your WiFi 5Ghz networks. You should be able to advise the chip which region you are in and be able to enable the DFS channels, but that does not seem to work. It is unknown if this is a hardware or driver issue at this time. The workaround for this issue is to set your access point/router to not use the DFS channels and then you should be able to see your networks in the list. This only applies to the Raspberry Pi systems that have a WiFi 5Ghz capable chip, PI3 and above.

An [extensive table of Pi hardware](https://en.wikipedia.org/wiki/Raspberry_Pi#Specifications) is available in the English Wikipedia.
