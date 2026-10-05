<!-- source: https://wiki.gentoo.org/wiki/OpenRC/Prefix | group: Gentoo Wiki (Main) | wiki-title: OpenRC/Prefix -->
---
title: OpenRC/Prefix
url: https://wiki.gentoo.org/wiki/OpenRC/Prefix
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2024-03-31"
fingerprint: be823251128dabce
license: CC BY-SA 4.0
---

# OpenRC/Prefix

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

This work is a reponse to [bug #196294](https://bugs.gentoo.org/show_bug.cgi?id=196294).

The following guideline applies to a Gentoo [Prefix](https://wiki.gentoo.org/wiki/Prefix) on RHEL-5.6 amd64 and on Debian 6.0 amd64, for other setups it should be similar. Feel free to documents corner cases here.

## Getting openrc on Prefix

`root #``emerge --ask sys-apps/openrc`
Specify the system to be Prefix:

**`EPREFIX/etc/rc.conf`**

## Tips

### Start OpenRC Automatically

As a service manager, OpenRC is more useful when started automatically.

#### /etc/rc.local

Many distributions provide /etc/rc.local as a site specific place start services.

If you have root access to the host, start OpenRC from /etc/rc.local:

**`/etc/rc.local`**

#### crontab

If you do not have root access to the host, but the host has cron, you can hook OpenRC into crontab on every reboot:

**`crontab -e`**

#### .profile

If the host does not even have cron, OpenRC can be started from login shell. Taking bash for example:

**`${HOME}/.profile`**

OpenRC does nothing if a runlevel is entered again. Logging in (and entering default runlevel) many times does not have side-effects.

## Trouble Shooting

### XXX is already starting

`user $``rc-service ntpd start`
\* WARNING: ntpd is already starting

OpenRC needs to enter some runlevel to initiate the runtime status directores in EPREFIX/var/run/openrc to really start anything.

`user $``openrc`
## Example: tinc

[tinc](https://www.tinc-vpn.org) is a decentralized VPN. A tinc started from Gentoo Prefix can serve as a relay node in the VPN, if tun/tap is not available for normal user.

Emerge tinc, or re-emerge to get its {init,conf}.d files back with our new portage

`root #``emerge --ask tinc`
### Prefixify init script

`user $``EPREFIX=$(portageq envvar EPREFIX)``user $``sed -i -r -e "s,(\W)/(etc|bin|sbin|usr|var),\1${EPREFIX}/\2,g" ${EPREFIX}/etc/init.d/tincd`
### Fire up tincd from OpenRC

Make necessary changes in ${EPREFIX}/etc/conf.d/tinc.networks and ${EPREFIX}/etc/tinc according to your specific setup.

Add tincd into default runlevel:

`user $``rc-update add tincd default`
Finally we are done with:

`user $``openrc`
\* Starting tinc VPN networks ...
 \* Starting tinc network manifold ... \[ ok \]
 \* Starting local                     \[ ok \]

## Example: nginx

[nginx](https://wiki.gentoo.org/wiki/Nginx) is a small, robust and high-performance web server.

Emerge nginx, or re-emerge to get its initd and confd files back with our new portage:

`root #``emerge --ask nginx`
### Prefixify init script

`user $``EPREFIX=$(portageq envvar EPREFIX)``user $``sed -i -r -e "s,(\W)/(etc|bin|sbin|usr|var),\1${EPREFIX}/\2,g" ${EPREFIX}/etc/init.d/nginx`
### Prefixify nginx.conf

`user $``EPREFIX=$(portageq envvar EPREFIX)``user $``sed -i -r -e "s,(\W)/(etc|bin|sbin|usr|var),\1${EPREFIX}/\2,g" ${EPREFIX}/etc/nginx/nginx.conf`
### Normal privilege

Remove "user nginx nginx;" from the first line of ${EPREFIX}/etc/nginx/nginx.conf.

Listening on port 80 requires root privileges. Change it to 8008:

**`${EPREFIX}/etc/nginx/nginx.conf`**

**listen on 8008**

### Add nginx to default runlevel and fire up

`user $``rc-update add nginx default``user $``openrc`
\* Checking nginx' configuration ... \[ ok \]
 \* Starting nginx ...                \[ ok \]

`root #``ss -tlpn | grep  nginx````
LISTEN 0      0                  *:8008             *:*    users:(("nginx",pid=19509,fd=7),("nginx",pid=19508,fd=7))
```
