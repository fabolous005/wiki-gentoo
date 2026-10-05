<!-- source: https://wiki.gentoo.org/wiki/I8k | group: Gentoo Wiki (Main) | wiki-title: I8k -->
---
title: i8k
url: https://wiki.gentoo.org/wiki/I8k
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2024-09-30"
fingerprint: f2763e780ca6b9ce
license: CC BY-SA 4.0
---

# i8k

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)


This article describes the setup of *i8k*, a kernel module to monitor cpu temperature and control fan speed on Dell laptops.

## Installation

There is no need to install i8k kernel module as i8k kernel module comes ready in most recent (2014) kernels.

### Kernel

If compiling a customized kernel, the following options are required:

When the module is loaded, there is a check. If the user's laptop is supported, ok. If not, the module will fail to load.

If the user want to load the module anyway, put the option `force=1` right after the command as shown below:

`root #``modprobe i8k force=1`
### Software

- [app-laptop/i8kutils](https://packages.gentoo.org/packages/app-laptop/i8kutils) - Fan control for Dell laptops

i8kutils contains the user-space programs needed to handle the data provided by the i8k kernel module.

#### Add-ons

- [x11-plugins/i8krellm](https://packages.gentoo.org/packages/x11-plugins/i8krellm) - [GKrellM](https://wiki.gentoo.org/wiki/GKrellM)2 plugin
