<!-- source: https://wiki.gentoo.org/wiki/Sparc/Sun_hardware_compatibility_list | group: Gentoo Wiki (Main) | wiki-title: Sparc/Sun hardware compatibility list -->
---
title: Sparc/Sun hardware compatibility list
url: https://wiki.gentoo.org/wiki/Sparc/Sun_hardware_compatibility_list
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-04-16"
fingerprint: "1e161b59ced2b58c"
license: CC BY-SA 4.0
---

# Sparc/Sun hardware compatibility list

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

This hardware compatibility list details different Sun SPARC64 systems and Linux kernel combinations that have been tested by the developer and user community with their known caveats.

## About this document

This document provides a listing of different Sun SPARC64 machine models and their compatibility with different Linux kernel versions.

This document does not guarantee that you will have similar results with the same hardware, but simply documents the experiences of others. Please keep this in mind if you are thinking of making purchasing decisions based on this document.

## Recommended kernel versions

We currently recommend 2.6-series kernels since 2.4-series are being phased out for Gentoo/SPARC and don't support the latest hardware like UltraSPARC-T1 processors. Usually the latest stable is preferred.

## Hardware compatibility reports

### 2.4 series kernels

| Model | Kernel | Stability | OBP | Net | Sound | Framebuffer | Submitter | Comments | 
|---|---|---|---|---|---|---|---|---|
| E250 | 2.4.32-sparc-r4 | Excellent | 3.22.0 | YES | N/A | Permedia 2 (PCI) | gustavoz | Permedia for console only using PROM\_CONSOLE, RSC isn't supported at all | 
| E4000 | 2.4.32-sparc-r2 | Excellent | N/A | YES | N/A | Turbo GX | Afsin Taskiran | Used as firewall with 9 ethernet interfaces | 
| E420R | 2.4.32-sparc-r1 | Excellent | 3.23.0 | YES | N/A | ATI Rage (PCI) | gustavoz | 4x450 mhz | 
| E6500 | 2.4.33.3-sparc | Excellent | 3.2.30 | YES | N/A | N/A | alunduil | 7x400mhz | 
| Ultra 2 | 2.4.32-sparc-r1 | Excellent | 3.25.0 | YES | YES | Creator3D Series 1 | Weeve | 2x300MHz | 
| Ultra 5/10 | 2.4.32-sparc-r4 | Excellent | 3.31.0 | YES | YES | Onboard ATI (PCI) | gustavoz | None | 
| Netra X1 | 2.4.32-sparc-r5 | Excellent | N/A | YES | N/A | N/A | tomric | 400/500 mhz IIe | 

### 2.6 series kernels

