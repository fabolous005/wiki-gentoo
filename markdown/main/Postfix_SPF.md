<!-- source: https://wiki.gentoo.org/wiki/Postfix/SPF | group: Gentoo Wiki (Main) | wiki-title: Postfix/SPF -->
---
title: Postfix/SPF
url: https://wiki.gentoo.org/wiki/Postfix/SPF
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2017-03-01"
fingerprint: b6bdd1422c12b605
license: CC BY-SA 4.0
---

# Postfix/SPF

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

Sender Policy Framework (SPF) allows domain owners to state in their DNS records which IP addressess should be allowed to send mails from their domain. This will prevent spammers from spoofing the `Return-Path`.

## Setup

### Outbound

First, domain owners have to create a special `TXT` DNS record. Then an SPF-enabled MTA can read this and if the mail originates from a server that is not described in the SPF record the mail can be rejected. An example entry could look like this:

The `-all` means to reject all mail by default but allow mail from the `A`( `a` ), `MX`( `mx` ) and `PTR`( `ptr` ) DNS records. For more info consult further resources below.

### Inbound

Apparently there are now a few different SPF-related packages in portage:

- perl-based
  - *dev-perl/Mail-SPF*
  - *dev-perl/Mail-SPF-Query*
- python-based
  - *dev-python/pyspf*
  - *mail-filter/pypolicyd-spf*
- C-based
  - *mail-filter/libspf2*

All seem well used implementations.

#### Apparently old/outdated info based on perl implementation

grab the spf.pl with:

`root #``cp postfix-<version>/examples/smtpd-policy/spf.pl /usr/local/bin/`
This Perl script also needs some Perl libraries that are not in portage but it is still quite simple to install them:

`root #``emerge Mail-SPF-Query Net-CIDR-Lite Sys-Hostname-Long`
Now that we have everything in place all we need is to configure Postfix to use this new policy.

**`master.cf`**

**use SPF**

Now add the SPF check in main.cf . Properly configured SPF should do no harm so we could check SPF for all domains:

**`main.cf`**

**use SPF**

## Testing

A restart or reload may be required to synchronize this new record to the secondary servers and propagated through the DNS system. Once the record is visible in the DNS system, it will begin to be used. Keep this in mind if testing fails, check the domain's TXT record(s).

`user $``dig domain.tld txt`
Or, the same command using a specific DNS server.

`user $``dig @some.dns.server.tld domain.tld txt`
