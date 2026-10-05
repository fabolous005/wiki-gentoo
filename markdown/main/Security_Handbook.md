<!-- source: https://wiki.gentoo.org/wiki/Security_Handbook | group: Gentoo Wiki (Main) | wiki-title: Security Handbook -->
---
title: Security Handbook
url: https://wiki.gentoo.org/wiki/Security_Handbook
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-09-07"
fingerprint: "8733dd0094a5231c"
license: CC BY-SA 4.0
---

# Security Handbook

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

The **Security Handbook** supplements the [Gentoo Handbook](https://wiki.gentoo.org/wiki/Handbook:Main_Page) and aims to provide valuable guidance on Gentoo Linux security and cybersecurity in general.

As with the Gentoo Handbook, this document is broken up into multiple sections. These are linked individually below and may be followed in-order; for convenience the all-in-one-page Security handbook may be found [here](https://wiki.gentoo.org/wiki/Security_Handbook/Full).

This handbook is informed by industry best practice (e.g. the [Australian Cyber Security Centre's Information Security Manual (ISM)](https://www.cyber.gov.au/sites/default/files/2023-03/Information%20Security%20Manual%20-%20%28March%202023%29.pdf) and other similar documents).

It is important to note that cyber security is not a static field. As such, this handbook will be updated as new information becomes available and users are advised to check back regularly.

## Contents

### Introduction and theory

- [Security concepts](https://wiki.gentoo.org/wiki/Security_Handbook/Concepts)
- Important concepts to consider
- [General security guidance](https://wiki.gentoo.org/wiki/Security_Handbook/General_Guidance)
- Some general security guidance for those that want a TL;DR

### Hardware security

### Firmware security

- [Firmware security](https://wiki.gentoo.org/wiki/Security_Handbook/Firmware_security)
- Firmware security considerations.

### Software security

#### Local

- [Staying up-to-date](https://wiki.gentoo.org/wiki/Security_Handbook/Staying_up-to-date)
- Ensuring the latest security updates.
- [Boot Path Security](https://wiki.gentoo.org/wiki/Security_Handbook/Boot_Path_Security)
- Security between the Boot ROM and the Linux Kernel
- [Mounting partitions](https://wiki.gentoo.org/wiki/Security_Handbook/Mounting_partitions)
- /etc/fstab provides many security options.
- [Kernel security](https://wiki.gentoo.org/wiki/Security_Handbook/Kernel_security)
- Instructions for securing the kernel.
- [Linux security modules](https://wiki.gentoo.org/wiki/Security_Handbook/Linux_Security_Modules)
- An overview of mandatory access control options.
- [User and group limitations](https://wiki.gentoo.org/wiki/Security_Handbook/User_and_group_limitations)
- provides detail on controlling the system's resource usage of users via limits and quotas.
- [File permissions](https://wiki.gentoo.org/wiki/Security_Handbook/File_permissions)
- Securing local files.
- [PAM](https://wiki.gentoo.org/wiki/Security_Handbook/PAM)
- Pluggable Authentication Modules.

#### Remote

- [Firewalls and network security](https://wiki.gentoo.org/wiki/Security_Handbook/Firewalls_and_Network_Security)
- A guide on packet filtering and network security options in the kernel.
- [iptables](https://wiki.gentoo.org/wiki/Iptables)
- [nftables](https://wiki.gentoo.org/wiki/Nftables)
- [Securing services](https://wiki.gentoo.org/wiki/Security_Handbook/Securing_services)
- Help on ensuring system daemons are secure and controlling access to services.
- [Chrooting and virtual servers](https://wiki.gentoo.org/wiki/Security_Handbook/Chrooting_and_virtual_servers)
- Isolating servers.

### Data and information security

- [Information Security](https://wiki.gentoo.org/wiki/Security_Handbook/Information_Security)
- Keeping data secure

### Logs and auditing

- [Logging](https://wiki.gentoo.org/wiki/Security_Handbook/Logging)
- Choose between (at least) three different system loggers.
- [Intrusion detection](https://wiki.gentoo.org/wiki/Security_Handbook/Intrusion_detection)
- How to discover if intruders have entered a system.