| Model | Kernel | Stability | OBP | Net | Sound | Framebuffer | Submitter | Comments | 
|---|---|---|---|---|---|---|---|---|
| Blade 100 | 2.6.16-rc1 | Excellent | 4.17.1 | YES | YES | Onboard ATI Rage XL (PCI) | Weeve | 500MHz, works great with Sun compatible Symbios PCI SCSI controller and drives. | 
| Blade 1000 | 2.6.15.1 | Very good | 4.16.4 | YES | YES | XVR-100 (PCI) | Weeve | 2x750MHz, BBC\_I2C driver impacts system performance, using in kernel ALSA drivers for onboard CS4231 and Hercules Game Theater XP sound devices. | 
| Blade 1000 | 2.6.25-gentoo-r6 | Excellent | 4.13.0 | YES | YES | Elite3D m3 | fmccor | 1x900MHz+1x750MHz, using kernel ALSA sound driver for snd\_ens1371 (PCI card). | 
| E3000 | 2.6.22-gentoo-r9 | Excellent | 3.2.20 | YES | N/A | None | brendan | 4x336mhz, 4MB cache, 4GB ram, Sun FC-AL | 
| E420R | 2.6.15-gentoo-r1 | Very good | 3.23.0 | YES | N/A | ATI Rage (PCI) | gustavoz | 4x450mhz | 
| E420R | 2.6.25-gentoo-r6 | Very stable | 3.31.0 | YES | N/A | None | Sebastian Sitaru | 4x450mhz, 4096MB | 
| Netra T1-105 | 2.6.15-gentoo-r7 | Very good | 3.10.24 | YES | N/A | N/A | the\_eye | 360Mhz | 
| Netra T1-105 | 2.6.17-gentoo-r9 | Very good | 3.10.27 | YES | N/A | N/A | gablau | 440Mhz, 1GB ram, 2x18GB SCSI RAID1 software, SILO 1.4.13 | 
| Ultra 2 | 2.6.15.1 | Very good | 3.25.0 | YES | YES | Creator3D Series 1 | Weeve | 2x300MHz, using kernel provided ALSA drivers, high I/O loads on rare occasion cause a lockup (just seems to be when I'm intentionally trying to do this these days) | 
| Ultra 2 | 2.6.20-gentoo-r7 | Excellent | 3.19.0 | YES | N/A | Creator3D Series 3 | fmccor | 2x400MHz, Up 124 days now without any failures at all. Old problems are fixed. | 
| Ultra 2 | 2.6.17-gentoo-r8 | Excellent | 3.25.0 | YES | YES | CG6 | alexbuell | 2x300Mhz, 2GB, no lockups, CG6 does console right but fails with xorg 7.1.1 | 
| Ultra 5/10 | 2.6.20-gentoo-r4 | Excellent | 3.31.0 | YES | YES | Onboard ATI (PCI) | gustavoz | snd-sun-cs4231 for alsa audio. | 
| Ultra 60 | 2.6.16-rc2 | Excellent | 3.31.0 | YES | YES | Creator3D Series 3 | Weeve | 1x450MHz, Haven't tested X on this particular machine, but this framebuffer and system combination known to work for others | 
| Ultra 60 | 2.6.15-gentoo-r4 | Excellent | 3.29.0 | YES | NO | Elite3D m3 | fmccor | 2x450MHz, X (X-modular = xorg-x11-7.0-r1) on this system is fine. | 
| Ultra 80 | 2.6.16-rc2 | Very good | 3.23.0 | YES | Untested | Elite3D | Etaoin Shrdlu | 4x450Mhz, server tasks, no X | 
| T2000 | 2.6.17-rc2 | Unknown | 4.19.0 | YES | N/A | N/A | pkessler,Pyretic | Needs silo 1.4.11+, vanilla-sources-2.6.17-rc2+ and the latest experimental 2.6 livecd to install. | 
| T2000 | 2.6.20-gentoo-r4 | Very good | 4.25.0 | YES | N/A | N/A | gustavoz | Install with 2006.1 or newer media | 
| Fire 280R | 2.6.15-gentoo-r8 | Very good | 4.16.4 | YES | N/A | N/A | gustavoz | 2x900Mhz Cu, BBC\_I2C is b0rked as in Blade 1000/2000 | 
| Fire V100 | 2.6.16-gentoo-r3 | Very good | 4.0.18 | YES | N/A | N/A | gustavoz | Using tulip driver for net, ports are somewhat sensitive to bad cabling | 
| Fire V120 | 2.6.17-gentoo-r8 | Very good | 4.0.12 | YES | N/A | N/A | Weeve | Onboard interfaces work perfectly via sungem driver. PCI addon board with Sun Cassini interface causes a lockup when the driver is loaded however. | 
| Fire V210 | 2.6.20-gentoo-r4 | Very good | 4.22.23 | YES | N/A | N/A | gustavoz | Installed using 2006.1 media, uniprocessor @ 1ghz. | 
| Fire V240 | 2.6.17-gentoo | Very good | 4.17.1 | YES | N/A | N/A | gustavoz | Needs silo 1.4.10+ for 2.6 kernels. | 
| Fire V250 | 2.6.16-gentoo-r9 | Very good | 4.17.1 | YES | N/A | N/A | Chris Cosby | Using silo 1.4.11+ and experimental 2.6 LiveCD | 
| Fire V880 | 2.6.17-gentoo-r9 | Excellent | 4.5.6 | YES | N/A | 3D Rage XL | Rui Pimenta | 2x750MHz, silo 1.4.13, Eth0+1: Sun GEM, QLogic QLA22xx, Xorg 7.1.1 + gnome | 

### 4.0 series kernels

| Model | Kernel | Stability | OBP | Net | Sound | Framebuffer | Submitter | Comments | 
|---|---|---|---|---|---|---|---|---|
| Fire T5140 | 4.0.5-gentoo | Excellent | 4.28.8 | 4x 10Gbps Sun Neptune | N/A | Sun XVR300x8/FireMv 2200 x8 128M panics with 3.14 kernel | James McMechan | 64x1.2GHz -- auto build iso panics due to 3.14 kernel, silo 1.4.14\_git20120819\_p1 wants zImage not image, netboot requires make tftpboot.img | 
| Fire T5120 | 4.5.0-gentoo | Very Good | 4.33.6.f | 1x 1G Sun VNET | N/A | N/A | Malcolm Staudinger | SILO 1.4.14\_git20120819\_p1. Built in LDOM. Bootstrapped from Oracle Linux for Sparc 1.0 image (recent gentoo minimal livecd hung on hwraid initialization) | 

### 6.15 series kernels

| Model | Kernel | Stability | OBP | Net | Sound | Framebuffer | Submitter | Comments | 
|---|---|---|---|---|---|---|---|---|
| Fire V240 | 6.15.0-gentoo | Excellent | OBP 4.30.4.a 2010/01/06 | 4x 1Gbps Broadcom BCM5704 | N/A | N/A | Stian Halseth | 2xTI UltraSparc IIIi (Jalapeno) @1.5GHz -- booting with Grub | 
| SPARC T4-1 | 6.15.1-gentoo | Excellent | OBP 4.36.2 2014/10/24 08:13 | 4xIntel Corporation 82576 Gigabit Network Connection (rev 01) | N/A | N/A | Stian Halseth | 1xUltraSparc T4 (Niagara4)-- booting with Grub | 

### 6.17 series kernels

| Model | Kernel | Stability | OBP | Net | Sound | Framebuffer | Submitter | Comments | 
|---|---|---|---|---|---|---|---|---|
| Fire V240 | 6.17.9-gentoo | Excellent | OBP 4.30.4.a 2010/01/06 | 4x 1Gbps Broadcom BCM5704 | N/A | N/A | Stian Halseth | 2xTI UltraSparc IIIi (Jalapeno) @1.5GHz -- booting with Grub | 
| SPARC T4-1 | 6.17.9-gentoo | Excellent | OBP 4.36.2 2014/10/24 08:13 | 4xIntel Corporation 82576 Gigabit Network Connection (rev 01) | N/A | N/A | Stian Halseth | 1xUltraSparc T4 (Niagara4)-- booting with Grub | 

### 7.0 series kernels

| Model | Kernel | Stability | OBP | Net | Sound | Framebuffer | Submitter | Comments | 
|---|---|---|---|---|---|---|---|---|
| Fire V240 | 7.0.0-gentoo | Excellent | OBP 4.30.4.a 2010/01/06 | 4x 1Gbps Broadcom BCM5704 | N/A | N/A | Stian Halseth | 2xTI UltraSparc IIIi (Jalapeno) @1.5GHz -- booting with Grub (target=sparc64-ieee1275) | 
| SPARC T4-1 | 7.0.0-gentoo | Excellent | OBP 4.36.2 2014/10/24 08:13 | 4xIntel Corporation 82576 Gigabit Network Connection (rev 01) | N/A | N/A | Stian Halseth | 1xUltraSparc T4 (Niagara4)-- booting with Grub (target=sparc64-ieee1275) |
