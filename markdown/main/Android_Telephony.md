<!-- source: https://wiki.gentoo.org/wiki/Android/Telephony | group: Gentoo Wiki (Main) | wiki-title: Android/Telephony -->
---
title: Android/Telephony
url: https://wiki.gentoo.org/wiki/Android/Telephony
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2023-04-05"
fingerprint: "4791078cf18b7bd6"
license: CC BY-SA 4.0
---

# Android/Telephony

From Gentoo Wiki

\< [Android](https://wiki.gentoo.org/wiki/Android)

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

## Installation

### Emerge

FILE **`/etc/portage/sets/telephony`**

```
# Telephony Stack
net-misc/networkmanager
net-misc/ofono
net-voip/telepathy-rakia
```
`root #``emerge --ask @telephony`
