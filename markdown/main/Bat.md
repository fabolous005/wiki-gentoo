<!-- source: https://wiki.gentoo.org/wiki/Bat | group: Gentoo Wiki (Main) | wiki-title: Bat -->
---
title: bat
url: https://wiki.gentoo.org/wiki/Bat
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2025-06-16"
fingerprint: c2a0497f13a6fbda
license: CC BY-SA 4.0
---

# bat

From Gentoo Wiki

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)


**bat** is a cat(1) clone with syntax highlighting and [Git](https://wiki.gentoo.org/wiki/Git) integration.

## Installation

### USE flags


### USE flags for
            [sys-apps/bat](https://packages.gentoo.org/packages/sys-apps/bat)
            
            cat(1) clone with syntax highlighting and Git integration

| [debug](https://packages.gentoo.org/useflags/debug) | Enable extra debug codepaths, like asserts and extra output. If you want to get meaningful backtraces see https://wiki.gentoo.org/wiki/Project:Quality\_Assurance/Backtraces | 

### Emerge

`root #``emerge --ask sys-apps/bat`
## Usage

### Printing a file

To print the contents of a file, run bat:

`user $``bat file.txt`
### Listing languages

To list available languages, use `-L` or `--list-languages`:

`user $``bat --list-languages`
### Selecting a language

To select a language type of the file, use the `-l` or `--language` arguments:

`user $``bat --language py script`
## Configuration

#### Default config

To generate the default config one can run

`user $``bat --generate-config-file`


#### Use terminal colors

To make bat use terminal colors instead of it's built-in colorscheme one can use

`user $``bat --theme ansi`
or

Set the theme in the config file

FILE **`~/.config/bat/config`****bat config**

```
--theme="ansi"
```


#### Make bat work more like cat

Set the pager to never and the style to plain in the config file

FILE **`~/.config/bat/config`****bat config**

```
--pager=never
--style=plain
```
