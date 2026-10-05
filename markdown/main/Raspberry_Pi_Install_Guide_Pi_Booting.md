<!-- source: https://wiki.gentoo.org/wiki/Raspberry_Pi_Install_Guide/Pi_Booting | group: Gentoo Wiki (Main) | wiki-title: Raspberry Pi Install Guide/Pi Booting -->
---
title: Raspberry Pi Install Guide/Pi Booting
url: https://wiki.gentoo.org/wiki/Raspberry_Pi_Install_Guide/Pi_Booting
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2024-05-27"
fingerprint: "3fb702175c7ca3eb"
license: CC BY-SA 4.0
---

# Raspberry Pi Install Guide/Pi Booting

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

## Hardware

The first Raspberry Pis were monolithic. Almost everything was on the same piece of silicon, except the RAM, which was attached on top of the main chip, in a technique known as Package on Package (PoP). Over the years, buses have been added and functions have been split out. However the booting methodology remains unchanged.

## Booting

Every computer needs some instructions to execute at power on. The Pi is no exception. On the PC, the instructions are in the firmware and the CPU has a hard coded (in the CPU) start here address. Reset loads it into the Program Counter.

On the Pi, the GPU gets things started. The ARM CPU is held reset until the kernel, initrd and device tree binary are loaded. Then the ARM CPU is released from reset and begins executing the kernel.

Up to and including the Pi3 the startup code is fixed in the GPU at manufacture. There are no user serviceable parts inside.

From the Pi4, the DRAM bring up would not fit into the GPU, so the Pi4 and later have an upgradeable EEPROM. There have been upgrades to fix bugs and add new functionality. Pi4 and Pi5 users should update the EEPROM from time to time.

One of the features added was support for booting from GPT.
