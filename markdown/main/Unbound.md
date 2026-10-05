<!-- source: https://wiki.gentoo.org/wiki/Unbound | group: Gentoo Wiki (Main) | wiki-title: Unbound -->
---
title: Unbound
url: https://wiki.gentoo.org/wiki/Unbound
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-06-20"
fingerprint: cf411bdf088e3cd4
license: CC BY-SA 4.0
---

# Unbound

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**Unbound** is a validating, recursive, caching DNS resolver.

It is developed by NLnet Labs, a non-profit organization based in the Netherlands.

Its sister package is [net-dns/nsd](https://packages.gentoo.org/packages/net-dns/nsd). Together, *unbound* (recursive, caching resolver) and *nsd* (authoritative name server) fulfil the same role as [BIND](https://wiki.gentoo.org/wiki/BIND).

## Installation

### USE flags


| [+ecdsa](https://packages.gentoo.org/useflags/+ecdsa) | Enable ECDSA support | 
| [+http2](https://packages.gentoo.org/useflags/+http2) | Enable HTTP/2 support for DoH via net-libs/nghttp2 | 
| [+tfo](https://packages.gentoo.org/useflags/+tfo) | Enable TCP Fast Open client+server | 
| [debug](https://packages.gentoo.org/useflags/debug) | Enable extra debug codepaths, like asserts and extra output. If you want to get meaningful backtraces see https://wiki.gentoo.org/wiki/Project:Quality\_Assurance/Backtraces | 
| [dnscrypt](https://packages.gentoo.org/useflags/dnscrypt) | Enable DNSCrypt support | 
| [dnstap](https://packages.gentoo.org/useflags/dnstap) | Enable dnstap support | 
| [ecs](https://packages.gentoo.org/useflags/ecs) | Enable EDNS client subnet support | 
| [gost](https://packages.gentoo.org/useflags/gost) | Enable GOST support | 
| [python](https://packages.gentoo.org/useflags/python) | Add optional support/bindings for the Python language | 
| [redis](https://packages.gentoo.org/useflags/redis) | Enable cache db backend which uses dev-libs/hiredis | 
| [selinux](https://packages.gentoo.org/useflags/selinux) | !!internal use only!! Security Enhanced Linux support, this must be set by the selinux profile or breakage will occur | 
| [static-libs](https://packages.gentoo.org/useflags/static-libs) | Build static versions of dynamic libraries as well | 
| [systemd](https://packages.gentoo.org/useflags/systemd) | Enable use of systemd-specific libraries and features like socket activation or session tracking | 
| [test](https://packages.gentoo.org/useflags/test) | Enable dependencies and/or preparations necessary to run tests (usually controlled by FEATURES=test but can be toggled independently) | 
| [verify-sig](https://packages.gentoo.org/useflags/verify-sig) | Verify upstream signatures on distfiles | 

### Emerge

`root #``emerge --ask net-dns/unbound`
### Additional software

For testing DNS resolving, use the dig tool from the [net-dns/bind](https://packages.gentoo.org/packages/net-dns/bind) package.

## Configuration

### Files

- /etc/unbound/unbound.conf - Global (system wide) configuration file.

### Recursive resolver

Unbound can run as a recursive resolver, starting from the *root hints* file. This is unlike the 'usual' case where another DNS server performs this on your behalf.

First, fetch the hints file:

Then tell Unbound where to find it:

**`/etc/unbound/unbound.conf`**

Note that *tls-upstream* is incompatible with this unless a fallback *forward-zone* is configured (which would defeat the point of a recursive resolver).

Running a recursive resolver is a de facto requirement for a mail server which performs DNSBL lookups, as providers need to estimate the volume of queries to prevent abuse.

### Forwarder

One may wish to run a forwarder instead: in this setup, queries are dispatched to an external recursive resolver. This is faster but it means the relevant upstream resolvers that get contacted can see queries, and if they are unavailable, another resolver has to be used. Unbound supports multiple.

To forward all lookups to a selection of CloudFlare, Google, and Quad9:

**`/etc/unbound/unbound.conf`**

#### DNS over TLS

Unbound supports DNS over TLS. This protects the content of queries from those observing the network, though the remote resolver will still see the query contents.

**`/etc/unbound/unbound.conf`**

### DNSSEC

To be able to use DNSSEC validation, a trust anchor file needs to be available on the system.

#### dnssec-root package

The *unbound* package is configured with a default anchor location (/etc/dnssec/root-anchors.txt) matching [net-dns/dnssec-root](https://packages.gentoo.org/packages/net-dns/dnssec-root). The package already depends on that, so it should work out-of-the-box.

It will be kept up-to-date via the package manager with no work required. This is the preferred method.

#### unbound-anchor

Another option is to use the unbound-anchor tool from [net-dns/unbound](https://packages.gentoo.org/packages/net-dns/unbound) to create the initial trust anchor.

The upstream Unbound documentation [discusses some caveats](https://nlnetlabs.nl/documentation/unbound/howto-anchor/) with its use.

Once created, the anchor will need regular updates. Assuming the file is named /etc/unbound/var/dnssec-trust-anchors.key, one can instruct Unbound to perform these updates:

**`/etc/unbound/unbound.conf`**

Alternatively, the *unbound-anchor* service can be used with systemd.

#### Manual fetch

The zone file can be downloaded from [https://www.internic.net/domain/root.zone](https://www.internic.net/domain/root.zone). The anchor can be extracted by searching for *DNSKEY*. Care is needed to validate this is indeed the correct public key.

### Service

#### OpenRC

`root #````
rc-update add unbound
```
`root #````
rc-service unbound start
```
## Removal

### Unmerge

`root #``emerge --ask --depclean --verbose net-dns/unbound`
## See Also

- [BIND](https://wiki.gentoo.org/wiki/BIND) — a popular free software DNS server
