<!-- source: https://wiki.gentoo.org/wiki/WiMAX | group: Gentoo Wiki (Main) | wiki-title: WiMAX -->
---
title: WiMAX
url: https://wiki.gentoo.org/wiki/WiMAX
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2019-01-09"
fingerprint: e60cdb775de2c30f
license: CC BY-SA 4.0
---

# WiMAX

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

The **WiMAX** (**Worldwide Interoperability for Microwave Access**) system provides users mobile broadband Internet using the 2G and 3G networks. This article explains the setup of WiMAX USB dongles.

## Supported hardware

This workflow is tested on the following hardware:

| Manufacturer | Model | USB IDs | Works? | 
|---|---|---|---|
| Alcatel | One Touch X220L | 1bbb:f000 1bbb:0017 |  | 
| Intel | Intel Corporation WiMAX/WiFi Link 5150 | 8086:423d |  | 

## Installation

### Prerequisites

Set [PPP](https://wiki.gentoo.org/wiki/PPP) first.

### Kernel

You need to activate the following kernel options:

### USB\_ModeSwitch

Most USB WiMAX dongles have a double mode. See the [USB\_ModeSwitch](https://wiki.gentoo.org/wiki/USB_ModeSwitch) article for more information and instructions.
