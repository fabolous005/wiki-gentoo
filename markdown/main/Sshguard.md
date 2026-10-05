<!-- source: https://wiki.gentoo.org/wiki/Sshguard | group: Gentoo Wiki (Main) | wiki-title: Sshguard -->
---
title: sshguard
url: https://wiki.gentoo.org/wiki/Sshguard
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-03-01"
fingerprint: eb9dc849cc234be0
license: CC BY-SA 4.0
---

# sshguard

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

sshguard is an intrusion prevention system that parses server logs, determines malicious activity, and uses the system firewall to block the IP addresses of malicious connections. sshguard is written in C so it does not tax an interpreter.

## How it works

sshguard is a simple daemon that continuously tracks one or more log files. It parses the log events that daemons send out in case of failed login attempts and then blocks any further attempts from those connections by updating the system's firewall.

Unlike what the name implies, sshguard does not only parse SSH logs. It also supports many mail systems as well as a few FTP ones. A full listing of supported services can be found on the [sshguard.net website](https://www.sshguard.net).

## Installation

### Emerge

Install [app-admin/sshguard](https://packages.gentoo.org/packages/app-admin/sshguard):

`root #``emerge --ask app-admin/sshguard`
### Additional software

#### Logging

Running a local [Logging](https://wiki.gentoo.org/wiki/Logging) daemon is mandatory to be able to setup and run sshguard.

#### OpenSSH

**sshguard** works only with [OpenSSH](https://wiki.gentoo.org/wiki/OpenSSH).

#### Firewall

Depending on the init system and the desired firewall backend to be used by sshguard, additional software is required to be emerged in order for sshguard to block malicious actors.

More information on various supported backends can be found by reading the [sshguard-setup(7)](https://man.archlinux.org/man/sshguard-setup.7.en) [manpage.](https://wiki.gentoo.org/wiki/Special:MyLanguage/man_page)

##### nftables

When nftables are being used as the system firewall:

`root #``emerge --ask net-firewall/nftables`
More information about configuring and using nftables can be found on the [nftables](https://wiki.gentoo.org/wiki/Nftables) article.

##### iptables

When iptables are being used as the system firewall:

`root #``emerge --ask net-firewall/iptables`
More information about configuring and using iptables can be found on the [iptables](https://wiki.gentoo.org/wiki/Iptables) article.

## Configuration

### nftables backend

Following settings are required before starting sshguard. Set the BACKEND path to use **nftables** and configure the FILES paths /etc/sshguard.conf.

**`/etc/sshguard.conf`**

**`BACKEND` and `FILES` mandatory settings**

```
#!/bin/sh
# sshguard.conf -- SSHGuard configuration
 
# Options that are uncommented in this example are set to their default
# values. Options without defaults are commented out.
 
#### REQUIRED CONFIGURATION ####
# Full path to backend executable (required, no default)
BACKEND="/usr/libexec/sshg-fw-nft-sets"
 
# Space-separated list of log files to monitor. (optional, no default)
FILES="/var/log/auth.log /var/log/messages"
# ...
```
Additional sshguard settings are optional, and can be left at the default value now, and be adjusted at a later time.

#### Verification

Once the **sshguard** daemon has been started, **nftables** will list the created sshguard ruleset. Use the **nft list ruleset** command to show the current status:

`root #``nft list ruleset````
table ip sshguard {
        set attackers {
                type ipv4_addr
                flags interval
        }
        chain blacklist {
                type filter hook input priority filter - 10; policy accept;
                ip saddr @attackers drop
        }
}
table ip6 sshguard {
        set attackers {
                type ipv6_addr
                flags interval
        }
        chain blacklist {
                type filter hook input priority filter - 10; policy accept;
                ip6 saddr @attackers drop
        }
}
```
### iptables backend

Verify that the appropriate path to the **iptables** backend library is set in /etc/sshguard.conf:

**`/etc/sshguard.conf`**

**`BACKEND` and `FILES` mandatory settings**

```
#!/bin/sh
# sshguard.conf -- SSHGuard configuration
 
# Options that are uncommented in this example are set to their default
# values. Options without defaults are commented out.
 
#### REQUIRED CONFIGURATION ####
# Full path to backend executable (required, no default)
BACKEND="/usr/libexec/sshg-fw-iptables"
 
# Space-separated list of log files to monitor. (optional, no default)
FILES="/var/log/auth.log /var/log/messages"
# ...
```
Additional settings are optional and can be left at the default value for now, or be adjusted at a later time.

#### Preparing the firewall

When sshguard blocks any malicious users (by blocking their IP addresses), it will use the sshguard chain.

Prepare the chain using **iptables** and make sure it is also triggered when new incoming connections are detected:

`root #````
iptables -N sshguard
```
`root #````
iptables -A INPUT -j sshguard
```
#### Verification

Once the **sshguard** daemon has been started **iptables** will list the Chain sshguard at the bottom, and a reference in INPUT chain to it. Use following iptables command to verify the status:

`root #``iptables -L`
Chain INPUT (policy ACCEPT)
target     prot opt source               destination
sshguard   all  --  anywhere             anywhere
 
Chain FORWARD (policy ACCEPT)
target     prot opt source               destination
 
Chain OUTPUT (policy ACCEPT)
target     prot opt source               destination
 
Chain sshguard (1 references)
target     prot opt source               destination

### Services

#### OpenRC

Have sshguard be started by default by adding it to the default runlevel, and then start it:

`root #````
rc-update add sshguard default
```
`root #````
rc-service sshguard start
```
#### systemd

Use systemd's conventional way to enable it, and then start it:

`root #````
systemctl enable sshguard
```
`root #````
systemctl restart sshguard
```
### Blacklisting hosts

With the blacklisting option after a number of abuses the IP address of the attacker or a IP subnet will be blocked permanently. The blacklist will be loaded at each startup and extended with new entries during operation. sshguard inserts a new address after it exceeded a threshold of abuses.

Blacklisted addresses are never scheduled to be released (allowed) again.

To enable blacklisting, create an appropriate directory and file:

`root #````
mkdir -p /var/lib/sshguard
```
`root #````
touch /var/lib/sshguard/blacklist.db
```
While defining a blacklist it is important to exclude trusted IP networks and hosts in a whitelist.

To enable whitelisting, create an appropriate directory and file:

`root #````
mkdir -p /etc/sshguard
```
`root #````
touch /etc/sshguard/whitelist
```
The whitelist has to include the loopback interface, and should have at least 1 IP trusted network f.e. 192.0.2.0/24.

**`/etc/sshguard/whitelist`**

**Whitelisting trusted networks**

```
127.0.0.0/8
::1/128
192.0.2.0/24
```
Add the `BLACKLIST_FILE` and `WHITELIST_FILE` file to the configuration. Example configuration listed blocks all hosts after the first login attempt. To setup a less agressive blocking policy, adjust the `THRESHOLD` and `BLACKLIST_FILE` integer, and set it to f.e. **10** instead of **2**:

**`/etc/sshguard.conf`**

**Configuring sshguard to blacklist abusers**

```
BACKEND="/usr/libexec/sshg-fw-nft-sets"
FILES="/var/log/auth.log"
#
THRESHOLD=2
BLOCK_TIME=120
DETECTION_TIME=1800
#
IPV6_SUBNET=128
IPV4_SUBNET=32
#
# Add following lines
BLACKLIST_FILE=2:/var/lib/sshguard/blacklist.db
WHITELIST_FILE=/etc/sshguard/whitelist
```
Restart the sshguard daemon to have the changes take effect. On OpenRC:

`root #````
rc-service sshguard restart
```
Or on systemd:

`root #````
systemctl restart sshguard
```
## Troubleshooting

### File '/var/log/auth.log' vanished while adding!

When starting up, sshguard reports the following error:

Such an error (the file path itself can be different) occurs when the target file is not available on the system. Make sure that it is created, or update the sshguard configuration to not add it for monitoring.

On a syslog-ng system with OpenRC, the following addition to syslog-ng.conf can suffice:

**`/etc/syslog-ng/syslog-ng.conf`**

**creating auth.log file**

```
 { source(src); destination(messages); };
log { source(src); destination(console_all); };
 
destination authlog {file("/var/log/auth.log"); };
filter f_auth { facility(auth); };
filter f_authpriv { facility(auth, authpriv); };
log { source(src);  filter(f_authpriv);  destination(authlog);  };
```
Reload the configuration for the changes to take effect:

`root #``rc-service syslog-ng reload`
## See also

- [Fail2ban](https://wiki.gentoo.org/wiki/Fail2ban) — a system denying hosts causing multiple authentication errors access to a service.
- [Iptables](https://wiki.gentoo.org/wiki/Iptables) — a program used to configure and manage the kernel's netfilter modules.
- [Nftables](https://wiki.gentoo.org/wiki/Nftables) — a Linux packet filtering framework for the Netfilter subsystem, providing a unified interface for configuring packet filtering, connection tracking, NAT, and related network functionality.
- [OpenSSH](https://wiki.gentoo.org/wiki/OpenSSH) — the ubiquitous tool for logging into and working on remote machines securely.

## External resources

The [sshguard documentation](http://www.sshguard.net/docs/) provides all the information needed to further tune the application.
