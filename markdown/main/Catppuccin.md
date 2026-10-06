<!-- source: https://wiki.gentoo.org/wiki/Catppuccin | group: Gentoo Wiki (Main) | wiki-title: Catppuccin -->
---
title: catppuccin
url: https://wiki.gentoo.org/wiki/Catppuccin
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2024-08-23"
fingerprint: ab95976e8f2ffbee
license: CC BY-SA 4.0
---

# catppuccin

From Gentoo Wiki

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)


**catppuccin** is a soothing pastel theme.

## Installation

### Emerge

Catppuccin is in the **GURU** repository.

First, enable the repository:

`root #``eselect repository enable guru`
Sync the repository:

`root #``emerge --sync guru`
Finally, emerge **catppuccin-gtk**:

`root #``emerge --ask x11-themes/catppuccin-gtk`
A temporary workaround for now due to  problems with 1.0.3 is to emerge **=catppuccin-gtk-0.7.5**.

For icons, emerge **tela-icon-theme**:

`root #``emerge --ask x11-themes/tela-icon-theme`
## Kvantum

For a qt theme, install **catppuccin-kvantum**.

`root #``emerge --ask x11-themes/catppuccin-kvantum`
Then, edit your **.profile**:

FILE **`~/.profile`**

```
export QT_STYLE_OVERRIDE=kvantum
```
