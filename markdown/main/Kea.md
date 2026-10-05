<!-- source: https://wiki.gentoo.org/wiki/Kea | group: Gentoo Wiki (Main) | wiki-title: Kea -->
---
title: Kea
url: https://wiki.gentoo.org/wiki/Kea
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2025-09-08"
fingerprint: c701215e9185eec8
license: CC BY-SA 4.0
---

# Kea

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**Kea** is a DHCP server developed by the Internet Systems Consortium. It is a newer (next generation) DHCP server from the same authors of ISC DHCP. Kea has several features ISC lacks, including: Modular components, JSON configuration with a REST API, data isolation, and a web based GUI.

## Installation

### USE flags


| [+openssl](https://packages.gentoo.org/useflags/+openssl) | Use dev-libs/openssl instead of dev-libs/botan | 
| [debug](https://packages.gentoo.org/useflags/debug) | Enable extra debug codepaths, like asserts and extra output. If you want to get meaningful backtraces see https://wiki.gentoo.org/wiki/Project:Quality\_Assurance/Backtraces | 
| [doc](https://packages.gentoo.org/useflags/doc) | Add extra documentation (API, Javadoc, etc). It is recommended to enable per package instead of globally | 
| [kerberos](https://packages.gentoo.org/useflags/kerberos) | Add kerberos support | 
| [mysql](https://packages.gentoo.org/useflags/mysql) | Add interface to MySQL for lease, host reservations and/or server config | 
| [postgres](https://packages.gentoo.org/useflags/postgres) | Add interface to PostgreSQL for lease, host reservations and/or server config | 
| [selinux](https://packages.gentoo.org/useflags/selinux) | !!internal use only!! Security Enhanced Linux support, this must be set by the selinux profile or breakage will occur | 
| [shell](https://packages.gentoo.org/useflags/shell) | Install kea-shell text management client for Control Agent | 
| [test](https://packages.gentoo.org/useflags/test) | Enable dependencies and/or preparations necessary to run tests (usually controlled by FEATURES=test but can be toggled independently) | 
| [verify-sig](https://packages.gentoo.org/useflags/verify-sig) | Verify upstream signatures on distfiles | 

### Emerge

`root #``emerge --ask net-misc/kea`
## Configuration

### Files

Kea's configuration files are located in /etc/kea:

- /etc/kea/kea-dhcp4.conf - Configuration for the kea-dhcp4 daemon
- /etc/kea/kea-dhcp6.conf - Configuration for the kea-dhcp6 daemon
- /etc/kea/kea-ddns.conf - Configuration for the kea-ddns daemon
- /etc/kea/kea-ctrl-agent.conf - Configuration for the kea-ctrl-agent daemon

Many configuration directives are similar or shared between files.

### interface-config

The **interface-config** directive is used to define which interfaces and IPs Kea listens on:

**`/etc/kea/kea-dhcp4.conf`**

```
{
    "Dhcp4": {
        "interfaces-config": {
            "interfaces": [ "ethernet2/192.168.2.1", "fib.lan", "ax1800" ]
        }
    }
}
```
### subnet4

IPv4 subnet configuration in Kea is relatively straightforward. Each structure in this list must contain a **subnet**. In most cases,  **pools** and **option-data** are also defined. **reservations** allow for advanced configuration, basic usage is described below.

#### subnet

The subnet, in *{ip}/{cidr}* format is mandatory for each **subnet4** and defines which subnet is defined by the structure:

**`/etc/kea/kea-dhcp4.conf`**

**Define subnet 192.168.2.0/24**

```
{
    "Dhcp4": {
        "subnet4": [
            {
                "subnet": "192.168.2.0/24"
            }
        ]
    }
}
```
#### pools

Although not strictly required, **pools** are typically defined for each subnet, and define where hosts without reservations pull IPs:

**`/etc/kea/kea-dhcp4.conf`**

**Define the pool to range from 192.169.2.100 to 192.168.2.200**

```
{
    "Dhcp4": {
        "subnet4": [
            {
                "pools": [ { "pool": "192.168.2.100 - 192.168.2.200" } ]
            }
        ]
    }
}
```
#### option-data

In most cases, at least a *router* is provided by DHCP servers, but other DHCP options/codes can be defined here:

**`/etc/kea/kea-dhcp4.conf`**

**Define the router as 192.168.2.1**

```
{
    "Dhcp4": {
        "subnet4": [
            {
                "option-data": [
                    {
                        "name": "routers",
                        "data": "192.168.2.1"
                    }
                ]
            }
        ]
    }
}
```
#### Reservations

Kea offers several options for identifying and configuring hosts:

**`/etc/kea/kea-dhcp4.conf`**

**Reserve 192.168.2.10 for aa:bb:cc:11:22:33 with 1.1.1.1 as a DNS server.**

```
{
    "Dhcp4": {
        "subnet4": [
            {
                "reservations": [
                    {
                        "hw-address": "aa:bb:cc:11:22:33",
                        "ip-address": "192.168.2.10",
                        "hostname": "larry",
                        "option-data": [ {
                            "name": "domain-name-servers",
                            "data": "1.1.1.1"
                        } ]
                    }
                ]
            }
        ]
    }
}
```
### control-socket

Control sockets can be used to reload Kea's config at runtime, by default the following **control-socket** is defined:

**`/etc/kea/kea-dhcp4.conf`**

```
{
    "Dhcp4": {
        "control-socket": {
            "socket-type": "unix",
            "socket-name": "/run/kea/kea4-ctrl-socket"
        }
    }
}
```
### lease-database

By default, Kea uses a *memfile* **lease-database** backend. If compiled with the **mysql** *USE* flag, a SQL database can be used as the backend. The default **lease-database** config is as follows:

**`/etc/kea/kea-dhcp4.conf`**

```
{
    "Dhcp4": {
        "lease-database": {
            "type": "memfile",
            "lfc-interval": 3600
        }
    }
}
```
### expired-leases-processing

As described in the example config:

**`/etc/kea/kea-dhcp4.conf`**

**Annotated default config**

```
{
    "Dhcp4": {
        "expired-leases-processing": {
            "reclaim-timer-wait-time": 10,  // Expired leases will be reclaimed every 10 seconds
            "flush-reclaimed-timer-wait-time": 25,  // Every 25 seconds, reclaimed leases which have expired more than then hold-reclaimed-time will be removed
            "hold-reclaimed-time": 3600,  // The amount of time leases must be expire3d before they can be reclaimed
            "max-reclaim-leases": 100,  // The maximum number of leases which can be reclaimed during each cycled, defined by max-reclaim-time
            "max-reclaim-time": 250,  // The amount of time in ms between each reclaim cycle
            "unwarned-reclaim-cycles": 5  // The number of reclamation cycles allowed before an expired lease still present in the database will trigger an alert
        }
    }
}
```
### loggers

To make Kea log to syslog, the following adjustments can be made:

**`/etc/kea/kea-dhcp4.conf`**

**Configure Kea to use syslog**

```
{
    "Dhcp4": {
        "loggers": [
            {
                "name": "kea-dhcp4",
                "output_options": [
                    {
                        "output": "syslog"
                    }
                ],
    
                "severity": "INFO",  // One of FATAL, ERROR, WARN, INFO, DEBUG
                "debuglevel": 0  // 0 is least verbose, 99 is most verbose. Kea can generate LOTS of log information
            }
        ]
    }
}
```
### Service

#### OpenRC

After installing, the default configuration files are found in /etc/kea and are prefixed with *kea-*, e.g. kea-dhcp4.conf.

The way services are started in version 3.x (which is yet to be stabilized) has changed from version 2. Each service can be started and stopped individually, whereas with version 2.x there is only /etc/conf.d/kea that contains settings for dhcp4, dhcp6, ddns and ctrl-agent. In version 3 there are individual files per service. That is, /etc/conf.d/kea-dhcp4, /etc/conf.d/kea-dhcp6, etc.

For example, to start the dhcp4 service in version 3, do the following:

`root #````
rc-update add kea-dhcp4
```
`root #````
rc-service kea-dhcp4 start
```
And similarly for dhcp6, ddns, etc.

In version 2 there is only one service *kea* and which daemons are launched is dependent on which is enabled in /etc/conf.d/kea

`root #````
rc-update add kea
```
`root #````
rc-service kea start
```
##### High Availability

To use the high availability feature of Kea, the kea-ctrl-agent must be started to allow the primary & standby server(s) to communicate.

##### Starting after the logger

To ensure Kea starts after eth0 has started, and logging is available, the following can be added to Kea's service configuration:

**`/etc/conf.d/kea`**

```
# snip
rc_need="net.eth0 logger"
# snip
```
## Usage

### Check config

To check a Kea config file, kea-dhcp4 -t can be used:

`root #``kea-dhcp4 -t /etc/kea/kea-dhcp4.conf`
2023-08-08 10:41:23.712 INFO  \[kea-dhcp4.hosts/4527.140104273917056\] HOSTS\_BACKENDS\_REGISTERED the following host backend types are available: 
2023-08-08 10:41:23.713 INFO  \[kea-dhcp4.dhcpsrv/4527.140104273917056\] DHCPSRV\_CFGMGR\_USE\_ADDRESS listening on address 192.168.2.1, on interface ethernet2
2023-08-08 10:41:23.713 INFO  \[kea-dhcp4.dhcpsrv/4527.140104273917056\] DHCPSRV\_CFGMGR\_SOCKET\_TYPE\_DEFAULT "dhcp-socket-type" not specified , using default socket type raw
2023-08-08 10:41:23.714 INFO  \[kea-dhcp4.dhcpsrv/4527.140104273917056\] DHCPSRV\_CFGMGR\_NEW\_SUBNET4 a new subnet has been added to configuration: 192.168.2.0/24 with params: t1=900, t2=1800, valid-lifetime=3600

### Hook Libraries

#### Run Script Support

Kea has a useful interface to run scripts on certain events. For example this can be used to update DNS records when leases are allocated or revoked.
