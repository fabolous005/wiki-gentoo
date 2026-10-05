<!-- source: https://wiki.gentoo.org/wiki/Embedded_Handbook/Boards/Pine64_STAR64 | group: Gentoo Wiki (Main) | wiki-title: Embedded Handbook/Boards/Pine64 STAR64 -->
---
title: Embedded Handbook/Boards/Pine64 STAR64
url: https://wiki.gentoo.org/wiki/Embedded_Handbook/Boards/Pine64_STAR64
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2024-05-26"
fingerprint: "5291fad8c1dd7bca"
license: CC BY-SA 4.0
---

# Embedded Handbook/Boards/Pine64 STAR64

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

This page gives details on my attempts to install Gentoo in a standard desktop install on a RISC-V SBC.

## Introduction

This page aims to provide a comprehensive introduction to the STAR64 device and running Gentoo on RISC-V as though it was a standard x86\_64 desktop. All packages will be compiled locally.

The majority of the information here is specific to the STAR64 and VisionFive 2 Single Board Computers, (SBC/SBCs). Please apply some common sense when adapting these instructions for other devices and do **not** blindly copy and paste commands without understanding what they do.

It should also be noted that the processes described hereafter are not always the most efficient in terms of commands used, I am still learning the GCC compilation toolchain and Gentoo in general. Additionally, the STAR64 has limited support opposed to the VisionFive 2 and there have been many moments where I have made questionable decisions regarding the firmware and bootloader of my device, these difficulties will be detailed in other sections below.
