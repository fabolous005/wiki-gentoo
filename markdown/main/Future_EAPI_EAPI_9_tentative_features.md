<!-- source: https://wiki.gentoo.org/wiki/Future_EAPI/EAPI_9_tentative_features | group: Gentoo Wiki (Main) | wiki-title: Future EAPI/EAPI 9 tentative features -->
---
title: Future EAPI/EAPI 9 tentative features
url: https://wiki.gentoo.org/wiki/Future_EAPI/EAPI_9_tentative_features
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-06-20"
fingerprint: "3e6f9e7f8262f95c"
license: CC BY-SA 4.0
---

# Future EAPI/EAPI 9 tentative features

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

This is a working page that contains references to all features that have been suggested for EAPI 9.

## Accepted

The following list of features accepted for EAPI 9 is based on the [Gentoo Council meetings](https://wiki.gentoo.org/wiki/Project:Council/Meeting_logs) of 2022-02-13, 2024-12-08, 2025-01-12, 2025-02-09, 2025-05-11 and 2025-06-08.

| Feature | Bug | Spec | Implementation |  | Notes | 
|---|---|---|---|---|---|
|  |  |  | Portage | Pkgcore |  | 
| New features |  |  |  |  |  | 
| `use.stable` and `package.use.stable` | [bug #955833](https://bugs.gentoo.org/show_bug.cgi?id=955833) |  |  |  |  | 
| `pipestatus` | [bug #566342](https://bugs.gentoo.org/show_bug.cgi?id=566342) |  |  |  |  | 
| `ver_replacing` | [bug #947530](https://bugs.gentoo.org/show_bug.cgi?id=947530) |  |  |  |  | 
| `edo` | [bug #744880](https://bugs.gentoo.org/show_bug.cgi?id=744880) |  |  |  |  | 
| Other changes |  |  |  |  |  | 
| EAPI of profiles defaults to repository EAPI | [bug #806181](https://bugs.gentoo.org/show_bug.cgi?id=806181) |  |  |  |  | 
| Bash 5.3 | [bug #946193](https://bugs.gentoo.org/show_bug.cgi?id=946193) |  |  |  |  | 
| Variables no longer exported | [bug #721088](https://bugs.gentoo.org/show_bug.cgi?id=721088) [bug #948001](https://bugs.gentoo.org/show_bug.cgi?id=948001) |  |  |  |  | 
| No longer rewrite absolute symlinks | [bug #934514](https://bugs.gentoo.org/show_bug.cgi?id=934514) |  |  |  |  | 
| econf: Ensure proper end of string in `configure --help` output | [bug #815169](https://bugs.gentoo.org/show_bug.cgi?id=815169) |  |  |  | Added retroactively for option names beginning with `with-`, `disable-` or `enable-` | 
| Removals and bans |  |  |  |  |  | 
| Ban `assert` | [bug #566342](https://bugs.gentoo.org/show_bug.cgi?id=566342) |  |  |  |  | 
| Ban `domo` | [bug #951502](https://bugs.gentoo.org/show_bug.cgi?id=951502) |  |  |  |  | 

## Not accepted

| Feature | Bug | Spec | Implementation |  | Notes | 
|---|---|---|---|---|---|
|  |  |  | Portage | Pkgcore |  | 
| New features |  |  |  |  |  | 
| Eclass revisions | [bug #806592](https://bugs.gentoo.org/show_bug.cgi?id=806592) |  |  |  |  | 
| `edov` | [bug #744880](https://bugs.gentoo.org/show_bug.cgi?id=744880) |  |  |  |  |
