<!-- source: https://wiki.gentoo.org/wiki/Nload | group: Gentoo Wiki (Main) | wiki-title: Nload -->
---
title: Nload
url: https://wiki.gentoo.org/wiki/Nload
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2022-05-13"
fingerprint: "8fddf17efe937fc8"
license: CC BY-SA 4.0
---

# Nload

From Gentoo Wiki

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

nload is a super simple, command-line network interface monitoring tool.

## Installation

### USE flags


### Emerge

`root #``emerge --ask net-analyzer/nload`
## Configuration

### Files

- Global - /etc/nload.conf
- Local - ${HOME}/.nload

## Usage

### Invocation

`user $``nload --help````
nload version 0.7.4
Copyright (C) 2001 - 2012 by Roland Riegel <feedback@roland-riegel.de>
nload comes with ABSOLUTELY NO WARRANTY. This is free software, and you are
welcome to redistribute it under certain conditions. For more details see the
GNU General Public License Version 2 (http://www.gnu.org/copyleft/gpl.html).
Command line syntax:
nload [options] [devices]
nload --help|-h
Options:
-a period       Sets the length in seconds of the time window for average
                calculation.
                Default is 300.
-i max_scaling  Specifies the 100% mark in kBit/s of the graph indicating the
                incoming bandwidth usage. Ignored if max_scaling is 0 or the
                switch -m is given.
                Default is 10240.
-m              Show multiple devices at a time; no traffic graphs.
-o max_scaling  Same as -i but for the graph indicating the outgoing bandwidth
                usage.
                Default is 10240.
-t interval     Determines the refresh interval of the display in milliseconds.
                Default is 500.
-u h|b|k|m|g    Sets the type of unit used for the display of traffic numbers.
   H|B|K|M|G    h: auto, b: Bit/s, k: kBit/s, m: MBit/s etc.
                H: auto, B: Byte/s, K: kByte/s, M: MByte/s etc.
                Default is h.
-U h|b|k|m|g    Same as -u, but for a total amount of data (without "/s").
   H|B|K|M|G    Default is H.
devices         Network devices to use.
                Default is to use all auto-detected devices.
--help
-h              Print this help.
example: nload -t 200 -i 1024 -o 128 -U M
The options above can also be changed at run time by pressing the 'F2' key.
```
## Removal

### Unmerge

`root #``emerge --ask --depclean --verbose net-analyzer/nload`
## See also

- [Iotop](https://wiki.gentoo.org/wiki/Iotop) — a top-like utility that monitors system **i**nput/**o**utput.
