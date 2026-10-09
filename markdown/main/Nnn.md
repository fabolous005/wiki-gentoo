<!-- source: https://wiki.gentoo.org/wiki/Nnn | group: Gentoo Wiki (Main) | wiki-title: Nnn -->
---
title: Nnn
url: https://wiki.gentoo.org/wiki/Nnn
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-10-08"
fingerprint: "4ee6d318898e78c2"
license: CC BY-SA 4.0
---

# Nnn

From Gentoo Wiki

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

[**nnn**](https://github.com/jarun/nnn) is a small, minimalistic and lightweight terminal based file manager written in C. It is very tiny, and the file size is around **\~150** KiB.

## Installation

Preferably, you can just emerge **nnn**.

## Emerge

`root #``emerge --ask app-misc/nnn`
## USE Flags


### USE flags for
            [app-misc/nnn](https://packages.gentoo.org/packages/app-misc/nnn)
            
            The missing terminal file browser for X

| [+readline](https://packages.gentoo.org/useflags/+readline) | Enable support for libreadline, a GNU line-editing library that almost everyone wants | 
| [colemak](https://packages.gentoo.org/useflags/colemak) | Key bindings for Colemak keyboard layout | 
| [emoji](https://packages.gentoo.org/useflags/emoji) | Display icons using emoji | 
| [gitstatus](https://packages.gentoo.org/useflags/gitstatus) | Add git status column to the detail view | 
| [icons](https://packages.gentoo.org/useflags/icons) | Display icons using icons-in-terminal | 
| [namefirst](https://packages.gentoo.org/useflags/namefirst) | Print filenames first in the detail view | 
| [nerdfonts](https://packages.gentoo.org/useflags/nerdfonts) | Display icons using nerdfonts icons | 
| [pcre](https://packages.gentoo.org/useflags/pcre) | Add support for Perl Compatible Regular Expressions | 
| [qsort](https://packages.gentoo.org/useflags/qsort) | Use Alexey Tourbin's quick sort implementation | 
| [restorepreview](https://packages.gentoo.org/useflags/restorepreview) | Add pipe to close and restore preview-tui for internal undetached edits | 

## Icons

FILE **`/etc/portage/package.use/nnn`**

```
app-misc/nnn icons
```
## Usage

To use **nnn**, simply run it in your terminal of choice!

`user $``nnn`
## Plugins

To install **all** plugins on **nnn**, you can run their command.

`user $``sh -c "$(curl -Ls` [https://raw.githubusercontent.com/jarun/nnn/master/plugins/getplugs](https://raw.githubusercontent.com/jarun/nnn/master/plugins/getplugs))"
If you already have plugins installed, this command will attempt to just update them to their latest version.

Plugins are usually installed in the **\~/.config** directory. To be exact, **\~/.config/nnn/plugins**.

## Troubleshooting

You can view **nnn'**s [GitHub](https://github.com/jarun/nnn/wiki/Troubleshooting) page to learn more about troubleshooting.
