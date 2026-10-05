<!-- source: https://wiki.gentoo.org/wiki/Piper | group: Gentoo Wiki (Main) | wiki-title: Piper -->
---
title: Piper
url: https://wiki.gentoo.org/wiki/Piper
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2023-12-28"
fingerprint: "975748c27733783f"
license: CC BY-SA 4.0
---

# Piper

From Gentoo Wiki

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)



Piper is a GTK+ application to configure gaming mice. Piper is merely a graphical frontend to the ratbagd DBus daemon.

## Installation

### USE flags


### Emerge

`root #``emerge --ask app-misc/piper`
For non root access to [ratbagd](https://wiki.gentoo.org/wiki/Ratbagd) user should be in group `plugdev`

`root #``usermod -aG plugdev larry`
