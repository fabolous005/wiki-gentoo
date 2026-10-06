<!-- source: https://wiki.gentoo.org/wiki/Dtc | group: Gentoo Wiki (Main) | wiki-title: Dtc -->
---
title: Dtc
url: https://wiki.gentoo.org/wiki/Dtc
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2025-05-26"
fingerprint: "3600183d8fcbb2f8"
license: CC BY-SA 4.0
---

# Dtc

From Gentoo Wiki

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**Device Tree Compiler** (DTC) is a tool to create device tree binaries (dtbs) from device tree source.

## Installation

### USE flags


### USE flags for
            [sys-apps/dtc](https://packages.gentoo.org/packages/sys-apps/dtc)
            
            Open Firmware device tree compiler

| [python](https://packages.gentoo.org/useflags/python) | Add optional support/bindings for the Python language | 
| [static-libs](https://packages.gentoo.org/useflags/static-libs) | Build static versions of dynamic libraries as well | 
| [test](https://packages.gentoo.org/useflags/test) | Enable dependencies and/or preparations necessary to run tests (usually controlled by FEATURES=test but can be toggled independently) | 
| [yaml](https://packages.gentoo.org/useflags/yaml) | support .yaml-encoded device trees | 

### Emerge

`root #``emerge --ask sys-apps/dtc`
## Usage

Used when creating device tree binaries. Typically when create a primary bootloader like U-boot

### Decompiling dtb to dts

dtc can be used to decompile a **dtb** (*device tree blob*) back into a **dts** (*device tree source*) with:

`user $``dtc -I dtb infile.dtb -O dts -o outfile.dts`
## Removal

### Unmerge

`root #``emerge --ask --depclean --verbose sys-apps/dtc`
