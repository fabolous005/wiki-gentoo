<!-- source: https://wiki.gentoo.org/wiki/Google_Summer_of_Code/2019/Ideas/Improve_OpenPGP_support_for_bugzilla | group: Gentoo Wiki (Main) | wiki-title: Google Summer of Code/2019/Ideas/Improve OpenPGP support for bugzilla -->
---
title: Google Summer of Code/2019/Ideas/Improve OpenPGP support for bugzilla
url: https://wiki.gentoo.org/wiki/Google_Summer_of_Code/2019/Ideas/Improve_OpenPGP_support_for_bugzilla
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2019-02-05"
fingerprint: "663288dccfea05d8"
license: CC BY-SA 4.0
---

# Google Summer of Code/2019/Ideas/Improve OpenPGP support for bugzilla

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

Currently the OpenPGP bugzilla support is defunct in at least three ways:

1. It encrypts to the first public key it considers viable<sup>[\[1\]](https://wiki.gentoo.org#cite_note-1)</sup>, not respecting [usage flags](http://tools.ietf.org/html/rfc4880#section-5.2.3.21), leading to scenarios where the message is un-decryptable.
2. There is no mechanism for refreshing public keys from known public sources (e.g HKP keyservers) leading to a situation where subkey rotation or changers to primary certificate (e.g due to expiry or revocation) is not picked up automatically and needs to be manually adjusted, failure to do so can lead to encryption to a known non-viable certificate.
3. There is no group definition where multiple public keys can be assigned e.g to an alias account (security@) in bugzilla.

Having support for OpenPGP is necessary to retain confidentiality of restricted bugs in bugzilla, a lack of this results in information leakage. Alternatively, bug emails for group restricted bugs should not include metadata or data that can identify the issue, but merely report e.g "bug XXX has been updated, please log in to see the changes"

More details and proposed approaches are discussed [here](https://bugs.gentoo.org/624262).



| Contacts | Required Skills | 
|---|---|
|  |  |
