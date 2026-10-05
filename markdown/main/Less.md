<!-- source: https://wiki.gentoo.org/wiki/Less | group: Gentoo Wiki (Main) | wiki-title: Less -->
---
title: less
url: https://wiki.gentoo.org/wiki/Less
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2023-12-25"
fingerprint: f2035b2a03851b96
license: CC BY-SA 4.0
---

# less

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)


**less** is a [pager](https://wiki.gentoo.org/wiki/Pager) for displaying text files.

## Installation

### USE flags


| [pcre](https://packages.gentoo.org/useflags/pcre) | Add support for Perl Compatible Regular Expressions | 
| [test](https://packages.gentoo.org/useflags/test) | Enable dependencies and/or preparations necessary to run tests (usually controlled by FEATURES=test but can be toggled independently) | 
| [verify-sig](https://packages.gentoo.org/useflags/verify-sig) | Verify upstream signatures on distfiles | 

### Emerge

**less** is part of [the @system set](<https://wiki.gentoo.org/wiki/System_set_(Portage)>), so is installed by default.

`root #``emerge --ask sys-apps/less`
## Configuration

**less** is configured via various environment variables and files. For detailed information, refer to the [less(1)](https://man.archlinux.org/man/less.1.en) [man page.](https://wiki.gentoo.org/wiki/Special:MyLanguage/man_page)

| Variable | Description | 
|---|---|
| `LESS` | Command-line options to pass to every invocation of **less**. | 
| `LESSHISTFILE` | Path to the file in which store a history of commands issued while using **less**. By default, this file will be one of $XDG\_STATE\_HOME/lesshst, $HOME/.local/state/lesshst, $XDG\_DATA\_HOME/lesshst or $HOME/.lesshst. | 
| `LESSKEYIN` | Path to a file containing custom keybinding definitions, as described in the [lesskey(1)](https://man.archlinux.org/man/lesskey.1.en) [man page.](https://wiki.gentoo.org/wiki/Special:MyLanguage/man_page)  | 

## Usage

`user $``less file.txt`
To view line `n` of the input, provide an argument of `+`:
`n`

`user $``less +40 file.txt`
There are many bindings available for movement within **less**; refer to the [less(1)](https://man.archlinux.org/man/less.1.en) [man page for details. Some basic keybindings:](https://wiki.gentoo.org/wiki/Special:MyLanguage/man_page)

- To move line-by-line, use the vi keys (`j` to move down, `k` to move up) or the arrow keys.

- To move down a page, use `Space`.

- To jump to the beginning, use `g`; to jump to the end, use `G`.

To search for text within **less**, type `/`, followed by `textEnter`. For example, to search for the text "less", type `/less` `Enter`; if the text is found after the current position of the cursor, it will be highlighted. To search for the next match, type `n`. To clear highlighting of matches, type `ESCu`. Note that search is case-sensitive by default; this can be changed by passing the `-I` / `--IGNORE-CASE` option to the **less** command.

To access a summary of **less** commands, type `h`.

To exit **less**, type `q`.

By default, input to **less** is piped through [lesspipe(1)](https://man.archlinux.org/man/lesspipe.1.en) [before being displayed.](https://wiki.gentoo.org/wiki/Special:MyLanguage/man_page) **lesspipe** performs various types of preprocessing, based on the input contents (e.g. if the input is HTML), in order to display them appropriately. To disable this, pass the `-L` / `--no-lessopen` option to the **less** command. Alternatively, a different preprocessor can be specified via the `LESSOPEN` environment variable.

## See also

- The [less(1)](https://man.archlinux.org/man/less.1.en)
