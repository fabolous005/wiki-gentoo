<!-- source: https://wiki.gentoo.org/wiki/Ncmpcpp | group: Gentoo Wiki (Main) | wiki-title: Ncmpcpp -->
---
title: ncmpcpp
url: https://wiki.gentoo.org/wiki/Ncmpcpp
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-02-17"
fingerprint: "1fcd561c736a7dc3"
license: CC BY-SA 4.0
---

# ncmpcpp

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)


ncmpcpp (**N**Curses **M**usic **P**layer **C**lient **P**lus **P**lus) is a [sys-libs/ncurses](https://packages.gentoo.org/packages/sys-libs/ncurses) based [MPD](https://wiki.gentoo.org/wiki/MPD) (**M**usic **P**layer **D**aemon) client similar to ncmpc, with some new and improved features.

## Installation

### USE flags


### Emerge

`root #``emerge --ask media-sound/ncmpcpp`
## Configuration

### Local

After installation the user should make a .ncmpcpp directory within their specific /home directory. This is where ncmpcpp will read the configuration file and output any error messages.

`user $``mkdir ~/.ncmpcpp && touch ~/.ncmpcpp/config`
Below is an example of a local user's ncmpcpp configuration file:

**`~/.ncmpcpp/config`**

```
ncmpcpp_directory =         "~/.ncmpcpp"
mpd_host =                  "localhost"
mpd_port =                  "6600"
mpd_music_dir =	            "/var/lib/mpd/music/"
```
## See also

- [MPD](https://wiki.gentoo.org/wiki/MPD) - Documentation on MPD, a necessary ncmpcpp component.
