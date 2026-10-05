<!-- source: https://wiki.gentoo.org/wiki/Vi | group: Gentoo Wiki (Main) | wiki-title: Vi -->
---
title: vi
url: https://wiki.gentoo.org/wiki/Vi
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-09-20"
categories: ['app-editors']
fingerprint: "7e2b82db63df3868"
license: CC BY-SA 4.0
---

# vi

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)


vi is a powerful *modal* [text-based editor](https://wiki.gentoo.org/wiki/Text_editor) with a long history in Unix and Unix-like operating systems. The original vi has served as a basis for many clones and derivatives over the years, notably [vim](https://wiki.gentoo.org/wiki/Vim).

A new Gentoo installation includes [nano](https://wiki.gentoo.org/wiki/Nano) by default - emerge a package from the following section to provide a vi-like editor. Beware that emerging a vi editor on a new installation may allow nano to be depcleaned - refer to the ["Default, fallback, and virtual packages" section](https://wiki.gentoo.org/wiki/Text_editor#Default.2C_fallback.2C_and_virtual_packages) in the "Text editor" article.

Although not part of [GNU Coreutils](https://wiki.gentoo.org/wiki/GNU_Coreutils), a vi-like editor is almost universally included in Linux distributions. vi has parentage with the [ed](https://wiki.gentoo.org/wiki/Ed) [line editor](https://en.wikipedia.org/wiki/Line_editor).

| Name | Package | Description | 
|---|---|---|
| levee | [app-editors/levee](https://packages.gentoo.org/packages/app-editors/levee) | Really tiny vi clone. | 
| [Neovim](https://wiki.gentoo.org/wiki/Neovim) | [app-editors/neovim](https://packages.gentoo.org/packages/app-editors/neovim) | Neovim is a hyperextensible Vim-based text editor. | 
| pyvim | [app-editors/pyvim](https://packages.gentoo.org/packages/app-editors/pyvim) | Implementation of Vim in Python. | 
| [Vim](https://wiki.gentoo.org/wiki/Vim) | [app-editors/vim](https://packages.gentoo.org/packages/app-editors/vim) | Modern vi(like) editor, probably the most used clone. Optional GUI called Gvim. | 
| vim-classic | [app-editors/vim-classic](https://packages.gentoo.org/packages/app-editors/vim-classic) | Fork of Vim 8.x with no AI usage in development | 
| vis | [app-editors/vis](https://packages.gentoo.org/packages/app-editors/vis) | A vi-like editor based on Plan 9's structural regular expressions. | 

More vi-like editors can be found online in the [app-editors](https://packages.gentoo.org/categories/app-editors) category or by running:

`user $``eix "app-editors/*"`
A vi-like editor can usually be launched with the vi command. Vim can be launched with the vim command. [Neovim](https://wiki.gentoo.org/wiki/Neovim) is launched with the nvim command.

If a vi-like editor is not installed, [busybox's vi](https://wiki.gentoo.org/wiki/Busybox#vi) may be available:

`user $``busybox vi -h````
BusyBox v1.34.1 (2021-11-23 09:49:04 CET) multi-call binary.
Usage: vi [-c CMD] [-R] [-H] [FILE]...
Edit FILE
        -c CMD  Initial command to run ($EXINIT also available)
        -R      Read-only
        -H      List available features
```
If [Vim](https://wiki.gentoo.org/wiki/Vim) is installed, the vi and vim commands become synonymous due to the following link:

`user $``ls -al /usr/bin/vi`
lrwxrwxrwx 1 root root 3 Nov 25 19:59 /usr/bin/vi -> vim

To manage this link, use eselect vi, provided by the [app-eselect/eselect-vi](https://packages.gentoo.org/packages/app-eselect/eselect-vi) package. eselect vi can currently select between vim, nvim, nvi, elvis, vile, gvim, qvim, xvile, pyvim, and busybox.

The synonymous use also holds when setting [editor defaults](https://wiki.gentoo.org/wiki/Text_editor#Setting_system_default).

- [Text editor](https://wiki.gentoo.org/wiki/Text_editor) — a program to create and edit text files.
- [Vim](https://wiki.gentoo.org/wiki/Vim) — a [vi]-like [text editor](https://wiki.gentoo.org/wiki/Text_editor), originally descended from the [Stevie](<https://en.wikipedia.org/wiki/Stevie_(text_editor)>) vi clone.
- [Vim/Guide](https://wiki.gentoo.org/wiki/Vim/Guide) — explain basic usage for users new to [vi-like](https://en.wikipedia.org/wiki/vi) text editors in general, and vim in particular.
