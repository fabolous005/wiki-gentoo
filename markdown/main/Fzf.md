<!-- source: https://wiki.gentoo.org/wiki/Fzf | group: Gentoo Wiki (Main) | wiki-title: Fzf -->
---
title: fzf
url: https://wiki.gentoo.org/wiki/Fzf
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-09-20"
fingerprint: "944298dea285cb96"
license: CC BY-SA 4.0
---

# fzf

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)


**fzf** is an interactive fuzzy finder for the command-line that can be used with any list of data.

## Installation

### Emerge

`root #``emerge --ask app-shells/fzf`
### Integration with Bash

To integrate fzf into Bash, the following line may be appended to \~/.bashrc (for the current user) or /etc/bash/bashrc (for all users):

**`~/.bashrc`**

```
eval "$(fzf --bash)"
```
## Usage

### Finding a string in a filename

To find a string in a filename, run fzf and begin typing in the TUI to find the string:

`user $``fzf`
### Exact matching

The `-e` / `--exact` option can be used to tell fzf to only report exact matches:

`user $``fzf --exact`
### File search with previews

To show a preview of a file's content during search, the `--preview` flag can be used:

`user $``fzf --preview="cat {}"`
The selected file may also be opened immediately afterwards in a text editor like [vim](https://wiki.gentoo.org/wiki/Vim):

`user $``vim $(fzf --preview="cat {}")`
### Usage with Bash

After enabling Bash-integration, fzf can be invoked in various ways for searching, for example:

- `ctrl` + `r`: Search the shell command history.
- `alt` + `c`: Search directory-names in the current path, then change into the selected one.
- `ctrl` + `t`: Opens fzf's file picker, allowing fast tabcompletion


fzf can also be used to search through entries of [ssh](https://wiki.gentoo.org/wiki/Ssh)'s known\_hosts and /etc/hosts by typing \*\* + `tab`:

`user $``ssh **`
When used together with other commands (like less or cd), the normal file picker will be opened.

## See also

- [find](https://wiki.gentoo.org/wiki/Find) — a utility to search for files in a directory hierarchy.
