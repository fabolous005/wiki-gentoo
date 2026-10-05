<!-- source: https://wiki.gentoo.org/wiki/Fail2ban | group: Gentoo Wiki (Main) | wiki-title: Fail2ban -->
---
title: Fail2ban
url: https://wiki.gentoo.org/wiki/Fail2ban
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-09-26"
fingerprint: f30af3f9fd8795c0
license: CC BY-SA 4.0
---

# Fail2ban

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**Fail2ban** is a system denying hosts causing multiple authentication errors access to a service.

The service scans log files for patterns of specific repeated attempts (for instance, unsuccessful [SSH](https://wiki.gentoo.org/wiki/SSH) authentication attempts or high volume GET/POST requests on a [web server](https://wiki.gentoo.org/wiki/Category:Web_servers)) and, when detected, automatically creates a firewall or [TCP wrappers](https://wiki.gentoo.org/wiki/Security_Handbook/Securing_services#TCP_wrappers) drop or deny rule to ensure the service availability is not jeopardized.

Although the service supports many services out-of-the-box, it is very versatile in its configuration and can easily be enhanced.

## Installation

### USE flags


| [selinux](https://packages.gentoo.org/useflags/selinux) | !!internal use only!! Security Enhanced Linux support, this must be set by the selinux profile or breakage will occur | 
| [systemd](https://packages.gentoo.org/useflags/systemd) | Enable use of systemd-specific libraries and features like socket activation or session tracking | 
| [test](https://packages.gentoo.org/useflags/test) | Enable dependencies and/or preparations necessary to run tests (usually controlled by FEATURES=test but can be toggled independently) | 

### Emerge

`root #``emerge --ask net-analyzer/fail2ban`
## Features

### Jailing

The primary purpose of **fail2ban** is to *jail* services. When a service, such as SSHd, is jailed, then fail2ban will continuously look in the log(s) of that service for possible repeated attempts. The moment that a given number (`maxretry`) of attempts is detected within a particular time window (`findtime`) then a blocking rule (such as through [nftables](https://wiki.gentoo.org/wiki/Nftables)) is automatically set for a given time period (`bantime`).

The settings of these jails is done through /etc/fail2ban/jail.conf. By default, fail2ban already provides a nice jail.conf file, but all jails are by default disabled so that the service, when started by the administrator, wouldn't accidentally filter out valid requests.

**`/etc/ssh/sshd_config.d/99-logging.conf`**

**Example code to enable verbose logging in sshd**

```
LogLevel VERBOSE
```
**`/etc/fail2ban/jail.conf`**

**Example code for SSH jail**

```
[DEFAULT]
ignoreip = 127.0.0.1
ignoreip = 192.168.100.24 ; Management network
bantime = 86400 ; 1 day (in seconds)
findtime = 300 ; 5 minutes (in seconds)
maxretry = 3 ; default repeat count
# Jail entry for SSH, using nftables for firewall
[ssh-nftables]
enabled = true  ; Note that it is by default disabled
filter = sshd
action = nftables[name=SSH, port=ssh, protocol=tcp]
logpath = /var/log/auth.log
maxretry = 5 ; Override the default of 3
```
#### jail.d

Jails can and should be broken into individual jail files. Individual jails are easier to sort through, and disable or enable. Fail2ban uses jail.d/\*.conf syntax, so moving sshd.conf to sshd.conf.backup will disable the jail.

**`/etc/fail2ban/jail.d/sshd.conf`**

**syslog-ng & ufw example**

```
[ssh-nftables]
enabled  = true
filter = sshd
action = ufw[name=SSH, port=ssh, protocol=tcp]
logpath = /var/log/messages
maxretry = 5 ; Override the default of 3
```
### Filter expressions

Inside /etc/fail2ban/filter.d various filtering definitions can be created. Generally, these files contain regular expressions that match attempts. When a regular expression is matched on a file, then the counter for that jail and the offending host is increased.

### Actions

Inside /etc/fail2ban/action.d various action definitions can be created. These files contain commands to execute to ban and unban a given host. By default, rules exist for iptables, nftables, tcpwrappers, shorewall and more.

### Log scanning

The fail2ban service supports both file polling or more efficient file modification notifications; when [dev-python/pyinotify](https://packages.gentoo.org/packages/dev-python/pyinotify) or [app-admin/gamin](https://packages.gentoo.org/packages/app-admin/gamin) is installed and the user did not change the `backend` directive, then pyinotify or gamin will be used, otherwise polling is done. This can of course be configured in /etc/fail2ban/jail.conf.

## Using fail2ban

### Configuration

To configure fail2ban, go to /etc/fail2ban.

Start with jail.conf as that contains which rules to use (and which services to control) and only override the appropriate settings and enable the rules in jail.d/\*.conf. If necessary, create new filters or actions if the included configuration does not satisfy requirements.

For example to enable the default SSH filters for rsyslog users:

**`/etc/fail2ban/jail.d/sshd.conf`**

**rsyslog**

```
[sshd]
enabled  = true
```
Or for syslog-ng users:

**`/etc/fail2ban/jail.d/sshd.conf`**

**syslog-ng**

```
[sshd]
enabled  = true
logpath = /var/log/messages
```
When finished, start the fail2ban service and to add it to the default runlevel.

`root #````
rc-service fail2ban start
```
`root #``rc-update add fail2ban default`
#### systemd

On systemd profiles, fail2ban needs to be configured to read logs from the systemd journal:

**`/etc/fail2ban/jail.d/sshd.conf`**

**sshd jail config for systemd**

```
[sshd]
enabled = true
backend = systemd
```
### Interacting

As part of the fail2ban service, there is also a **fail2ban-client** available that can query the fail2ban service.

For instance, to see the running jails:

`root #``fail2ban-client status`
Status
|- Number of jail:    1
\`- Jail list:         sshd

To obtain specific information about each jail, such as the list of currently banned addresses, executed filters, etc:

`root #``fail2ban-client status sshd`
Status for the jail: sshd
|- filter
|  |- File list:	/var/log/auth.log 
|  |- Currently failed:	1
|  \`- Total failed:	12
\`- action
   |- Currently banned:	1
   |  \`- IP list:	192.168.100.50 
   \`- Total banned:	2

To get a compact version for all jails, including banned IPs:

`root #``fail2ban-client banned``[{'sshd': ['192.168.100.50']}, {'apache-auth': []}]`
### Troubleshooting

When the filters are not working properly, use **fail2ban-regex** to try them out. Pass it the log file to check and the filter to run, and it will give back what it found.

`root #``fail2ban-regex /var/log/auth.log /etc/fail2ban/filter.d/sshd.conf````
Running tests
=============
Use regex file : /etc/fail2ban/filter.d/sshd.conf
Use log file   : /var/log/auth.log
Results
=======
Failregex
|- Regular expressions:
|  [1] ^\s*(?:\S+ )?(?:@vserver_\S+ )?(?:(?:\[\d+\])?:\s+[\[\(]?sshd(?:\(\S+\))?[\]\)]?:?|[\[\(]?sshd(?:\(\S+\))?[\]\)]?:?(?:\[\d+\])?:)?\s*(?:error: PAM: )?Authentication failure for .* from <HOST>\s*$
|  [2] ^\s*(?:\S+ )?(?:@vserver_\S+ )?(?:(?:\[\d+\])?:\s+[\[\(]?sshd(?:\(\S+\))?[\]\)]?:?|[\[\(]?sshd(?:\(\S+\))?[\]\)]?:?(?:\[\d+\])?:)?\s*(?:error: PAM: )?User not known to the underlying authentication module for .* from <HOST>\s*$
|  [3] ^\s*(?:\S+ )?(?:@vserver_\S+ )?(?:(?:\[\d+\])?:\s+[\[\(]?sshd(?:\(\S+\))?[\]\)]?:?|[\[\(]?sshd(?:\(\S+\))?[\]\)]?:?(?:\[\d+\])?:)?\s*Failed (?:password|publickey) for .* from <HOST>(?: port \d*)?(?: ssh\d*)?$
|  [4] ^\s*(?:\S+ )?(?:@vserver_\S+ )?(?:(?:\[\d+\])?:\s+[\[\(]?sshd(?:\(\S+\))?[\]\)]?:?|[\[\(]?sshd(?:\(\S+\))?[\]\)]?:?(?:\[\d+\])?:)?\s*ROOT LOGIN REFUSED.* FROM <HOST>\s*$
|  [5] ^\s*(?:\S+ )?(?:@vserver_\S+ )?(?:(?:\[\d+\])?:\s+[\[\(]?sshd(?:\(\S+\))?[\]\)]?:?|[\[\(]?sshd(?:\(\S+\))?[\]\)]?:?(?:\[\d+\])?:)?\s*[iI](?:llegal|nvalid) user .* from <HOST>\s*$
|  [6] ^\s*(?:\S+ )?(?:@vserver_\S+ )?(?:(?:\[\d+\])?:\s+[\[\(]?sshd(?:\(\S+\))?[\]\)]?:?|[\[\(]?sshd(?:\(\S+\))?[\]\)]?:?(?:\[\d+\])?:)?\s*User \S+ from <HOST> not allowed because not listed in AllowUsers$
|  [7] ^\s*(?:\S+ )?(?:@vserver_\S+ )?(?:(?:\[\d+\])?:\s+[\[\(]?sshd(?:\(\S+\))?[\]\)]?:?|[\[\(]?sshd(?:\(\S+\))?[\]\)]?:?(?:\[\d+\])?:)?\s*authentication failure; logname=\S* uid=\S* euid=\S* tty=\S* ruser=\S* rhost=<HOST>(?:\s+user=.*)?\s*$
|  [8] ^\s*(?:\S+ )?(?:@vserver_\S+ )?(?:(?:\[\d+\])?:\s+[\[\(]?sshd(?:\(\S+\))?[\]\)]?:?|[\[\(]?sshd(?:\(\S+\))?[\]\)]?:?(?:\[\d+\])?:)?\s*refused connect from \S+ \(<HOST>\)\s*$
|  [9] ^\s*(?:\S+ )?(?:@vserver_\S+ )?(?:(?:\[\d+\])?:\s+[\[\(]?sshd(?:\(\S+\))?[\]\)]?:?|[\[\(]?sshd(?:\(\S+\))?[\]\)]?:?(?:\[\d+\])?:)?\s*reverse mapping checking getaddrinfo for .* \[<HOST>\] .* POSSIBLE BREAK-IN ATTEMPT\!\s*
|  [10] ^\s*(?:\S+ )?(?:@vserver_\S+ )?(?:(?:\[\d+\])?:\s+[\[\(]?sshd(?:\(\S+\))?[\]\)]?:?|[\[\(]?sshd(?:\(\S+\))?[\]\)]?:?(?:\[\d+\])?:)?\s*User \S+ from <HOST> not allowed because none of user's groups are listed in AllowGroups$
|
`- Number of matches:
   [1] 30 match(es)
   [2] 0 match(es)
   [3] 0 match(es)
   [4] 0 match(es)
   [5] 0 match(es)
   [6] 0 match(es)
   [7] 0 match(es)
   [8] 0 match(es)
   [9] 0 match(es)
   [10] 0 match(es)
Ignoreregex
|- Regular expressions:
|
`- Number of matches:
Summary
=======
Addresses found:
[1]
    192.168.100.50 (Wed Dec 28 12:46:56 2011)
    192.168.100.50 (Wed Dec 28 12:47:00 2011)
    192.168.100.50 (Wed Dec 28 12:47:03 2011)
    192.168.100.50 (Wed Dec 28 12:47:15 2011)
    192.168.100.50 (Wed Dec 28 12:47:18 2011)
    192.168.100.50 (Wed Dec 28 12:47:21 2011)
    192.168.100.50 (Wed Dec 28 14:23:08 2011)
    192.168.100.50 (Wed Dec 28 14:23:12 2011)
    192.168.100.50 (Wed Dec 28 14:23:23 2011)
    192.168.100.50 (Wed Dec 28 14:23:28 2011)
    192.168.100.50 (Wed Dec 28 14:23:31 2011)
    192.168.100.50 (Wed Dec 28 14:23:35 2011)
    192.168.100.50 (Wed Dec 28 15:15:09 2011)
    192.168.100.50 (Wed Dec 28 15:15:12 2011)
    192.168.100.50 (Wed Dec 28 15:15:14 2011)
    192.168.100.50 (Wed Dec 28 15:15:17 2011)
    192.168.100.50 (Wed Dec 28 15:15:20 2011)
    192.168.100.50 (Wed Dec 28 15:15:23 2011)
    192.168.100.50 (Wed Dec 28 15:21:29 2011)
    192.168.100.50 (Wed Dec 28 15:21:32 2011)
    192.168.100.50 (Wed Dec 28 15:21:34 2011)
    192.168.100.50 (Wed Dec 28 15:21:38 2011)
    192.168.100.50 (Wed Dec 28 15:21:41 2011)
    192.168.100.50 (Wed Dec 28 15:21:43 2011)
    192.168.100.50 (Wed Dec 28 17:36:00 2011)
    192.168.100.50 (Wed Dec 28 17:36:03 2011)
    192.168.100.50 (Wed Dec 28 17:36:05 2011)
    192.168.100.50 (Wed Dec 28 17:36:10 2011)
    192.168.100.50 (Wed Dec 28 17:36:13 2011)
    192.168.100.50 (Wed Dec 28 17:36:16 2011)
[2]
[3]
[4]
[5]
[6]
[7]
[8]
[9]
[10]
Date template hits:
2120 hit(s): MONTH Day Hour:Minute:Second
0 hit(s): WEEKDAY MONTH Day Hour:Minute:Second Year
0 hit(s): WEEKDAY MONTH Day Hour:Minute:Second
0 hit(s): Year/Month/Day Hour:Minute:Second
0 hit(s): Day/Month/Year Hour:Minute:Second
0 hit(s): Day/MONTH/Year:Hour:Minute:Second
0 hit(s): Month/Day/Year:Hour:Minute:Second
0 hit(s): Year-Month-Day Hour:Minute:Second
0 hit(s): Day-MONTH-Year Hour:Minute:Second[.Millisecond]
0 hit(s): Day-Month-Year Hour:Minute:Second
0 hit(s): TAI64N
0 hit(s): Epoch
0 hit(s): ISO 8601
0 hit(s): Hour:Minute:Second
0 hit(s): <Month/Day/Year@Hour:Minute:Second>
Success, the total number of match is 30
However, look at the above section 'Running tests' which could contain important
information.
```
## See also

- [Sshguard](https://wiki.gentoo.org/wiki/Sshguard) — an intrusion prevention system that parses server logs, determines malicious activity, and uses the system firewall to block the IP addresses of malicious connections.
- [Iptables](https://wiki.gentoo.org/wiki/Iptables) — a program used to configure and manage the kernel's netfilter modules.
- [Nftables](https://wiki.gentoo.org/wiki/Nftables) — a Linux packet filtering framework for the Netfilter subsystem, providing a unified interface for configuring packet filtering, connection tracking, NAT, and related network functionality.
