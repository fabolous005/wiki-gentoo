<!-- source: https://wiki.gentoo.org/wiki/Security_Handbook/Firewalls_and_Network_Security | group: Gentoo Wiki (Main) | wiki-title: Security Handbook/Firewalls and Network Security -->
---
title: Security Handbook/Firewalls and Network Security
url: https://wiki.gentoo.org/wiki/Security_Handbook/Firewalls_and_Network_Security
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2024-03-26"
fingerprint: "6615cb19e4237b61"
license: CC BY-SA 4.0
---

# Security Handbook/Firewalls and Network Security

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

This section is on filtering packets and network-based access control.



## Introduction

Firewalls can exist at multiple points throughout a network, and multiple firewall types can often be used simultaneously if there is a desire to minimize the possibility of bugs in a single vendor's software being a source of compromise. However, this increases complexity and cost, and misconfiguration of such a setup could also result in having less security than a single firewall done properly.

### Types

Basically there are three types of firewalls:

- Packet filter.
- Circuit relay.
- Application gateway.

#### Packet filtering

All Internet Protocol traffic is sent in the form of packets. Large amounts of traffic is split up into small packets, so it can be handled by L2 devices, and is reassembled at the destination. The packet header contains information where it should be delivered to - as well as the origin. Packet filtering firewalls use IP header information to make filtering decisions on forwarded packets.

Generally, information such as the following is considered:

- Source/destination IP address
- Source/destination port
- Protocol
- Protocol Flags
- Connection state

Weaknesses:

- Address information in a packet can is not authenticated and can be spoofed.
- Data or requests within the allowed packet may contain unwanted data that the attacker can use to exploit known bugs in the services on or behind the firewall.
- Blocking IPs and ports doesn't provide much more security, but setting allowlists is unwieldy.

Advantages:

- In-kernel in the case of Netfilter based firewalls such as nftables, and firewalls which build on the Netfilter Framework.
- Implementation can be straightforward.
- Can give warnings of a possible attack before it happens (e.g. by detecting port scans).
- Good for stopping SYN attacks.

Examples of current free packet filters on Linux include:

