<!-- source: https://wiki.gentoo.org/wiki/NTPsec | group: Gentoo Wiki (Main) | wiki-title: NTPsec -->
---
title: NTPsec
url: https://wiki.gentoo.org/wiki/NTPsec
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-10-26"
fingerprint: "96c9f15bb9e698e6"
license: CC BY-SA 4.0
---

# NTPsec

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**NTPsec** is a secure, hardened, and improved implementation of the [Network Time Protocol](https://wiki.gentoo.org/wiki/Network_Time_Protocol) derived from NTP Classic, Dave Mills’s original.

## Use cases

There are several use cases to consider:

- Installing [net-misc/ntpsec](https://packages.gentoo.org/packages/net-misc/ntpsec) on a system where no other NTP package is installed.
- Migrating to NTPsec from [net-misc/ntp](https://packages.gentoo.org/packages/net-misc/ntp).
- Using [net-misc/ntpsec](https://packages.gentoo.org/packages/net-misc/ntpsec) as an NTP server, with something like a [GPS](https://en.wikipedia.org/wiki/GPS) clock as a time reference.
- Using [net-misc/ntpsec](https://packages.gentoo.org/packages/net-misc/ntpsec) as an NTP client for one-shot synchronization.

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

##### Server

Add the `ntp` service to the `default` runlevel:

`root #``rc-update add ntp default`
Start the service:

`root #``rc-service ntp start`
##### Client

**Todo:**

- The "Client" subsection of the "Migrating from net-misc/ntp" section currently describes an `ntp-oneshot` service, but as noted there, such a service should be provided by the [net-misc/ntpsec](https://packages.gentoo.org/packages/net-misc/ntpsec) package. Once such a service is available, information about enabling and starting it should be added to this section.

## Migrating from net-misc/ntp

This section describes how to migrate from [net-misc/ntp](https://packages.gentoo.org/packages/net-misc/ntp) to NTPsec.

### Installation

Firstly, remove [net-misc/ntp](https://packages.gentoo.org/packages/net-misc/ntp) from the **@world** set:

`root #``emerge --deselect net-misc/ntp`
This allows Portage to uninstall it when [net-misc/ntpsec](https://packages.gentoo.org/packages/net-misc/ntpsec) is installed; otherwise, the packages block one another.

After configuring [#USE flags] appropriately, install NTPsec:

`root #``emerge --ask net-misc/ntpsec`
### Services

#### OpenRC

##### Server

The NTP server service provided by [net-misc/ntp](https://packages.gentoo.org/packages/net-misc/ntp) is called `ntpd`, rather than `ntp`.

Firstly, stop that service:

`root #``rc-service ntpd stop`
Remove the `ntpd` service from the `default` runlevel:

`root #``rc-update delete ntpd default`
Add the NTPsec server service, `ntp`, to the `default` runlevel:

`root #``rc-update add ntpd default`
Start the service:

`root #``rc-service ntp start`
##### Client

**Todo:**

- If a one-shot synchronization service is required, such a service should be provided by the [net-misc/ntpsec](https://packages.gentoo.org/packages/net-misc/ntpsec) package, not via the wiki; an issue should be opened on bugs.gentoo.org for this.

The [net-misc/ntp](https://packages.gentoo.org/packages/net-misc/ntp) package provides an `ntp-client` service, intended to be run before the `ntpd` service is started, which calls ntpdate to perform one-shot synchronization of the time.

The analogous command in NTPsec is [ntpdig(1)](https://manpages.debian.org/trixie/ntpsec-ntpdig/ntpdig.1.en.html)[, which can be used as an](https://wiki.gentoo.org/wiki/Special:MyLanguage/man_page) [SNTP](https://en.wikipedia.org/wiki/SNTP) client for one-shot synchronization.

ntpdig doesn't use the /etc/ntp.conf configuration file or any files in the /etc/ntp.d/ directory. To use these configuration files, an `ntp-oneshot` service can be created, with its own configuration file in the /etc/conf.d/ directory.

First create the service file:

`root #``touch /etc/init.d/ntp-oneshot`
Make it executable:

`root #``chmod 0755 /etc/init.d/ntp-oneshot`
Edit that file to contain the following content:

**`/etc/init.d/ntp-oneshot`**

```
#!/sbin/openrc-run
# Copyright 1999-2026 Gentoo Authors
# Distributed under the terms of the GNU General Public License v2
# one-shot script, which depends on "/etc/init.d/ntp" and inherits its variables and functions
ntp_init_file="/etc/init.d/ntp"
if [ -f "${ntp_init_file}" ]; then
    # inherit variables and functions from the init script "/etc/init.d/ntp"
    . "${ntp_init_file}"
else
    eerror "Could not find init file: '${ntp_init_file}'"
    return 1
fi
# overwrite variables
description="ntpd - the network time protocol daemon (one-shot)"
unset command_args_background
# always fork to background, otherwise, there will be an uninterruptible infinite foreground loop, if there is no internet connection
command_background="yes"
start_pre() {
    if [ -f "${pidfile}" ]; then
        # prevent multiple executions
        if start-stop-daemon --quiet --quiet --test --stop --pidfile="${pidfile}"; then
            eerror "${RC_SVCNAME} is still running."
            return 1
        else
            # remove stale pid file before starting
            ewarn "Removing stale PID file: '${pidfile}'"
            if ! /bin/rm --force -- "${pidfile}"; then
                return "${?}"
            fi
        fi
    fi
}
# always mark the service as "stopped", otherwise it will be marked as "crashed"
mark_service_stopped "${RC_SVCNAME}"
```
By sourcing the /etc/init.d/ntp service, all variables and functions - especially the `depend` function - will be inherited. These variables and functions can be overwritten by unsetting or defining them with different values.

Now create a configuration file in the /etc/conf.d/ directory:

`root #``touch /etc/conf.d/ntp-oneshot`
Add the following configuration:

**`/etc/conf.d/ntp-oneshot`**

```
# /etc/conf.d/ntp-oneshot
# terminate "ntpd" ("--quit") after the time has been synchronized
NTPD_OPTS="--panicgate --user='ntp:ntp' --quit"
```
The `--quit` option causes ntpd to not daemonize and terminates it after the time has been synchronized.

Test the service:

`root #``rc-service --verbose ntp-oneshot start`
\* Executing: /usr/libexec/rc/sh/openrc-run.sh /etc/init.d/ntp-oneshot start
\* Starting ntp-oneshot ...
\* Detaching to start \`/usr/sbin/ntpd' ...
2026-10-24T23:34:46 ntpd\[1088978\]: INIT: ntpd ntpsec-1.2.5: Starting
2026-10-24T23:34:46 ntpd\[1088978\]: INIT: Command line: /usr/sbin/ntpd --panicgate --user=ntp:ntp --quit
2026-10-24T23:34:46 ntpd\[1088978\]: INIT: successfully locked into RAM
2026-10-24T23:34:46 ntpd\[1088978\]: INIT: sandbox: seccomp enabled.
2026-10-24T23:34:46 ntpd\[1088978\]: CONFIG: readconfig: parsing file: /etc/ntp.conf
2026-10-24T23:34:46 ntpd\[1088978\]: CONFIG: readconfig: automatically walking directory: /etc/ntp.d
2026-10-24T23:34:46 ntpd\[1088978\]: CONFIG: opening \</etc/ntp.d/default.conf> from dir \</etc/ntp.d>
\[...\]
2026-10-24T23:34:46 ntpd\[1088978\]: SYNC: Found 10 servers, suggest minsane at least 3
2026-10-24T23:34:46 ntpd\[1088978\]: INIT: MRU 10922 entries, 13 hash bits, 65536 bytes
2026-10-24T23:34:46 ntpd\[1088978\]: INIT: OpenSSL 3.5.8 25 Aug 2026, 30500080
2026-10-24T23:34:46 ntpd\[1088978\]: NTSc: Using system default root certificates.
2026-10-24T23:34:47 ntpd\[1088978\]: DNS: dns\_probe: 3.gentoo.pool.ntp.org, cast\_flags:8, flags:101
2026-10-24T23:34:47 ntpd\[1088978\]: DNS: dns\_check: processing 3.gentoo.pool.ntp.org, 8, 101
\[...\]
2026-10-24T23:34:47 ntpd\[1088978\]: DNS: dns\_take\_status: 3.gentoo.pool.ntp.org=>good, 8
\[...\]
2026-10-24T23:34:53 ntpd\[1088978\]: CLOCK: time slew +0.081539 s

Use the rc-status command to check if the service is stopped:

`root #``rc-status --unused | grep "ntp-oneshot"`
ntp-oneshot                                                       \[  stopped  \]

If the service was successfully stopped, add the `ntp-oneshot` service to the `default` runlevel:

`root #``rc-update add ntp-oneshot default`
Check if there is a new entry in the `default` runlevel:

`root #``rc-update show default | grep "ntp-oneshot"`
ntp-oneshot | default

## See also

- [Network Time Protocol](https://wiki.gentoo.org/wiki/Network_Time_Protocol) — used to synchronize the [system time](https://wiki.gentoo.org/wiki/System_time) with other devices over the network.
