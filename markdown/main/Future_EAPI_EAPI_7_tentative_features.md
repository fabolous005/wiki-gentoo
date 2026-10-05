<!-- source: https://wiki.gentoo.org/wiki/Future_EAPI/EAPI_7_tentative_features | group: Gentoo Wiki (Main) | wiki-title: Future EAPI/EAPI 7 tentative features -->
---
title: Future EAPI/EAPI 7 tentative features
url: https://wiki.gentoo.org/wiki/Future_EAPI/EAPI_7_tentative_features
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2024-11-12"
fingerprint: "1e8d9f7be4608e68"
license: CC BY-SA 4.0
---

# Future EAPI/EAPI 7 tentative features

From Gentoo Wiki

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

This is a working page that contains references to all features that have been suggested for EAPI 7.

The following list of features accepted or not accepted for EAPI 7 is based on Gentoo Council meetings of [2017-11-12](https://projects.gentoo.org/council/meeting-logs/20171112-summary.txt) and [2018-04-08](https://projects.gentoo.org/council/meeting-logs/20180408-summary.txt).

## Accepted

### New features

- `BDEPEND` and `SYSROOT`
  - [bug #317337](https://bugs.gentoo.org/show_bug.cgi?id=317337)
    - Rejected from EAPI 6 (called `HDEPEND` there; now heavily modified)

- Profile-defined unsetting of variables (`ENV_UNSET`)

- New `eqawarn` command

- Controllable stripping and `dostrip`

- Functions for version comparison and version component expansion

### Enhancements of existing features

- Directory support for profiles/package.mask
  - [bug #282296](https://bugs.gentoo.org/show_bug.cgi?id=282296)
    - Not intended for gentoo-x86 tree, only to be used in overlays
    - From original EAPI 6 feature list

- Directory support for profile files
  - [bug #282296](https://bugs.gentoo.org/show_bug.cgi?id=282296)
    - Not intended for gentoo-x86 tree, only to be used in overlays
    - From original EAPI 6 feature list

- Implement `nonfatal` as both a function and an external command

- Allow `die` in subshell/subcommand

### Other changes

- Empty `|| ( )` and `^^ ( )` groups no longer count as being matched

- Remove trailing slash from `{,E}ROOT` and `{,E}D`

- Require GNU patch 2.7

- Require `einfo` and other output functions not to pollute stdout

- Make `domo` install to /usr instead of `DESTTREE`

### Removals and bans

- Ban package.provided in profiles

- Ban `PORTDIR` and `ECLASSDIR` variables

- Ban `DESTTREE` and `INSDESTTREE` variables

- Ban `dohtml` function
  - [bug #520546](https://bugs.gentoo.org/show_bug.cgi?id=520546)
    - From original EAPI 6 feature list; `dohtml` was deprecated in EAPI 6

- Ban `dolib` and `libopts` commands

## Not accepted

- Bash 4.3

- Runtime-switchable USE flags
  - [bug #424283](https://bugs.gentoo.org/show_bug.cgi?id=424283)
    - From original EAPI 6 feature list

- Variant of `|| ( )` with defined runtime behaviour
  - [bug #489458](https://bugs.gentoo.org/show_bug.cgi?id=489458)
    - From original EAPI 6 feature list

- Automatic use enforcing ([GLEP 73](https://www.gentoo.org/glep/glep-0073.html))

- Sandbox control (`sandbox{off,save,restore}`)
