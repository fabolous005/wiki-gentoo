<!-- source: https://wiki.gentoo.org/wiki//etc/portage/color.map | group: Gentoo Wiki (Main) | wiki-title: /etc/portage/color.map -->
---
title: "/etc/portage/color.map"
url: https://wiki.gentoo.org/wiki//etc/portage/color.map
hostname: gentoo.org
sitename: "/etc/portage/color.map"
date: "2022-05-12"
fingerprint: "4621d63e2499d22f"
license: CC BY-SA 4.0
---

# /etc/portage/color.map

From Gentoo Wiki

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

**/etc/portage/color.map** is a file containing variables that define color classes used by Portage. Portage will check this file first for color class settings. If no color class has been set in /etc/portage/color.map, Portage uses default values defined internally.

## Format

- Variable = \[space delimited list of attributes or ANSI code pattern\]
- Attribute = \[space delimited list of colors (see below) or ANSI code pattern\]

## Color map defaults

| Variable | Default attribute | Description | 
|---|---|---|
| `NORMAL` | normal | Defines color used for some words occurring in other contexts than those below. | 
| `BAD` | red | Defines color used for some words occurring in bad context. | 
| `BRACKET` | blue | Defines color used for brackets. | 
| `GOOD` | green | Defines color used for some words occurring in good context. | 
| `HILITE` | teal | Defines color used for highlighted words. | 
| `INFORM` | darkgreen | Defines color used for informational words. | 
| `MERGE_LIST_PROGRESS` | yellow | Defines color used for numbers indicating merge progress. | 
| `PKG_BLOCKER` | red | Defines color used for unsatisfied blockers. | 
| `PKG_BLOCKER_SATISFIED` | darkblue | Defines color used for satisfied blockers. | 
| `PKG_MERGE` | darkgreen | Defines color used for packages planned to be merged. | 
| `PKG_MERGE_SYSTEM` | darkgreen | Defines color used for system packages planned to be merged. | 
| `PKG_MERGE_WORLD` | green | Defines color used for world packages planned to be merged. | 
| `PKG_BINARY_MERGE` | purple | Defines color used for packages planned to be merged using a binary package. | 
| `PKG_BINARY_MERGE_SYSTEM` | purple | Defines color used for system packages planned to be merged using a binary package. | 
| `PKG_BINARY_MERGE_WORLD` | fuchsia | Defines color used for world packages planned to be merged using a binary package. | 
| `PKG_NOMERGE` | darkblue | Defines color used for packages not planned to be merged. | 
| `PKG_NOMERGE_SYSTEM` | darkblue | Defines color used for system packages not planned to be merged. | 
| `PKG_NOMERGE_WORLD` | blue | Defines color used for world packages not planned to be merged. | 
| `PKG_UNINSTALL` | red | Defines color used for packages planned to be uninstalled in order to resolve conflicts. | 
| `PROMPT_CHOICE_DEFAULT` | green | Defines color used for the default choice at a prompt. | 
| `PROMPT_CHOICE_OTHER` | red | Defines color used for a non-default choice at a prompt. | 
| `SECURITY_WARN` | red | Defines color used for security warnings. | 
| `UNMERGE_WARN` | red | Defines color used for unmerge warnings. | 
| `WARN` | yellow | Defines color used for warnings. | 

## Attributes

| Foreground colors | Background colors | Other | 
|---|---|---|
| black | bg\_black | normal | 
| darkgray | bg\_darkred | no-attr | 
| darkred | bg\_darkgreen | reset | 
| red | bg\_brown = bg\_darkyellow | faint | 
| darkgreen | bg\_darkblue | standout | 
| green | bg\_purple | no-standout | 
| brown | bg\_teal | underline | 
| yellow | bg\_lightgray | no-underline | 
| darkyellow |  | blink | 
| darkblue |  | no-blink | 
| blue |  | overline | 
| purple |  | no-overline | 
| fuchsia |  | reverse | 
| teal |  | no-reverse | 
| turquoise = darkteal |  | invisible | 
| lightgray |  |  | 
| white |  |  | 

## External resources

[COLOR.MAP](https://dev.gentoo.org/~zmedico/portage/doc/man/color.map.5.html) man page.
