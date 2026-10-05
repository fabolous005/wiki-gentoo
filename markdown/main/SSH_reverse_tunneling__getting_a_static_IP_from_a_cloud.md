<!-- source: https://wiki.gentoo.org/wiki/SSH_reverse_tunneling:_getting_a_static_IP_from_a_cloud | group: Gentoo Wiki (Main) | wiki-title: SSH reverse tunneling: getting a static IP from a cloud -->
---
title: "SSH reverse tunneling: getting a static IP from a cloud"
url: https://wiki.gentoo.org/wiki/SSH_reverse_tunneling:_getting_a_static_IP_from_a_cloud
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2024-08-07"
fingerprint: "241d616fc1a99bd6"
license: CC BY-SA 4.0
---

# SSH reverse tunneling: getting a static IP from a cloud

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

SSH reverse tunneling is a method of forwarding any type of traffic **from a remove machine to your local laptop** - so you can have, for example, [AWS free tier](https://aws.amazon.com/free/) with static ipv4/ipv6 - and calls to that IP will be forwarded to your laptop (and answers from your laptop back to the client). It can be useful if you want to share something directly from your laptop but you are behind [NAT](https://wiki.gentoo.org/wiki/NAT) (most ISP have this - because of [IPv4 address exhaustion](https://en.wikipedia.org/wiki/IPv4_address_exhaustion)) and without static IP at your home. Another direction - tunneling *from your local laptop to remove machine* is called [SSH tunneling](https://wiki.gentoo.org/wiki/SSH_tunneling).

### Server (remote with static IP)

**`/etc/ssh/sshd_config`**

**Make sure to have GatewayPorts yes**

### Client (your laptop)

How to connect without a config file:

`user $``ssh -i key.pem -R <port-incomming>:localhost:<port-local> algo@xx.xx.xx.xx -p <for-for-connecting>`
It can be simplier with config file that is supplied by **-F**

**`ssh_config_reverse_tunnel`**

**Port 63368 of remote incoming will be forwarded to your local port 8000**

And now you command is shorter:

`user $``ssh -F ssh_config_reverse_tunnel xx.xx.xx.xx`
It works even if you connected to the same machine by [VPN](https://wiki.gentoo.org/wiki/VPN).

### See also

[https://serveo.net](https://serveo.net): SSH reverse tunneling as-a-service
