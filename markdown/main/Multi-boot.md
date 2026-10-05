<!-- source: https://wiki.gentoo.org/wiki/Multi-boot | group: Gentoo Wiki (Main) | wiki-title: Multi-boot -->
---
title: Multi-boot
url: https://wiki.gentoo.org/wiki/Multi-boot
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-05-20"
fingerprint: "76c67b5a81e785c6"
license: CC BY-SA 4.0
---

# Multi-boot

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

Startup is the process in which, when a user hits the machine ON button ( ⏻ ), it brings it to a usable state. The startup process includes a list of steps where there is no clear consensus on where one starts and another begins; it includes the machine’s firmware doing a power on self test (POST) and passing control to the boot-loader, the loader finds and passes control to the kernel which in turn starts and passes control to the machine init-system. When the init-system finishes the user has a usable system.

A single-boot is a boot setup where only one operating system (OS) is installed and set to run on the machine; usually taking ownership of the hard drive (HDD), or solid-state drive (SSD). When the user starts the machine, it boots directly into the OS without offering options for choosing OSes. This configuration is the default on most machines.

Consequently, a multi-boot is a boot setup where there are more than one OSes installed on a single machine; on startup, a boot- manager or boot-loader shows a screen with OSes options for users to boot into. The multi-boot can be on one or multiple drives.

There are commonly two partitioning schemes used for drives in desktops and laptops; The legacy master boot record (MBR) and the modern GUID partition table (GPT), they differ in the way they boot and the tools used to configure and manage them.

## Creating the dual-boot

The user should start by reading their machine's architecture installation article found on [Handbook's Main Page](https://wiki.gentoo.org/wiki/Handbook:Main_Page), up to and including section [Creating filesystems](https://wiki.gentoo.org/wiki/Handbook:AMD64/Full/Installation#Creating_file_systems) before going into the following decision tree.

1. If the user's drive already holds an OS using the whole drive and can't be modified they'll need to get another one and proceed from step 3.
2. If the user's drive already holds an OS using the whole drive but can be modified e.g. some partitions could be resized and/or deleted, the user needs to do so, and proceed from step 4.
3. If the drive is empty the user needs to create at least two partitions and install their first OS in the first one; verify that it boots correctly before proceeding from step 4.
4. Proceed to install Gentoo on the second partition as per the machine's architecture installation article in the [Handbook's Main Page](https://wiki.gentoo.org/wiki/Handbook:Main_Page).
5. Re-boot the machine; the user should see a boot menu on startup.
6. Verify that both OSes boot correctly when selected from the menu.
7. Enjoy.

## Troubleshooting

If you follow the Handbook's installation instructions you'll be going through more decision trees and maybe following optional steps to install your Gentoo system. Here are a few suggestions:

- Note any error messages on the terminal.
- Retrace your steps.
- Try again.
- [Gentoo online.](https://wiki.gentoo.org/wiki/Handbook:AMD64/Full/Installation#Gentoo_online)
