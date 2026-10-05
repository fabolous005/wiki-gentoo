<!-- source: https://wiki.gentoo.org/wiki/OpenRC/Stacked_runlevel | group: Gentoo Wiki (Main) | wiki-title: OpenRC/Stacked runlevel -->
---
title: OpenRC/Stacked runlevel
url: https://wiki.gentoo.org/wiki/OpenRC/Stacked_runlevel
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2023-02-03"
fingerprint: b633ff5f922a4b83
license: CC BY-SA 4.0
---

# OpenRC/Stacked runlevel

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

This page documents a tutorial for setting up complicated networking with the help of stacked runlevel. Along the tutorial other tricked for [OpenRC](https://wiki.gentoo.org/wiki/OpenRC) are also expected.

The short version is that

**`rc-update manpage`**

**stack runlevel**

## Scenario

We have a university library providing wireless network, with the following steps to use:

1. associate with the AP with no authentication
2. setup a [PPTP](https://en.wikipedia.org/wiki/Point-to-Point_Tunneling_Protocol) VPN through NAT against an internal server
3. using an [HTTP proxy](https://en.wikipedia.org/wiki/Proxy_server) to access the websites

There are big disadvantages:

- [GRE](https://en.wikipedia.org/wiki/Generic_Routing_Encapsulation) protocol used by PPTP uses source IP to identify client, thus do not support tunneling through NAT. There is enhanced GRE for such purpose, but routers has different implementations and only considered PPTP clients for Microsoft Windows and Apple Mac OS X, not for Linux.
- The author, as a server administrator, need ssh at least. HTTP proxy, a limit gateway to the internet, sucks.

Besides complaining to library manager and network technician, then waiting forever, we can hack our own reliable way out.

## Solution

- For Linux box not being able to use PPTP VPN through NAT, the author selected his phone to do the job. Apple's Iphone has Mac OS X, and its PPTP client happens to work with the router in the library.

- Actually we can also use a virtual machine running Windows for the PPTP connection. That would be another story. The author's laptop is too weak to have virtual machine running smoothly.

- For pushing the limit of http proxy, the author selected [tinc](https://www.tinc-vpn.org) to connect to his home router as the full functioned gateway.

## Tutorial

The solution is there. The procedures are complex and very boring and error-prone to set it up manually. OpenRC's stacked runlevel comes as a rescue.

We can make a new runlevel right away for the remaining tutorial:

`root #``mkdir /etc/runlevels/lwifi`
Add our default runlevel into it:

`root #``rc-update -s add default lwifi`
### iPhone tethering

Documented at [Iphone USB tethering](https://wiki.gentoo.org/wiki/Iphone_USB_tethering) and we use static configuration instead of DHCP.

**`/etc/conf.d/net`**

**static config eth1**

```
config_eth1="172.20.10.2/28"
routes_eth1="zzz.xxx.xxx.xxx/yy via 172.20.10.1"
dns_servers_eth1="zzz.xxx.xxx.xxx zzz.xxx.xxx.xxx"
```
The entry in **routes\_eth1** is the network of cellular mobile, which can be identified by [whois](https://wiki.gentoo.org/index.php?title=Whois&action=edit&redlink=1) containing **dns\_servers\_eth1**. We set this up because there is no other easily reachable DNS server. Tinc, in later step, needs DNS to query IP address of its entry node.

Now associate the library wifi with iPhone, and dial PPTP VPN. Consult your iPhone manual for details.

### socks proxy on iPhone

iPhone has an ill design: during tethering, packets can only be routed to cellular mobile network, even if you set up a wifi connection. Therefore we cannot just access the http proxy directly through eth1, a socks proxy server is needed to transmit it.

There are many methods to set up a socks proxy in iPhone. One way to achieve is by sshd.

- First by all means install an sshd in iPhone. (needs link)

- Next, use public key to access the sshd.

- Finally, write a OpenRC initscript file.

**`/etc/init.d/socksIphone`**

**OpenRC initscript ssh socks**

```
#!/sbin/openrc-run
# Copyright 1999-2012 Gentoo Foundation
# Distributed under the terms of the GNU General Public License v2
description="ssh into Iphone as a socks proxy on localhost:1080"
depend() {
        need net.eth1
        before tincd.*
}
start() {
        ebegin "starting $RC_SVCNAME"
        start-stop-daemon --start --make-pidfile --background --pidfile /run/$RC_SVCNAME.pid --exec /usr/bin/ssh \
        -- -nN -D127.0.0.1:1080 root@172.20.10.1
        eend $?
}
stop() {
        ebegin "stopping $RC_SVCNAME"
        start-stop-daemon --stop --pidfile /run/socksIphone.pid
        eend $?
}
```
and add it to our runlevel

`root #``rc-update add socksIphone lwifi`
### proxychains

We are in an interesting realm: an http proxy is behind a socks proxy. Hopefully, [net-misc/proxychains](https://packages.gentoo.org/packages/net-misc/proxychains) can carry us out.

`root #``emerge --ask net-misc/proxychains`
and set up its config file.

**`/etc/proxychains.conf`**

**proxy chain setup**

In the ProxyList, the first one is our socks proxy in the previous section, the second one happens to be http proxy provided by the library.

### tinc initscript

We need to adjust tinc initscript to make use of proxychains.

**`/etc/init.d/tincd.manifold-lwifi`**

**tinc initscript with proxychains**

```
#!/sbin/openrc-run
# Copyright 1999-2012 Gentoo Foundation
# Distributed under the terms of the GNU General Public License v2
extra_started_commands="reload"
EXEC="${EXEC:-$EPREFIX/usr/sbin/tincd}"
NETNAME="${RC_SVCNAME#*.}"
PIDFILE="$EPREFIX/run/tinc.${NETNAME}.pid"
CONFIG="$EPREFIX/etc/tinc/${NETNAME}/tinc.conf"
[ "x${SYSLOG}" = "xyes" ] && LOG="" || LOG=" --logfile=/var/log/tinc.${NETNAME}.log"
OPTS_EXEC="--net=${NETNAME}${LOG} --pidfile ${PIDFILE}"
OPTS_FOREGROUND="-D"
depend() {
        use logger dns
        need net
}
check_config() {
        if [ ! -f "${CONFIG}" ]; then
                eerror "Cannot start network ${NETNAME}."
                eerror "Please set up ${CONFIG} !"
                return 1
        fi
}
reload() {
        if [ -f "${PIDFILE}" ] ; then
                        ebegin "Reloading tinc network ${NETNAME}"
                        start-stop-daemon --signal HUP --pidfile "${PIDFILE}"
                        eend $?
        fi
}
```
The keyidea, though, is to set proxychains in LD\_PRELOAD. Write you own version if necessary.

**`/etc/conf.d/tincd.manifold-lwifi`**

**LD\_PRELOAD proxychains**

```
EXEC_ENV="LD_PRELOAD=libproxychains.so.3"
```
Add this one into our runlevel.

`root #``rc-update add tincd.manifold-lwifi`
#### set up tinc

Notice that the http proxy only allows CONNECT to port 443 (https protocol), our first hop node should have tinc listening on port 443.

For how to configure and use tinc, refer to [tinc](https://wiki.gentoo.org/wiki/Tinc) [\[1\]](https://www.tinc-vpn.org/)

## test this all out

`root #``rc-update | grep lwifi````
             net.eth1 |                    lwifi                 
          socksIphone |                    lwifi                 
 tincd.manifold-lwifi |                    lwifi 
```
Pluggin the phone and fire it up.

`root #``rc lwifi`
Now that we have access to our own VPN, we can access any host inside and use any node as our gateway. The possibility is limitless. From now on the library becomes our second home ;-)
