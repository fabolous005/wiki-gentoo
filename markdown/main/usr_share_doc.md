<!-- source: https://wiki.gentoo.org/wiki//usr/share/doc/ | group: Gentoo Wiki (Main) | wiki-title: /usr/share/doc/ -->
---
title: "/usr/share/doc/"
url: https://wiki.gentoo.org/wiki//usr/share/doc/
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2023-04-23"
fingerprint: "27fa9cbc4ba79ff4"
license: CC BY-SA 4.0
---

# /usr/share/doc/

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

The /usr/share/doc/ directory, on Gentoo systems, contains documentation that is bundled with each package.

Many of the files in /usr/share/doc/ are compressed, using [Bzip2](https://wiki.gentoo.org/wiki/Bzip2) by default. To view such a compressed text file easily, use less:

`user $``less /usr/share/doc/openrc-0.44.9/README.md.bz2`
This works because the less invocation is piped to lesspipe by default via the `LESSOPEN` shell variable.

## External resources

- [devmanual.gentoo.org](https://devmanual.gentoo.org/function-reference/install-functions) — Explains docinto dodoc dohtml einstalldocs DOCS and HTML\_DOCS
