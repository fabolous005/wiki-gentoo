<!-- source: https://wiki.gentoo.org/wiki/WKD | group: Gentoo Wiki (Main) | wiki-title: WKD -->
---
title: WKD
url: https://wiki.gentoo.org/wiki/WKD
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2025-12-30"
fingerprint: "66b993d734d5bff5"
license: CC BY-SA 4.0
---

# WKD

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**Web Key Directory (WKD)** is one of mechanisms for distributing [GnuPG](https://wiki.gentoo.org/wiki/GnuPG) keys. WKD works by querying a *.well-known* directory on a domain, extracted from an email address, and obtains the keys from there. It avoids reliance on a centralized keyserver.

## Use in Gentoo

[Gemato](https://wiki.gentoo.org/wiki/Gemato), which is used for repository verification by Portage, uses WKD as one method for refreshing keys safely.

### Using Web Key Directory on gentoo.org

The public key from each Gentoo developer can be fetched via the command line:

`user $``gpg --auto-key-locate clear,nodefault,wkd --locate-keys [devname]@gentoo.org`
## See also

- [GnuPG](https://wiki.gentoo.org/wiki/GnuPG) — a free implementation of the OpenPGP standard (RFC 4880).

## External resources

- Test WKD providers online [https://metacode.biz/openpgp/web-key-directory](https://metacode.biz/openpgp/web-key-directory)
- [https://wiki.gnupg.org/WKD](https://wiki.gnupg.org/WKD)
- [https://www.gentoo.org/news/2019/07/03/sks-key-poisoning.html](https://www.gentoo.org/news/2019/07/03/sks-key-poisoning.html)
- Draft of the IETF WKS standard [https://datatracker.ietf.org/doc/html/draft-koch-openpgp-webkey-service-10](https://datatracker.ietf.org/doc/html/draft-koch-openpgp-webkey-service-10)
- [eix-sync: Refreshing keys via WKD always fails](https://forums.gentoo.org/viewtopic-t-1107842.html)
