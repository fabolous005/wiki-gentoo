<!-- source: https://wiki.gentoo.org/wiki/ClamAV_Unofficial_Signatures | group: Gentoo Wiki (Main) | wiki-title: ClamAV Unofficial Signatures -->
---
title: ClamAV Unofficial Signatures
url: https://wiki.gentoo.org/wiki/ClamAV_Unofficial_Signatures
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2020-10-23"
fingerprint: "2e35ad7e27b768e0"
license: CC BY-SA 4.0
---

# ClamAV Unofficial Signatures

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

There are two good approaches to using unofficial signatures on Gentoo (and elsewhere). The first is to use [app-antivirus/fangfrisch](https://packages.gentoo.org/packages/app-antivirus/fangfrisch), and the second is to use freshclam itself. The eXtremeSHOK clamav-unofficial-sigs script is **not** a secure option.

## Using freshclam

Freshclam now supports https URLs, so if your unofficial signatures are available direct from an http(s) URL, then adding them to freshclam is easy. For example,

There are only a few downsides to using freshclam:

1. Freshclam can't rename the downloaded file, so if the source file is incorrectly named, freshclam will fail to validate it (because clamav won't know how to read it).
2. Freshclam only supports http(s), so you're out of luck if your database is only served over rsync.
3. There's currently [a bug in freshclam](https://bugzilla.clamav.net/show_bug.cgi?id=12522) that causes it to validate malformed databases, which will crash clamav. So if there's a chance that you'll download a bad database, freshclam may not be the best choice (until that bug is fixed).
