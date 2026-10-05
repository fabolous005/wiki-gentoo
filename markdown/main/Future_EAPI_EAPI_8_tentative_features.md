<!-- source: https://wiki.gentoo.org/wiki/Future_EAPI/EAPI_8_tentative_features | group: Gentoo Wiki (Main) | wiki-title: Future EAPI/EAPI 8 tentative features -->
---
title: Future EAPI/EAPI 8 tentative features
url: https://wiki.gentoo.org/wiki/Future_EAPI/EAPI_8_tentative_features
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2024-11-12"
fingerprint: "3ee53e7b9402ea7c"
license: CC BY-SA 4.0
---

# Future EAPI/EAPI 8 tentative features

From Gentoo Wiki

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

This is a working page that contains references to all features that have been suggested for EAPI 8.

The following list of features accepted for or rejected from EAPI 8 is based on Gentoo Council meetings of 2020-11-08 and 2021-06-13.

## Accepted

| Feature | Bug | Spec | Implementation |  | Notes | 
|---|---|---|---|---|---|
|  |  |  | Portage | pkgcore |  | 
| New features |  |  |  |  |  | 
| Selective fetch restriction | [bug #371413](https://bugs.gentoo.org/show_bug.cgi?id=371413) |  |  |  |  | 
| Install-time CBUILD dependencies | [bug #660306](https://bugs.gentoo.org/show_bug.cgi?id=660306) |  |  |  |  | 
| Enhancements of existing features |  |  |  |  |  | 
| Pass `--datarootdir` to configure | [bug #651958](https://bugs.gentoo.org/show_bug.cgi?id=651958) |  |  |  |  | 
| Pass `--disable-static` to configure | [bug #744871](https://bugs.gentoo.org/show_bug.cgi?id=744871) |  |  |  |  | 
| Accumulate PROPERTIES & RESTRICT over eclasses and ebuilds | [bug #701132](https://bugs.gentoo.org/show_bug.cgi?id=701132) |  |  |  |  | 
| `dosym -r` to create symlinks relative to link location | [bug #708360](https://bugs.gentoo.org/show_bug.cgi?id=708360) |  |  |  |  | 
| Second optional argument for `usev` | [bug #744868](https://bugs.gentoo.org/show_bug.cgi?id=744868) |  |  |  |  | 
| Empty working directory in `pkg_*` phases | [bug #595030](https://bugs.gentoo.org/show_bug.cgi?id=595030) |  |  |  |  | 
| Other changes |  |  |  |  |  | 
| Less strict naming rules for files in updates directory | [bug #692774](https://bugs.gentoo.org/show_bug.cgi?id=692774) |  |  |  |  | 
| Bash 5.0 | [bug #636652](https://bugs.gentoo.org/show_bug.cgi?id=636652) |  |  |  |  | 
| Default `src_prepare` accepts only file names in `PATCHES` | [bug #752486](https://bugs.gentoo.org/show_bug.cgi?id=752486) |  |  |  |  | 
| More consistent `insopts`/`exeopts` | [bug #657580](https://bugs.gentoo.org/show_bug.cgi?id=657580) |  |  |  |  | 
| Removals and bans |  |  |  |  |  | 
| `unpack`: Remove support for 7-Zip, RAR, and LHA | [bug #690968](https://bugs.gentoo.org/show_bug.cgi?id=690968) |  |  |  |  | 
| Ban `useq`, `hasq`, and `hasv` functions | [bug #199722](https://bugs.gentoo.org/show_bug.cgi?id=199722) |  |  |  |  | 

## Not accepted

| Feature | Bug | Spec | Implementation |  | Notes | 
|---|---|---|---|---|---|
|  |  |  | Portage | pkgcore |  | 
| Enhancements of existing features |  |  |  |  |  | 
| Variant of `\|\| ( )` with defined runtime behaviour | [bug #489458](https://bugs.gentoo.org/show_bug.cgi?id=489458) | [EAPI 7 commit](https://gitweb.gentoo.org/proj/pms.git/commit/?h=deferred-7&id=dc94676f869c8449417afdb4d101613b346901a2) |  |  | From original EAPI 6 (and 7) feature list | 
| RESTRICT value for network-restricted tests | [bug #553696](https://bugs.gentoo.org/show_bug.cgi?id=553696) |  |  |  | Added retroactively as an optional PROPERTIES token |
