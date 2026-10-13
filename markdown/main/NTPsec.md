<!-- source: https://wiki.gentoo.org/wiki/NTPsec | group: Gentoo Wiki (Main) | wiki-title: NTPsec -->
---
title: NTPsec
url: https://wiki.gentoo.org/wiki/NTPsec
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-10-13"
fingerprint: "870dca5ff9e6f8ec"
license: CC BY-SA 4.0
---

# NTPsec

From Gentoo Wiki

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**NTPsec** is a secure, hardened, and improved implementation of the [Network Time Protocol](https://wiki.gentoo.org/wiki/Network_Time_Protocol) derived from NTP Classic, Dave Mills’s original.

## Installation

### USE flags


### USE flags for
            [net-misc/ntpsec](https://packages.gentoo.org/packages/net-misc/ntpsec)
            
            The NTP reference implementation, refactored

| [arbiter](https://packages.gentoo.org/useflags/arbiter) | Arbiter 1088A/B GPS driver | 
| [debug](https://packages.gentoo.org/useflags/debug) | Enable extra debug codepaths, like asserts and extra output. If you want to get meaningful backtraces see https://wiki.gentoo.org/wiki/Project:Quality\_Assurance/Backtraces | 
| [doc](https://packages.gentoo.org/useflags/doc) | Add extra documentation (API, Javadoc, etc). It is recommended to enable per package instead of globally | 
| [early](https://packages.gentoo.org/useflags/early) | Drop root privileges early | 
| [generic](https://packages.gentoo.org/useflags/generic) | Enable support for generic time radios and GPSDOs | 
| [gpsd](https://packages.gentoo.org/useflags/gpsd) | Enable gpsd daemon support | 
| [heat](https://packages.gentoo.org/useflags/heat) | Install contrib heat generating scripts | 
| [hpgps](https://packages.gentoo.org/useflags/hpgps) | Hewlett Packard GPS driver | 
| [jjy](https://packages.gentoo.org/useflags/jjy) | JJY driver | 
| [libbsd](https://packages.gentoo.org/useflags/libbsd) | Use libbsd instead of internal funcs | 
| [local](https://packages.gentoo.org/useflags/local) | Support for undisciplined local clock (not recommended) | 
| [modem](https://packages.gentoo.org/useflags/modem) | NIST/USNO/PTB Modem Time Services | 
| [nist](https://packages.gentoo.org/useflags/nist) | Set local clock deps | 
| [nmea](https://packages.gentoo.org/useflags/nmea) | NMEA GPS driver | 
| [ntpviz](https://packages.gentoo.org/useflags/ntpviz) | Make visualizations of offsets, jiffies, etc. | 
| [oncore](https://packages.gentoo.org/useflags/oncore) | Oncore driver | 
| [pps](https://packages.gentoo.org/useflags/pps) | PPS cesium clock driver | 
| [samba](https://packages.gentoo.org/useflags/samba) | Add support for SAMBA (Windows File and Printer sharing) | 
| [seccomp](https://packages.gentoo.org/useflags/seccomp) | Enable seccomp (secure computing mode) to perform system call filtering at runtime to increase security of programs | 
| [selinux](https://packages.gentoo.org/useflags/selinux) | !!internal use only!! Security Enhanced Linux support, this must be set by the selinux profile or breakage will occur | 
| [shm](https://packages.gentoo.org/useflags/shm) | Obtain refclock info from shared memory-segment | 
| [smear](https://packages.gentoo.org/useflags/smear) | Specify the interval over which a leap second is applied (experimental) | 
| [spectracom](https://packages.gentoo.org/useflags/spectracom) | Spectracom driver | 
| [test](https://packages.gentoo.org/useflags/test) | Enable dependencies and/or preparations necessary to run tests (usually controlled by FEATURES=test but can be toggled independently) | 
| [trimble](https://packages.gentoo.org/useflags/trimble) | Trimble driver | 
| [truetime](https://packages.gentoo.org/useflags/truetime) | Trutime driver (deprecated) | 
| [verify-sig](https://packages.gentoo.org/useflags/verify-sig) | Verify upstream signatures on distfiles | 
| [zyfer](https://packages.gentoo.org/useflags/zyfer) | Zyfer driver | 

### Emerge

`root #``emerge --ask net-misc/ntpsec`
## Configuration

### Service

#### OpenRC

Add the `ntp` service to the `default` runlevel:

`root #``rc-update add ntp default`
Start the service:

`root #``rc-service ntp start`
## See also

- [Network Time Protocol](https://wiki.gentoo.org/wiki/Network_Time_Protocol) — used to synchronize the [system time](https://wiki.gentoo.org/wiki/System_time) with other devices over the network.
