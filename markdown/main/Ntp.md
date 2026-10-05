<!-- source: https://wiki.gentoo.org/wiki/Ntp | group: Gentoo Wiki (Main) | wiki-title: Ntp -->
---
title: ntp
url: https://wiki.gentoo.org/wiki/Ntp
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-02-20"
fingerprint: d7289559700428e3
license: CC BY-SA 4.0
---

# ntp

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)


[net-misc/ntp](https://packages.gentoo.org/packages/net-misc/ntp) is a suite of tools utilizing [Network Time Protocol](https://wiki.gentoo.org/wiki/Network_Time_Protocol). Their purpose is to keep the system clock in time.

## Installation

### USE flags


| [+threads](https://packages.gentoo.org/useflags/+threads) | Add threads support for various packages. Usually pthreads | 
| [caps](https://packages.gentoo.org/useflags/caps) | Use Linux capabilities library to control privilege | 
| [debug](https://packages.gentoo.org/useflags/debug) | Enable extra debug codepaths, like asserts and extra output. If you want to get meaningful backtraces see https://wiki.gentoo.org/wiki/Project:Quality\_Assurance/Backtraces | 
| [openntpd](https://packages.gentoo.org/useflags/openntpd) | Allow ntp to be installed alongside openntpd | 
| [parse-clocks](https://packages.gentoo.org/useflags/parse-clocks) | Add support for PARSE clocks | 
| [readline](https://packages.gentoo.org/useflags/readline) | Enable support for libreadline, a GNU line-editing library that almost everyone wants | 
| [samba](https://packages.gentoo.org/useflags/samba) | Provide support for Samba's signing daemon (needed for Active Directory domain controllers) | 
| [selinux](https://packages.gentoo.org/useflags/selinux) | !!internal use only!! Security Enhanced Linux support, this must be set by the selinux profile or breakage will occur | 
| [snmp](https://packages.gentoo.org/useflags/snmp) | Add support for the Simple Network Management Protocol if available | 
| [ssl](https://packages.gentoo.org/useflags/ssl) | Add support for SSL/TLS connections (Secure Socket Layer / Transport Layer Security) | 
| [vim-syntax](https://packages.gentoo.org/useflags/vim-syntax) | Pulls in related vim syntax scripts | 
| [zeroconf](https://packages.gentoo.org/useflags/zeroconf) | Support for DNS Service Discovery (DNS-SD) | 

### Emerge

Install the suite of NTP programs:

`root #``emerge --ask net-misc/ntp`
## Configuration

### Ntp-client

The service ntp-client is used for one-time sync, usually during bootup. Its purpose is to help with the ntpd startup, as ntpd initially waits before attempting to correct the clock skew, and may even refuse to do so if the skew is too large.

Starting the ntp-client service will run the sync only once. It is then expected that ntpd is used for maintaining the time until the next reboot.

#### Upstream servers

To adjust ntp-client's command and upstream servers, edit the ntp-client configuration file. The default configuration is populated with:

**`/etc/conf.d/ntp-client`**

```
NTPCLIENT_CMD="ntpdate"
NTPCLIENT_OPTS="-s -b -u \
	0.gentoo.pool.ntp.org 1.gentoo.pool.ntp.org \
	2.gentoo.pool.ntp.org 3.gentoo.pool.ntp.org"
```
#### Usage

To run the ntpdate sync command manually:

`root #``ntpdate -b -u 0.gentoo.pool.ntp.org`
##### OpenRC

To run the ntp-client:

`root #``rc-service ntp-client start`
To have the ntp-client run at boot:

`root #``rc-update add ntp-client default`
##### systemd

To run the client service:

`root #``systemctl start ntpdate.service`
To have the client service run at boot:

`root #``systemctl enable ntpdate.service`
### Ntpd

#### Upstream servers

In /etc/ntp.conf the servers that will be used to synchronize the local time for ntpd can be specified. The default configuration is populated with:

**`/etc/ntp.conf`**

By default the configuration uses the Gentoo NTP servers. A list of available servers can be found on [ntp.org](http://www.pool.ntp.org/en/#top). A private server can also be used.

#### Network not always available?

On systems where a network connection is not always available at boot (laptops, portable computers, etc.), it is useful to add the following lines to server configuration:

**`/etc/ntp.conf`**

This sets localhost as a server with low priority, so that the ntp daemon will start properly even without a network connection and will switch to using network servers when a network connection has been (re)established.

#### Solving "Exiting, name server cannot be used: Temporary failure in name resolution (-3) \* Failed to set clock"

**`/etc/local.d/ntp.start`**

Make this file executable by **chmod +x ntp.start**

Explanation:

**sleep 15** seconds because we waiting for the network up.

**> /dev/null** to suppress ntpdate output.

**&** at the end - to not block the boot process.

See more at [https://www.reddit.com/r/Gentoo/comments/107pplr/netmiscntp\_on\_boot\_exiting\_name\_server\_cannot\_be](https://www.reddit.com/r/Gentoo/comments/107pplr/netmiscntp_on_boot_exiting_name_server_cannot_be/?utm_source=share&utm_medium=web2x&context=3)

#### Permissions

Permission are used to control who is allowed to synchronize or change permissions.

To enable time syncing, the ntpd server must be reachable by the other ntp services. `noserve` can be used for blocking.

To prevent other machines from reconfiguring the server, use `nomodify`.

To prevent the  server from being used in [Denial of Service attacks](https://www.bbc.co.uk/news/technology-26136774), use `noquery`.

**`/etc/ntp.conf`**

Access to NTP service allowed only from the 192.0.2.0/24 network:

**`/etc/ntp.conf`**

To deny access to monlist functionality, used for querying traffic stats, but which may also be exploited in a [Denial of Service attack](https://en.wikipedia.org/wiki/Denial-of-service_attack):

**`/etc/ntp.conf`**

#### Usage

##### OpenRC

To start the ntpd service:

`root #``rc-service ntpd start`
To have the ntpd service start at boot:

`root #``rc-update add ntpd default`
To monitor status of the ntpd service:

`root #``rc-service ntpd status`
##### systemd

To start the ntpd service:

`root #``systemctl start ntpd.service`
To have the ntpd service start at boot:

`root #``systemctl enable ntpd.service`
To monitor status of the ntpd service:

`root #``systemctl status ntpd.service`
#### Troubleshooting

##### ntpd command not running

If ntpd is already running in daemon mode, it will automatically exit when attempting to be manually run from the commandline.

For example:

`root #``ntpd -q -g`
ntpd -q -g
22 Jan 02:13:08 ntpd\[5413\]: ntpd 4.2.8p15@1.3728-o Fri Jul  2 16:04:29 UTC 2021 (1): Starting
22 Jan 02:13:08 ntpd\[5413\]: Command line: ntpd -q -g
22 Jan 02:13:08 ntpd\[5413\]: ----------------------------------------------------
22 Jan 02:13:08 ntpd\[5413\]: ntp-4 is maintained by Network Time Foundation,
22 Jan 02:13:08 ntpd\[5413\]: Inc. (NTF), a non-profit 501(c)(3) public-benefit
22 Jan 02:13:08 ntpd\[5413\]: corporation.  Support and training for ntp-4 are
22 Jan 02:13:08 ntpd\[5413\]: available at https://www.nwtime.org/support
22 Jan 02:13:08 ntpd\[5413\]: ----------------------------------------------------
22 Jan 02:13:08 ntpd\[5413\]: proto: precision = 0.481 usec (-21)
22 Jan 02:13:08 ntpd\[5413\]: basedate set to 2021-06-20
22 Jan 02:13:08 ntpd\[5413\]: gps base set to 2021-06-20 (week 2163)
22 Jan 02:13:08 ntpd\[5413\]: unable to bind to wildcard address :: - another process may be running - EXITING

This can be corrected by stopping the daemon service, running ntpd, then (re)starting the service. On OpenRC systems, this is accomplished as follows:

`root #````
rc-service ntpd stop
```
`root #````
ntpd -q -g
```
`root #````
rc-service ntpd start
```
## See also

- [Chrony](https://wiki.gentoo.org/wiki/Chrony) — a versatile implementation of the [Network Time Protocol](https://wiki.gentoo.org/wiki/Network_Time_Protocol) (NTP).
- [OpenNTPD](https://wiki.gentoo.org/wiki/OpenNTPD) — a lightweight [NTP](https://wiki.gentoo.org/wiki/Network_Time_Protocol) server ported from OpenBSD.
- [Network Time Protocol](https://wiki.gentoo.org/wiki/Network_Time_Protocol) — used to synchronize the [system time](https://wiki.gentoo.org/wiki/System_time) with other devices over the network.
- [System time](https://wiki.gentoo.org/wiki/System_time) — is used in Unix systems to keep track of time.
- [Home router](https://wiki.gentoo.org/wiki/Home_router) — how to turn an old Gentoo machine into a router for connecting a home network to the Internet.
