<!-- source: https://wiki.gentoo.org/wiki/BIND/Guide | group: Gentoo Wiki (Main) | wiki-title: BIND/Guide -->
---
title: BIND/Guide
url: https://wiki.gentoo.org/wiki/BIND/Guide
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2024-09-25"
fingerprint: b0a18559f0b3b9c7
license: CC BY-SA 4.0
---

# BIND/Guide

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

This guide details the installation and configuration of BIND for a domain and a local network.

## Introduction

BIND is the most used DNS server on Internet. This guide explains how to configure BIND for a domain using different configurations, one for a local network and one for the rest of the world. Two views will be used to do so:

1. View of the internal zone (the local network).
2. View for the external zone (rest of the world).

## Data used in the examples

| Keyword | Explanation | Example | 
|---|---|---|
| YOUR\_DOMAIN | Your domain name | gentoo.org | 
| YOUR\_PUBLIC\_IP | The public ip that ISP gives to you | 204.74.99.100 | 
| YOUR\_LOCAL\_IP | The local ip address | 192.168.1.5 | 
| YOUR\_LOCAL\_NETWORK | The local network | 192.168.1.0/24 | 
| SLAVE\_DNS\_SERVER | The ip address of the slave DNS server for your domain. | 209.177.148.228 | 
| ADMIN | The DNS server administrator's name. | root | 
| MODIFICATION | The modification date of the file zone, with a number added | 2009062901 | 

## Configuring BIND

### Installation

First, install [net-dns/bind](https://packages.gentoo.org/packages/net-dns/bind).

`root #``emerge --ask net-dns/bind`
### Configuring /etc/bind/named.conf

The first thing to configure is /etc/bind/named.conf. The first part of this step is specifying bind's root directory, the listening port with the IPs, the pid file, and a line for IPv6 protocol.

**`/etc/bind/named.conf`**

**options section**

The second part of named.conf is the internal view used for our local network.

**`/etc/bind/named.conf`**

**Internal view**

The third part of named.conf is the external view used to resolve our domain name for the rest of the world and to resolve all other domain names for us (and anyone who wants to use our DNS server).

**`/etc/bind/named.conf`**

**External view**

The final part of named.conf is the logging policy.

**`/etc/bind/named.conf`**

**External view**

The /var/log/named/ directory must be exist and belong to `named`:

`root #````
mkdir -p /var/log/named/
```
`root #````
chmod 770 /var/log/named/
```
`root #````
touch /var/log/named/named.log
```
`root #````
chmod 660 /var/log/named/named.log
```
`root #````
chown -R named /var/log/named/
```
`root #````
chgrp -R named /var/log/named/
```
### Creating the internal zone file

We use the hostnames and IP addresses of the picture network example. Note that almost all (not all) domain names finish with "." (dot).

**`/var/bind/pri/YOUR_DOMAIN.internal`**

### Creating the external zone file

Here we only have the subdomains we want for external clients (www, mail, and ns).

**`/var/bind/pri/YOUR_DOMAIN.external`**

### Finishing configuration

You'll need to add `named` to the default runlevel:

`root #``rc-update add named default`
## Configuring clients

Now you can use your own DNS server in all machines of your local network to resolve domain names. Modify the /etc/resolv.conf file of all machines of your local network.

**`/etc/resolv.conf`**

Note that YOUR\_DNS\_SERVER\_IP is the same as YOUR\_LOCAL\_IP we used in this document. In the picture the example is 192.168.1.5.

## Testing

We are able to test our new DNS server. First, we need to start the service.

`root #``/etc/init.d/named start`
Now, we are going to make some `host` commands to some domains. We can use any computer of our local network to do this test. If you don't have `net-dns/host` installed you can use `ping` instead. Otherwise, first run `emerge host` .

`user $``host www.gentoo.org`
www.gentoo.org has address 209.177.148.228
www.gentoo.org has address 209.177.148.229

`user $``host hell`
hell.YOUR\_DOMAIN has address 192.168.1.3

`user $``host router`
router.YOUR\_DOMAIN has address 192.168.1.1

## Protecting the server with iptables

When running the DNS service, iptables can be configured with these rules for added protection:
