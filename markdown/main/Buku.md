<!-- source: https://wiki.gentoo.org/wiki/Buku | group: Gentoo Wiki (Main) | wiki-title: Buku -->
---
title: Buku
url: https://wiki.gentoo.org/wiki/Buku
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2024-03-08"
fingerprint: e31510d6f1e27a77
license: CC BY-SA 4.0
---

# Buku

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

buku is a command-line manager for your bookmark, which can be seen as a *personal textual mini-web* according to the author. It allows to look for tags or keywords easily and can help you manage large amount of bookmarks. Data is stored on a local database but can easily be exported to the text format.

## Installation

### USE flags


### Emerge

`root #``emerge --ask www-misc/buku`
### Files

- \~/.local/share/buku/bookmarks.bd - Local SQLite database for the bookmarks.

## Usage

Add a link with tags:

Add a link with a custom title:

Search for keyword:

`user $``buku -s context`
Open a link, where 1 is the number of the result

`user $``buku -s context``user $``1`
Edit a bookmark in your editor:

`user $``buku -w 320`
Update a title (requires to know the bookmark id) :

`user $``buku -u 320 --title "Nouveau titre"`
Search by tag:

`user $``buku neovim`
Export to markdown and save to git

`user $``cd ~/.local/share/buku && buku -e bookmarks.md && git add bookmarks.md && git commit -am "Save" && git push`
## Removal

### Unmerge

`root #``emerge --ask --depclean --verbose www-misc/buku`
