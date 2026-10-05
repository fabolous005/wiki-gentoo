<!-- source: https://wiki.gentoo.org/wiki/Knowledge_Base:Edit_a_config_file | group: Gentoo Knowledge | wiki-title: Knowledge_Base:Edit_a_config_file -->
---
title: Knowledge Base:Edit a configuration file
url: https://wiki.gentoo.org/wiki/Knowledge_Base:Edit_a_configuration_file
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2022-03-30"
fingerprint: "941cf87b2837bb81"
license: CC BY-SA 4.0
---

# Knowledge Base:Edit a configuration file

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

## Synopsis

This document is a step by step guide to edit a configuration file.

## Environment

All the time.

## Analysis

Configuration files are the default method to configure most software. Use a [text editor](https://wiki.gentoo.org/wiki/Text_editor) to edit them. The default text editor in Gentoo is [nano](https://wiki.gentoo.org/wiki/Nano).

## Resolution

Different text editors may be used on the command line to edit configuration files. Nano will be installed by default on a new Gentoo installation, but it is ultimately up to the user what editor to install and use. See the [text editor](https://wiki.gentoo.org/wiki/Text_editor) article for details on selecting and installing a text editor.

Nano will probably be the most accessible editor for beginners, because it behaves a little like many GUI editors, and provides a help bar at the bottom of the screen by default.

The [sudo](https://wiki.gentoo.org/wiki/Sudo) command may be needed to edit system configuration files owned by root.

### Emacs

Start [Emacs](https://wiki.gentoo.org/wiki/Emacs) by typing emacs in the terminal, followed by file path and name:

`user $``emacs <filename>`
Move around with the arrow keys, insert text freely. To save a file, type `C-x C-s` (`Ctrl`+`x` followed by `Ctrl`+`s`).

To close Emacs, type `C-x C-c` (`Ctrl`+`x` followed by `Ctrl`+`c`).

For "quick-start" documentation directly in Emacs, type: `C-h t` (`Ctrl`+`h` followed by `t`). For further help on how to use Emacs, type `C-h r` (`Ctrl`+`h` followed by `r`).

For more information online:

### Nano

Start [Nano](https://wiki.gentoo.org/wiki/Nano) by typing nano in the terminal, followed by file path and name:

`user $``nano <filename>`
Nano now shows the content of the text file. Navigate through the text with the arrow keys and make edits as desired.

At the bottom, Nano shows shortcuts for common actions (save, exit, etc.). The shortcut to save a file is shown as "^O". Users must prefix the shortcut with the `Ctrl` key, so to save a file press `Ctrl`+`O`. Nano will create the file upon saving, if no file with the given path exists yet.

To exit press `Ctrl`+`X`.

### sed

[sed](https://wiki.gentoo.org/wiki/Sed) can be used for more complex edits and scripted changes.  As an example, the following command would remove all lines containing the string **foobar** from [/etc/portage/package.accept\_keywords](https://wiki.gentoo.org/wiki//etc/portage/package.accept_keywords):

`root #``sed -e '/foobar/d' -i /etc/portage/package.accept_keywords`
### Vim

Start [Vim](https://wiki.gentoo.org/wiki/Vim) by typing vim in the terminal, followed by file path and name (using the vi command should also be possible):

`user $``vim <filename>`
Vim will create the file upon saving, if no such file exists yet.

Vim is a *modal* editor, it will start in command mode, and can be switched between this and insert mode. Use `h`, `j`, `k`, `l` (by default) to move the cursor in command mode. Enter insert mode, to enter text, by pressing `i`, and return to command mode by pressing `Esc`. Save by opening the Vim command line, by pressing `:`, then `w`, `Enter`. Quit with `:`, `q`, `Enter`. Add `!` after `q` to quit without saving changes.

See [Vim/Guide](https://wiki.gentoo.org/wiki/Vim/Guide) for more instructions on using Vim. See [vi](https://wiki.gentoo.org/wiki/Vi) about vi-like editors. Of course, check out the [Vim](https://wiki.gentoo.org/wiki/Vim) article.

## See also

- [Backup](https://wiki.gentoo.org/wiki/Backup) — prevent loss of data by ensuring it can be recovered.
- [Etckeeper](https://wiki.gentoo.org/wiki/Etckeeper) — a collection of tools to store /etc in a VCS (version control) repository, to keep a backup of changes to system configuration files
- [Text editor](https://wiki.gentoo.org/wiki/Text_editor) — a program to create and edit text files.
