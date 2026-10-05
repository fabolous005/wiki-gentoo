<!-- source: https://wiki.gentoo.org/wiki/Ufw | group: Gentoo Wiki (Main) | wiki-title: Ufw -->
---
title: Ufw
url: https://wiki.gentoo.org/wiki/Ufw
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-04-09"
fingerprint: ee384c5ab7331df2
license: CC BY-SA 4.0
---

# Ufw

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**Ufw** is the **u**ncomplicated **f**ire**w**all, and is designed to be very simple to implement. It uses logs such as those obtained by [syslog-ng](https://wiki.gentoo.org/wiki/Syslog-ng) for monitoring, and uses [iptables](https://wiki.gentoo.org/wiki/Iptables) as a back end.  Ufw supports both IPv4 and IPv6.

## Installation

### Kernel

The following kernel configuration must be made before ufw will work.

**IPv4 settings**

IP version 6 is not required, however it is highly recommended.

**IPv6 settings**

### USE flags


### Emerge

`root #``emerge --ask net-firewall/ufw`
## Service

To allow [ssh](https://wiki.gentoo.org/wiki/Ssh) by default:

`root #``ufw allow ssh`
If you get the warning "ERROR: problem running" then this can be solved by installing [dev-python/pip](https://packages.gentoo.org/packages/dev-python/pip)

`root #``emerge --ask dev-python/pip`
### OpenRC

To start ufw at boot:

`root #``rc-update add ufw default`
To start ufw immediately:

`root #``rc-service ufw start`
### systemd

To start ufw at boot:

`root #``systemctl enable ufw`
To start ufw immediately:

`root #``systemctl start ufw`
### Configuration

To create a simple configuration, run:

`root #``ufw default deny incoming``root #``ufw allow from 192.168.0.0/24``root #``ufw allow <application-name>`
To get a list of possible applications to add, run:

`root #``ufw app list`
Then replace \<application-name> with the name of the desired application. For example, to allow incoming Deluge traffic:

`root #``ufw allow Deluge`
Next run

`root #``ufw enable`
The last step is only required only the first time you install the package.

After changes to the rules, restart the firewall:

`root #``ufw reload`
Specific use-cases and applications follow:

#### KDE Connect

To allow KDE Connect to work on the local network (192.168.0.x), ports 1714 through 1764 have to be opened for both UDP and TCP.

`root #````
ufw allow proto udp from 192.168.0.0/24 to any port 1714:1764
```
`root #````
ufw allow proto tcp from 192.168.0.0/24 to any port 1714:1764
```
