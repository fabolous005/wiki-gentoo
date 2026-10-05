<!-- source: https://wiki.gentoo.org/wiki/ReText | group: Gentoo Wiki (Main) | wiki-title: ReText -->
---
title: ReText
url: https://wiki.gentoo.org/wiki/ReText
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2024-01-22"
fingerprint: "3a237898be1d55f2"
license: CC BY-SA 4.0
---

# ReText

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**ReText** is a Linux ready, simple (yet powerful) text editor for [Markdown](https://en.wikipedia.org/wiki/Markdown) and reStructured text. ReText includes a built in "Live preview" function so that writers can see their changes in real-time.

## Installation

### USE flags


### Emerge

Emerge ReText just like any standard program:

`root #``emerge --ask app-editors/retext`
## Troubleshooting

### No icons in toolbar

If the icons are missing the icon theme needs to be specified in gconf or in the ReText configuration file.

**`~/.config/ReText project/ReText.conf`**

Available icon themes are located in /usr/share/icons/ (hicolor will not work).
