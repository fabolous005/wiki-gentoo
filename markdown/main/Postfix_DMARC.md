<!-- source: https://wiki.gentoo.org/wiki/Postfix/DMARC | group: Gentoo Wiki (Main) | wiki-title: Postfix/DMARC -->
---
title: Postfix/DMARC
url: https://wiki.gentoo.org/wiki/Postfix/DMARC
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2023-04-27"
fingerprint: ef8ab992a4929895
license: CC BY-SA 4.0
---

# Postfix/DMARC

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

This guide will show how to configure [Postfix](https://wiki.gentoo.org/wiki/Postfix) to use DMARC. 
To read more about DMARC, see its [wiki](https://en.wikipedia.org/wiki/DMARC) page. Because DMARC is based on them, it may be desirable to set up SPF and [DKIM](https://wiki.gentoo.org/wiki/Postfix/DKIM) first.

## Overview

A DMARC policy allows a sender's domain to indicate that their emails are protected by SPF and/or DKIM, and tells a receiver what to do if neither of those authentication methods passes - such as junk or reject the message. DMARC removes guesswork from the receiver's handling of these failed messages, limiting or eliminating the user's exposure to potentially fraudulent & harmful messages. DMARC also provides a way for the email receiver to report back to the sender's domain about messages that pass and/or fail DMARC evaluation.

## Alignment

Separate to DKIM or SPF validation, DMARC can also require a message pass 'alignment'. For SPF, the message must PASS the SPF check, and the domain in the From: header must match the domain used to validate SPF (must exactly match for strict alignment, or must be a sub-domain for relaxed alignment). For DKIM, the message must be validly signed and the d= domain of the valid signature must align with the domain in the From: header (must exactly match for strict alignment, or must be a sub-domain for relaxed alignment). Under DMARC a message can fail even if it passes SPF or DKIM, but fails alignment.

## DNS

DMARC policies are published in the public Domain Name System (DNS) as text (TXT) resource records (RR) named \_dmarc.domain.com and announce what an email receiver should do with non-aligned mail it receives.

To ensure the sender trusts this process and knows the impact of publishing a policy different than p=none (monitor mode), the receiver sends daily aggregate reports indicating to the sender how many emails have been received and if these emails passed SPF and/or DKIM and were aligned.

DMARC may have a positive impact on deliverability for legitimate senders, at least Google recommends the use of DMARC for bulk email senders.

## Policies

DMARC policies are published by domain owners and applied by mail receivers to the messages that don't pass the alignment test. The domain being queried is the author domain, that is the domain to the right of @ in the From: header field. The policy can be one of none the so-called **monitor** mode, **quarantine** to treat the message with suspicion according to the receiver capabilities, or **reject** to reject the message outright. Reject policy is fine for domains that don't have individual human users, or for companies with firm staff policies that all mail goes through the company mail server, and employees don't join mailing lists and the like using company addresses, or the company provides a separate less strictly managed domain for its staff mail. Strict policies will never be appropriate for public webmail systems. Human use of a mail address may involve email forwarding from a dismissed address, and mailing lists, which are frequent causes of legitimate breakage of the original author's domain DKIM signature and therefore DMARC alignment. Various workarounds have been proposed to cope with domains that publish strict policies unwittingly.

## Setup

Setting up DMARC on a domain requires creation of sub-domains starting with an underscore. The full specification of how to do this is at [RFC7489](http://tools.ietf.org/html/rfc7489), and a summary is available at [Google Apps](https://support.google.com/a/answer/2466563?hl=en).

## Example

The simplest possible record is something like this. For the full list of fields, see the [General Record Format](https://tools.ietf.org/html/draft-kucherawy-dmarc-base-13#section-6.3) section of the RFC.

**`/var/bind/domain.tld.hosts`**

## Deployment

A restart or reload may be required to synchronize this new record to the secondary servers and propagated through the DNS system. Once the record is visible in the DNS system, it will begin to be used. Keep this in mind if testing fails, check the domains TXT record.

`user $``dig _dmarc.domain.tld txt`
Or, the same command using a specific DNS server.

`user $``dig @some.dns.server.tld _dmarc.domain.tld txt`
