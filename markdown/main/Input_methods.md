<!-- source: https://wiki.gentoo.org/wiki/Input_methods | group: Gentoo Wiki (Main) | wiki-title: Input methods -->
---
title: Input methods
url: https://wiki.gentoo.org/wiki/Input_methods
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2024-05-11"
fingerprint: "6a50841b233cd814"
license: CC BY-SA 4.0
---

# Input methods

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

An **input method** is a way to use any data as input, for example to use a standard Latin keyboard for writing Chinese characters. Gentoo has several input method frameworks available:

- [fcitx](https://wiki.gentoo.org/wiki/Fcitx) — supporting multiple languages and IMEs, mostly for Simplified Chinese, but also other scripts
- [IBus](https://wiki.gentoo.org/wiki/IBus) — supporting multiple languages and IMEs
- [SCIM](https://wiki.gentoo.org/wiki/SCIM) — supporting multiple languages and IMEs
- [uim](https://wiki.gentoo.org/wiki/Uim) — supporting multiple languages and IMEs

The [gentoo-zh](https://github.com/microcai/gentoo-zh) overlay includes some more packages (and not just for Chinese).

On [GTK](https://wiki.gentoo.org/wiki/GTK) based applications, the key sequence for hexadecimal Unicode input is `Ctrl`+`Shift`+`u`+`<hex digit>`. As an example, the unicode character ✔ which has unicode number [U+2714](http://unicode-table.com/en/2714/) can be written as `Ctrl`+`Shift`+`u`+`2714`+`ENTER`, being rendered as `✔`. [IBus](https://wiki.gentoo.org/wiki/IBus) is needed for support in other applications.
