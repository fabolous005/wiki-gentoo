<!-- source: https://wiki.gentoo.org/wiki/LibreSSL | group: Gentoo Wiki (Main) | wiki-title: LibreSSL -->
---
title: LibreSSL
url: https://wiki.gentoo.org/wiki/LibreSSL
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2022-05-14"
fingerprint: f3f9ae1fb5311a6a
license: CC BY-SA 4.0
---

# LibreSSL

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

[LibreSSL](http://www.libressl.org/) is a fork of, and drop-in replacement for [OpenSSL](https://openssl.org/).  It was originally a response to the infamous [heartbleed](http://heartbleed.com/) vulnerability, which was a serious security flaw in one of the most popular SSL providers in use. So, some OpenBSD developers decided that they would fork in order to "modernize the codebase, improve security, and apply best practice development processes." While LibreSSL has not been without its own vulnerabilities (see below), some of which it shared with OpenSSL, there has been improvements with its emphasis on security.  For example, the LibreSSL team's response to the [POODLE](https://en.wikipedia.org/wiki/POODLE) vulnerability was to disable the use of SSL 3.0 by default.

LibreSSL is not without its criticisms, however. Despite the fact that LibreSSL has as a stated goal complete API compatibility with OpenSSL, as a fork, its ABI may diverge over time, creating subtle incompatibility as a simple drop-in replacement. This is worse than some package simply not build against LibreSSL. Rather, a package built against OpenSSL may later link and run against LibreSSL, should one library be replaced with the other, but that package may misbehave. The size of some struct be off by a few bytes, or the parameters of some function might not align in the same way. This in turn can lead to unpredictable behavior, crashes or worse, exploits. So LibreSSL has been criticized for using the same library file name and even the same SONAME as OpenSSL.
