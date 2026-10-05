<!-- source: https://wiki.gentoo.org/wiki/Xp-pen | group: Gentoo Wiki (Main) | wiki-title: Xp-pen -->
---
title: Xp-pen
url: https://wiki.gentoo.org/wiki/Xp-pen
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2025-07-03"
fingerprint: f859a5d79c6b98c
license: CC BY-SA 4.0
---

# Xp-pen

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

This article describes how to install [XP-PEN](https://en.wikipedia.org/wiki/XP-PEN) drivers on Gentoo.

## Prerequisites

### Kernel

The driver requires user level driver (`CONFIG_INPUT_UINPUT`) support, or else it won't be able to move the cursor.

**Enabling user level driver support**

### Dependencies

It also requires dependencies like [dev-qt/qtx11extras](https://packages.gentoo.org/packages/dev-qt/qtx11extras), [dev-qt/qtnetwork](https://packages.gentoo.org/packages/dev-qt/qtnetwork) and probably [x11-libs/libXrandr](https://packages.gentoo.org/packages/x11-libs/libXrandr).

## Installation

## Ebuild

An ebuild (x11-drivers/xf86-input-xppen) is provided in [GURU](https://wiki.gentoo.org/wiki/GURU). [Enable the GURU repository](https://wiki.gentoo.org/wiki/Project:GURU/Information_for_End_Users) on the system if it is not enabled already, then install with:

`root #``emerge --ask x11-drivers/xf86-input-xppen`
## Manual Install

Fetch and extract the XP-PEN driver installer:

`user $````
wget "https://download01.xp-pen.com/file/2024/07/XPPenLinux3.4.9-240131.tar.gz" -O xp-pen-driver.tar.gz
```
`user $````
tar -xvzpf xp-pen-driver.tar.gz
```
Run the installer:

`user $``cd XPPenLinux3.4.9-240131/``root #``./install.sh`
## Running

To launch the driver, run a script:

`user $``/usr/lib/pentablet/pentablet.sh`
## Removal

Go to the directory where you extracted the driver, and run:

`root #``./uninstall.sh`
