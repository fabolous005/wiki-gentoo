<!-- source: https://wiki.gentoo.org/wiki/Text_editor | group: Gentoo Wiki (Main) | wiki-title: Text editor -->
---
title: Text editor
url: https://wiki.gentoo.org/wiki/Text_editor
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-08-19"
categories: ['app-editors']
fingerprint: "7e2f529b231f3c48"
license: CC BY-SA 4.0
---

# Text editor

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

A **text editor** is a program to create and edit text files. Although it is not impossible to edit files without using one, text editors make it easy, and are handy for editing configuration files.

The Gentoo [@system](<https://wiki.gentoo.org/wiki/System_set_(Portage)>) set contains the [virtual/editor](https://packages.gentoo.org/packages/virtual/editor) package to make sure at least one editor is installed.

## Default, fallback, and virtual packages

As with most things Gentoo, text editor choice is up to the user. As a text editor will be necessary during and just after installation, the [virtual package](https://wiki.gentoo.org/wiki/Virtual_packages), [virtual/editor](https://packages.gentoo.org/packages/virtual/editor) (part of the [system set](<https://wiki.gentoo.org/wiki/System_set_(Portage)>)), will pull in [app-editors/nano](https://packages.gentoo.org/packages/app-editors/nano)<sup>[\[1\]](https://wiki.gentoo.org#cite_note-1)</sup> (as the first "any of many" dependency of the ebuild) as a *fallback* - until another ["virtual/editor" package](https://packages.gentoo.org/packages/virtual/editor/dependencies) is emerged.

Thus, after a [stage 3](https://wiki.gentoo.org/wiki/Stage_file#Stage_3) installation, the nano command will be available once [chrooted](https://wiki.gentoo.org/wiki/Handbook:AMD64/Installation/Base#Entering_the_new_environment) to a newly installed Gentoo. As the stage 3 files only contain packages that are strictly necessary for every system, Nano will be the only text editor available in the stage 3 chroot. A replacement editor may be emerged on the new system, as soon as the [Gentoo ebuild repository](https://wiki.gentoo.org/wiki/Ebuild_repository#The_Gentoo_ebuild_repository) is [installed and optionally updated](https://wiki.gentoo.org/wiki/Handbook:AMD64/Installation/Base#Optional:_Updating_the_Gentoo_ebuild_repository).

The *default* editors for the CLI will be used by many programs to determine which text editor to start up, when needed. Programs such as CLI file managers will use this default, or when invoking an editor from bash using `Ctrl`+`x Ctrl`+`e`. The default editors are set using the `VISUAL` and `EDITOR` [environment variables](https://wiki.gentoo.org/wiki/Handbook:AMD64/Working/EnvVar). Generally,  `VISUAL` will take precedence over `EDITOR`, which is used for less capable terminals.

See the [setting system default](https://wiki.gentoo.org/wiki/Text_editor#Setting_system_default) section.

## Available software

Text editor options can be found online in the [app-editors](https://packages.gentoo.org/categories/app-editors) category or by running:

`user $``eix "app-editors/*"`
### CLI editors

| Name | Package | Skill level | Features | Description | 
|---|---|---|---|---|
| [Emacs](https://wiki.gentoo.org/wiki/Emacs) | [app-editors/emacs](https://packages.gentoo.org/packages/app-editors/emacs) | Advanced | Huge | Class of powerful, extensible, self-documenting text editors. | 
| [Helix](https://wiki.gentoo.org/wiki/Helix) | [app-editors/helix](https://packages.gentoo.org/packages/app-editors/helix) | Medium | Advanced | Modal text editor with built-in LSP support, tree-sitter syntax highlighting, and minimal configuration. | 
| [Kakoune](https://wiki.gentoo.org/wiki/Kakoune) | [app-editors/kakoune](https://packages.gentoo.org/packages/app-editors/kakoune) | Medium | Advanced | Modern, actively developed editor for the command line, inspired by vi. | 
| Micro | [app-editors/micro](https://packages.gentoo.org/packages/app-editors/micro) | Easy | Advanced | Modern and intuitive terminal-based text editor. Still in testing branch as of 2022-11. | 
| [Nano](https://wiki.gentoo.org/wiki/Nano) | [app-editors/nano](https://packages.gentoo.org/packages/app-editors/nano) | Easy | Advanced | Easy to use text editor. | 
| [Neovim](https://wiki.gentoo.org/wiki/Neovim) | [app-editors/neovim](https://packages.gentoo.org/packages/app-editors/neovim) | Advanced | Huge | Vim fork focused on extensibility and agility. | 
| [Vim](https://wiki.gentoo.org/wiki/Vim) | [app-editors/vim](https://packages.gentoo.org/packages/app-editors/vim) | Advanced | Huge | Text editor based on the vi text editor. | 

See the [vi](https://wiki.gentoo.org/wiki/Vi) article for more vi(like) editors.

### GUI editors

| Name | Package | Description | 
|---|---|---|
| [Emacs](https://wiki.gentoo.org/wiki/Emacs) | [app-editors/emacs](https://packages.gentoo.org/packages/app-editors/emacs) | Class of powerful, extensible, self-documenting text editors. | 
| [FeatherPad](https://en.wikipedia.org/wiki/Featherpad) | [app-editors/featherpad](https://packages.gentoo.org/packages/app-editors/featherpad) | Lightweight Qt5 Plain-Text Editor for Linux. | 
| [Gedit](https://wiki.gentoo.org/wiki/Gedit) | [app-editors/gedit](https://packages.gentoo.org/packages/app-editors/gedit) | Text editor for the GNOME desktop. | 
| [GVim](https://wiki.gentoo.org/wiki/Vim#Gvim) | [app-editors/gvim](https://packages.gentoo.org/packages/app-editors/gvim) | GUI-based version of the vi text editor. | 
| [Leafpad](https://wiki.gentoo.org/wiki/Leafpad) | [app-editors/leafpad](https://packages.gentoo.org/packages/app-editors/leafpad) | Simple GTK2 text editor | 
| [jEdit](https://en.wikipedia.org/wiki/Jedit) | [app-editors/jedit](https://packages.gentoo.org/packages/app-editors/jedit) | jEdit is a programmer's text editor written in Java. | 
| [Kate](<https://en.wikipedia.org/wiki/Kate_(text_editor)>) | [kde-apps/kate](https://packages.gentoo.org/packages/kde-apps/kate) | KDE text editor. Development oriented. | 
| [Mousepad](<https://en.wikipedia.org/wiki/Mousepad_(software)>) | [app-editors/mousepad](https://packages.gentoo.org/packages/app-editors/mousepad) | Bare-bones text editor for Xfce that starts up extremely quickly. | 
| [NEdit](https://en.wikipedia.org/wiki/NEdit) | [app-editors/nedit](https://packages.gentoo.org/packages/app-editors/nedit) | Motif-based editor for X11. | 
| [Pluma](<https://en.wikipedia.org/wiki/Pluma_(text_editor)>) | [app-editors/pluma](https://packages.gentoo.org/packages/app-editors/pluma) | A fork of Gedit 2 by MATE. Small and lightweight UTF-8 text editor for the MATE environment. | 
| [SciTE](https://en.wikipedia.org/wiki/Scite) | [app-editors/scite](https://packages.gentoo.org/packages/app-editors/scite) | Very powerful editor for programmers. Oriented towards source editing. | 
| [Sublime Text](https://en.wikipedia.org/wiki/Sublime_Text) | [app-editors/sublime-text](https://packages.gentoo.org/packages/app-editors/sublime-text) | Editor for code, markup and prose. | 
| [VSCode](https://wiki.gentoo.org/wiki/Vscode) | [app-editors/vscode](https://packages.gentoo.org/packages/app-editors/vscode) | Highly extensible, electron-based text editor from Microsoft. | 
| [VSCodium](https://wiki.gentoo.org/wiki/Vscode) | [app-editors/vscodium](https://packages.gentoo.org/packages/app-editors/vscodium) | Free/Libre Open Source Software Binaries of Microsoft's VSCode. | 
| [Zed](<https://en.wikipedia.org/wiki/Zed_(text_editor)>) | [app-editors/zed](https://packages.gentoo.org/packages/app-editors/zed) | Fast text editor with collaboration features written in Rust. | 

### Visudo editor

Due to the sensitive nature of /etc/sudoers it may only edited via the visudo command which in turn is limited to a predefined selection of editors. Type man visudo for more information.

The `EDITOR` [environment variable](https://wiki.gentoo.org/wiki/Handbook:AMD64/Working/EnvVar) defines the default text editor for the CLI interface. Gentoo provides methods for setting environment variables globally in /etc/env.d/. Users may override these defaults for a running shell, or in their shell configuration files.

The old method of setting the `EDITOR` variable in /etc/rc.conf is no longer supported. See [this](https://wiki.gentoo.org/wiki/OpenRC/Baselayout_1_to_2_migration#EDITOR_and_PAGER) article for details.

### Setup with eselect

The default editor can be set with the eselect utility, which will automatically modify /etc/env.d/99editor to set the `EDITOR` environment variable.

To list editors that are installed and available to be set with eselect:

`root #``eselect editor list`
Available targets for the EDITOR variable:
  \[1\]   /bin/nano
  \[2\]   /bin/ed
  \[3\]   /usr/bin/emacs
  \[4\]   /usr/bin/ex
  \[5\]   /usr/bin/vi
  \[ \]   (free form)

If using Vim or Neovim, select vi, then see the [this article](https://wiki.gentoo.org/wiki/Vi#.2Fusr.2Fbin.2Fvi_symlink).

To set a new editor, replace `<NUMBER>` in the following command with a number corresponding to the text editor of choice:

`root #``eselect editor set <NUMBER>`
Next, the current environment must be updated by running the following command (for bash compatible shells):

`root #``. /etc/profile`
The `EDITOR` environment variable should now be set to the new value for the current shell.

### Manual setup

A system wide default text editor can be defined in the /etc/env.d/99editor file (this file is not present on a new installation, but can be created), for example:

**`/etc/env.d/99editor`**

**System wide text editor default**

```
EDITOR="/usr/bin/vim"
```
After editing this file, run env-update to update the environment files:

`root #``env-update`
Finally, update the current environment (for bash compatible shells):

`root #``. /etc/profile`
## Caveats

### Binary files

Many text editors won't be able to handle [binary files](https://en.wikipedia.org/wiki/Binary_file). Use a [hex editor](https://wiki.gentoo.org/wiki/Hex_editor) for such files.

If binary data gets improperly output to the terminal, it can sometimes "garble" the display, see [this section](https://wiki.gentoo.org/wiki/Terminal_emulator#Garbled_display) of the [terminal emulator](https://wiki.gentoo.org/wiki/Terminal_emulator) article for help.

## See also

- [Knowledge Base:Edit a configuration file](https://wiki.gentoo.org/wiki/Knowledge_Base:Edit_a_configuration_file)
- [Hex editor](https://wiki.gentoo.org/wiki/Hex_editor) — an application to allow viewing and editing of [binary files](https://en.wikipedia.org/wiki/Binary_file), as opposed to [text files].
- [Pager](https://wiki.gentoo.org/wiki/Pager) — a tool for displaying the contents of files or other output on the terminal, in a user friendly way, across several screens if needed.
