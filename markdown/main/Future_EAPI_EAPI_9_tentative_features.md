<!-- source: https://wiki.gentoo.org/wiki/Future_EAPI/EAPI_9_tentative_features | group: Gentoo Wiki (Main) | wiki-title: Future EAPI/EAPI 9 tentative features -->
---
title: Future EAPI/EAPI 9 tentative features
url: https://wiki.gentoo.org/wiki/Future_EAPI/EAPI_9_tentative_features
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-06-20"
fingerprint: "3eafde7f9062ff7c"
license: CC BY-SA 4.0
---

# Future EAPI/EAPI 9 tentative features

From Gentoo Wiki

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

This is a working page that contains references to all features that have been suggested for EAPI 9.

## Accepted

The following list of features accepted for EAPI 9 is based on the [Gentoo Council meetings](https://wiki.gentoo.org/wiki/Project:Council/Meeting_logs) of 2022-02-13, 2024-12-08, 2025-01-12, 2025-02-09, 2025-05-11 and 2025-06-08.

| Feature | Bug | Spec | Implementation |  | Notes | 
|---|---|---|---|---|---|
|  |  |  | Portage | Pkgcore |  | 
| New features |  |  |  |  |  | 
| `use.stable` and `package.use.stable` | [bug #955833](https://bugs.gentoo.org/show_bug.cgi?id=955833) | done | done | done |  | 
| `pipestatus` | [bug #566342](https://bugs.gentoo.org/show_bug.cgi?id=566342) | done | done | done |  | 
| `ver_replacing` | [bug #947530](https://bugs.gentoo.org/show_bug.cgi?id=947530) | done | done | done |  | 
| `edo` | [bug #744880](https://bugs.gentoo.org/show_bug.cgi?id=744880) | done | done | done |  | 
| Other changes |  |  |  |  |  | 
| EAPI of profiles defaults to repository EAPI | [bug #806181](https://bugs.gentoo.org/show_bug.cgi?id=806181) | done | done | done |  | 
| Bash 5.3 | [bug #946193](https://bugs.gentoo.org/show_bug.cgi?id=946193) | done | done | done |  | 
| Variables no longer exported | [bug #721088](https://bugs.gentoo.org/show_bug.cgi?id=721088) [bug #948001](https://bugs.gentoo.org/show_bug.cgi?id=948001) | done | done | done |  | 
| No longer rewrite absolute symlinks | [bug #934514](https://bugs.gentoo.org/show_bug.cgi?id=934514) | done | done | done |  | 
| econf: Ensure proper end of string in `configure --help` output | [bug #815169](https://bugs.gentoo.org/show_bug.cgi?id=815169) | dropped | partially | partially | Added retroactively for option names beginning with `with-`, `disable-` or `enable-` | 
| Removals and bans |  |  |  |  |  | 
| Ban `assert` | [bug #566342](https://bugs.gentoo.org/show_bug.cgi?id=566342) | done | done | done |  | 
| Ban `domo` | [bug #951502](https://bugs.gentoo.org/show_bug.cgi?id=951502) | done | done | done |  | 

## Not accepted

| Feature | Bug | Spec | Implementation |  | Notes | 
|---|---|---|---|---|---|
|  |  |  | Portage | Pkgcore |  | 
| New features |  |  |  |  |  | 
| Eclass revisions | [bug #806592](https://bugs.gentoo.org/show_bug.cgi?id=806592) | not done | not done | not done |  | 
| `edov` | [bug #744880](https://bugs.gentoo.org/show_bug.cgi?id=744880) | not done | not done | not done |  |
