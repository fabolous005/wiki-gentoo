<!-- source: https://wiki.gentoo.org/wiki/Notmuch/aerc | group: Gentoo Wiki (Main) | wiki-title: Notmuch/aerc -->
---
title: Notmuch/aerc
url: https://wiki.gentoo.org/wiki/Notmuch/aerc
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2025-07-26"
fingerprint: "6fd1879cf3723ce3"
license: CC BY-SA 4.0
---

# Notmuch/aerc

From Gentoo Wiki

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

## Setup

To setup [aerc](https://wiki.gentoo.org/wiki/Aerc) to use Notmuch, ensure it is built with support first:

`user $``equery u aerc`
\[ Legend : U - final flag setting for installation\]
\[        : I - package is installed with flag     \]
\[ Colors : set, unset                             \]
 \* Found these USE flags for mail-client/aerc-0.20.1:
 U I
 + + notmuch : Enable support for net-mail/notmuch

## Configuration

Next, ensure the `accounts.conf` to point to Notmuch:

FILE **`/home/larry/.config/aerc/accounts.conf`**
