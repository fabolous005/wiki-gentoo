<!-- source: https://wiki.gentoo.org/wiki/Sysctl | group: Gentoo Wiki (Main) | wiki-title: Sysctl -->
---
title: Sysctl
url: https://wiki.gentoo.org/wiki/Sysctl
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2024-11-01"
fingerprint: a010492da1072b03
license: CC BY-SA 4.0
---

# Sysctl

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

sysctl is provided by [sys-process/procps](https://packages.gentoo.org/packages/sys-process/procps), and can be used to configure kernel parameters at system runtime.

## Introduction

sysctl can be used to manage the system's kernel configuration, available through /proc/sys/.

## Installation

### Kernel

**Enable procfs support**

### Emerge

`root #``emerge --ask sys-process/procps`
## Configuration

Kernel parameters can be configured by editing *.conf* files under /etc/sysctl.d.

For example setting a custom value of `net.ipv4.tcp_retries2` can be achieved with file:

**`/etc/sysctl.d/99-tcp-retransmission.conf`**

```
net.ipv4.tcp_retries2=3
```
### Service

#### OpenRC

The OpenRC sysctl init script is enabled by default, it simply executes sysctl --system.

#### Systemd

The Systemd *systemd-sysctl* service is enabled by default.

## Usage

### Viewing current values

Current kernel configuration values can be printed with:

`root #``sysctl --all`
### Setting values

Values can be updated using:

`root #``sysctl --write {parameter}={value}` ### Reloading values

Configuration values can be reloaded as they would be read on boot with:

`root #``sysctl --system`
### Filtering values

**--pattern** can be used to filter to parameters which match the supplied extended regex. This can be used for both printing and setting values.

To reload all system net values:

`root #``sysctl --system --pattern 'net.*'`
