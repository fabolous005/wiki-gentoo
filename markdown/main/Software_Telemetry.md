<!-- source: https://wiki.gentoo.org/wiki/Software_Telemetry | group: Gentoo Wiki (Main) | wiki-title: Software Telemetry -->
---
title: Software Telemetry
url: https://wiki.gentoo.org/wiki/Software_Telemetry
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2021-10-24"
fingerprint: "1e178e804aaba0a2"
license: CC BY-SA 4.0
---

# Software Telemetry

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

Some programs collect data or usage statistics and send these upstream ([https://en.wikipedia.org/wiki/Phoning\_home](https://en.wikipedia.org/wiki/Phoning_home) [https://en.wikipedia.org/wiki/Telemetry#Software](https://en.wikipedia.org/wiki/Telemetry#Software)). Even an automatic check for updates on the upstream website enables upstream to collect more data than some users want to spread.

Ideally the user is informed about the connection and the transferred data. 
A Gentoo package can allow the user to enable or disable this feature with the [useflag](https://wiki.gentoo.org/wiki/USE_flag) telemetry (and phonehome).

Tickets about adding the use flag to a package are tracked in [bug #820062](https://bugs.gentoo.org/show_bug.cgi?id=820062)

### Solutions

#### Patch

A patch could disable the automatic connection.

#### Sandbox

Running a program in a [Simple\_sandbox](https://wiki.gentoo.org/wiki/Simple_sandbox) which does not allow any connection.
