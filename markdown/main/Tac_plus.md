<!-- source: https://wiki.gentoo.org/wiki/Tac_plus | group: Gentoo Wiki (Main) | wiki-title: Tac plus -->
---
title: tac plus
url: https://wiki.gentoo.org/wiki/Tac_plus
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2025-11-28"
fingerprint: bc19ff5b0a80ad95
license: CC BY-SA 4.0
---

# tac plus

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)


This article is **archived (obsolete)**. Contents are surely incorrect for current usage, and are intended for historical reference only.

TLDR:

**Do not use this article!**



*TACACS+ ebuild has been removed from portage Removal on 2024-02-06. See Bug #918536.*

From the [TACACS+ article](https://en.wikipedia.org/wiki/TACACS%2B) at Wikipedia, the free encyclopedia:

- In computer networking, TACACS+ (*Terminal Access Controller Access-Control System Plus*) is a AAA protocol which provides access control for user Authentication for routers, network access servers and other networked computing devices via one or more centralized servers. TACACS+ provides separate authentication, authorization and accounting services.

TACACS+ is a protocol for AAA services (Authentication, Authorization, Accounting) similar to RADIUS. 
A system that provides logins to users is often called a NAS (Network Access Server), not to be confused with NAS - (Network Attached Storage). A NAS can be a *client* to an *AAA server*, such as a  RADIUS, LDAP, or TACACS daemon. The client must use the authentication protocol appropriate for the server. A Linux system may act as an authentication client when when logging in a user. Based on the PAM configuration, the Linux system can use a RADIUS, LDAP, or TACACS server or may perform purely local authentication. To use TACACS, the Linux (or other) client must have IP access to a **TACACS server**, which is usually a separate physical server that provides authentication services to many clients. This page describes how to configure a Linux system to act as a TACACS server using the **tac plus** software package. It is often useful to have a TACACS server to support authentication for proprietary systems on your network, such as Cisco routers, that implement TACACS clients. With such a server, you can add or delete a new router administrator on all of your routers at the same time in one place. If some of your Linux systems are acting as network elements that should be accessed only by your network administrators, you may choose to configure these systems to also use your TACACS server for AAA.

## About

This document describes how to configure and use the most recent version of tac\_plus provided by [Shrubbery Networks](https://www.shrubbery.net/tac_plus/).

This installation howto uses tac\_plus-4.0.4.27a as reference. General configuration and troubleshooting tips should also apply to older tac\_plus versions available in the portage.

## Installation

### USE flags

*net-nds/tac\_plus*correct?

### Emerge

Be sure to review and set USE flags accordingly before emerging the package:

`root #``emerge --ask net-nds/tac_plus`
## Configuration

Shrubbery tac\_plus is lacking a good documentation. General configuration is split up in 3 main sections:

- ACL (Access Lists)
- group
- users

Further configuration tips at [tac\_plus FAQ](https://www.shrubbery.net/tac_plus/FAQ)

Ways to configure user authentication with tac\_plus:

- Authentication to local passwd file /etc/passwd
- Authentication to LDAP server with **PAM**
- Authentication to password configured in /etc/tac\_plus/tac\_plus.conf

### Authentication with passwd file

User authentication to local passwd file /etc/passwd example:

**`/etc/tac_plus/tac_plus.conf`**

### Authentication with PAM

User authentication with **PAM** example:

**`/etc/tac_plus/tac_plus.conf`**

### Authentication to tac\_plus.conf

User authentication to password configured in /etc/tac\_plus/tac\_plus.conf example:

**`/etc/tac_plus/tac_plus.conf`**

tac\_plus uses the crypt() library in the underlying operating system and asks it to hash a given password against the hash in *tac\_plus.conf*.

As such, one can transparently put any hash value you like in tac\_plus.conf as long as glibc crypt() supports it. On Linux systems these days with *>=glibc-2.7*

- Blowfish
- SHA-512
- SHA-256
- MD5

are supported. To show supported encryption methodes on the tac\_plus server use following command:

`user $``man 3 crypt`
Password hash generation can be done using following tools:

- Blowfish

- `user $``htpasswd -bnBC 13 "" Secret-Password | tr -d ':\n'`

- SHA512

- `user $``mkpasswd  -m SHA-512`

- SHA256

- `user $``mkpasswd  -m SHA-256`

- MD5

- `user $``openssl passwd -1`

## Network equipment configuration

A variety of systems implements the client side of the TACACS+ protocol. The following platforms implemented TACACS+ protocol communication:

- Cisco (CatOS, IOS, IOS-XE, IOS-XR, NX-OS)
- Juniper (ScreenOS, JUNOS)
- Huawei (VRP)
- HP (ComWare, ArubaOS, AOS-CX)
- OneAccess
- Linux-based systems (via PAM)

Most operating systems and vendors have TACACS+ client implementation, or other AAA protocol like RADIUS or Diameter.

Basic AAA (Authentication, Authorization, Accounting) configuration on a Cisco IOS component:

- Substitute *tacacs-server host* with IP address of the tac\_plus server
- For *key* choose the key which is configured in /etc/tac\_plus/tac\_plus.conf

!
aaa new-model
aaa authentication login default group tacacs+ local
aaa authentication enable default group tacacs+
aaa authorization exec default group tacacs+ local
!
tacacs-server host 192.0.2.10 key 123-my\_tacacs\_key
!
line con 0
 login authentication default
!
line vty 0 15
 login authentication default
!

## Final configuration steps

Start tac\_plus daemon:

`root #``/etc/init.d/tac_plus start`
Add tac\_plus to the default runlevel:

`root #``rc-update add tac_plus default`
Verify tac\_plus is running:

`root #````
ps -ef |grep tac_plus
```
root      8123     1  0 21:29 ?        00:00:00 /usr/bin/tac\_plus -C /etc/tac\_plus/tac\_plus.conf

## Troubleshooting

Verifying the interfaces and ports on which tac\_plus is listening:

`root #````
netstat -tulpen | grep tac_plus
```
tcp        0      0 0.0.0.0:49              0.0.0.0:\*               LISTEN      0          27930913   8455/tac\_plus

Looking for configuration errors if daemon fails to start:

`root #````
tail -f /var/log/messages
```
2011-04-09T21:26:28.847493+02:00 server tac\_plus\[7749\]: Reading config
2011-04-09T21:26:28.847605+02:00 server tac\_plus\[7749\]: Error Unrecognised keyword default for user on line 51
2011-04-09T21:26:28.851096+02:00 server /etc/init.d/tac\_plus\[7738\]: ERROR: tac\_plus failed to start

Tacacs communication between tacacs-server and a network component. Example output of a a successful user session:

Run tcpdump on the local tacacs-server:

`root #````
tcpdump -i eth0 tcp port 49
```
22:53:01.692185 IP switch.11384 > server.tacacs: S 2173305858:2173305858(0) win 4128 \<mss 1460>
22:53:01.692221 IP server.tacacs > switch.11384: S 4283961231:4283961231(0) ack 2173305859 win 5840 \<mss 1460>
22:53:01.693690 IP switch.11384 > server.tacacs: . ack 1 win 4128
22:53:01.793233 IP switch.11384 > server.tacacs: P 1:43(42) ack 1 win 4128
22:53:01.793282 IP server.tacacs > switch.11384: . ack 43 win 5840
22:53:01.808601 IP server.tacacs > switch.11384: P 1:29(28) ack 43 win 5840
22:53:01.993368 IP switch.11384 > server.tacacs: P 43:68(25) ack 29 win 4100
22:53:02.002160 IP server.tacacs > switch.11384: P 29:47(18) ack 68 win 5840
22:53:02.002187 IP server.tacacs > switch.11384: F 47:47(0) ack 68 win 5840
22:53:02.004152 IP switch.11384 > server.tacacs: . ack 48 win 4082
22:53:02.096209 IP switch.11384 > server.tacacs: FP 68:68(0) ack 48 win 4082
22:53:02.096231 IP server.tacacs > switch.11384: . ack 69 win 5840
22:53:02.123615 IP switch.11385 > server.tacacs: S 4146347262:4146347262(0) win 4128 \<mss 1460>
22:53:02.123641 IP server.tacacs > switch.11385: S 4294861878:4294861878(0) ack 4146347263 win 5840 \<mss 1460>
22:53:02.127410 IP switch.11385 > server.tacacs: . ack 1 win 4128
22:53:02.229706 IP switch.11385 > server.tacacs: P 1:62(61) ack 1 win 4128
22:53:02.229751 IP server.tacacs > switch.11385: . ack 62 win 5840
22:53:02.229890 IP server.tacacs > switch.11385: P 1:52(51) ack 62 win 5840
22:53:02.229923 IP server.tacacs > switch.11385: F 52:52(0) ack 62 win 5840
22:53:02.232297 IP switch.11385 > server.tacacs: . ack 53 win 4077
22:53:02.330097 IP switch.11385 > server.tacacs: FP 62:62(0) ack 53 win 4077
22:53:02.330118 IP server.tacacs > switch.11385: . ack 63 win 5840

To get debug ouput from tac\_plus run tac\_plus from shell with following command:

`root #``tac_plus -C /etc/tac_plus/tac_plus.conf -L -p 49 -d128 -g`
for used command line options in this command read the tac\_plus manual:

`user $``man tac_plus`
## See also

- [FreeRADIUS](https://wiki.gentoo.org/wiki/FreeRADIUS) — implementation of the Remote Authentication Dial-In User Service (RADIUS) protocol
