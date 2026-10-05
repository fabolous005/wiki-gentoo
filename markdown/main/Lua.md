<!-- source: https://wiki.gentoo.org/wiki/Lua | group: Gentoo Wiki (Main) | wiki-title: Lua -->
---
title: Lua
url: https://wiki.gentoo.org/wiki/Lua
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2023-11-27"
fingerprint: feee5f2f8db3aedf
license: CC BY-SA 4.0
---

# Lua

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**Lua** is a powerful light-weight programming language designed for extending applications. It can be embedded into various application as scripting language.

## Installation

### USE flags


### Emerge

`root #``emerge --ask dev-lang/lua`
## Configuration

### Enabling different Lua versions

The [app-eselect/eselect-lua](https://packages.gentoo.org/packages/app-eselect/eselect-lua) package provides an [eselect](https://wiki.gentoo.org/wiki/Eselect) module to switch between different Lua slots. Once this package has been installed, list available Lua versions by running:

`user $``eselect lua list`
To enable the version designated by number n in the list, use `lua set`:

`user $``eselect lua set n`
