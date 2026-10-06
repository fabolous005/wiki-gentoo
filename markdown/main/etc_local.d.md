<!-- source: https://wiki.gentoo.org/wiki//etc/local.d | group: Gentoo Wiki (Main) | wiki-title: /etc/local.d -->
---
title: "/etc/local.d"
url: https://wiki.gentoo.org/wiki//etc/local.d
hostname: gentoo.org
sitename: "/etc/local.d"
date: "2024-09-07"
fingerprint: b20b80d3370c8bef
license: CC BY-SA 4.0
---

# /etc/local.d

[/etc](https://wiki.gentoo.org/wiki//etc)

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)


**Resources**

**/etc/local.d/** can contain small programs or light scripts to be run when the local service is started or stopped. The local service is part of [OpenRC](https://wiki.gentoo.org/wiki/OpenRC).

Scripts in /etc/local.d/ with the suffix .start will be executed at boot time, scripts with suffix .stop at shutdown time. Scripts should have the execute permission. Files are processed in lexical order.

This system is intended only for very light scripts that will terminate quickly, and are at no risk of hanging, e.g. to write a value to a file in /proc/.

The local service is not considered started or stopped until everything is processed, so if there is a .start or .stop process which takes some time to run, it can delay - possibly even freeze - boot or shutdown.

<u>*This infrastructure should not be used to start other scripts or programs in the background.*</u> If the /etc/init.d/local service script is restarted several times, then those scripts or programs will be started in the background several times, possibly resulting in race conditions. A .stop script for terminating background processes started this way would be needed, but may easily fail, e.g. if the .start script had been executed several times.

## Configuration

Here is a script simply as an example of what can be put in /etc/local.d/. This script will set parameters for the cpu governor, it is a good candidate to be used with the local service as it simply writes to some files in proc and terminates:

**`/etc/local.d/cpu_governor.start`**

```
#!/bin/bash
#set cpu governors to "performance"
for c in $(ls -d /sys/devices/system/cpu/cpu[0-9]*); do echo performance >$c/cpufreq/scaling_governor; done
```
## Usage

### Invocation

The local service will usually be started on boot by adding it to the default runlevel (see the next section), however it can be started explicitly:

`root #``rc-service local start`
If the local service is in the default runlevel, but stopped, it can be started by having OpenRC check for stopped services:

`root #``openrc`
### Add local service to default runlevel

To start the local.d scripts at boot time, add it to the default runlevel:

`root #``rc-update add local default`
## Troubleshooting

By default, the local service will silence all output. Adding -v option to rc-service will cause it to show which scripts were run and their output, if any:

`root #``rc-service local restart -v`
## See also

- [OpenRC](https://wiki.gentoo.org/wiki/Openrc) — a dependency-based [init system](https://wiki.gentoo.org/wiki/Init_system) for Unix-like systems that maintains compatibility with the system-provided init system
