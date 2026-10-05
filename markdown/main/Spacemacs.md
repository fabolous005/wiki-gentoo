<!-- source: https://wiki.gentoo.org/wiki/Spacemacs | group: Gentoo Wiki (Main) | wiki-title: Spacemacs -->
---
title: Spacemacs
url: https://wiki.gentoo.org/wiki/Spacemacs
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2022-10-03"
fingerprint: f22191792ea3e3e9
license: CC BY-SA 4.0
---

# Spacemacs

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**Spacemacs** is a sophisticated and polished Emacs set-up focused on ergonomics, mnemonics and consistency.

## Installation

Spacemacs is basically a distribution for Emacs packages - it configures and combines them for a great out-of-box experience.

The installation is done by cloning the Spacemacs configuration files git repository to \~/.emacs.d/

First, install [GNU Emacs](https://wiki.gentoo.org/wiki/GNU_Emacs) with the correct USE flags:

### USE flags

Ensure Emacs is built with the `xft` USE flag[\[1\]](https://wiki.gentoo.org#cite_note-1)<sup>[\[2\]](https://wiki.gentoo.org#cite_note-2)</sup>:

**`/etc/portage/package.use`**

### Emerge Emacs

Install Emacs, or reinstall if USE flags have been changed:

`root #``emerge --ask app-editors/emacs`
### Download and install Spacemacs

Spacemacs is installed by cloning a git repository containing Emacs configuration.

First, backup or delete any old configuration files \~/.emacs.d and \~/.emacs, if they already exist:

`user $``mv ~/.emacs.d ~/.emacs.d.bak-$(date +%FT%T)``user $``mv ~/.emacs ~/.emacs.bak-$(date +%FT%T)`
Now that old configs are out of the way, clone the Spacemacs git repository into \~/.emacs.d:

`user $``git clone https://github.com/syl20bnr/spacemacs ~/.emacs.d`
To finish installing Spacemacs, start Emacs (via menu, a [launcher](https://wiki.gentoo.org/wiki/Recommended_applications#Application_launchers) or terminal) and follow the install prompt at the bottom of the screen:

`user $``emacs`
## Configuration file

Custom configuration and features can be set in the file \~/.spacemacs, written in [Elisp](https://wiki.gentoo.org/index.php?title=Elisp&action=edit&redlink=1).

To directly open this file in Spacemacs, press the keys `ESC` -> `SPACE f e d`.

As Emacs (and Spacemacs) is self-documenting, learn about possible configuration options by pressing `SPC h SPC`.

## Usage

### Invocation

Because Spacemacs replaces the main Emacs configuration, by default, simply launch Emacs to open Spacemacs:

`user $``emacs`
## See also

- [Emacs](https://wiki.gentoo.org/wiki/Emacs) — a class of powerful, extensible, self-documenting text editors.
- [Knowledge Base:Edit a configuration file](https://wiki.gentoo.org/wiki/Knowledge_Base:Edit_a_configuration_file)
- [Text editor](https://wiki.gentoo.org/wiki/Text_editor) — a program to create and edit text files.

## References

1. [↑](https://wiki.gentoo.org#cite_ref-1) Konstantinos Tsardounis. [Unable to use Source Code Pro fonts?](https://github.com/syl20bnr/spacemacs/issues/10162), [Spacemacs GutHub](https://github.com/syl20bnr/spacemacs), January 16th, 2018. Retrieved on March 19th, 2019.
2. [↑](https://wiki.gentoo.org#cite_ref-2) Wiki authors. [Xft support for GNU Emacs](https://wiki.gentoo.org/wiki/Xft_support_for_GNU_Emacs), [Gentoo Wiki](https://wiki.gentoo.org/wiki/Main_Page), July 11th, 2013. Retrieved on March 19th, 2019.