iptables has been included in the kernel since version 2.4.[\[1\]](https://wiki.gentoo.org#cite_note-1)<sup>[\[2\]](https://wiki.gentoo.org#cite_note-2)</sup> Prior to that was ipchains (since 2.1.102[\[3\]](https://wiki.gentoo.org#cite_note-3)[\[4\]](https://wiki.gentoo.org#cite_note-4)<sup>[\[5\]](https://wiki.gentoo.org#cite_note-5)</sup>), and prior had been ipfirewall or ipfw since 1.1,<sup>[\[6\]](https://wiki.gentoo.org#cite_note-6)</sup> ported from BSD<sup>[\[7\]](https://wiki.gentoo.org#cite_note-7)</sup> and still part of FreeBSD today<sup>[\[8\]](https://wiki.gentoo.org#cite_note-8)</sup>.

nftables succeeded iptables in 2014 in version 3.13.<sup>[\[9\]](https://wiki.gentoo.org#cite_note-9)</sup> Current documentation is available at the following links:

- [https://netfilter.org/projects/nftables/](https://netfilter.org/projects/nftables/)
- [https://wiki.nftables.org/wiki-nftables/index.php/Main\_Page](https://wiki.nftables.org/wiki-nftables/index.php/Main_Page)
- [Moving from iptables to nftables](https://wiki.nftables.org/wiki-nftables/index.php/Moving_from_iptables_to_nftables)
- [Why nftables?](https://wiki.nftables.org/wiki-nftables/index.php/What_is_nftables%3F#Why_nftables.3F)

#### Circuit relay

A circuit level gateway is a firewall that validates connections before allowing data to be exchanged. This means that it does not simply allow or deny packets based on the packet header but determines whether the connection between both ends is valid according to configurable rules before it opens a session and allows data to be exchanged. Filtering is based on:

- Source/destination IP address
- Source/destination port
- A period of time
- Protocol
- User
- Password

All traffic is validated and monitored, and unwanted traffic can be dropped.

Weakness:

- Operates at the Transport Layer and may require substantial modification of the programs that normally provide transport functions

#### Application gateway

The application level gateway is a proxy for applications, exchanging data with remote systems on behalf of the clients. It is kept away from the public safely behind a DMZ (De-Militarized Zone: the portion of a private network that is visible through the firewall) or a firewall allowing no connections from the outside. Filtering is based on:

- Allow or disallow based on source/destination IP address.
- Based on the packet's content.
- Limiting file access based on file type or extension.

Advantages:

- Can cache files, increasing network performance.
- Detailed logging of all connections.
- Scales well (some proxy servers can "share" the cached data).
- No direct access from the outside.
- Can even alter the packet content on the fly.

Weakness:

- Configuration is complex.
- Application gateways are considered to be the most secure solution since they do not have to run as root and the hosts behind them are not reachable from the Internet.

Example of a free application gateway:

### iptables

In order to use [iptables](https://wiki.gentoo.org/wiki/Iptables), it must be enabled in the kernel. iptables can be loaded as modules (the iptables command will load them as they are needed) or compiled into the kernel if one intends to disable Loadable Kernel Modules as discussed previously). For more information on how to configure the kernel for iptables go to the iptables Tutorial Chapter 5: Preparations. After you have compiled the kernel (or while compiling the kernel), add the iptables command via emerge iptables.

Now test that it works by running iptables -L. If this fails something is wrong and you have to check your configuration once more.

iptables is the new and heavily improved packet filter in the Linux 2.4.x kernel. It is the successor of the previous ipchains packet filter in the Linux 2.2.x kernel. One of the major improvements is that iptables is able to perform stateful packet filtering. With stateful packet filtering it is possible to keep track of each established TCP connection.

A TCP connection consists of a series of packets containing information about source IP address, destination IP address, source port, destination port, and a sequence number so the packets can be reassembled without losing data. TCP is a connection-oriented protocol, in contrast to UDP, which is connectionless.

By examining the TCP packet header, a stateful packet filter can determine if a received TCP packet is part of an already established connection or not and decide either to accept or drop the packet.

With a stateless packet filter it is possible to fool the packet filter into accepting packets that should be dropped by manipulating the TCP packet headers. This could be done by manipulating the SYN flag or other flags in the TCP header to make a malicious packet appear to be a part of an established connection (since the packet filter itself does not do connection tracking). With stateful packet filtering it is possible to drop such packets, as they are not part of an already established connection. This will also stop the possibility of "stealth scans", a type of port scan in which the scanner sends packets with flags that are far less likely to be logged by a firewall than ordinary SYN packets.

iptables provides several other features like NAT (Network Address Translation) and rate limiting. Rate limiting is extremely useful when trying to prevent certain DoS (Denial of Service) attacks like SYN floods.

A TCP connection is established by a so called three-way handshake. When establishing a TCP connection the client-side sends a packet to the server with the SYN flag set. When the server-side receives the SYN packet it responds by sending a SYN+ACK packet back to the client-side. When the SYN+ACK is received the client-side responds with a third ACK packet in effect acknowledging the connection.

A SYN flood attack is performed by sending the SYN packet but failing to respond to the SYN+ACK packet. The client-side can forge a packet with a fake source IP address because it does not need a reply. The server-side system will add an entry to a queue of half-open connections when it receives the SYN packet and then wait for the final ACK packet before deleting the entry from the queue. The queue has a limited number of slots and if all the slots are filled it is unable to open any further connections. If the ACK packet is not received before a specified timeout period the entry will automatically be deleted from the queue. The timeout settings vary but will typically be 30-60 seconds or even more. The client-side initiates the attack by forging a lot of SYN packets with different source IP addresses and sends them to the target IP address as fast as possible and thereby filling up the queue of half-open connections and thus preventing other clients from establishing a legitimate connection with the server.

This is where the rate limit becomes handy. It is possible to limit the rate of accepted SYN packets by using the -m limit --limit 1/s. This will limit the number of SYN packets accepted to one per second and therefore restricting the SYN flood on our resources.

Now some practical stuff!

When iptables is loaded in the kernel it has 5 hooks where you can place your rules. They are called INPUT, OUTPUT, FORWARD, PREROUTING and POSTROUTING. Each of these is called a chain and consists of a list of rules. Each rule says if the packet header looks like this, then here is what to do with the packet. If the rule does not match the packet the next rule in the chain is consulted.

You can place rules directly in the 5 main chains or create new chains and add them to as a rule to an existing chain. iptables supports the following options:

First we will try to block all ICMP packets to our machine, just to get familiar with iptables.

Block all ICMP packets:

`root #``iptables -A INPUT -p icmp -j DROP`
First we specify the chain our rule should be appended to, then the protocol of the packets to match, and finally the target. The target can be the name of a user specified chain or one of the special targets `ACCEPT`, `DROP`, `REJECT`, `LOG`, `QUEUE`, or `MASQUERADE`. In this case we use `DROP`, which will drop the packet without responding to the client.

Now try ping localhost. You will not get any response, since iptables will drop all incoming ICMP messages. You will also not be able to ping other machines, since the ICMP reply packet will be dropped as well. Now flush the chain to get ICMP flowing again:

`root #``iptables -F`
Now lets look at the stateful packet filtering in iptables. If we wanted to enable stateful inspection of packets incoming on eth0 we would issue the command:

`root #``iptables -A INPUT -i eth0 -m state --state ESTABLISHED,RELATED -j ACCEPT`
This will accept any packet from an already established connection or related in the INPUT chain. And you could drop any packet that is not in the state table by issuing iptables -A INPUT -i eth0 -m state --state INVALID -j DROP just before the previous command. This enables the stateful packet filtering in iptables by loading the extension "state". If you wanted to allow others to connect to your machine, you could use the flag --state NEW. iptables contains some modules for different purposes. Some of them are:

| Module/Match | Description | Extended options | 
|---|---|---|
| mac | Matching extension for incoming packets mac address. | --mac-source | 
| state | Enables stateful inspection | --state (states are ESTABLISHED,RELATED, INVALID, NEW) | 
| limit | Rate matching limiting | --limit, --limit-burst | 
| owner | Attempt to match various characteristics of the packet creator | --uid-owner userid --gid-owner groupid --pid-owner processid --sid-owner sessionid | 
| unclean | Various random sanity checks on packets | Example | 

Lets try to create a user-defined chain and apply it to one of the existing chains.

First create a new chain with one rule:

`root #````
iptables -X mychain
```
`root #````
iptables -N mychain
```
`root #````
iptables -A mychain -i eth0 -m state --state ESTABLISHED,RELATED -j ACCEPT
```
The default policy is all outgoing traffic is allowed. Incoming is dropped:

`root #````
iptables -P OUTPUT ACCEPT
```
`root #````
iptables -P INPUT DROP
```
And add it to the INPUT chain:

`root #````
iptables -A INPUT -j mychain
```
By applying the rule to the input chain we get the policy: All outgoing packets are allowed and all incoming packets are dropped.

One can find documentation at Netfilter/iptables documentation.

Lets see a full blown example. In this case my firewall/gateway policy states:

- Connections to the firewall are only allowed through SSH (port 22).
- The local network should have access to HTTP, HTTPS and SSH (DNS should also be allowed).
- ICMP traffic can contain payload and should not be allowed. Of course we have to allow some ICMP traffic.
- Port scans should be detected and logged.
- SYN attacks should be avoided.
- All other traffic should be dropped and logged.

**`/etc/init.d/firewall`**

```
#!/sbin/openrc-run
IPTABLES=/sbin/iptables
IPTABLESSAVE=/sbin/iptables-save
IPTABLESRESTORE=/sbin/iptables-restore
FIREWALL=/etc/firewall.rules
DNS1=212.242.40.3
DNS2=212.242.40.51
#inside
IIP=10.0.0.2
IINTERFACE=eth0
LOCAL_NETWORK=10.0.0.0/24
#outside
OIP=217.157.156.144
OINTERFACE=eth1
opts="${opts} showstatus panic save restore showoptions rules"
depend() {
  need net
}
rules() {
  stop
  ebegin "Setting internal rules"
  einfo "Setting default rule to drop"
  $IPTABLES -P FORWARD DROP
  $IPTABLES -P INPUT   DROP
  $IPTABLES -P OUTPUT  DROP
  #default rule
  einfo "Creating states chain"
  $IPTABLES -N allowed-connection
  $IPTABLES -F allowed-connection
  $IPTABLES -A allowed-connection -m state --state ESTABLISHED,RELATED -j ACCEPT
  $IPTABLES -A allowed-connection -i $IINTERFACE -m limit -j LOG --log-prefix \
      "Bad packet from ${IINTERFACE}:"
  $IPTABLES -A allowed-connection -j DROP
  #ICMP traffic
  einfo "Creating icmp chain"
  $IPTABLES -N icmp_allowed
  $IPTABLES -F icmp_allowed
  $IPTABLES -A icmp_allowed -m state --state NEW -p icmp --icmp-type \
      time-exceeded -j ACCEPT
  $IPTABLES -A icmp_allowed -m state --state NEW -p icmp --icmp-type \
      destination-unreachable -j ACCEPT
  $IPTABLES -A icmp_allowed -p icmp -j LOG --log-prefix "Bad ICMP traffic:"
  $IPTABLES -A icmp_allowed -p icmp -j DROP
  #Incoming traffic
  einfo "Creating incoming ssh traffic chain"
  $IPTABLES -N allow-ssh-traffic-in
  $IPTABLES -F allow-ssh-traffic-in
  #Flood protection
  $IPTABLES -A allow-ssh-traffic-in -m limit --limit 1/second -p tcp --tcp-flags \
      ALL RST --dport ssh -j ACCEPT
  $IPTABLES -A allow-ssh-traffic-in -m limit --limit 1/second -p tcp --tcp-flags \
      ALL FIN --dport ssh -j ACCEPT
  $IPTABLES -A allow-ssh-traffic-in -m limit --limit 1/second -p tcp --tcp-flags \
      ALL SYN --dport ssh -j ACCEPT
  $IPTABLES -A allow-ssh-traffic-in -m state --state RELATED,ESTABLISHED -p tcp --dport ssh -j ACCEPT
  #outgoing traffic
  einfo "Creating outgoing ssh traffic chain"
  $IPTABLES -N allow-ssh-traffic-out
  $IPTABLES -F allow-ssh-traffic-out
  $IPTABLES -A allow-ssh-traffic-out -p tcp --dport ssh -j ACCEPT
  einfo "Creating outgoing dns traffic chain"
  $IPTABLES -N allow-dns-traffic-out
  $IPTABLES -F allow-dns-traffic-out
  $IPTABLES -A allow-dns-traffic-out -p udp -d $DNS1 --dport domain \
      -j ACCEPT
  $IPTABLES -A allow-dns-traffic-out -p udp -d $DNS2 --dport domain \
     -j ACCEPT
  einfo "Creating outgoing http/https traffic chain"
  $IPTABLES -N allow-www-traffic-out
  $IPTABLES -F allow-www-traffic-out
  $IPTABLES -A allow-www-traffic-out -p tcp --dport www -j ACCEPT
  $IPTABLES -A allow-www-traffic-out -p tcp --dport https -j ACCEPT
  #Catch portscanners
  einfo "Creating portscan detection chain"
  $IPTABLES -N check-flags
  $IPTABLES -F check-flags
  $IPTABLES -A check-flags -p tcp --tcp-flags ALL FIN,URG,PSH -m limit \
      --limit 5/minute -j LOG --log-level alert --log-prefix "NMAP-XMAS:"
  $IPTABLES -A check-flags -p tcp --tcp-flags ALL FIN,URG,PSH -j DROP
  $IPTABLES -A check-flags -p tcp --tcp-flags ALL ALL -m limit --limit \
      5/minute -j LOG --log-level 1 --log-prefix "XMAS:"
  $IPTABLES -A check-flags -p tcp --tcp-flags ALL ALL -j DROP
  $IPTABLES -A check-flags -p tcp --tcp-flags ALL SYN,RST,ACK,FIN,URG \
      -m limit --limit 5/minute -j LOG --log-level 1 --log-prefix "XMAS-PSH:"
  $IPTABLES -A check-flags -p tcp --tcp-flags ALL SYN,RST,ACK,FIN,URG -j DROP
  $IPTABLES -A check-flags -p tcp --tcp-flags ALL NONE -m limit \
      --limit 5/minute -j LOG --log-level 1 --log-prefix "NULL_SCAN:"
  $IPTABLES -A check-flags -p tcp --tcp-flags ALL NONE -j DROP
  $IPTABLES -A check-flags -p tcp --tcp-flags SYN,RST SYN,RST -m limit \
      --limit 5/minute -j LOG --log-level 5 --log-prefix "SYN/RST:"
  $IPTABLES -A check-flags -p tcp --tcp-flags SYN,RST SYN,RST -j DROP
  $IPTABLES -A check-flags -p tcp --tcp-flags SYN,FIN SYN,FIN -m limit \
      --limit 5/minute -j LOG --log-level 5 --log-prefix "SYN/FIN:"
  $IPTABLES -A check-flags -p tcp --tcp-flags SYN,FIN SYN,FIN -j DROP
  # Apply and add invalid states to the chains
  einfo "Applying chains to INPUT"
  $IPTABLES -A INPUT -m state --state INVALID -j DROP
  $IPTABLES -A INPUT -p icmp -j icmp_allowed
  $IPTABLES -A INPUT -j check-flags
  $IPTABLES -A INPUT -i lo -j ACCEPT
  $IPTABLES -A INPUT -j allow-ssh-traffic-in
  $IPTABLES -A INPUT -j allowed-connection
  einfo "Applying chains to FORWARD"
  $IPTABLES -A FORWARD -m state --state INVALID -j DROP
  $IPTABLES -A FORWARD -p icmp -j icmp_allowed
  $IPTABLES -A FORWARD -j check-flags
  $IPTABLES -A FORWARD -o lo -j ACCEPT
  $IPTABLES -A FORWARD -j allow-ssh-traffic-in
  $IPTABLES -A FORWARD -j allow-www-traffic-out
  $IPTABLES -A FORWARD -j allowed-connection
  einfo "Applying chains to OUTPUT"
  $IPTABLES -A OUTPUT -m state --state INVALID -j DROP
  $IPTABLES -A OUTPUT -p icmp -j icmp_allowed
  $IPTABLES -A OUTPUT -j check-flags
  $IPTABLES -A OUTPUT -o lo -j ACCEPT
  $IPTABLES -A OUTPUT -j allow-ssh-traffic-out
  $IPTABLES -A OUTPUT -j allow-dns-traffic-out
  $IPTABLES -A OUTPUT -j allow-www-traffic-out
  $IPTABLES -A OUTPUT -j allowed-connection
  #Allow client to route through via NAT (Network Address Translation)
  $IPTABLES -t nat -A POSTROUTING -o $OINTERFACE -j MASQUERADE
  eend $?
}
start() {
  ebegin "Starting firewall"
  if [ -e "${FIREWALL}" ]; then
    restore
  else
    einfo "${FIREWALL} does not exists. Using default rules."
    rules
  fi
  eend $?
}
stop() {
  ebegin "Stopping firewall"
  $IPTABLES -F
  $IPTABLES -t nat -F
  $IPTABLES -X
  $IPTABLES -P FORWARD ACCEPT
  $IPTABLES -P INPUT   ACCEPT
  $IPTABLES -P OUTPUT  ACCEPT
  eend $?
}
showstatus() {
  ebegin "Status"
  $IPTABLES -L -n -v --line-numbers
  einfo "NAT status"
  $IPTABLES -L -n -v --line-numbers -t nat
  eend $?
}
panic() {
  ebegin "Setting panic rules"
  $IPTABLES -F
  $IPTABLES -X
  $IPTABLES -t nat -F
  $IPTABLES -P FORWARD DROP
  $IPTABLES -P INPUT   DROP
  $IPTABLES -P OUTPUT  DROP
  $IPTABLES -A INPUT -i lo -j ACCEPT
  $IPTABLES -A OUTPUT -o lo -j ACCEPT
  eend $?
}
save() {
  ebegin "Saving Firewall rules"
  $IPTABLESSAVE > $FIREWALL
  eend $?
}
restore() {
  ebegin "Restoring Firewall rules"
  $IPTABLESRESTORE < $FIREWALL
  eend $?
}
restart() {
  svc_stop; svc_start
}
showoptions() {
  echo "Usage: $0 {start|save|restore|panic|stop|restart|showstatus}"
  echo "start)      will restore setting if exists else force rules"
  echo "stop)       delete all rules and set all to accept"
  echo "rules)      force settings of new rules"
  echo "save)       will store settings in ${FIREWALL}"
  echo "restore)    will restore settings from ${FIREWALL}"
  echo "showstatus) Shows the status"
}
```
Some advice when creating a firewall:

1. Create your firewall policy before implementing it.
2. Keep it simple.
3. Know how each protocol works (read the relevant [RFC](http://www.ietf.org/) (request For comments))
4. Keep in mind that a firewall is just another piece of software running as root.
5. Test your firewall.

If you think that iptables is hard to understand or takes to long to setup a decent firewall you could use [Shorewall](http://www.shorewall.net/). It basically uses iptables to generate firewall rules, but concentrates on rules and not specific protocols.

## Proxies

### Squid

[Squid](https://wiki.gentoo.org/wiki/Squid) is a very powerful proxy server. It can filter traffic based on time, regular expressions on path/URI, source and destination IP addresses, domain, browser, authenticated user name, MIME type, and port number (protocol). I probably forgot some features, but it can be hard to cover the entire list right here.

In the following example I have added a banner filter instead of a filter based on porn sites. The reason for this is that Gentoo.org should not be listed as some porn site. And I do not want to waste my time trying to find some good sites for you.

In this case, my policy states:

- Surfing (HTTP/HTTPS) is allowed during work hours (Mon-Fri 8-17 and Sat 8-13), but if employees are here late they should work, not surf
- Downloading files is not allowed (.exe, .com, .arj, .zip, .asf, .avi, .mpg, .mpeg, etc.)
- We do not like banners, so they are filtered and replaced with a transparent gif (this is where you get creative!).
- All other connections to and from the Internet are denied.

This is implemented in 4 easy steps:

**`/etc/squid/squid.conf`**

```
# Bind to a ip and port
http_port 10.0.2.1:3128
# Standard configuration
hierarchy_stoplist cgi-bin ?
acl QUERY urlpath_regex cgi-bin \?
no_cache deny QUERY
# Add basic access control lists
acl all src 0.0.0.0/0.0.0.0
acl manager proto cache_object
acl localhost src 127.0.0.1/255.255.255.255
# Add who can access this proxy server
acl localnet src 10.0.0.0/255.255.0.0
# And ports
acl SSL_ports port 443
acl Safe_ports port 80
acl Safe_ports port 443
acl purge method PURGE
# Add access control list based on regular
# expressions within urls
acl archives urlpath_regex "/etc/squid/files.acl"
acl url_ads url_regex "/etc/squid/banner-ads.acl"
# Add access control list based on time and day
acl restricted_weekdays time MTWHF 8:00-17:00
acl restricted_weekends time A 8:00-13:00
acl CONNECT method CONNECT
#allow manager access from localhost
http_access allow manager localhost
http_access deny manager
# Only allow purge requests from localhost
http_access allow purge localhost
http_access deny purge
# Deny requests to unknown ports
http_access deny !Safe_ports
# Deny CONNECT to other than SSL ports
http_access deny CONNECT !SSL_ports
# My own rules
# Add a page do be displayed when
# a banner is removed
deny_info NOTE_ADS_FILTERED url_ads
# Then deny them
http_access deny url_ads
# Deny all archives
http_access deny archives
# Restrict access to work hours
http_access allow localnet restricted_weekdays
http_access allow localnet restricted_weekends
# Deny the rest
http_access deny all
```
Next fill in the files you do not want your users to download files. I have added zip, viv, exe, mp3, rar, ace, avi, mov, mpg, mpeg, au, ra, arj, tar, gz, and z files.

**`/etc/squid/files.acl`**

```
\.[Zz][Ii][pP]$
\.[Vv][Ii][Vv].*
\.[Ee][Xx][Ee]$
\.[Mm][Pp]3$
\.[Rr][Aa][Rr]$
\.[Aa][Cc][Ee]$
\.[Aa][Ss][Ff]$
\.[Aa][Vv][Ii]$
\.[Mm][Oo][Vv]$
\.[Mm][Pp][Gg]$
\.[Mm][Pp][Ee][Gg]$
\.[Aa][Uu]$
\.[Rr][Aa]$
\.[Aa][Rr][Jj]$
\.[Tt][Aa][Rr]$
\.[Gg][Zz]$
\.[Zz]$
```


Next we add the regular expressions for identifying banners. You will probably be a lot more creative than I:

**`/etc/squid/banner-ads.acl`**

```
/adv/.*\.gif$
/[Aa]ds/.*\.gif$
/[Aa]d[Pp]ix/
/[Aa]d[Ss]erver
/[Aa][Dd]/.*\.[GgJj][IiPp][FfGg]$
/[Bb]annerads/
/adbanner.*\.[GgJj][IiPp][FfGg]$
/images/ad/
/reklame/
/RealMedia/ads/.*
^http://www\.submit-it.*
^http://www\.eads.*
^http://ads\.
^http://ad\.
^http://ads02\.
^http://adaver.*\.
^http://adforce\.
adbot\.com
/ads/.*\.gif.*
_ad\..*cgi
/Banners/
/SmartBanner/
/Ads/Media/Images/
^http://static\.wired\.com/advertising/
^http://*\.dejanews\.com/ads/
^http://adfu\.blockstackers\.com/
^http://ads2\.zdnet\.com/adverts
^http://www2\.burstnet\.com/gifs/
^http://www.\.valueclick\.com/cgi-bin/cycle
^http://www\.altavista\.com/av/gifs/ie_horiz\.gif
```

And as the last part we want this file to be displayed when a banner is removed. It is basically a half html file with a 4x4 transparent gif image.

**`/etc/squid/errors/NOTE_ADS_FILTERED`**

```
<HTML>
<HEAD>
<META HTTP-EQUIV="REFRESH" CONTENT="0; URL=http://localhost/images/4x4.gif">
<TITLE>ERROR: The requested URL could not be retrieved</TITLE>
</HEAD>
<BODY>
<H1>Add filtered!</H1>
```
As you can see, Squid has a lot of possibilities and it is very effective at both filtering and proxying. It can even use alternative Squid proxies to scale on very large networks. The configuration I have listed here is mostly suited for a small network with 1-20 users.

But combining the packet filter (iptables) and the application gateway (Squid) is probably the best solution, even if Squid is located somewhere safe and nobody can access it from the outside. We still need to be concerned about attacks from the inside.

Now you have to configure your clients browsers to use the proxy server. The gateway will prevent the users from having any contact with the outside unless they use the proxy.

It can also be done transparently by using iptables to forward all outbound traffic to a Squid proxy. This can be done by adding a forwarding/prerouting rule on the gateway:

`root #````
iptables -t nat -A PREROUTING -p tcp --dport 80 -j DNAT --to proxyhost:3128
```
`root #````
iptables -t nat -A PREROUTING -p tcp --dport 443 -j DNAT --to proxyhost:3128
```
We have learned that:

1. A firewall can be a risk in itself. A badly configured firewall is worse than not having one at all.
2. How to setup a basic gateway and a transparent proxy.
3. The key to a good firewall is to know the protocols you want do allow.
4. That IP traffic does not always contain legitimate data, e.g. ICMP packets, which can contain a malicious payload.
5. How to prevent SYN attack.
6. Filtering HTTP traffic by removing offensive pictures and downloads of viruses.
7. Combining packet filters and application gateways provides better control.

Now, if you really need to, go create a firewall that matches your needs.

## The proc filesystem

Many kernel parameters can be altered through the [/proc file system](https://wiki.gentoo.org/wiki/Procfs) or by using sysctl.

To dynamically change kernel parameters and variables on the fly, you need `CONFIG_SYSCTL` enabled in the kernel. This is on by default in a standard 4.0+ kernel.

Deactivate IP forwarding:

`root #``/bin/echo "0" > /proc/sys/net/ipv4/ip_forward`
Make sure that IP forwarding is turned off. We only want this for a multi-homed host. It's advised to set or unset this flag before all other flags since it enabled/disables other flags as well.

Drop ping packets:

`root #``/bin/echo "1" > /proc/sys/net/ipv4/icmp_echo_ignore_all`
This will cause the kernel to simply ignore all ping messages (also known as ICMP type 0 messages). The reason for this is that an IP packet carrying an ICMP message can contain a payload with information other than you think. Administrators use ping as a diagnostic tool and often complain if it is disabled, but there is no reason for an outsider to be able to ping. However, since it sometimes can be handy for insiders to be able to ping, you can disable ICMP type 0 messages in the firewall (allowing local administrators to continue to use this tool).

Ignore broadcast pings:

`root #``/bin/echo "1" > /proc/sys/net/ipv4/icmp_echo_ignore_broadcasts`
This disables response to ICMP broadcasts and will prevent Smurf attacks. The Smurf attack works by sending an ICMP type 0 (ping) message to the broadcast address of a network. Typically the attacker will use a spoofed source address. All the computers on the network will respond to the ping message and thereby flood the host at the spoofed source address.

Disable source routed packets:

`root #``/bin/echo "0" > /proc/sys/net/ipv4/conf/all/accept_source_route`
Do not accept source routed packets. Attackers can use source routing to generate traffic pretending to originate from inside your network, but that is actually routed back along the path from which it came, so attackers can compromise your network. Source routing is rarely used for legitimate purposes, so it is safe to disable it.

Disable redirect acceptance:

`root #``/bin/echo "0" > /proc/sys/net/ipv4/conf/all/accept_redirects`
Do not accept ICMP redirect packets. ICMP redirects can be used to alter your routing tables, possibly to a malicious end.

Enable protection against bad/bogus error message responses:

`root #``/bin/echo "1" > /proc/sys/net/ipv4/icmp_ignore_bogus_error_responses`
Disable [TCP timestamps](https://www.whonix.org/wiki/Disable_TCP_and_ICMP_Timestamps):

`root #``/bin/echo "0" > /proc/sys/net/ipv4/tcp_timestamps`
Enable reverse path filtering:

`root #````
for i in /proc/sys/net/ipv4/conf/*; do
```
/bin/echo "1" > $i/rp_filter

done
Turn on reverse path filtering. This helps make sure that packets use legitimate source addresses by automatically rejecting incoming packets if the routing table entry for their source address does not match the network interface they are arriving on. This has security advantages because it prevents IP spoofing. We need to enable it for each net/ipv4/conf/\* otherwise source validation isn't fully functional.

Log spoofed packets, source routed packets and redirect packets:

`root #``/bin/echo "1" > /proc/sys/net/ipv4/conf/all/log_martians`
All these settings will be reset when the machine is rebooted. I suggest that you add them to /etc/sysctl.conf, which is automatically sourced by the /etc/init.d/bootmisc init script.

The syntax for /etc/sysctl.conf is pretty straightforward. Strip off the /proc/sys/ from the previously mentioned paths and substitute / with .:

**`/etc/sysctl.conf`**

```
net.ipv4.ip_forward = 0
```
The following command will show you *all* actual settings of the /etc/sysctl.conf in a ready to use manner:

`root #``/usr/sbin/sysctl -a`
\<!--snip-->
net.ipv4.conf.all.accept\_local = 0
net.ipv4.conf.all.accept\_redirects = 0
net.ipv4.conf.all.accept\_source\_route = 0
net.ipv4.conf.all.arp\_accept = 0
net.ipv4.conf.all.arp\_announce = 0
net.ipv4.conf.all.arp\_filter = 0
net.ipv4.conf.all.arp\_ignore = 0
net.ipv4.conf.all.arp\_notify = 0
net.ipv4.conf.all.bc\_forwarding = 0
net.ipv4.conf.all.bootp\_relay = 0
net.ipv4.conf.all.disable\_policy = 0
net.ipv4.conf.all.disable\_xfrm = 0
net.ipv4.conf.all.drop\_gratuitous\_arp = 0
net.ipv4.conf.all.drop\_unicast\_in\_l2\_multicast = 0
net.ipv4.conf.all.force\_igmp\_version = 0
net.ipv4.conf.all.forwarding = 0
net.ipv4.conf.all.igmpv2\_unsolicited\_report\_interval = 10000
net.ipv4.conf.all.igmpv3\_unsolicited\_report\_interval = 1000
net.ipv4.conf.all.ignore\_routes\_with\_linkdown = 0
net.ipv4.conf.all.log\_martians = 1
net.ipv4.conf.all.mc\_forwarding = 0
net.ipv4.conf.all.medium\_id = 0
net.ipv4.conf.all.promote\_secondaries = 0
net.ipv4.conf.all.proxy\_arp = 0
net.ipv4.conf.all.proxy\_arp\_pvlan = 0
net.ipv4.conf.all.route\_localnet = 0
net.ipv4.conf.all.rp\_filter = 1
net.ipv4.conf.all.secure\_redirects = 1
net.ipv4.conf.all.send\_redirects = 1
net.ipv4.conf.all.shared\_media = 1
net.ipv4.conf.all.src\_valid\_mark = 0
net.ipv4.conf.all.tag = 0
net.ipv4.conf.default.accept\_local = 0
net.ipv4.conf.default.accept\_redirects = 1
net.ipv4.conf.default.accept\_source\_route = 1
net.ipv4.conf.default.arp\_accept = 0
net.ipv4.conf.default.arp\_announce = 0
net.ipv4.conf.default.arp\_filter = 0
net.ipv4.conf.default.arp\_ignore = 0
net.ipv4.conf.default.arp\_notify = 0
net.ipv4.conf.default.bc\_forwarding = 0
net.ipv4.conf.default.bootp\_relay = 0
net.ipv4.conf.default.disable\_policy = 0
net.ipv4.conf.default.disable\_xfrm = 0
net.ipv4.conf.default.drop\_gratuitous\_arp = 0
net.ipv4.conf.default.drop\_unicast\_in\_l2\_multicast = 0
net.ipv4.conf.default.force\_igmp\_version = 0
net.ipv4.conf.default.forwarding = 0
net.ipv4.conf.default.igmpv2\_unsolicited\_report\_interval = 10000
net.ipv4.conf.default.igmpv3\_unsolicited\_report\_interval = 1000
net.ipv4.conf.default.ignore\_routes\_with\_linkdown = 0
net.ipv4.conf.default.log\_martians = 0
net.ipv4.conf.default.mc\_forwarding = 0
net.ipv4.conf.default.medium\_id = 0
net.ipv4.conf.default.promote\_secondaries = 0
net.ipv4.conf.default.proxy\_arp = 0
net.ipv4.conf.default.proxy\_arp\_pvlan = 0
net.ipv4.conf.default.route\_localnet = 0
net.ipv4.conf.default.rp\_filter = 0
net.ipv4.conf.default.secure\_redirects = 1
net.ipv4.conf.default.send\_redirects = 1
net.ipv4.conf.default.shared\_media = 1
net.ipv4.conf.default.src\_valid\_mark = 0
net.ipv4.conf.default.tag = 0
\<!--snip-->

## See also

## External resources

## References
