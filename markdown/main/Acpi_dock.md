<!-- source: https://wiki.gentoo.org/wiki/Acpi_dock | group: Gentoo Wiki (Main) | wiki-title: Acpi dock -->
---
title: ACPI dock
url: https://wiki.gentoo.org/wiki/Acpi_dock
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2019-01-09"
fingerprint: "9ec78b5ed1e69b84"
license: CC BY-SA 4.0
---

# ACPI dock

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)


This article describes the setup of an [ACPI](https://wiki.gentoo.org/wiki/ACPI) based docking stations and media bays.

## Installation

Activate the following kernel options for dock support:

**Enabling ACPI dock support**

## Usage

After the driver is loaded, for each docking station or media bay a directory /sys/devices/platform/dock.0, /sys/devices/platform/dock.1, etc. will be created. The files and their information can be used in scripts to adjust power management, monitors (screens), or any dock related settings.
