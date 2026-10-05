<!-- source: https://wiki.gentoo.org/wiki/Live_ebuilds | group: Gentoo Wiki (Main) | wiki-title: Live ebuilds -->
---
title: Live ebuilds
url: https://wiki.gentoo.org/wiki/Live_ebuilds
hostname: gentoo.org
sitename: wiki.gentoo.org
date: "2026-01-24"
fingerprint: "4f389a2f95c1872e"
license: CC BY-SA 4.0
---

# Live ebuilds

[Jump to:navigation](https://wiki.gentoo.org#mw-head)

[Jump to:search](https://wiki.gentoo.org#searchInput)

Live ebuilds are ebuilds with a version number of 9999. These ebuilds fetch updates directly from the upstream project's source code repository and will build the associated package from the latest commit.

## Live ebuilds are different than regular ebuilds

Most Gentoo systems are built entirely from **versioned** ebuilds, which build **one specific version** of a package. This is one meaning of the word *stable*: unchanging and predictable results (USE flags not withstanding) from a single ebuild.

Conversely, a *live* ebuild has a fake version number of 9999. Instead of building from a specific version, it always fetches the very latest source (sometimes called *tip of tree*) from the package's upstream repository. This version could change from one minute to the next, depending on activity within the upstream project.

This is what the above warning means by "extremely unstable": not necessarily buggy or broken, but **unpredictable**. Because it is impossible to know the state of the build ahead of time, a live ebuild can't be *tested* in the same way a versioned ebuild is tested.

For example, the upstream project may make a change which requires a different version of one of its dependencies, or a new dependency altogether. If the maintainer of the Gentoo ebuild has not yet had a chance to reflect this change in the ebuild, there will likely be problems with building or running the package.

As noted below, packages installed from live ebuilds are not routinely updated with emerge --update

For these reasons, using live ebuilds should be done with careful consideration of the ramifications, or used when advised by someone who has made these considerations.

## Unmasking live ebuilds

### A single package

Live builds will require manual unmasking in order to be installed by the package manager. For example, to unmask the live version of [app-emulation/wine-staging](https://packages.gentoo.org/packages/app-emulation/wine-staging), the following

**`/etc/portage/package.accept_keywords`**

**Unmasking a single live ebuilds**

```
# Note: Accepting all keywords removes all levels of quality assurance from the system
# and is likely to result in build failures, unstable software, or other problems.
# Here be dragons! Proceed keeping these risks in mind...
app-emulation/wine-staging **
```
### System wide

While possible to set system wide, there is very little to gain from doing so, as even the bug reports produced have so many unknown quirks that it makes them unreliable.

Consider this as a banned practice in Gentoo.

## Updating live packages

All presently installed live packages can be updated by re-installing via the @live-rebuild set. This is a crude and effective approach, since Portage will re-pull and then recompile each of the packages in the set, which take a considerable amount of time.

`root #``emerge --ask @live-rebuild`
### Smart live rebuild

The [app-portage/smart-live-rebuild](https://packages.gentoo.org/packages/app-portage/smart-live-rebuild) package provides a framework which will intelligently *pull* updates from each upstream source code repository for each live package installed on the system. It will only update packages which have a newer set of changes available upstream, therefore avoiding the crude approach of having to recompile all packages within the @live-rebuild set.

## See also

- [/etc/portage/package.accept\_keywords](https://wiki.gentoo.org/wiki//etc/portage/package.accept_keywords) — files or directories of files containing definitions for per-package `ACCEPT_KEYWORDS` statements.
- [ACCEPT\_KEYWORDS](https://wiki.gentoo.org/wiki/ACCEPT_KEYWORDS)
- [Bisecting\_with\_live\_ebuilds](https://wiki.gentoo.org/wiki/Bisecting_with_live_ebuilds) — Using live ebuilds and git bisect can be enormously helpful for debugging issues.
