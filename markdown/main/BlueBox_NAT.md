<!-- source: https://wiki.gentoo.org/wiki/BlueBox/NAT | group: Gentoo Wiki (Main) | wiki-title: BlueBox/NAT -->
---
title: BlueBox/NAT
url: https://wiki.gentoo.org/wiki/BlueBox/NAT
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2013-04-09"
fingerprint: d4a4bddbdeaeafaa
license: CC BY-SA 4.0
---

# BlueBox/NAT

[BlueBox](https://wiki.gentoo.org/wiki/BlueBox)

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

NOTE: This wiki for getting shorewall NAT traversal for SIP working applies equally to [Asterisk](https://wiki.gentoo.org/wiki/Asterisk)/FreePBX -- 

however, Asterisk/FreePBX does not use multiple domains and only listens for SIP connections on a single port (by default 5060) so just delete/comment out the rules referencing ports 5070, 5080, etc.

I use [net-firewall/iptables](http://www.iptables.org) and also use NAT and need to forward the following udp ports from my router to my FreeSWITCH server:

- 5060 (standard SIP -- Authenticated SIP)
- 5070 (alternate SIP -- Authenticated SIP -- NAT)
- 5080 (alternate SIP -- UnAuthenticated SIP)

as well as the port range for RTP (10000 to 20000).

In addition I use [net-firewall/shorewall](http://www.shorewall.net) for the firewall on my FreeSWITCH server, 

so I need the following in my /etc/shorewall/rules:

\# ASTERISK and FreeSWITCH
ACCEPT          loc             $FW     tcp     5038 # AMP -- Asterisk Manager Protocol
ACCEPT          all             $FW     udp     5036 # iax
ACCEPT          all             $FW     udp     4569 # iax2
# SIP
ACCEPT          all             $FW     udp     5060 # sip
ACCEPT          all             $FW     tcp     5060 # sip Some SIP servers need tcp as well
ACCEPT          all             $FW     udp     5061 # sips
ACCEPT          all             $FW     tcp     5061 # sips
ACCEPT          all             $FW     udp     5070 # FreeSWITCH sip udp
ACCEPT          all             $FW     tcp     5070 # FreeSWITCH sip tcp
ACCEPT          all             $FW     udp     5080 # FreeSWITCH sip udp
ACCEPT          all             $FW     tcp     5080 # FreeSWITCH sip tcp
ACCEPT          all             $FW     udp     10000:20000     # rtp
#ACCEPT         all             $FW     udp     2727 # MGCP media gateway control protocol
#H323 tcp 522, 1503, 1720, 1731 and 8080

Following the Forum Thread 
[How do I configure shorewall to work with VoIP SIP? (FIXED!)](http://www.mail-archive.com/shorewall-users@lists.sourceforge.net/msg03965.html)

I first tried disabling kernel connection tracking for sip in /etc/shorewall/start:

modprobe -r nf\_nat\_sip        &> /dev/null

modprobe -r nf\_conntrack\_sip &> /dev/null

#

modprobe -r nf\_nat\_h323 &> /dev/null

modprobe -r nf\_conntrack\_h323 &> /dev/null


And I also added the above ports to /etc/shorewall/notrack:

NOTRACK          loc             192.168.1.0/24     udp     5060 # sip

NOTRACK loc 192.168.1.0/24 tcp 5060 # sip

NOTRACK $FW 192.168.1.0/24 udp 5060 # sip

NOTRACK $FW 192.168.1.0/24 tcp 5060 # sip

and so on for ports 5061, 5070, 5080, 10000:20000


But (not unexpectedly) after calling the DID and picking up my extension, there was no audio on either my cell phone or sip phone. So next I edited the "Unauthenticated SIP" interface and changed the "Default Interface" to "Authenticated SIP -- NAT" and checked the following:

- Server is behind NAT?: ON
- NAT detection mechanism: "Detect IP via uPnP"
- SIP Ping Registered Devices: ON
- Aggressive NAT Detection: ON
- Use Network IP & Port for RTP: OFF

Now I had audio on my cell phone but still no audio on my sip phone. 



After reading the [FreeSwitch Firewall WIKI](http://wiki.freeswitch.org/wiki/Firewall)
I solved the one-way audio problem by instead using the kernel's conntrack  modules for sip.
So I removed the above ports from /etc/shorewall/notrack
and removed the entries for conntrack modules for nat, sip, h323 in /etc/shorewall/start



Because BlueBox/FreeSwitch uses multiple ports {5060, 5070, 5080} to distinguish multiple interfaces for multiple domains, it is necessary to compile the kernel with connection tracking as modules so that the module can be loaded specifying which ports to track instead of the default 5060.

`root #``zgrep -i conntrack /proc/config.gz`
CONFIG\_NF\_CONNTRACK=m
CONFIG\_NF\_CONNTRACK\_SIP=m

Then I enabled module loading for these modules in /etc/shorewall/modules.essential

loadmodule nf\_nat\_sip                               

loadmodule nf\_conntrack\_sip    ports=5060,5070,5080 


And now we have proper SIP NAT traversal with two-way audio!
