<!-- source: https://wiki.gentoo.org/wiki/TuneD | group: Gentoo Wiki (Main) | wiki-title: TuneD -->
---
title: TuneD
url: https://wiki.gentoo.org/wiki/TuneD
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2025-06-07"
fingerprint: "2402b04aa5b199a6"
license: CC BY-SA 4.0
---

# TuneD

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**TuneD** is a daemon for monitoring and adaptive tuning of system devices.

## Installation

### Emerge

`root #``emerge --ask sys-apps/tuned`
## Configuration

View the man page for all configuration options.

`user $``man 5 tuned-main.conf`
### Service

#### OpenRC

Start the daemon:

`root #``rc-service tuned start`
Optionally, add the service to the default runlevel to start it on boot:

`root #``rc-update add tuned default`
## Usage

After enabling the daemon, the current active profile can be viewed:

`root #``tuned-adm active`
The list of available profiles can be viewed:

`root #``tuned-adm list`
The `profile` subcommand can be used to switch to a different profile. For instance, one could use the following for aggressive power saving on laptops:

`root #``tuned-adm profile laptop-battery-powersave`
## See also

- [Power management](https://wiki.gentoo.org/wiki/Power_management) — describes methods to save energy for longer battery runtimes, a quieter computer, lower power bills, and an environmentally friendly impact.
- [PowerTOP](https://wiki.gentoo.org/wiki/PowerTOP) — a Linux utility that can monitor and display a system's electrical power usage.
