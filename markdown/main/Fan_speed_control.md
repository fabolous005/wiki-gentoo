<!-- source: https://wiki.gentoo.org/wiki/Fan_speed_control | group: Gentoo Wiki (Main) | wiki-title: Fan speed control -->
---
title: Fan speed control
url: https://wiki.gentoo.org/wiki/Fan_speed_control
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2025-03-25"
fingerprint: f56a1afe2a900b2c
license: CC BY-SA 4.0
---

# Fan speed control

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

## Introduction

When a computer works it gets warm, when it works hard it may get hot. Too hot is not good for the electronics of a computer. Therefore most computers need active cooling, through fans or in some cases water. In x86 type of systems the speed of the fans is typically controlled by the [BIOS](https://wiki.gentoo.org/wiki/BIOS), but it may be possible to control the fan speed with the [kernel](https://wiki.gentoo.org/wiki/Kernel) and userspace utilities.

Gentoo supports various of those userspace utilities, e.g. [fancontrol](https://wiki.gentoo.org/wiki/Fan_speed_control/lm_sensors%27_fancontrol), and [Thinkfan](https://wiki.gentoo.org/wiki/Fan_speed_control/thinkfan).

Reasons for wanting to deviate from BIOS controlled fans could be that the BIOS makes them spin too much (resulting in too much noise), or too slow (resulting in too high temperatures).

## Requirements

Controlling fan speed is not supported on all computers. The computer needs to have:

- A BIOS that allows users to control the fan speed
- A motherboard capable of regulating fan speeds
- Fans that allow their speed to be controlled

## Sensors

Specific hardware may have specific needs with respect to kernel configuration. See [lm\_sensors](https://wiki.gentoo.org/wiki/Lm_sensors) for generic details.

## Fans

Fans have 2, 3, or 4 wires:

1. ground
2. fan voltage
3. tacho
4. PWM


Fans with 2 or 3 wires may have voltage regulation: higher voltage (typically up to 12 Volt) results in higher rotation per minute (RPM). Fans with 4 wires are controlled through pulse width modulation (PWM), in theory a more efficient way of controlling the fan speed. The third wire provides feedback on the actual RPM of the fan.
