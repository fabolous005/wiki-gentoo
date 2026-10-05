<!-- source: https://wiki.gentoo.org/wiki/Postfix/Blacklisting | group: Gentoo Wiki (Main) | wiki-title: Postfix/Blacklisting -->
---
title: Postfix/Blacklisting
url: https://wiki.gentoo.org/wiki/Postfix/Blacklisting
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-06-16"
fingerprint: a59d994724582e84
license: CC BY-SA 4.0
---

# Postfix/Blacklisting

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**Blacklisting** is the process of banning specific IP addresses or address ranges from sending email to your server entirely.

## Background

In the email server administration world, blacklisting generally refers to using a Realtime Black(hole) List (usually abbreviated to **RBL**) service operating over the DNS protocol.

There are various RBL services available. A good overview is [over at Wikipedia's list of DNS blacklists](https://en.wikipedia.org/wiki/Comparison_of_DNS_blacklists). Another fairly useful resource is [DNSBL.info](http://www.dnsbl.info) which allows you to check if your server's IP address block has been listed in any of the major RBLs.

Basically, when a computer asks your system to receive some mail, your system sends a DNS query to the RBL provider(s) including the identity of that sending system. If the system is blacklisted, the response will indicate so, and the mail will be rejected.

Using an RBL is equivalent to trusting them to audit all inbound mail. This is a very significant type of trust to extend to a third party.

If the RBL goes down, all inbound mail may get rejected: [example](http://www.dnsbl.info/dul-ru-offline.php). Therefore, one may be better off picking a smaller number of more widely used RBLs than simply enabling all of them!

In addition, use of a DNSBL broadcasts the identities of connecting systems in an unencrypted protocol across the internet, which may be monitored by ISPs and state actors in order to efficiently monitor encrypted email. One may wish to obfuscate the DNS query path of a mail server or think very carefully before enabling address-level query types (see note below).

## Prerequisites

A local recursive resolver is essentially required, as RBLs will blacklist generic public resolvers because they cannot be used to estimate query volumes and detect abuse.

[Unbound](https://wiki.gentoo.org/wiki/Unbound#Recursive_resolver) is a popular choice.

## Setup

To enable them, add `reject_rbl_client` entries to main.cf file in the `smtpd_recipient_restriction` directive.

Its contents are based upon the services selected, as follows:

**`/etc/postfix/main.cf`**

**Use blacklists**

There is another directive, `reject_rhsbl_sender`, which apparently operates at the sender email address degree of granularity rather than the server level. RBL providers of this type seem to include `dsn.rfc-ignorant.org`. However, using such services may be a serious privacy violation.

## Deployment

Finally, tell Postfix to reload its configuration for the changes to take effect:

`root #``/etc/init.d/postfix reload`
