<!-- source: https://wiki.gentoo.org/wiki/Helix | group: Gentoo Wiki (Main) | wiki-title: Helix -->
---
title: Helix
url: https://wiki.gentoo.org/wiki/Helix
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-04-14"
fingerprint: "5609567b33a63fd9"
license: CC BY-SA 4.0
---

# Helix

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**Helix** is a 'Post-Modern' modal [text editor](https://wiki.gentoo.org/wiki/Text_editor) based on [Neovim](https://wiki.gentoo.org/wiki/Neovim) and [Kakoune](https://wiki.gentoo.org/wiki/Kakoune) that is written in [Rust](https://wiki.gentoo.org/wiki/Rust). Helix comes with a built-in language server, multiple selections, and smart syntax highlighting.

## Installation

### USE flags


### Emerge

`root #``emerge --ask app-editors/helix`
## Configuration

Helix uses the [TOML](https://en.wikipedia.org/wiki/TOML) format for configuration, making the configuration much easier and cleaner than comparable text editors. The configuration is stored at \~/.config/helix/config.toml.

The runtime directory is installed into /usr/share/helix/runtime, and needs to be linked to the configuration of the user accounts running helix:

`user $``ln -s /usr/share/helix/runtime ~/.config/helix/runtime`
## Usage

Helix can be started with hx:

`user $``hx`
## Themes

Helix already has a large amount of themes built-in. Setting the theme can be done during a session with `:theme catppucin_latte` or inside the configuration file:

**`~/.config/helix/config.toml`**

```
theme = "catppuccin_frappe"
```
## Language support

Helix support languages through LSP with [several LSP already pre-configured](http://docs.helix-editor.com/lang-support.html).

The server for the corresponding language can be installed by placing the executable in `PATH`. For example, marksman for markdown will allow the user to jump between headings. This package is not available yet on Gentoo but a binary can be found [on Github](https://github.com/artempyanykh/marksman/releases).

Configuration can be done in languages.toml. For example, setting [texlab](https://github.com/latex-lsp/texlab) with [tectonic](http://tectonic-typesetting.github.io/en-US/index.html) (both not yet packaged by Gentoo but easily installed):

**`~/.config/helix/languages.toml`**

```
[language-server]
texlab = {  command = "texlab",  config = {  texlab.build = {  executable = "tectonic",  args = ["-X", "compile", "%f", "--synctex", "--keep-logs", "--keep-intermediates" ] } } } , onSave  = true } }
```
}

Using a LSP in [LaTeX](https://wiki.gentoo.org/wiki/LaTeX) allows for example to have a completion upon the bibliography with the \cite{} command.

## See Also

- [Vim](https://wiki.gentoo.org/wiki/Vim) — a [vi](https://wiki.gentoo.org/wiki/Vi)-like [text editor](https://wiki.gentoo.org/wiki/Text_editor), originally descended from the [Stevie](<https://en.wikipedia.org/wiki/Stevie_(text_editor)>) vi clone.
- [Neovim](https://wiki.gentoo.org/wiki/Neovim) — a [vi](https://wiki.gentoo.org/wiki/Vi)-like [text editor](https://wiki.gentoo.org/wiki/Text_editor) designed around a relatively small but extendable core that aims to be a versatile and powerful text editor
- [Kakoune](https://wiki.gentoo.org/wiki/Kakoune) — a modern, actively developed [editor](https://wiki.gentoo.org/wiki/Text_editor) for the [command line](https://wiki.gentoo.org/wiki/Shell), inspired by [vi](https://wiki.gentoo.org/wiki/Vi).
