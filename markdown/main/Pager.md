<!-- source: https://wiki.gentoo.org/wiki/Pager | group: Gentoo Wiki (Main) | wiki-title: Pager -->
---
title: Pager
url: https://wiki.gentoo.org/wiki/Pager
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-06-06"
fingerprint: fbcb537d3207fec8
license: CC BY-SA 4.0
---

# Pager

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

A pager is a tool for displaying the contents of files or other output on the terminal, in a user friendly way, across several screens if needed.

## Available software

This is just a partial selection of pagers available in Gentoo.

| Name | Package | Description | 
|---|---|---|
| Bat | [sys-apps/bat](https://packages.gentoo.org/packages/sys-apps/bat) | cat(1) clone with syntax highlighting and Git integration. | 
| [Less](https://wiki.gentoo.org/wiki/Less) | [sys-apps/less](https://packages.gentoo.org/packages/sys-apps/less) | Free, open-source file pager, almost ubiquitous on Linux. Included in the [@system set](<https://wiki.gentoo.org/wiki/System_set_(Portage)>). | 
| [More](https://wiki.gentoo.org/wiki/Util-linux) | [sys-apps/util-linux](https://packages.gentoo.org/packages/sys-apps/util-linux) | Basic pager, part or [util-linux](https://wiki.gentoo.org/wiki/Util-linux) included in the [@system set](<https://wiki.gentoo.org/wiki/System_set_(Portage)>). | 
| Most | [sys-apps/most](https://packages.gentoo.org/packages/sys-apps/most) | Paging program that displays, one windowful at a time, the contents of a file. | 
| Nvimpager | [app-editors/neovim](https://packages.gentoo.org/packages/app-editors/neovim) | Use nvim as a pager to view manpages, diffs, etc with nvim's syntax highlighting. | 
| w3m | [virtual/w3m](https://packages.gentoo.org/packages/virtual/w3m) | It is a pager and a terminal web-browser with optional configuration. A better substitute for \`less\` and actually a quite good web browser, if correctly configure. Exceptionally good for man pages, since it supports going to next man pages and has tabs, history etc. Also, it is possible to get rendered text into your EDITOR. | 

Also consider emerging [app-editors/vim](https://packages.gentoo.org/packages/app-editors/vim) with `USE=vim-pager`.

## Default pager

The default pager can be configured by setting the `PAGER` [environment variable](https://wiki.gentoo.org/wiki/Handbook:Parts/Working/EnvVar), and similarly, the default [manpager](https://wiki.gentoo.org/wiki/Man_page#Pager) can be configured by setting the `MANPAGER` environment variable:

**`~/.bashrc`**

**Set PAGER and MANPAGER variable**

```
PAGER="less"
MANPAGER="less"
```
The default pager can be modified using the [eselect](https://wiki.gentoo.org/wiki/Eselect#Pager) command.

## See also

- [Hex editor](https://wiki.gentoo.org/wiki/Hex_editor) — an application to allow viewing and editing of [binary files](https://en.wikipedia.org/wiki/Binary_file), as opposed to [text files](https://wiki.gentoo.org/wiki/Text_editor).
- [Terminal emulator](https://wiki.gentoo.org/wiki/Terminal_emulator) — emulates a video terminal within another display architecture (e.g. in [X](https://wiki.gentoo.org/wiki/X_server)).
- [Text editor](https://wiki.gentoo.org/wiki/Text_editor) — a program to create and edit text files.
